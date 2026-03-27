# AI Observability

AI observability is the practice of monitoring, tracing, and understanding the behavior of [[Large Language Models|LLM]]-powered systems in production. Traditional software observability (logs, metrics, traces) is necessary but insufficient — AI systems require additional instrumentation for model behavior, cost, quality, and safety.

## Why AI Observability Is Different

Traditional software is deterministic — the same input produces the same output. LLM-based systems are:
- **Non-deterministic** — Same prompt can produce different outputs
- **Opaque** — No stack trace explains why the model said what it said
- **Expensive** — Each request has variable token cost
- **Safety-critical** — Outputs can be harmful, biased, or factually wrong
- **Multi-step** — [[AI Agents and Sub-Agents|Agents]] make chains of decisions, each affecting the next

## The Three Pillars + AI Extensions

### 1. Metrics (Traditional + AI)

| Traditional Metric | AI-Specific Metric |
|---|---|
| Request latency (p50, p95, p99) | Time to first token (TTFT) |
| Error rate | Hallucination rate (sampled) |
| Throughput (requests/sec) | Tokens per second (generation speed) |
| CPU/memory usage | GPU utilization, VRAM usage |
| | Token cost per request |
| | [[Prompt Caching]] hit rate |
| | Tool call success/failure rate |
| | Context window utilization (%) |

### 2. Logs (Traditional + AI)

| Traditional Log | AI-Specific Log |
|---|---|
| Request/response payloads | Full prompt + completion text |
| Error messages | Model refusals and safety triggers |
| Audit trail | Tool calls with arguments and results |
| | Reward model scores (if applicable) |
| | Token counts (input, output, cached, thinking) |

### 3. Traces (Traditional + AI)

Traditional distributed tracing tracks a request across microservices. AI tracing tracks a request through the **agent loop**:

```
Trace: user_request_abc123
├── Span: prompt_assembly (12ms)
│   ├── system_prompt (cached, 5K tokens)
│   ├── tool_definitions (cached, 3K tokens)
│   └── user_message (200 tokens)
├── Span: llm_inference (2,400ms)
│   ├── model: claude-sonnet-4-6
│   ├── input_tokens: 8,200
│   ├── output_tokens: 350
│   ├── cache_read_tokens: 8,000
│   └── thinking_tokens: 1,200
├── Span: tool_call_read_file (45ms)
│   ├── path: src/auth/login.ts
│   └── result_tokens: 800
├── Span: llm_inference (1,800ms)
│   ├── input_tokens: 9,350
│   └── output_tokens: 500 (includes edit tool call)
├── Span: tool_call_edit_file (12ms)
│   └── path: src/auth/login.ts
├── Span: tool_call_bash (3,200ms)
│   ├── command: "npm test"
│   └── exit_code: 0
└── Span: llm_inference (900ms)
    ├── output_tokens: 150 (final response)
    └── total_cost: $0.034
```

This trace shows every decision point, every tool call, and every token spent.

## Observability Tools

### LLM-Specific Platforms

| Platform | Key Feature |
|---|---|
| **LangSmith** (LangChain) | Full trace visualization for LangChain/LangGraph agents |
| **Arize Phoenix** | Open-source LLM observability, embedding drift detection |
| **Weights & Biases (W&B)** | Experiment tracking, prompt management, evaluation |
| **Helicone** | LLM proxy with automatic logging, cost tracking, caching |
| **Braintrust** | Evaluation and logging for LLM applications |
| **Langfuse** | Open-source LLM observability, prompt management |

### Built Into Agent Frameworks

| Framework | Observability |
|---|---|
| [[AI Agent Frameworks\|Claude Agent SDK]] | Built-in tracing, hook callbacks |
| [[AI Agent Frameworks\|OpenAI Agents SDK]] | Built-in tracing with spans |
| [[AI Agent Frameworks\|LangGraph]] | LangSmith integration |
| [[Claude Code]] | `--output-format stream-json` for programmatic trace consumption |

### Generic (Adapted for AI)

| Tool | AI Usage |
|---|---|
| **OpenTelemetry** | Standard traces enriched with LLM-specific attributes |
| **Datadog** | LLM Observability product (traces, cost, quality) |
| **Grafana** | Dashboard for token cost, latency, error metrics |
| **Prometheus** | Alert on cost spikes, latency degradation |

## Key Dashboards

### Cost Dashboard
```
Total token spend: $1,247.32 (last 7 days)
├── By model: Opus $890, Sonnet $312, Haiku $45
├── By feature: Code review $456, Chat $389, CI/CD $402
├── Trend: +12% vs previous week
└── Alert: Agent session exceeded $50 budget (3 occurrences)
```

### Quality Dashboard
```
Hallucination rate: 3.2% (sampled, human-evaluated)
User satisfaction: 4.1/5.0
Tool call success rate: 94.7%
Refusal rate: 2.1% (target: <5%)
Prompt injection attempts detected: 17 (blocked: 17)
```

### Latency Dashboard
```
TTFT (p50): 280ms | (p95): 890ms | (p99): 2,100ms
Full response (p50): 3.2s | (p95): 12.4s
Agent turn count (avg): 6.3 turns per task
Agent session duration (avg): 4.2 minutes
```

## Evaluation in Production

Beyond metrics, production AI systems need periodic **quality evaluation**:

### Online Evaluation
- **A/B testing** — Compare model versions on real users
- **Thumbs up/down** — Lightweight user feedback
- **Implicit signals** — Did the user accept the suggestion? Retry? Abandon?

### Offline Evaluation
- **Golden set testing** — Run new models against curated test cases
- **LLM-as-judge** — Use a strong model to evaluate a weaker model's output (see [[Evaluation and Metrics]])
- **Regression testing** — Ensure model updates don't degrade specific capabilities

## Alerting

| Alert | Trigger | Action |
|---|---|---|
| Cost spike | Session cost > $50 | Kill agent, notify team |
| Latency degradation | p95 > 15s for 5 min | Check model status, fall back to faster model |
| Safety trigger spike | Content filter rate > 2× normal | Investigate attack or model regression |
| Tool failure spike | Tool error rate > 10% | Check external service health |
| Prompt cache miss | Cache hit rate < 50% | Check for prompt template changes |

## Related

- [[AI Agents in Production]]
- [[Evaluation and Metrics]]
- [[Tokenomics of AI]]
- [[Responsible AI Deployment]]
- [[Prompt Caching]]
- [[Batch Processing and Streaming]]
- [[AI Coding Harnesses]]
