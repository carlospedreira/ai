# Agent Client Protocol

Agent Client Protocol (ACP) is an open protocol for communication between AI coding agents and development environments (IDEs, editors, terminals). While [[Model Context Protocol]] (MCP) standardizes how agents connect to *tools and data*, ACP standardizes how agents connect to *editors and IDEs*.

## The Problem ACP Solves

Before ACP, each AI coding agent built its own IDE integration:
- [[Claude Code]] had a VS Code extension with custom IPC
- [[AI Coding Landscape|Cursor]] had its own embedded agent system
- [[AI Coding Landscape|Cline]] was a VS Code-only extension
- None of these worked in JetBrains, Neovim, Zed, or Emacs without separate development

ACP provides a universal protocol: build an agent once, and it works in any ACP-compatible editor.

## Architecture

```
┌────────────────────────┐        ACP Protocol        ┌───────────────────┐
│     IDE / Editor       │◄──────────────────────────►│   AI Agent        │
│                        │    (JSON-RPC over stdio    │                   │
│  - File operations     │     or WebSocket)          │  - LLM inference  │
│  - Terminal access     │                            │  - Tool execution │
│  - Diagnostics         │                            │  - Planning       │
│  - Diff rendering      │                            │  - Memory         │
│  - User interaction    │                            │                   │
└────────────────────────┘                            └───────────────────┘
```

### The IDE Provides (to the agent):
- File reading and editing
- Terminal / command execution
- Language server diagnostics (errors, warnings)
- User interface elements (diffs, approvals, progress)
- Selection context (what the user is looking at)
- Workspace information (project structure, open files)

### The Agent Provides (to the IDE):
- Code changes (diffs, file creates/edits)
- Explanations and reasoning
- Multi-step plans
- Tool call status
- Suggestions and alternatives

## History and Adoption

### Origins
- **January 2026:** JetBrains and Zed jointly announced ACP as an open standard
- Motivated by the proliferation of incompatible agent integrations
- Goal: any agent should work in any editor, like LSP did for language intelligence

### Relationship to LSP

| Protocol | Standardizes | Analogy |
|---|---|---|
| **LSP** (Language Server Protocol) | Language intelligence (completions, diagnostics, go-to-def) | "How editors understand code" |
| **[[Model Context Protocol\|MCP]]** | Agent ↔ external tools (databases, APIs, services) | "How agents connect to the world" |
| **ACP** | Agent ↔ editor communication | "How agents connect to editors" |

Together they form a protocol stack:
```
┌────────────────────────┐
│  Editor / IDE          │
├────────────────────────┤ ← ACP (agent ↔ editor)
│  AI Agent              │
├────────────────────────┤ ← MCP (agent ↔ tools)
│  External Tools        │
├────────────────────────┤ ← LSP (editor ↔ language server)
│  Language Intelligence │
└────────────────────────┘
```

### Current Support

| Tool | ACP Support | Notes |
|---|---|---|
| **Zed** | Native | Co-created the protocol |
| **JetBrains IDEs** | Native | Co-created the protocol; all IDEs (IntelliJ, PyCharm, etc.) |
| **Cursor** | Native | Uses ACP for its agent architecture |
| **Cline** | Yes | Works across editors via ACP |
| **Roo Code** | Yes | Extended ACP support |
| **[[OpenCode]]** | Yes | `opencode acp` launches an ACP server |
| **Claude Code** | Partial | VS Code extension uses custom IPC; JetBrains via plugin |
| **Neovim** | Community plugins | Via ACP-compatible plugins |
| **Emacs** | Community plugins | Early support |

## ACP vs Custom Integrations

### Before ACP
```
Claude Code ──custom──→ VS Code extension
Claude Code ──custom──→ JetBrains plugin
Cline       ──custom──→ VS Code extension (only)
Cursor      ──custom──→ Built-in (fork, only)
= 4 agents × N editors = many separate integrations
```

### With ACP
```
Claude Code ──ACP──→ Any ACP editor
Cline       ──ACP──→ Any ACP editor
Cursor      ──ACP──→ Any ACP editor
= Each agent implements ACP once; works everywhere
```

## Capabilities

ACP defines a set of capabilities that editors and agents negotiate at connection time:

### Editor Capabilities
- `fileOperations` — Read, write, create, delete files
- `terminal` — Execute commands, read output
- `diagnostics` — Language server errors and warnings
- `diff` — Show proposed changes in the editor's diff viewer
- `approval` — Present approval dialogs to the user
- `selection` — Current cursor position and selection
- `workspace` — Project structure, open files, git state

### Agent Capabilities
- `codeGeneration` — Produce code changes
- `explanation` — Provide reasoning and explanations
- `planning` — Multi-step task plans
- `toolUse` — External tool execution (via [[Model Context Protocol|MCP]])
- `multiFile` — Changes spanning multiple files

## Impact

ACP is still early (announced January 2026), but its potential impact mirrors LSP's:
- **LSP (2016)** eliminated the M×N problem for language intelligence (M languages × N editors → M+N implementations)
- **ACP (2026)** aims to eliminate the M×N problem for AI agents (M agents × N editors → M+N implementations)

If adopted widely, it would mean:
- Developers choose their editor and their AI agent independently
- Agents compete on quality, not IDE lock-in
- New editors automatically get AI agent support
- New agents automatically work in all editors

## Related

- [[Model Context Protocol]]
- [[AI Coding Harnesses]]
- [[AI Coding Landscape]]
- [[OpenCode]]
- [[Claude Code]]
- [[Tool Use in AI]]
