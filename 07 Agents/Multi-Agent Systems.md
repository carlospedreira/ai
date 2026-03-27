# Multi-Agent Systems

Multi-agent systems are architectures where multiple [[AI Agents and Sub-Agents|AI agents]] collaborate, compete, or negotiate to accomplish tasks that are too complex, too large, or too diverse for a single agent. They represent the frontier of agentic AI — moving from a single intelligent worker to coordinated teams.

## Why Multiple Agents?

| Single Agent Limitation | Multi-Agent Solution |
|---|---|
| **Context window limit** | Each agent has its own context — total capacity scales linearly |
| **Single model bottleneck** | Different agents can use different models (cheap for simple tasks, powerful for complex) |
| **No parallelism** | Agents work simultaneously on different sub-tasks |
| **Role confusion** | Each agent has a focused role and system prompt |
| **Single point of failure** | Other agents can compensate if one fails |

## Coordination Patterns

### Orchestrator Pattern

A central "manager" agent distributes tasks and collects results:

```
        Orchestrator
       /     |      \
  Agent A  Agent B  Agent C
  (research) (code)  (review)
       \     |      /
        Orchestrator
        (synthesize)
```

**Used by:** [[Claude Code]] (parent agent spawns sub-agents), [[AI Agent Frameworks|CrewAI]] (manager process)

### Pipeline Pattern

Agents form a sequential chain, each processing and passing results forward:

```
Planner → Coder → Reviewer → Tester → Deployer
```

Each agent specializes in one phase. The output of one becomes the input of the next.

**Used by:** Kiro (spec → design → tasks → code), some [[AI in CI-CD]] workflows

### Swarm / Handoff Pattern

Agents dynamically hand control to whoever is best suited for the current sub-task:

```
User message → Triage Agent
                   ↓
          "This is a coding question"
                   ↓
              Code Agent → "I need database info"
                   ↓
              Database Agent → "Here's the schema"
                   ↓
              Code Agent → "Done, here's the solution"
```

**Used by:** OpenAI Agents SDK (handoffs), [[AI Agent Frameworks|Claude Agent SDK]] (handoffs)

### Debate Pattern

Two agents argue opposing positions; a judge evaluates:

```
Agent A: "We should use PostgreSQL because..."
Agent B: "We should use MongoDB because..."
Judge:   "PostgreSQL is better for this use case because..."
```

Used in [[AI Safety and Alignment Research]] — debate as a scalable oversight mechanism.

### Consensus Pattern

Multiple agents independently solve the same problem; results are compared:

```
Agent 1 → Solution A ─┐
Agent 2 → Solution B ──┤→ Majority vote or merge
Agent 3 → Solution C ──┘
```

This is [[Chain of Thought#Self-Consistency|self-consistency]] scaled to multiple agents.

## Multi-Agent Systems in Coding

### Claude Code Agent Teams

[[Claude Code]] (Opus 4.6) supports full agent teams:
- Multiple agents with independent sessions and context windows
- Communicate via `SendMessage` tool
- Each agent can use different models and tools
- `TeammateIdle` hook for coordination

```
Team Lead Agent
  ├── Frontend Agent (implements UI changes)
  ├── Backend Agent (implements API changes)
  └── Test Agent (writes and runs tests for both)
```

### Cursor Parallel Agents

[[AI Coding Landscape|Cursor]] runs up to 8 sub-agents simultaneously:
- Each explores a different part of the codebase
- Custom Composer model optimizes for parallel execution
- Results synthesized by the main agent

### Devin's Internal Architecture

[[AI Coding Landscape|Devin]] uses multiple specialized internal agents:
- Planner (decomposes tasks)
- Coder (writes code)
- Browser agent (navigates web for documentation)
- Terminal agent (runs commands)

### Factory.ai Droids

Specialized autonomous agents for specific tasks:
- Code review droid
- Migration droid
- Documentation droid
- Test generation droid

## Communication Between Agents

### Message Passing
Agents exchange structured messages:
```json
{
  "from": "research_agent",
  "to": "code_agent",
  "content": "The relevant file is src/auth/jwt.ts. The token validation
              function on line 42 has a bug — it doesn't check expiry.",
  "artifacts": ["src/auth/jwt.ts:42"]
}
```

### Shared State
Agents read/write to a common data store:
- Shared filesystem (git repository)
- Shared database
- [[Knowledge Graphs for AI|Knowledge graph]]
- Task board (like [[Claude Code]]'s TodoWrite)

### Event-Driven
Agents react to events from other agents:
- "Agent A completed its task" → triggers Agent B
- "Test suite failed" → triggers Debug Agent
- [[Claude Code]] hooks: `TeammateIdle`, `TaskCompleted`

## Challenges

### Coordination Overhead
Communication between agents consumes tokens and time. The overhead can exceed the benefit for simple tasks:
```
Single agent: 10 turns × $0.05 = $0.50
Three-agent team: 10 turns each + 15 coordination messages = $2.00
```

Rule of thumb: multi-agent only when the task genuinely benefits from parallelism or specialization.

### Conflicting Actions
Two agents editing the same file simultaneously:
- **Solution:** Git worktree isolation ([[Claude Code]]'s `isolation: worktree`)
- **Solution:** File locking
- **Solution:** Turn-based coordination

### Context Drift
Each agent has its own context and may develop different understandings:
- Agent A thinks the auth module uses JWT
- Agent B thinks it uses OAuth
- **Solution:** Shared project context (CLAUDE.md), periodic sync messages

### Runaway Costs
More agents = more tokens = more cost. Without limits:
- Agent teams can 3× token consumption ([[Claude Code]] documentation)
- Debugging multi-agent coordination issues is harder than single-agent

### Emergent Behavior
Multi-agent systems can develop unexpected interaction patterns:
- Agents that agree with each other without genuine evaluation ("echo chamber")
- Agents that enter infinite delegation loops
- Agents that produce inconsistent results from slightly different contexts

## When to Use Multi-Agent

| Task | Single Agent | Multi-Agent |
|---|---|---|
| Fix a bug in one file | Yes | Overkill |
| Implement a feature touching 3 files | Yes | Optional |
| Refactor an entire module | Possible | Better — parallel exploration |
| Build a feature spanning frontend + backend + tests | Stretched | Ideal — specialized agents |
| Migrate a large codebase | Very hard | Necessary — parallel execution |

## Related

- [[AI Agents and Sub-Agents]]
- [[Autonomous AI Agents]]
- [[Agentic Design Patterns]]
- [[AI Agent Frameworks]]
- [[Claude Code]]
- [[AI Agents in Production]]
- [[AI Coding Workflows]]
- [[AI and Developer Productivity]]
