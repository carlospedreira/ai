# AI Agents and Sub-Agents

An AI agent is a system where a [[Large Language Models|Large Language Model]] operates in a loop, using tools to observe, reason, and act toward a goal. Sub-agents extend this by allowing an agent to delegate work to specialized child agents that run in parallel.

## What Makes Something an "Agent"

An agent is more than a chatbot. The key properties:

| Property | Chatbot | Agent |
|---|---|---|
| **Tool use** | None | Reads files, runs commands, calls APIs |
| **Autonomy** | Responds to each message | Plans and executes multi-step tasks |
| **State** | Stateless (or minimal) | Maintains context across many actions |
| **Self-correction** | None | Observes failures, retries with different approach |
| **Persistence** | Session-based | Can run for hours/days on a task |

## Agent Architectures

### ReAct (Reason + Act)

The most common pattern, used by [[Claude Code]], [[OpenAI Codex]], and [[OpenCode]]:

```
Think → Act → Observe → Think → Act → Observe → ... → Answer
```

The model alternates between reasoning about what to do and executing actions. Each observation (tool result) informs the next reasoning step.

### Plan-and-Execute

Separate planning from execution:

```
┌────────────┐     ┌────────────┐
│   Planner  │────►│  Executor  │
│ (makes plan│     │ (runs each │
│  of steps) │     │   step)    │
└────────────┘     └─────┬──────┘
                         │
                    ┌────┴────┐
                    │ Reviser │ (updates plan if step fails)
                    └─────────┘
```

Good for complex tasks where the full plan should be reviewed before execution.

### Tree of Thought

Explore multiple reasoning paths in parallel, evaluate which is most promising:

```
         Prompt
        /  |  \
     Path1 Path2 Path3
      |     |     |
    Eval   Eval  Eval
      \     |
       Best Path
          |
        Answer
```

Used for problems with multiple valid approaches where it's unclear which is best.

## Sub-Agents

Sub-agents are child agents spawned by a parent agent to handle parts of a task. This is a critical pattern in modern [[AI Coding Harnesses]].

### Why Sub-Agents?

1. **Parallelism** — Explore multiple files/modules simultaneously
2. **Context isolation** — Each sub-agent has its own context window, preventing pollution
3. **Specialization** — Different sub-agents can use different models or tools
4. **Scale** — Handle tasks too large for a single context window

### Sub-Agent Patterns

#### Fan-Out / Fan-In
Parent spawns multiple sub-agents, collects their results:

```
         Parent Agent
        /     |      \
    Search  Search  Search
    Module1 Module2 Module3
        \     |      /
         Parent Agent
         (synthesizes)
```

**Example in Claude Code:**
```
"Find all usages of the deprecated `auth.verify()` method across the codebase"
→ Spawns 3 sub-agents searching different directories in parallel
→ Parent collects results and presents unified list
```

#### Specialist Delegation
Parent delegates to sub-agents with specific roles:

```
         Parent Agent
        /            \
  Code Explorer    Test Runner
  (reads, searches) (executes tests)
```

#### Pipeline
Sub-agents form a sequential chain:

```
Analyzer → Planner → Implementer → Reviewer
```

### Sub-Agents in Practice

| Harness | Sub-Agent Support |
|---|---|
| [[Claude Code]] | Yes — spawns background agents for search, exploration, and specialized tasks. Configurable agent types (Explore, Plan, general-purpose). |
| [[OpenAI Codex]] | Cloud Codex can spawn sub-tasks. CLI is single-agent. |
| [[OpenCode]] | Two built-in agents (build/plan). Custom agents via plugins. |
| **Cursor** | Up to 8 parallel sub-agents in Composer mode. |
| **Devin** | Multiple internal agents (planner, coder, reviewer). |

### Claude Code Sub-Agents Deep Dive

Claude Code's sub-agent system is particularly rich:

- **Explore agent** — Fast, read-only agent for codebase exploration. Uses Glob, Grep, Read tools.
- **Plan agent** — Designs implementation strategies. Read-only, returns step-by-step plans.
- **General-purpose agent** — Full agent with all tools for complex, multi-step tasks.
- **Background execution** — Sub-agents can run in the background while the parent continues.
- **Worktree isolation** — Sub-agents can operate in isolated git worktrees to avoid conflicts.

Example flow:
```
User: "Refactor the payment module to use the Strategy pattern"

Parent Agent:
  ├─ Spawns Explore agent → "Find all payment-related files"
  ├─ Spawns Plan agent → "Design the Strategy pattern refactoring"
  ├─ (waits for both)
  ├─ Reviews plan, adjusts based on exploration results
  ├─ Implements changes using its own tools
  ├─ Spawns test-runner agent → "Run payment module tests"
  └─ Reports results to user
```

## Agent Frameworks and SDKs

For building custom agents beyond coding:

| Framework | By | Language | Key Feature |
|---|---|---|---|
| **Claude Agent SDK** | Anthropic | Python | Built on Claude, tool use, handoffs |
| **OpenAI Agents SDK** | OpenAI | Python | Agents, handoffs, guardrails |
| **LangGraph** | LangChain | Python | Graph-based agent orchestration |
| **CrewAI** | Community | Python | Multi-agent role-playing |
| **AutoGen** | Microsoft | Python | Multi-agent conversations |
| **Mastra** | Community | TypeScript | Agent framework for JS/TS |

See [[AI Agent Frameworks]] for deeper coverage.

## Challenges

- **Cost** — Each sub-agent uses API tokens independently
- **Coordination** — Sub-agents may make conflicting changes
- **Context loss** — Information doesn't automatically flow between agents
- **Debugging** — Multi-agent systems are harder to trace
- **Runaway agents** — Without guardrails, agents can loop indefinitely

## Related

- [[AI Coding Harnesses]]
- [[Agentic Coding Paradigm]]
- [[AI Coding Workflows]]
- [[AI Agent Frameworks]]
- [[Tool Use in AI]]
- [[Claude Code]]
