# Bedrock Inference Profiles — From Flying Blind to Understanding Your AWS Bedrock Usage in Detail

Every organization is using Claude, Codex, Cursor, Cline, and many other wonderful tools right now to improve team productivity and, by a rippling effect, the products they ship. Most of these tools are developer-oriented, but a new generation of applications has been increasingly growing in numbers — agentic applications.

Rather than interacting with a simple, naive bank app where you click through menus and filters that barely work, you can now talk with them in natural language and ask things like "where is my money going this month?" That kind of feature is raising the bar on what a product delivers. And in my case as an AWS consultant, most of these agentic features are powered behind the scenes by AWS Bedrock — though you can also find them on other clouds with their respective LLMs, Ollama, or even direct OpenAI or Anthropic keys if you want.

Right now, several applications in your organization may already be using AWS Bedrock and improving your products. But are you aware of the ROI? Or do you only see a total bill without being able to answer:

- Which app is calling Claude the most?
- Which team is burning tokens on Opus when Haiku would do?
- Which application ran a loop last night and spent $40 on something trivial?

This is where Bedrock inference profiles help. You can now see, in every account and region, which users and applications are using which models — and how much it's costing.

---

## The solution: three layers

### Layer 1 — Capture every invocation

This is the foundation. You tell Bedrock to log every API call to two places:

- **S3** (`invocations/` prefix) — durable, cheap, queryable with Athena
- **CloudWatch** (`/aws/bedrock/invocations`) — for real-time tailing and alerting

Now you can see in CloudWatch each request and response, including the tokens used. Without this step, everything else is blind. **Run it once per account/region.**

### Layer 2 — Tag every invocation with an identity

This is the key insight. Logging alone tells you that a call happened and how many tokens it used — but not *who* made it from an application perspective. All calls to the same model look identical in the logs.

**Application inference profiles** solve this. Each app gets a profile that is a named copy of a system model, carrying tags:

```
tags: { app: "community-bank", team: "cto" }
```

The app swaps its `modelId` for the profile ARN. That's the only change required — no code changes, just configuration. The profile ARN flows into every log entry, so every token is now stamped with an app and team identity.

That's all you need. From here you can go to Athena and start answering your questions.

### Layer 3 — Query the data with Athena

Once logs are flowing with identity stamps, Athena turns your S3 bucket into a queryable warehouse:

- Tokens per app per day
- Estimated cost per app in USD
- Spend per IAM caller — catches developers calling Bedrock directly from their laptops, not through a profile

---

## Bonus: cross-region inference and data residency

There's one more concept worth understanding before you design your profiles. The source model ID used to create a profile has a geographic prefix:

```
us.anthropic.claude-haiku-4-5-20251001-v1:0
eu.anthropic.claude-haiku-4-5-20251001-v1:0
ap.anthropic.claude-haiku-4-5-20251001-v1:0
```

That prefix is not cosmetic. Bedrock has three geographic routing pools and when you copy from one of these system profiles, your application profile inherits that routing — meaning Bedrock automatically distributes traffic across regions within that pool for higher availability and better throughput.

| Prefix | Pool | Use case |
|---|---|---|
| `us.` | US cross-region | Production apps, US data |
| `eu.` | EU cross-region | GDPR, EU data residency |
| `ap.` | AP cross-region | Asia-Pacific latency |

If you have GDPR obligations or customers in Europe, source your profiles from `eu.` and data never leaves EU regions. This turns inference profiles into a **data governance tool**, not just a cost governance tool.

---

## The governance arc

1. **Before** — bill arrives, no idea who spent what
2. **Enable logging** — raw data flows, but it's all ARNs and roles, still hard to read
3. **Add profiles** — one config change per app unlocks full attribution, no code changes
4. **Athena** — token-level drill-down, estimated USD per app/day, per IAM caller
5. **Cost Explorer** — activate the `app`/`team` tags for budget-level visibility and alerts

From nothing to full observability. That's the journey.
