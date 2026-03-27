# AI Agent Frameworks

Agent frameworks provide the scaffolding to build custom [[AI Agents and Sub-Agents|AI agents]] — applications where [[Large Language Models]] reason, use tools, and act autonomously. They sit between the raw API and a full [[AI Coding Harnesses|coding harness]].

## Why Frameworks?

Building an agent from scratch requires solving:
- Tool calling and result handling
- Context window management
- Multi-turn conversation state
- Error recovery and retries
- Multi-agent coordination
- Guardrails and safety

Frameworks provide these building blocks so developers can focus on their agent's specific logic.

## Major Frameworks

### Claude Agent SDK (Anthropic)

Purpose-built for creating agents on Claude.

- **Language**: Python
- **Key concepts**: Agent, Tool, Handoff, Guardrail
- **Handoffs**: An agent can delegate to another agent, transferring the conversation
- **Built-in tools**: Computer use, text editor, bash, MCP integration
- **Tracing**: Built-in observability for debugging agent behavior

```python
from claude_agent_sdk import Agent, Tool

agent = Agent(
    model="claude-sonnet-4-6",
    tools=[read_file, write_file, run_tests],
    instructions="You are a coding assistant..."
)
result = agent.run("Fix the failing tests in src/auth/")
```

### OpenAI Agents SDK

OpenAI's framework for building multi-agent systems.

- **Language**: Python
- **Key concepts**: Agent, Runner, Handoff, Guardrail, Tracing
- **Handoffs**: Agents can transfer control to specialist agents
- **Guardrails**: Input/output validation with custom logic
- **Built-in tracing**: Full execution traces for debugging

### LangChain / LangGraph

The most mature and widely-adopted ecosystem.

- **Language**: Python, JavaScript
- **LangChain**: Core library for chains, tools, prompts, memory
- **LangGraph**: Graph-based orchestration for complex agent workflows
- **LangSmith**: Observability and evaluation platform
- **Key feature**: Cyclic graphs for agents that loop and branch

```python
from langgraph.graph import StateGraph

graph = StateGraph(AgentState)
graph.add_node("planner", plan_step)
graph.add_node("executor", execute_step)
graph.add_node("reviewer", review_step)
graph.add_edge("planner", "executor")
graph.add_conditional_edge("reviewer", should_revise, {
    True: "planner",
    False: END
})
```

### CrewAI

Multi-agent framework where agents have roles, goals, and backstories.

- **Language**: Python
- **Key concept**: Crews of agents with defined roles collaborate on tasks
- **Process types**: Sequential (one after another) or hierarchical (manager delegates)
- **Memory**: Shared and individual agent memory

```python
researcher = Agent(role="Senior Researcher", goal="Find relevant papers", ...)
writer = Agent(role="Technical Writer", goal="Write the report", ...)
crew = Crew(agents=[researcher, writer], tasks=[...], process=Process.sequential)
```

### AutoGen (Microsoft)

Multi-agent conversation framework.

- **Language**: Python
- **Key concept**: Agents communicate through messages in a group chat
- **Code execution**: Built-in sandboxed code execution
- **Human-in-the-loop**: Easy to insert human approval points

### Mastra

TypeScript-first agent framework.

- **Language**: TypeScript
- **Key feature**: First-class TypeScript support (most frameworks are Python-first)
- **Integrations**: Built-in connectors for common services
- **Workflows**: Declarative workflow definitions

## Comparison

| Framework | Language | Multi-Agent | MCP | Best For |
|---|---|---|---|---|
| Claude Agent SDK | Python | Handoffs | Yes | Claude-powered agents |
| OpenAI Agents SDK | Python | Handoffs | No | OpenAI-powered agents |
| LangGraph | Python/JS | Graph-based | Yes | Complex workflows |
| CrewAI | Python | Role-based | Limited | Team-of-agents scenarios |
| AutoGen | Python | Chat-based | No | Research, code generation |
| Mastra | TypeScript | Yes | Yes | JS/TS ecosystems |

## When to Use What

| Scenario | Recommendation |
|---|---|
| Building a coding harness | Claude Agent SDK or OpenAI Agents SDK |
| Complex multi-step workflow | LangGraph |
| Team of specialized agents | CrewAI |
| Quick prototype | LangChain |
| TypeScript project | Mastra |
| Research / experimentation | AutoGen |

## Agent Design Patterns

### Single Agent + Tools
Simplest pattern. One agent with access to multiple tools.
- Good for: focused tasks, clear scope
- Used in: simple chatbots, single-purpose agents

### Router Agent
A dispatcher agent that routes tasks to specialist agents:
```
User Request → Router → {CodeAgent, SearchAgent, DataAgent}
```

### Hierarchical Team
Manager agent coordinates worker agents:
```
Manager
├── Researcher (gathers info)
├── Coder (writes code)
└── Reviewer (checks quality)
```

### Swarm / Handoff
Agents transfer control based on the conversation:
```
Triage Agent → (coding question) → Code Agent
             → (data question) → Data Agent
             → (unclear) → Clarification Agent
```

## Related

- [[AI Agents and Sub-Agents]]
- [[Tool Use in AI]]
- [[AI Coding Harnesses]]
- [[Model Context Protocol]]
- [[Large Language Models]]
