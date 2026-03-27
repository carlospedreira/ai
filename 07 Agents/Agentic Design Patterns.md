# Agentic Design Patterns

Agentic design patterns are reusable architectural solutions for building reliable [[AI Agents and Sub-Agents|AI agent]] systems. Just as software engineering has design patterns (Observer, Strategy, Factory), agentic AI has its own emerging pattern language.

## Pattern 1: ReAct (Reason + Act)

The foundational pattern. The agent alternates between reasoning and acting.

```
Think: "I need to find the auth module"
Act:   grep("authenticate", "src/")
Observe: Found in src/auth/login.ts
Think: "Now I should read that file"
Act:   read_file("src/auth/login.ts")
Observe: [file contents]
Think: "The bug is on line 42..."
```

**When to use:** Default pattern for most agent tasks.
**Used by:** [[Claude Code]], [[OpenAI Codex]], [[OpenCode]], most [[AI Agent Frameworks]].

## Pattern 2: Plan-Then-Execute

Separate planning from execution. The planner creates a task list; the executor works through it.

```
┌──────────┐     ┌──────────────┐     ┌──────────┐
│  Planner │────►│  Task List   │────►│ Executor │
│          │     │  1. Read auth│     │          │
│          │     │  2. Fix bug  │     │          │
│          │     │  3. Add test │     │          │
│          │     │  4. Run tests│     │          │
└──────────┘     └──────────────┘     └──────────┘
                       ▲                    │
                       └────────────────────┘
                       (revise plan if step fails)
```

**When to use:** Complex tasks where reviewing the plan before execution is valuable. Prevents "wandering."
**Used by:** Claude Code (TodoWrite tool), OpenCode (plan agent), AWS Kiro (spec-driven).

## Pattern 3: Reflection

The agent evaluates its own output and iterates to improve it.

```
Generate → Critique → Revise → Critique → Accept
```

Example:
```
Generate: Write a function
Critique: "This doesn't handle the empty array edge case"
Revise:   Add empty array check
Critique: "Now it looks correct and handles all cases"
Accept:   Return final version
```

**When to use:** When output quality matters more than speed. Code review, writing, analysis.
**Variation:** External critic (a separate model or tool evaluates the output).

## Pattern 4: Tool Selection

The agent has many tools available and must choose the right one for each sub-task.

```
Task: "Find all usages of deprecated API"
Agent evaluates:
  - grep? → Good for text search
  - LSP find-references? → Better for semantic search
  - read all files? → Too expensive
Decision: Use grep for initial scan, then LSP for precise references
```

**Key principle:** Tool descriptions are critical. See [[Tool Use in AI]] — the model reads descriptions to decide which tool to use.
**Anti-pattern:** Giving the agent too many tools. [[Model Context Protocol|MCP]] Tool Search helps by loading tool definitions on demand.

## Pattern 5: Fan-Out / Fan-In

Spawn multiple sub-agents in parallel, collect and synthesize results.

```
         Parent Agent
        /     |      \
    Search  Search  Search    (fan-out)
    Module1 Module2 Module3
        \     |      /
         Parent Agent           (fan-in)
         (synthesize)
```

**When to use:** Tasks that benefit from parallel exploration. Searching large codebases, investigating multiple hypotheses.
**Used by:** [[Claude Code]] (sub-agents), Cursor (8 parallel agents).

## Pattern 6: Router / Dispatcher

A lightweight agent that classifies the request and routes to a specialist.

```
User request → Router → Is this a coding task? → Code Agent
                      → Is this a search task? → Search Agent
                      → Is this a review task? → Review Agent
                      → Unclear? → Ask for clarification
```

**When to use:** Multi-purpose systems where different tasks need different tools, models, or prompts.
**Used by:** [[AI Agent Frameworks|CrewAI]] (role-based), OpenAI Agents SDK (handoffs).

## Pattern 7: Guardrailed Execution

Wrap every agent action in safety checks.

```
Agent wants to act
       ↓
Pre-check (is this safe?)
       ↓ (pass)
Execute action
       ↓
Post-check (did it work? any damage?)
       ↓
Continue
```

**Implementations:**
- [[Claude Code]] hooks (PreToolUse, PostToolUse)
- [[OpenAI Codex]] sandbox (OS-level enforcement)
- [[Approval Flows and Sandboxing|Permission systems]] (allow/ask/deny)

**When to use:** Always, for any agent with write access or tool use.

## Pattern 8: Iterative Refinement with Feedback

The agent takes an action, observes the result, and refines until a success criterion is met.

```
Write code → Run tests → Tests fail → Read errors → Fix code → Run tests → Tests pass ✓
```

This is the heart of the [[AI Coding Workflows#The Prompt-Code-Test Loop|prompt-code-test loop]].

**When to use:** Any task with a verifiable success criterion (tests pass, build succeeds, output matches schema).
**Key insight:** Having a verification step (tests, linters, type checkers) dramatically improves agent reliability.

## Pattern 9: Memory-Augmented Agent

The agent reads and writes to persistent memory across sessions.

```
Session 1: Agent learns that "npm run test:auth" runs auth tests
         → Writes to MEMORY.md

Session 2: Agent reads MEMORY.md
         → Already knows how to run auth tests
```

**Implementations:**
- [[Claude Code]] auto-memory (MEMORY.md)
- Windsurf Cascade (persistent memory)
- Custom [[Retrieval-Augmented Generation|RAG]] over past sessions

**When to use:** Long-lived projects where the agent works on the same codebase repeatedly.

## Pattern 10: Escalation

The agent recognizes when it's stuck or uncertain and escalates to a human (or a more capable agent).

```
Agent tries fix → Fails → Retries → Fails again → "I'm stuck. Here's what I tried and where I'm blocked. Can you help?"
```

**Key behaviors:**
- Recognize when confidence is low
- Don't loop endlessly on the same approach
- Provide context about what was tried and why it failed
- Ask specific questions, not vague ones

**Anti-pattern:** Agent that retries the same failing approach 50 times instead of asking for help.

## Combining Patterns

Real-world agents combine multiple patterns:

```
Claude Code = ReAct + Plan-Then-Execute + Fan-Out (sub-agents)
            + Guardrailed Execution (hooks/permissions)
            + Iterative Refinement (test loop)
            + Memory-Augmented (CLAUDE.md + auto-memory)
            + Escalation (AskUserQuestion tool)
```

## Related

- [[AI Agents and Sub-Agents]]
- [[Agentic Coding Paradigm]]
- [[AI Agent Frameworks]]
- [[AI Coding Workflows]]
- [[Tool Use in AI]]
- [[Approval Flows and Sandboxing]]
- [[Chain of Thought]]
- [[Context Engineering]]
