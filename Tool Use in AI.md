# Tool Use in AI

Tool use (also called function calling) is the ability of a [[Large Language Models|Large Language Model]] to invoke external functions during a conversation. It is the foundation of the [[Agentic Coding Paradigm]] and what transforms a chatbot into an [[AI Agents and Sub-Agents|agent]].

## How It Works

### The Tool Use Loop

```
User: "What's the weather in Tokyo?"

1. LLM receives the message + a list of available tools
2. LLM decides to call: get_weather(city="Tokyo")
3. Harness executes the function, returns: {"temp": 22, "condition": "cloudy"}
4. LLM receives the result
5. LLM responds: "It's 22°C and cloudy in Tokyo."
```

The LLM never directly executes anything — it outputs a structured tool call (name + arguments), and the harness executes it.

### Tool Definition

Tools are described to the LLM as JSON schemas:

```json
{
  "name": "read_file",
  "description": "Read the contents of a file at the given path",
  "parameters": {
    "type": "object",
    "properties": {
      "path": {
        "type": "string",
        "description": "Absolute path to the file"
      },
      "offset": {
        "type": "number",
        "description": "Line number to start reading from"
      }
    },
    "required": ["path"]
  }
}
```

The model reads the description and schema to understand when and how to use each tool. **Good tool descriptions are critical** — they are a form of [[Prompt Engineering]] for the model's tool selection.

## Tool Categories

### In AI Coding Harnesses

| Category | Tools | Purpose |
|---|---|---|
| **File I/O** | Read, Write, Edit, Glob | Navigate and modify code |
| **Search** | Grep, symbol search, web search | Find relevant code and information |
| **Execution** | Bash, shell commands | Run builds, tests, scripts |
| **Code Intelligence** | LSP, go-to-definition, find references | Understand code structure |
| **Version Control** | Git operations | Manage changes |
| **Communication** | Web fetch, API calls | Access external information |

### Via MCP

[[Model Context Protocol]] extends the tool set dynamically:
- Database queries
- API integrations (Jira, Slack, GitHub)
- Custom business logic
- Browser automation

## Parallel Tool Calls

Modern models can request multiple tool calls simultaneously:

```
LLM response:
  1. read_file("src/auth/login.ts")
  2. read_file("src/auth/types.ts")
  3. grep("validateToken", "src/")
```

The harness executes all three in parallel and returns results together. This dramatically speeds up exploration.

## Tool Use Strategies by Model

Different models have different tool use capabilities:

| Model | Parallel Calls | Tool Choice | Notes |
|---|---|---|---|
| Claude (Anthropic) | Yes | Excellent | Detailed tool descriptions help most |
| GPT-4 / o-series (OpenAI) | Yes | Good | Structured output mode improves reliability |
| Gemini (Google) | Yes | Good | Strong at multi-step tool chains |

## The "Scratchpad" Pattern

Some agents give the LLM a scratchpad tool — a place to write notes to itself during complex reasoning:

```
Agent thinks: "I need to track which files I've already modified..."
→ Calls: scratchpad.write("Modified: auth.ts, types.ts. Still need: tests.ts")
```

This helps maintain state across long tool-use chains.

## Challenges

- **Tool selection errors** — Model picks the wrong tool for the job
- **Argument hallucination** — Model invents file paths or function names that don't exist
- **Over-tooling** — Model calls tools when it could answer from its training data
- **Under-tooling** — Model guesses instead of looking things up
- **Context cost** — Each tool definition and result consumes context window tokens

## Best Practices for Tool Design

1. **Clear descriptions** — Tell the model exactly when to use each tool
2. **Minimal required parameters** — Make optional what can be optional
3. **Useful error messages** — Help the model recover from failures
4. **Atomic operations** — Each tool does one thing well
5. **Idempotent when possible** — Safe to retry on failure

## Related

- [[AI Agents and Sub-Agents]]
- [[Agentic Coding Paradigm]]
- [[Model Context Protocol]]
- [[AI Coding Harnesses]]
- [[Large Language Models]]
