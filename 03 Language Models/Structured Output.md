# Structured Output

Structured output is the ability to constrain a [[Large Language Models|Large Language Model]] to generate output in a specific format — typically JSON, but also XML, YAML, CSV, or custom schemas. This is essential for [[Tool Use in AI|tool use]], [[AI Agents and Sub-Agents|agent systems]], and any application that needs to parse LLM output programmatically.

## Why It Matters

LLMs generate free-form text by default. For software systems, this creates problems:
- Parsing natural language is fragile
- JSON in markdown code blocks sometimes has syntax errors
- Slight format deviations break downstream code

Structured output solves this by guaranteeing the output conforms to a schema.

## Approaches

### 1. Prompt-Based (Weakest)

Ask the model to output a specific format:
```
Return your answer as JSON: {"name": string, "score": number}
```

Works most of the time, but can fail — the model might add commentary or break the schema.

### 2. JSON Mode (Medium)

OpenAI and Anthropic offer JSON mode — the model is constrained to output valid JSON:
```python
response = client.messages.create(
    model="claude-sonnet-4-6",
    messages=[...],
    response_format={"type": "json_object"}
)
```

Guarantees valid JSON, but doesn't enforce a specific schema.

### 3. Schema-Constrained (Strongest)

The model's output is constrained to match a JSON Schema at the token level:
```python
response = client.messages.create(
    model="claude-sonnet-4-6",
    messages=[...],
    response_format={
        "type": "json_schema",
        "schema": {
            "type": "object",
            "properties": {
                "name": {"type": "string"},
                "severity": {"enum": ["low", "medium", "high"]},
                "line_number": {"type": "integer"}
            },
            "required": ["name", "severity"]
        }
    }
)
```

The model literally cannot produce output that violates the schema. This is how [[Tool Use in AI|tool calling]] works under the hood.

## How Tool Calling Uses Structured Output

When an LLM makes a tool call, it's generating structured output:

```json
{
  "tool": "edit_file",
  "arguments": {
    "path": "/src/auth.ts",
    "old_string": "const token = null",
    "new_string": "const token = generateToken()"
  }
}
```

The [[AI Coding Harnesses|coding harness]] parses this JSON and executes the corresponding function. If the JSON were malformed, the entire agent loop would break.

## Structured Output in Practice

### Code Analysis
```json
{
  "issues": [
    {"file": "auth.ts", "line": 42, "type": "security", "message": "SQL injection risk"},
    {"file": "api.ts", "line": 15, "type": "performance", "message": "N+1 query"}
  ]
}
```

### Test Results
```json
{
  "passed": 47,
  "failed": 3,
  "failures": [
    {"test": "test_auth_expired_token", "error": "AssertionError: expected 401, got 200"}
  ]
}
```

### Multi-Step Plans
```json
{
  "steps": [
    {"action": "read", "target": "src/auth/", "reason": "Understand current auth flow"},
    {"action": "edit", "target": "src/auth/login.ts", "change": "Add rate limiting"},
    {"action": "test", "target": "src/auth/__tests__/", "reason": "Verify changes"}
  ]
}
```

## Related

- [[Tool Use in AI]]
- [[AI Agents and Sub-Agents]]
- [[Large Language Models]]
- [[Prompt Engineering]]
