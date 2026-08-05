# Claude Code Authentication: Subscription, API Key, Amazon Bedrock, and Claude Platform on AWS

I'm a big fan of using Claude and Claude Code for development. Many organizations are currently using these tools to improve developer productivity and ultimately build better products. Our role and our tools have changed — we went from powerful autocomplete to autonomous agents that can refactor, review, and implement features, most of the time better than we can on our own.

## Authentication methods

There are several authentication methods, each with different billing, cost tracking, and governance options. Depending on your organization, you will choose the one that fits best.

---

### Personal development — Anthropic API key

I use this for experimenting with the Anthropic library for learning and prototyping. You set `ANTHROPIC_API_KEY` in your environment (or a `.env` file), and the SDK picks it up automatically. Pay-as-you-go per token, no infrastructure needed.

```python
from dotenv import load_dotenv
load_dotenv()
import json
import anthropic

client = anthropic.Anthropic()

tools = [
    {
        "name": "get_weather",
        "description": (
            "Returns current weather for a city. Use ONLY for weather queries. "
            "Input: city name (string). Output: temperature in Celsius and conditions."
        ),
        "input_schema": {
            "type": "object",
            "properties": {"city": {"type": "string"}},
            "required": ["city"],
        },
    },
    {
        "name": "get_time",
        "description": (
            "Returns the current local time for a city. Use ONLY for time/timezone queries. "
            "Input: city name (string). Output: local time string."
        ),
        "input_schema": {
            "type": "object",
            "properties": {"city": {"type": "string"}},
            "required": ["city"],
        },
    },
]


def get_weather(city: str) -> dict:
    return {"city": city, "temp_c": 22, "conditions": "sunny"}

def get_time(city: str) -> dict:
    return {"city": city, "local_time": "14:35"}

TOOL_FUNCTIONS = {
    "get_weather": get_weather,
    "get_time": get_time,
}


def run_agent(user_message: str) -> str:
    messages = [{"role": "user", "content": user_message}]
    iteration = 0

    print(f"\n[user] {user_message}")

    while True:
        iteration += 1
        print(f"\n--- iteration {iteration}: calling Claude ---")

        response = client.messages.create(
            model="claude-sonnet-4-6",
            max_tokens=1024,
            tools=tools,
            messages=messages,
        )

        print(f"[sdk] stop_reason = {response.stop_reason!r}")
        print(f"[sdk] response.content blocks: {[b.type for b in response.content]}")

        if response.stop_reason == "end_turn":
            final = next(b.text for b in response.content if b.type == "text")
            print(f"\n[assistant] {final}")
            return final

        messages.append({"role": "assistant", "content": response.content})

        tool_results = []
        for block in response.content:
            if block.type == "tool_use":
                print(f"\n[tool_use] Claude wants to call: {block.name!r}")
                print(f"[tool_use] with input: {block.input}")

                fn = TOOL_FUNCTIONS[block.name]
                result = fn(**block.input)
                print(f"[tool_result] returned: {result}")

                tool_results.append({
                    "type": "tool_result",
                    "tool_use_id": block.id,
                    "content": json.dumps(result),
                })

        print(f"\n[loop] appending {len(tool_results)} tool result(s), looping back...")
        messages.append({"role": "user", "content": tool_results})


if __name__ == "__main__":
    run_agent("What's the weather and local time in Bogotá?")
```

---

### Claude Code with Amazon Bedrock

This is my preferred option for organizations. With Bedrock you get inference profiles, IAM roles, and solid audit trails through CloudTrail — no floating API key to rotate or leak. The same credential chain you use for any other AWS SDK call works here.

Configuration is just a few lines in `~/.claude/settings.json`:

```json
{
  "env": {
    "CLAUDE_CODE_USE_BEDROCK": "1",
    "AWS_REGION": "us-east-1",
    "ANTHROPIC_DEFAULT_OPUS_MODEL": "us.anthropic.claude-opus-5",
    "ANTHROPIC_DEFAULT_SONNET_MODEL": "global.anthropic.claude-sonnet-4-6",
    "ANTHROPIC_DEFAULT_HAIKU_MODEL": "global.anthropic.claude-haiku-4-5-20251001-v1:0",
    "CLAUDE_CODE_ENABLE_AUTO_MODE": "1",
    "AWS_PROFILE": "aws-community-builders"
  }
}
```

The `global.` prefix on the model IDs uses cross-region inference profiles, which route to the lowest-latency region automatically and give higher throughput limits than a single-region endpoint.

![Claude Bedrock](https://dev-to-uploads.s3.us-east-2.amazonaws.com/uploads/articles/26nu90zzsn5l81583xon.png)

---

### Claude Platform on AWS

This is the option for organizations that want AWS Marketplace billing combined with the full Anthropic API feature set. Unlike Bedrock (which routes requests through AWS's own inference infrastructure), Claude Platform on AWS sends requests directly to Anthropic's API — giving you the latest models on the same release schedule as the direct Claude API — while billing consolidates into your existing AWS spend through Marketplace.

It's a great fit when your organization has SSO set up through IAM Identity Center and wants a single sign-on experience without managing separate Anthropic credentials.

**Step 1 — log in with SSO:**

```bash
aws sso login --profile aws-community-sso
export AWS_PROFILE=aws-community-sso
```

**Step 2 — configure automatic credential refresh** so Claude Code re-authenticates when your SSO session expires, rather than dying mid-session. Add this to `~/.claude/settings.json`:

```json
{
  "awsAuthRefresh": "aws sso login --profile aws-community-sso"
}
```

![Login](https://dev-to-uploads.s3.us-east-2.amazonaws.com/uploads/articles/qhm18oluo1f7gevai1xj.png)

**Step 3 — point Claude Code at the platform:**

```bash
export CLAUDE_CODE_USE_ANTHROPIC_AWS=1
export ANTHROPIC_AWS_WORKSPACE_ID=wrkspc_01ABCDEFGHIJKLMN
export AWS_REGION=us-east-1
```

`ANTHROPIC_AWS_WORKSPACE_ID` is required on every request — it identifies your organization's workspace and isn't inferred from your AWS credentials.

![Claude on AWS](https://dev-to-uploads.s3.us-east-2.amazonaws.com/uploads/articles/kvhx2x315izuu2l9x3tz.png)

---

### Bonus: pin your model versions

Regardless of which auth method you use, always pin model versions before rolling out to a team. Without pinning, model aliases like `opus` and `sonnet` resolve to Claude Code's built-in defaults, which can change when Claude Code updates — and on Bedrock, that can silently move a Sonnet deployment to Opus pricing.

```bash
export ANTHROPIC_DEFAULT_OPUS_MODEL='us.anthropic.claude-opus-4-8'
export ANTHROPIC_DEFAULT_SONNET_MODEL='us.anthropic.claude-sonnet-4-6'
export ANTHROPIC_DEFAULT_HAIKU_MODEL='us.anthropic.claude-haiku-4-5-20251001-v1:0'
```

Run `/status` inside Claude Code to confirm which provider and models are actually active.

---

### Bonus: AWS Guardrails

Amazon Bedrock Guardrails let you implement content filtering for Claude Code. Create a guardrail in the Amazon Bedrock console, publish a version, then add the guardrail headers to your settings file. Enable cross-region inference on your guardrail if you're using cross-region inference profiles.

```json
{
  "env": {
    "ANTHROPIC_CUSTOM_HEADERS": "X-Amzn-Bedrock-GuardrailIdentifier: your-guardrail-id\nX-Amzn-Bedrock-GuardrailVersion: 1"
  }
}
```

---

## Conclusion

Each authentication method reflects a different stage of adoption and governance maturity:

- **Personal API key** — the fastest way to start experimenting. Zero infrastructure, pay per token, ideal for learning and prototyping.
- **Claude subscription (Pro/Max)** — best for individual developers who want flat pricing and access to Claude on the web alongside Claude Code.
- **Amazon Bedrock** — the right choice for teams already inside AWS. IAM authentication, CloudTrail audit logs, inference profiles, and no standalone API keys to manage. This is where I'd start for any production team deployment.
- **Claude Platform on AWS** — best when your organization wants AWS Marketplace billing and SSO, but also needs the latest models on Anthropic's release schedule without waiting for Bedrock to catch up.

Pick the option that matches your organization's current security and billing requirements — and remember that you can always migrate later. Start simple, and add governance as your usage grows.

---

**There has never been a better time to be an engineer and create value in society through software.**

- [LinkedIn](https://www.linkedin.com/in/jorgetovar-sa/)
- [Twitter](https://twitter.com/jorgetovar621)
- [GitHub](https://github.com/jorgetovar)

If you enjoyed the articles, visit my blog at [jorgetovar.dev](https://jorgetovar.dev).
