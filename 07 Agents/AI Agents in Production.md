# AI Agents in Production

Running [[AI Agents and Sub-Agents|AI agents]] in production — beyond demos and prototypes — introduces a set of engineering challenges around reliability, observability, cost control, and failure handling that don't exist in simple LLM API calls.

## Why Production Is Different

| Prototype | Production |
|---|---|
| Works on the happy path | Must handle every edge case |
| Cost doesn't matter | Token spend is a real budget line |
| Failure is acceptable | Failure requires recovery and alerting |
| Single user | Concurrent users, rate limits, queuing |
| No logging | Full observability and audit trail |
| Manual restart | Self-healing, automatic retry |

## Architecture for Production Agents

```
┌──────────────────────────────────────────┐
│              API Gateway                  │
│  (auth, rate limit, request routing)     │
└──────────────┬───────────────────────────┘
               ▼
┌──────────────────────────────────────────┐
│           Agent Orchestrator              │
│  (session management, tool dispatch,     │
│   turn limits, cost tracking)            │
└──────┬───────────┬───────────┬───────────┘
       ▼           ▼           ▼
┌──────────┐ ┌──────────┐ ┌──────────┐
│ LLM API  │ │  Tools   │ │  Memory  │
│ (Claude, │ │ (file,   │ │ (vector  │
│  GPT,    │ │  bash,   │ │  DB,     │
│  etc.)   │ │  MCP)    │ │  SQL)    │
└──────────┘ └──────────┘ └──────────┘
       │           │           │
       └───────────┴───────────┘
               ▼
┌──────────────────────────────────────────┐
│          Observability Layer              │
│  (traces, logs, metrics, alerts)         │
└──────────────────────────────────────────┘
```

## Key Engineering Concerns

### Reliability

**Retry with backoff:** LLM APIs fail — rate limits, timeouts, server errors:
```
try:
    response = call_llm(prompt)
except RateLimitError:
    wait(exponential_backoff)
    retry()
except TimeoutError:
    reduce context size → retry
```

**Idempotency:** Tool calls should be safe to retry. If a file edit partially applies and the agent retries, it shouldn't corrupt the file.

**Turn limits:** Hard cap on agent iterations to prevent infinite loops:
```
max_turns = 50  # Agent must finish or escalate within 50 tool calls
```

**Graceful degradation:** When the LLM is unavailable, fall back to:
- A smaller/cheaper model
- Cached responses
- Human handoff
- Informative error message

### Observability

**Tracing:** Record the full agent execution path:
```
Turn 1: User prompt → LLM response → tool_call(read_file)
Turn 2: Tool result → LLM response → tool_call(edit_file)
Turn 3: Tool result → LLM response → tool_call(bash)
Turn 4: Tool result → LLM response (final)
```

Tools for agent tracing:
- **LangSmith** (LangChain) — Full trace visualization
- **Weights & Biases** — Experiment tracking with LLM support
- **Arize Phoenix** — LLM observability
- **OpenTelemetry** — Generic distributed tracing
- Built-in tracing in [[AI Agent Frameworks|Claude Agent SDK and OpenAI Agents SDK]]

**Key metrics to track:**

| Metric | Why |
|---|---|
| Turns per task | Efficiency — are agents wandering? |
| Tokens per task | Cost — is spending predictable? |
| Tool call distribution | Which tools are used most? |
| Error rate by tool | Which tools fail most? |
| Task success rate | Is the agent actually helping? |
| Latency (p50, p95, p99) | User experience |
| Cost per task | Budget tracking |

### Cost Control

See [[Tokenomics of AI]]. Production cost management:

- **Budget caps per session** — Kill the agent if it exceeds $X
- **Model routing** — Use cheap models for simple tasks, expensive for complex ones
- **Caching** — Cache tool results that don't change (file contents within a session, API schemas)
- **[[Inference Optimization|Prompt caching]]** — Maximize prefix reuse
- **Turn limits** — Prevent runaway agents
- **Token budgets** — Track and alert on per-user/per-task spend

### Security

See [[Prompt Injection]] and [[Approval Flows and Sandboxing]]:

- **Sandboxed execution** — Never run agent tools with production credentials
- **Input validation** — Sanitize user inputs before they reach the agent
- **Output validation** — Check tool call arguments before execution
- **Secret management** — Never expose API keys, database passwords in agent context
- **Audit logging** — Record every action for compliance

### Concurrency

Multiple agents or multiple users running simultaneously:
- **Session isolation** — Each agent session has independent state
- **Resource locking** — Prevent two agents from editing the same file
- **Queue management** — Rate-limit LLM API calls across agents
- **Worktree isolation** — [[Claude Code]] uses git worktrees for parallel agents

## Failure Modes

| Failure | Cause | Mitigation |
|---|---|---|
| **Infinite loop** | Agent retries failing approach endlessly | Turn limits, loop detection |
| **Context overflow** | Too much tool output fills the window | [[Context Management\|Compaction]], selective reading |
| **Hallucinated tool calls** | Agent calls non-existent functions | Tool schema validation |
| **Cost explosion** | Long reasoning chain or excessive tool use | Budget caps, alerts |
| **Stale context** | Agent acts on outdated file contents | Re-read before critical operations |
| **Cascading failure** | One bad tool call leads to more bad calls | Error handling in tool execution |

## Production Patterns

### The Supervisor Pattern
A lightweight supervisor monitors the agent and intervenes:
```
Agent runs → Supervisor checks each action → Allow / Block / Modify
```

Implemented via [[Claude Code]]'s hooks or custom middleware.

### The Checkpoint Pattern
Save agent state at key points; roll back on failure:
```
Checkpoint → Agent acts → Success? → Continue
                       → Failure? → Restore checkpoint → Try different approach
```

### The Budget Envelope Pattern
Allocate a fixed token budget per task:
```
Task budget: 50K tokens
Remaining:   32K tokens (after 3 turns)
Action:      If remaining < threshold → wrap up or escalate
```

## Related

- [[AI Agents and Sub-Agents]]
- [[Autonomous AI Agents]]
- [[AI Agent Frameworks]]
- [[Tool Use in AI]]
- [[Tokenomics of AI]]
- [[Prompt Injection]]
- [[Approval Flows and Sandboxing]]
- [[Evaluation and Metrics]]
- [[Responsible AI Deployment]]
