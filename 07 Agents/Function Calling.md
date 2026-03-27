# Function Calling

Function calling (also called tool calling) is the API-level mechanism by which [[Large Language Models]] invoke external functions. While [[Tool Use in AI]] covers the concept broadly, this note focuses on the technical implementation — how function calling works at the API level and how to design function schemas effectively.

## The API Flow

### 1. Define Functions
Provide the model with JSON Schema definitions of available functions:

```json
{
  "tools": [{
    "type": "function",
    "function": {
      "name": "get_weather",
      "description": "Get the current weather for a location",
      "parameters": {
        "type": "object",
        "properties": {
          "location": {
            "type": "string",
            "description": "City name, e.g. 'San Francisco, CA'"
          },
          "unit": {
            "type": "string",
            "enum": ["celsius", "fahrenheit"],
            "description": "Temperature unit"
          }
        },
        "required": ["location"]
      }
    }
  }]
}
```

### 2. Model Decides to Call

Instead of generating text, the model outputs a structured tool call:

```json
{
  "role": "assistant",
  "tool_calls": [{
    "id": "call_abc123",
    "type": "function",
    "function": {
      "name": "get_weather",
      "arguments": "{\"location\": \"San Francisco, CA\", \"unit\": \"celsius\"}"
    }
  }]
}
```

### 3. Application Executes

Your code parses the tool call and runs the actual function:

```python
result = get_weather(location="San Francisco, CA", unit="celsius")
# Returns: {"temperature": 18, "condition": "foggy"}
```

### 4. Return Result to Model

Send the function result back as a tool message:

```json
{
  "role": "tool",
  "tool_call_id": "call_abc123",
  "content": "{\"temperature\": 18, \"condition\": \"foggy\"}"
}
```

### 5. Model Generates Final Response

The model incorporates the result into its response:

```
"It's currently 18°C and foggy in San Francisco."
```

## Parallel Function Calls

Modern models can request multiple functions simultaneously:

```json
{
  "tool_calls": [
    {"function": {"name": "read_file", "arguments": "{\"path\": \"src/auth.ts\"}"}},
    {"function": {"name": "read_file", "arguments": "{\"path\": \"src/types.ts\"}"}},
    {"function": {"name": "grep", "arguments": "{\"pattern\": \"TODO\", \"path\": \"src/\"}"}}
  ]
}
```

The application executes all three in parallel and returns results together. This is how [[AI Coding Harnesses]] achieve fast codebase exploration.

## Forced vs Auto Tool Choice

| Mode | Behavior |
|---|---|
| `auto` | Model decides whether to call a function or respond with text |
| `required` | Model must call at least one function (won't respond with text) |
| `none` | Model cannot call functions (text only) |
| `{"function": "name"}` | Model must call this specific function |

## Schema Design Best Practices

### Good Descriptions

The model reads descriptions to decide when and how to use each function. Quality descriptions are the most important factor:

```json
// BAD
{"name": "search", "description": "Search"}

// GOOD
{"name": "search_codebase",
 "description": "Search for text patterns across all files in the project using regex. Use this when you need to find where a function, variable, or pattern is used. Returns matching lines with file paths and line numbers."}
```

### Minimal Required Parameters

Only mark parameters as required if the function truly can't work without them:

```json
{
  "parameters": {
    "properties": {
      "query": {"type": "string", "description": "Search query (required)"},
      "max_results": {"type": "integer", "description": "Limit results (default: 10)"},
      "file_pattern": {"type": "string", "description": "Glob pattern to filter files (default: all files)"}
    },
    "required": ["query"]  // Only query is truly required
  }
}
```

### Enums for Constrained Choices

```json
{
  "severity": {
    "type": "string",
    "enum": ["low", "medium", "high", "critical"],
    "description": "Issue severity level"
  }
}
```

Enums prevent the model from hallucinating invalid values.

### Useful Error Returns

When a function fails, return an informative error — the model will use it to recover:

```json
// BAD
{"error": "failed"}

// GOOD
{"error": "File not found: src/auth.ts. Did you mean src/authentication/auth.ts? Similar files: src/authentication/auth.ts, src/auth/index.ts"}
```

## Function Calling in [[AI Coding Harnesses]]

Each harness implements function calling as its tool system:

| Harness | Tool Count | Parallel Calls | Custom Tools |
|---|---|---|---|
| [[Claude Code]] | ~15 built-in | Yes | Via [[Model Context Protocol\|MCP]] |
| [[OpenAI Codex]] | ~10 built-in | Yes | Via MCP |
| [[OpenCode]] | 13 built-in | Yes | Via MCP |
| [[AI Coding Landscape\|Cursor]] | IDE-integrated | Yes | Limited |

MCP servers are essentially function-calling endpoints with a standardized protocol.

## Structured Output vs Function Calling

| Feature | [[Structured Output]] | Function Calling |
|---|---|---|
| Purpose | Constrain the response format | Execute external actions |
| Output | JSON matching a schema | Function name + arguments |
| Execution | No side effects | Application runs real code |
| Use case | "Return analysis as JSON" | "Read this file" |

They complement each other: function calling triggers actions; structured output formats results.

## Token Cost of Function Definitions

Every function definition consumes input tokens:
- A typical function with description and parameters: 100–300 tokens
- 15 tools: ~2,000–4,500 tokens per request
- This is why [[Model Context Protocol#Tool Search|MCP Tool Search]] exists — loading only needed tool definitions when total tools exceed 10% of context

## Related

- [[Tool Use in AI]]
- [[Structured Output]]
- [[Model Context Protocol]]
- [[AI Agents and Sub-Agents]]
- [[AI Coding Harnesses]]
- [[Prompt Caching]]
