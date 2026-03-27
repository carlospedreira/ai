 # Model Context Protocol

Model Context Protocol (MCP) is an open standard introduced by Anthropic that defines how [[AI Coding Harnesses|AI coding agents]] and [[Large Language Models]] connect to external tools and data sources. It is the "USB-C for AI" — a universal interface between models and the tools they use.

## The Problem MCP Solves

Before MCP, every AI tool had its own proprietary way of connecting to external services:
- [[Claude Code]] had its own tool API
- [[OpenAI Codex]] had a different one
- Every IDE had yet another

This meant tool developers had to build separate integrations for each platform. MCP standardizes this: build a tool once, and it works in any MCP-compatible harness.

## How It Works

```
┌──────────────────┐     MCP Protocol     ┌──────────────────┐
│   MCP Client     │◄───────────────────►│   MCP Server     │
│  (Claude Code,   │   JSON-RPC over     │  (DB, API, Jira, │
│   Codex, OpenCode│    stdio/SSE)       │   Slack, custom) │
│   Cursor, etc.)  │                     │                  │
└──────────────────┘                     └──────────────────┘
```

### Components

- **MCP Client** — The AI harness (e.g., Claude Code, Cursor). Discovers available tools and calls them.
- **MCP Server** — A process that exposes tools, resources, and prompts to the client.
- **Transport** — Communication channel (stdio for local servers, SSE/HTTP for remote).

### What a Server Exposes

| Capability | Description | Example |
|---|---|---|
| **Tools** | Functions the LLM can call | `query_database`, `create_jira_ticket` |
| **Resources** | Data the LLM can read | Database schemas, API docs, file listings |
| **Prompts** | Reusable prompt templates | "Summarize this PR", "Review this code" |

## Example MCP Server Configuration

In Claude Code's `settings.json`:

```json
{
  "mcpServers": {
    "postgres": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-postgres"],
      "env": {
        "DATABASE_URL": "postgresql://..."
      }
    }
  }
}
```

This gives Claude Code access to query a PostgreSQL database directly during a session.

## The MCP Ecosystem

### Popular MCP Servers

| Server | Purpose |
|---|---|
| `server-postgres` | Query PostgreSQL databases |
| `server-sqlite` | Query SQLite databases |
| `server-github` | GitHub API (issues, PRs, repos) |
| `server-slack` | Read/send Slack messages |
| `server-filesystem` | Extended file operations |
| `server-puppeteer` | Browser automation |
| `server-memory` | Persistent knowledge graph |

### Compatible Clients

All major [[AI Coding Harnesses]] now support MCP:
- [[Claude Code]] — First-class support (Anthropic created MCP)
- [[OpenAI Codex]] — Added MCP support
- [[OpenCode]] — Full MCP support
- **Cursor** — MCP integration
- **Windsurf** — MCP support
- **Cline / Roo Code** — MCP support

## Building an MCP Server

MCP servers can be built in any language. SDKs are available for:
- TypeScript (`@modelcontextprotocol/sdk`)
- Python (`mcp`)
- Rust, Go, and other community SDKs

A minimal server defines its tools as functions with typed parameters and descriptions. The LLM reads these descriptions to understand when and how to use each tool.

## Why MCP Matters

1. **Interoperability** — Tools work across all harnesses, not just one
2. **Ecosystem growth** — Lower barrier to building integrations
3. **Composability** — Combine multiple MCP servers for rich capabilities
4. **Separation of concerns** — Tool logic is separate from the AI harness
5. **Security** — Each server runs as a separate process with its own permissions

## Related

- [[AI Coding Harnesses]]
- [[Claude Code]]
- [[OpenAI Codex]]
- [[OpenCode]]
- [[Agentic Coding Paradigm]]
