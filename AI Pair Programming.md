# AI Pair Programming

AI pair programming is the practice of working collaboratively with an AI assistant in real time — the developer provides intent and judgment, the AI provides implementation speed and breadth of knowledge. It is distinct from both [[Vibe Coding]] (developer disengaged) and traditional coding (no AI).

## The Pairing Dynamic

In traditional pair programming, two developers share a screen:
- **Driver** — Types code
- **Navigator** — Thinks about direction, catches mistakes

With AI pair programming:
- **Developer** — Navigator (provides intent, reviews, decides)
- **AI** — Driver (writes code, explores, executes)

The developer stays engaged but delegates the mechanical work.

## Modes of AI Pair Programming

### Chat Mode
Conversational back-and-forth. Developer describes a problem, AI proposes a solution, developer refines:
```
Developer: "The login endpoint returns 500 when the user doesn't exist"
AI: *reads the code* "The issue is on line 42 — getUserById throws
     instead of returning null. Here's the fix..."
Developer: "Good, but also add a test for that case"
AI: *writes the test, runs it*
```

### Inline Assistance
AI suggests completions as the developer types (GitHub Copilot, Cursor Tab):
```
Developer types: def validate_email(
AI suggests:     email: str) -> bool:
                     pattern = r'^[\w\.-]+@[\w\.-]+\.\w+$'
                     return bool(re.match(pattern, email))
```

Developer accepts, rejects, or modifies. Fastest feedback loop.

### Agent Mode
Developer describes a task, AI executes autonomously while developer watches:
```
Developer: "Refactor the payment module to use the Strategy pattern"
AI: *reads files, plans, implements across 5 files, runs tests*
Developer: *reviews the diff, requests adjustments*
```

See [[Agentic Coding Paradigm]] and [[AI Coding Workflows]] for detailed patterns.

## Best Practices

### 1. Stay Engaged
The biggest trap is zoning out while the AI works. Review what it produces:
- Read the diffs, don't just accept blindly
- Verify the logic, not just that tests pass
- Question architectural decisions

### 2. Give Good Context
The AI is only as effective as the context you provide:
- Write good [[Context Engineering|CLAUDE.md / AGENTS.md]] files
- Point to relevant files before asking for changes
- Explain the "why" not just the "what"

### 3. Iterate in Small Steps
```
Bad:  "Build the entire authentication system"
Good: "Add the login endpoint" → review → "Add token validation" → review → ...
```

Small steps keep you in control and make review manageable.

### 4. Use the AI's Strengths
AI excels at:
- **Boilerplate** — Repetitive code, CRUD operations, test scaffolding
- **Exploration** — Navigating unfamiliar codebases or APIs
- **Translation** — Porting code between languages or frameworks
- **Pattern application** — Applying known patterns (observer, strategy, etc.)
- **Bug hunting** — Reading error messages and tracing issues

Humans excel at:
- **Requirements** — Knowing what to build and why
- **Architecture** — System-level design decisions
- **Trade-offs** — Balancing competing concerns
- **Edge cases** — Knowing which unusual scenarios matter
- **Domain knowledge** — Business rules, user needs

### 5. Verify with Tests
Always have the AI run tests after changes:
```
"Make the change, then run the test suite to verify nothing broke"
```

The [[AI Coding Workflows#The Prompt-Code-Test Loop|prompt-code-test loop]] is the most reliable pattern.

## Tools for AI Pair Programming

### Terminal-Based
- [[Claude Code]] — Full agent with sub-agents, hooks, MCP
- [[OpenCode]] — Provider-agnostic, persistent sessions
- [[AI Coding Landscape|Aider]] — Git-native, auto-commits

### IDE-Based
- [[AI Coding Landscape|Cursor]] — Deepest IDE integration, parallel agents
- [[AI Coding Landscape|GitHub Copilot]] — Agent mode in VS Code
- [[AI Coding Landscape|Cline]] / Roo Code — Open-source VS Code extensions

### The Hybrid Approach
Many developers use multiple tools:
- **Copilot/Cursor** for inline completions while typing
- **Claude Code/Aider** for larger agent-driven tasks
- **ChatGPT/Claude web** for research and design discussions

Developer surveys (2026) show developers use an average of **2.3 AI tools** simultaneously.

## Measuring Effectiveness

| Metric | How to Measure |
|---|---|
| **Task completion time** | Time from start to working, reviewed code |
| **Code quality** | Defect rate, review feedback |
| **Developer satisfaction** | Self-reported flow state, frustration |
| **Learning** | Does the developer understand the AI's output? |
| **Sustainability** | Can the team maintain AI-generated code? |

## The Skill Shift

AI pair programming changes which skills matter:

| Less Important | More Important |
|---|---|
| Typing speed | [[Prompt Engineering]] and [[Context Engineering]] |
| Memorizing syntax | Understanding architecture and design |
| Writing boilerplate | Code review and validation |
| Searching Stack Overflow | Knowing what to ask the AI |

The best AI pair programmers are experienced developers who know *what* good code looks like, even if they let the AI *write* it.

## Related

- [[AI Coding Workflows]]
- [[AI Coding Harnesses]]
- [[Agentic Coding Paradigm]]
- [[Vibe Coding]]
- [[AI Code Review]]
- [[Context Engineering]]
- [[Prompt Engineering]]
