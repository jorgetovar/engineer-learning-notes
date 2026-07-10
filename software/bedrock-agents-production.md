# Building Production AI Agents on AWS Bedrock — Architecture and Code Decisions Worth Keeping in Mind

## The Context

Models are stateless. They process one request, produce a text result, and forget. They don't take actions, don't integrate by default with your data, or coordinate multi-step workflows on their own. Agents solve this by wrapping a model in a runtime system that gives it tools, memory, and a reasoning loop.

This application is a serverless rental management bot — landlords send natural-language messages and receive notifications through Telegram and email. The best part is that analysis of current payments, debts, and trends is done by an agent powered by Claude. It gives you insights about your data, it remembers your preferences, it's a real collaborator. That's what every agent can do. A chatbot is just text with no deep integrations with your data or systems — by definition, less capable.

Code on GitHub: https://github.com/jorgetovar/whatsapp-rental-manager

Agents are easy to build but not simple. We should keep in mind everything we've always done in backend systems and production applications: security, integrations, cohesion, coupling, data management, scalability, reliability, costs, operational excellence, latency and performance — all the non-functional requirements still apply.

---

## Architecture Learnings

### 1. Bedrock over calling the model API directly

The operational answer: IAM authentication. There's no API key to rotate, leak, or store in Secrets Manager. The same boto3 session pattern you use for DynamoDB works for Bedrock. Cross-region inference (`us.anthropic.claude-sonnet-4-6`) routes to the lowest-latency US region automatically and gives higher throughput limits than a single-region endpoint.

```python
def create_agent(model_id: str = None, extra_context: str = "") -> Agent:
    model_id = model_id or os.environ.get("BEDROCK_MODEL_ID", "us.anthropic.claude-sonnet-4-6")
    boto_session = boto3.Session(
        profile_name=os.environ.get("AWS_PROFILE"),
        region_name=os.environ.get("AWS_REGION_NAME", "us-east-1"),
    )
    model = BedrockModel(model_id=model_id, boto_session=boto_session)
    return Agent(model=model, system_prompt=SYSTEM_PROMPT + extra_context, tools=[...])
```

### 2. Agent memory

Lambda is stateless. Every invocation starts cold. To give Claude memory of the conversation, history must be persisted externally and rehydrated on each request.

I'm using DynamoDB to keep track of messages, but there are better options — AWS Strands has built-in session handling that can summarize history or keep an exact number of messages, which is what we're doing right now.

```python
_MAX_MESSAGES = 20    # ~10 turns — enough context, safe DynamoDB item size
_TTL_SECONDS = 86400  # 24h — conversations reset overnight

def save_history(chat_id: str, messages: list) -> None:
    trimmed = _sanitize_messages(_strip_metadata(messages)[-_MAX_MESSAGES:])
    get_table("conversations").put_item(Item={
        "chatId": str(chat_id),
        "messages": json.dumps(trimmed),
        "ttl": int(time.time()) + _TTL_SECONDS,
    })
```

DynamoDB TTL does the cleanup automatically — no cron job, no cost.

### 3. Dynamic context injection vs. conversation history

There are two ways to give the model current state: inject it into the system prompt per request, or let it accumulate in conversation history.

Conversation history is unreliable for state. It gets trimmed. It contains noise. The model's attention decays over long contexts. If the landlord added a new property three turns ago, you can't guarantee the model remembers it now.

The pattern that works:

```python
def _build_user_context(caller: dict) -> str:
    landlord = get_table("landlords").get_item(Key={"landlordId": landlord_id}).get("Item")
    active = sum(1 for l in leases if l.get("status") == "active")
    return (
        "\n\nESTADO DEL USUARIO (fijado por el sistema):\n"
        f"- Rol: ARRENDADOR — {name}\n"
        f"- Inmuebles activos: {active}\n"
        f"- Ya aceptó política de datos. NO repitas bienvenida."
    )

agent = create_agent(extra_context=user_ctx)
agent.messages = load_history(chat_id)
response = agent(text)
```

### 4. Tool design: the docstring is the API contract

With the Strands SDK, tools are Python functions decorated with `@tool`. The docstring is not documentation — it's the API contract with the model. Be explicit. This steers the agent in the right direction and produces reliable tool calls.

```python
@tool
def log_payment(
    tenant_name_or_phone: str,
    amount: int,
    period: str = "",
    method: str = "other",
) -> str:
    """Registra un pago recibido de un inquilino.
    Ej: tenant_name_or_phone='Juan', amount=800000.
    period en YYYY-MM (vacío = mes actual).
    method: nequi/daviplata/efectivo/other
    """
    ...
```

When the user says "Juan pagó 800k", Claude infers `log_payment(tenant_name_or_phone="Juan", amount=800000)` because the docstring is explicit about what each parameter means. Vague docstrings produce wrong tool calls.

### 5. Security in agents: the model is not the trust boundary

The most dangerous pattern in agent design: letting the model decide who the user is or what they're allowed to do.

In this system, identity comes from the phone number, which Telegram provides on every webhook. Role is resolved from DynamoDB by phone — never from model input or output.

```python
def resolve_caller_sync(phone: str) -> list[CallerContext]:
    contexts: list[CallerContext] = []

    # Check if registered landlord
    resp = landlords_table.get_item(Key={"landlordId": phone})
    if resp.get("Item"):
        contexts.append(CallerContext(role="landlord", landlordId=phone))

    # Check if active renter
    for item in paginated_query(leases_table, IndexName="tenantPhone-index", ...):
        if item.get("status") == "active":
            contexts.append(CallerContext(role="renter", landlordId=item["landlordId"], ...))

    return contexts
```

It's the same principle as a traditional backend application where a JWT token travels in every request and you extract the claims to get the `customer_id` — you never accept it from the request body, because that would be a spoofing vulnerability.

Webhook requests are also verified with a timing-safe HMAC check before any processing happens:

```python
def _verify_signature(secret: str, header: str) -> bool:
    return hmac.compare_digest(secret, header or "")
```

`hmac.compare_digest` takes constant time regardless of where the strings differ — an attacker can't probe the secret byte-by-byte by measuring response latency.

### 6. Infrastructure as Code

Individual Lambdas get exactly the permissions they need — the late-tenant reminder reads leases and tenants; it can't write. Deployments are a single command and that's it. Anyone can replicate this application easily.

```yaml
Policies:
  - DynamoDBReadPolicy:       # read-only for reminder Lambdas
      TableName: !Ref LeasesTable
  - DynamoDBCrudPolicy:       # full CRUD for the webhook Lambda
      TableName: !Ref PaymentsTable
  - !Ref SesPolicy            # shared managed policy across all reminder functions
```

Secrets Manager dynamic references resolve at deploy time:

```yaml
TELEGRAM_BOT_TOKEN: !Sub '{{resolve:secretsmanager:miarriendobot/telegram:SecretString:token}}'
```

Note: the secret value is baked into the Lambda environment variable at deploy time. Rotating the secret in Secrets Manager does not update the Lambda automatically — you must redeploy or update the environment variable directly.

### 7. Observability

Agents are not deterministic. Now more than ever it's important to understand what's happening in the system — what the integration points are, whether something is failing, how to get notified. Having alarms, tracing, and good logs is always paramount.

One non-obvious gap: the webhook Lambda must always return HTTP 200 — Telegram retries delivery indefinitely on anything else. The side effect is that API Gateway always looks healthy even when every invocation is throwing an exception. Standard uptime monitoring is completely blind here.

```yaml
WebhookErrorAlarm:
  Type: AWS::CloudWatch::Alarm
  Properties:
    MetricName: Errors
    Namespace: AWS/Lambda
    Dimensions:
      - Name: FunctionName
        Value: !Ref WebhookFunction
    Statistic: Sum
    Period: 300
    EvaluationPeriods: 1
    Threshold: 3
    ComparisonOperator: GreaterThanOrEqualToThreshold
    AlarmActions:
      - !Ref AlertTopic
```

A CloudWatch alarm on the Lambda `Errors` metric is the only real signal. Without it, you find out the bot is broken from a user complaint, not a page.

### 8. Data model design for DynamoDB

The denormalization decision is a real trade-off. Storing `canon` directly on the `Lease` even though `Property` also has it means reminder Lambdas fetch one item and have everything — no second read, no join.

```python
class Lease(BaseModel):
    leaseId: str
    landlordId: str
    propertyId: str
    tenantPhone: str
    canon: int      # duplicated from Property — intentional: reminders need one read
    dueDay: int
    startDate: str
    status: str = "active"
```

The explicit cost: `update_canon` must write both tables. DynamoDB is not relational — design for your read patterns, not for normalization.

### 9. Testability

Any job gated on a calendar date is untestable in CI without a bypass parameter. `job_monthly_summary(force=True)` runs immediately regardless of the date. Design for testability from day zero.

```python
def job_late_tenant_reminder(force: bool = False):
    today = datetime.now(timezone.utc)
    for lease in _get_all_active_leases():
        reminder_date = _due_date_this_month(due_day) + timedelta(days=5)
        if not force and today.date() != reminder_date.date():
            continue  # skip — not the right day
        # send reminder...
```

The same function runs locally via APScheduler and in production via EventBridge — no mocking, full parity.

### 10. AWS credentials — the one-liner that works in both environments

```python
boto3.Session(profile_name=os.environ.get("AWS_PROFILE"))
```

This single line works in both local development and Lambda without any branching. The key is `os.environ.get("AWS_PROFILE")` — when `AWS_PROFILE` is not set in Lambda, `.get()` returns `None`, and `boto3.Session(profile_name=None)` correctly falls back to the IAM execution role via IMDS.

Two mistakes to avoid:

- `os.environ.get("AWS_PROFILE", "")` returns `""` instead of `None` — boto3 then tries to find a profile named `""` and fails silently.
- Never set `AWS_PROFILE` as a Lambda environment variable — boto3 will look for a credentials file that doesn't exist in Lambda's runtime.

Locally, your `.env` sets `AWS_PROFILE=default-c1` and the named profile is used. In Lambda, the variable is absent and the execution role takes over automatically. No `if local else lambda` branching, no hardcoded profile names, no credentials in code.

### 11. Context is the most important part of any agent

Tools and good context make agents powerful. We pass just enough — but important — signals that allow for a coherent conversation. The model doesn't need everything; it needs the right things at the right time.

```plaintext
- Rol: ARRENDADOR — Jorge Tovar
- Inmuebles activos: 2
- Ya aceptó política de datos. NO repitas bienvenida.
```

The model knows the user's role, their current state, and what it should not ask for. That's the entire context it needs to behave correctly.

### 12. Never trim conversation history mid-tool-call

Always cut at complete exchange boundaries — never in the middle of a `toolUse`/`toolResult` pair. We shipped without this and Bedrock raised a `ValidationException` at turn 10. The fix walks the trimmed list and drops any incomplete exchange from the tail:

```python
def _sanitize_messages(messages: list) -> list:
    # Drop leading user messages that only contain toolResult blocks
    # (their corresponding assistant toolUse was trimmed away)
    while start < len(messages):
        msg = messages[start]
        has_text = any(isinstance(b, dict) and "text" in b for b in msg.get("content", []))
        if msg.get("role") != "user" or has_text:
            break
        start += 1

    # Drop any assistant toolUse not matched by the next message's toolResult
    if tool_use_ids and not tool_use_ids.issubset(result_ids):
        break  # missing toolResult — drop from here
```

The end of a complete reasoning loop is the only safe trim point.

### 13. Hooks are the preferred and deterministic way to validate

The model shouldn't be responsible for things that can be validated deterministically in code. Request-scoped context is set once at the start of every invocation and propagated automatically to every tool — no threading required, safe because Lambda processes one request at a time per execution environment.

```python
# lambda_handler.py — set before every agent call
CURRENT_CONTEXT.clear()
CURRENT_CONTEXT.update(caller.model_dump())

# any tool — reads from the same dict
def _landlord_id() -> str:
    return CURRENT_CONTEXT.get("landlordId", "")
```

The model receives that string, understands it's a business rule, and responds to the user naturally — "Sorry, that feature is only available for landlords."

### 14. Structured error responses matter

Make it explicit whether an error is retryable or a business rule violation. The model handles them differently, and so does the user.

---

## Conclusion

Building agents on AWS Bedrock is not fundamentally different from building any production backend system — the non-functional requirements don't disappear because there's a language model in the middle. Security, observability, testability, and data integrity matter just as much, and in some cases more, because agent behavior is harder to predict than deterministic code.

The code is open source — if you're building something similar, the architecture is yours to take and adapt.

---

**There has never been a better time to be an engineer and create value in society through software.**

- [LinkedIn](https://www.linkedin.com/in/jorgetovar-sa/)
- [Twitter](https://twitter.com/jorgetovar621)
- [GitHub](https://github.com/jorgetovar)

If you enjoyed the articles, visit my blog at [jorgetovar.dev](https://jorgetovar.dev).
