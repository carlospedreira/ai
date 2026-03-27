# Batch Processing and Streaming

Batch processing and streaming are the two fundamental modes for interacting with [[Large Language Models|LLM]] APIs. The choice between them affects latency, cost, throughput, and user experience — especially in [[AI Coding Harnesses]] where the [[Agentic Coding Paradigm#The Agent Loop|agent loop]] depends on fast response times.

## Streaming

Tokens are sent to the client as they are generated, one at a time (or in small chunks):

```
User sends prompt
    ↓
Model starts generating
    ↓
Token "The" → sent immediately
Token " bug" → sent immediately
Token " is" → sent immediately
Token " on" → sent immediately
Token " line" → sent immediately
Token " 42" → sent immediately
...
```

### Why Streaming Matters

**Time to first token (TTFT)** is typically 200–500ms. Without streaming, the user waits for the *entire* response (could be 10–30 seconds for a long answer). With streaming, they see output almost immediately.

For [[AI Coding Harnesses]], streaming enables:
- Real-time display of the agent's reasoning
- Early detection of wrong approaches (user can interrupt)
- Progressive rendering of code diffs
- Tool calls emitted as soon as the model decides (not after full generation)

### Server-Sent Events (SSE)

The standard transport for LLM streaming:

```
HTTP POST /v1/messages (stream=true)

Response (SSE):
data: {"type": "content_block_start", "content_block": {"type": "text"}}
data: {"type": "content_block_delta", "delta": {"text": "The "}}
data: {"type": "content_block_delta", "delta": {"text": "bug "}}
data: {"type": "content_block_delta", "delta": {"text": "is "}}
...
data: {"type": "content_block_stop"}
data: {"type": "message_stop"}
```

### Streaming Tool Calls

Modern APIs stream tool calls progressively:

```
data: {"type": "tool_use", "name": "read_file"}
data: {"type": "input_json_delta", "partial_json": "{\"path\":"}
data: {"type": "input_json_delta", "partial_json": " \"src/auth.ts\"}"}
data: {"type": "tool_use_end"}
```

The harness can begin preparing the tool execution before the full arguments are received.

## Batch Processing

Send many requests at once and receive all results later. No streaming — each request returns a complete response.

### Anthropic Batch API

```python
# Submit a batch of 1000 prompts
batch = client.batches.create(
    requests=[
        {"custom_id": "req_1", "params": {"model": "claude-sonnet-4-6", "messages": [...]}},
        {"custom_id": "req_2", "params": {"model": "claude-sonnet-4-6", "messages": [...]}},
        ...
    ]
)

# Poll for completion (hours later)
results = client.batches.retrieve(batch.id)
```

### Why Batch?

| Benefit | Detail |
|---|---|
| **50% cost reduction** | Anthropic and OpenAI offer batch API at half price |
| **Higher throughput** | Not constrained by real-time rate limits |
| **No latency requirement** | Results returned within 24 hours |
| **Bulk operations** | Process thousands of items efficiently |

### Batch Use Cases

| Use Case | Example |
|---|---|
| **[[AI Code Review]]** | Review 500 PRs overnight |
| **Test generation** | Generate tests for every file in a repo |
| **Documentation** | Document all public APIs |
| **Data labeling** | Classify thousands of data points |
| **Evaluation** | Run [[AI Benchmarks\|benchmarks]] across many test cases |
| **Translation** | Translate docs to multiple languages |

## Streaming vs Batch in Practice

| Aspect | Streaming | Batch |
|---|---|---|
| Latency | Low (TTFT ~200ms) | High (minutes to hours) |
| Cost | Full price | 50% discount |
| Rate limits | Per-minute token limits | Separate, higher limits |
| User experience | Interactive, real-time | Background processing |
| Error handling | Immediate feedback | Retry on batch completion |
| Best for | Interactive coding, chat | Bulk processing, CI/CD |

## Streaming in [[AI Coding Harnesses]]

All major harnesses use streaming for interactive sessions:

| Harness | Streaming | Batch |
|---|---|---|
| [[Claude Code]] | Yes — real-time token display | Headless mode (`-p`) for scripting; batch API for SDK |
| [[OpenAI Codex]] | Yes — progressive rendering | Cloud Codex processes asynchronously |
| [[OpenCode]] | Yes — SSE from server to TUI client | CLI mode for automation |

### The Agent Loop and Streaming

In the agent loop, streaming affects when tool execution begins:

**Without streaming:**
```
Send prompt → Wait 5s → Receive complete response with tool call → Execute tool
Total: 5s + tool execution time
```

**With streaming:**
```
Send prompt → 200ms: first token → 500ms: tool call name visible →
1s: tool arguments complete → Execute tool immediately
Total: 1s + tool execution time (saved 4s)
```

For a 20-turn agent session, streaming can save minutes of cumulative wait time.

## Output Formats

### Streaming Formats

| Format | Use Case |
|---|---|
| `text/event-stream` (SSE) | Browser and server clients |
| `stream-json` | Claude Code's `--output-format stream-json` for programmatic consumption |
| WebSocket | Some real-time applications |

### Non-Streaming Formats

| Format | Use Case |
|---|---|
| JSON (complete) | Simple API calls, batch results |
| `--output-format json` | Claude Code headless mode |

## Related

- [[Training and Inference]]
- [[Inference Optimization]]
- [[Tokenomics of AI]]
- [[AI Coding Harnesses]]
- [[AI in CI-CD]]
- [[AI Agents in Production]]
- [[Function Calling]]
