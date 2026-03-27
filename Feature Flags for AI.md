# Feature Flags for AI

Feature flags (feature toggles) applied to AI systems enable gradual rollouts, A/B testing, instant rollback, and model switching without code deploys. They are a critical operational practice for [[Responsible AI Deployment]] of [[Large Language Models|LLM]]-powered features.

## Why AI Needs Feature Flags

AI features are uniquely risky to deploy:
- **Non-deterministic** — Same input can produce different outputs
- **Hard to test exhaustively** — Edge cases emerge only in production
- **Model changes affect everything** — A new model version can change behavior across all features
- **Cost sensitivity** — Switching models changes token costs (see [[Tokenomics of AI]])
- **Safety-critical** — A hallucinating model can cause real harm (see [[Hallucination and Grounding]])

Feature flags provide a safety net that traditional deploy-or-rollback can't match.

## Flag Types for AI

### Model Selection Flag

```json
{
  "ai_model": {
    "default": "claude-sonnet-4-6",
    "variants": {
      "power_users": "claude-opus-4-6",
      "cost_sensitive": "claude-haiku-4-5",
      "beta_testers": "gpt-5.4"
    }
  }
}
```

Route different users to different models without code changes. Useful for:
- Testing new models on a subset of users
- Offering different quality tiers
- Quick fallback if a model degrades

### Prompt Version Flag

```json
{
  "system_prompt_version": {
    "default": "v3.2",
    "experiment": "v3.3_concise",
    "percentage": { "v3.2": 80, "v3.3_concise": 20 }
  }
}
```

A/B test different system prompts (see [[Conversation Design for AI]]) to measure which produces better outcomes.

### Feature Enablement Flag

```json
{
  "ai_code_review": {
    "enabled": true,
    "rollout_percentage": 25,
    "exclude_orgs": ["enterprise_client_a"]
  }
}
```

Gradually roll out new AI features:
```
Week 1: 5% of users
Week 2: 25% of users (monitor quality + cost)
Week 3: 50% of users
Week 4: 100% of users
```

### Safety Kill Switch

```json
{
  "ai_response_enabled": {
    "default": true,
    "kill_switch": false
  }
}
```

Instantly disable AI features across the entire system if something goes wrong — without a code deploy.

### Parameter Tuning Flag

```json
{
  "ai_temperature": { "default": 0.7, "creative_mode": 1.0 },
  "ai_max_tokens": { "default": 4096, "summary_mode": 512 },
  "ai_top_p": { "default": 0.95 }
}
```

Tune [[Beam Search and Sampling|sampling parameters]] per feature or user segment.

## A/B Testing AI Features

Feature flags enable rigorous A/B testing of AI:

```
Group A (50%): Current model (claude-sonnet-4-6)
Group B (50%): New model (claude-opus-4-6)

Metrics tracked:
- Task completion rate
- User satisfaction (thumbs up/down)
- Token cost per interaction
- Latency (TTFT, total)
- Error/refusal rate
- Hallucination rate (sampled)
```

### What to A/B Test

| Variable | Why Test |
|---|---|
| **Model version** | Does the new model improve quality enough to justify cost? |
| **System prompt** | Which phrasing produces better responses? |
| **Temperature** | Does lower temperature reduce errors or kill creativity? |
| **RAG strategy** | Does adding retrieval improve accuracy? |
| **Tool set** | Does giving the agent more tools help or confuse it? |
| **Context length** | More context vs faster responses |

## Operational Patterns

### Canary Deployment

```
1. Deploy new AI feature to 1% of traffic (canary)
2. Monitor: latency, errors, cost, quality metrics
3. If healthy after 1 hour → expand to 10%
4. If healthy after 24 hours → expand to 50%
5. If healthy after 48 hours → 100%
6. Any degradation → automatic rollback to previous version
```

### Shadow Mode

Run the new AI feature alongside the current one without showing results to users:

```
User request → Current model → response shown to user
            → New model → response logged but NOT shown

Compare: quality, cost, latency, safety
Only promote new model when metrics are clearly better
```

### Fallback Chains

```json
{
  "model_chain": [
    { "model": "claude-opus-4-6", "timeout": 10000 },
    { "model": "claude-sonnet-4-6", "timeout": 5000 },
    { "model": "claude-haiku-4-5", "timeout": 3000 }
  ]
}
```

If the primary model is slow or unavailable, automatically fall back to a faster/cheaper model.

## Feature Flags in [[AI Coding Harnesses]]

### Claude Code Configuration

[[Claude Code]]'s settings system is effectively a feature flag system:
```json
// .claude/settings.json — team-wide flags
{
  "permissions": { "allow": [...], "deny": [...] },
  "model": "opus",
  "effortLevel": "high"
}

// .claude/settings.local.json — personal overrides
{
  "model": "sonnet",
  "effortLevel": "medium"
}
```

Managed settings (IT-deployed) can enforce organization-wide policies — acting as feature flags for AI capabilities.

### MCP Server Toggles

Enable/disable [[Model Context Protocol|MCP]] servers per environment:
```json
{
  "mcpServers": {
    "production_db": { "enabled": false },
    "staging_db": { "enabled": true }
  }
}
```

### Approval Mode as Feature Flag

[[Approval Flows and Sandboxing|Permission modes]] are feature flags for agent autonomy:
```
Development: auto mode (high autonomy)
Staging:     accept-edits mode
Production:  plan mode (read-only)
```

## Tools

| Tool | Type | AI-Specific Features |
|---|---|---|
| **LaunchDarkly** | Commercial | AI config, model routing |
| **Statsig** | Commercial | Experimentation platform |
| **Unleash** | Open-source | Feature toggles |
| **PostHog** | Open-source | Feature flags + analytics |
| **Eppo** | Commercial | Experiment analysis |
| **Custom config** | In-house | Settings.json patterns |

## Related

- [[Responsible AI Deployment]]
- [[AI Observability]]
- [[AI Agents in Production]]
- [[Evaluation and Metrics]]
- [[Beam Search and Sampling]]
- [[Tokenomics of AI]]
- [[Claude Code]]
- [[Conversation Design for AI]]
