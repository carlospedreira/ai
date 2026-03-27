# Conversation Design for AI

Conversation design is the practice of crafting how an AI system communicates with users — its persona, turn structure, error handling, and interaction patterns. It sits at the intersection of UX design, [[Prompt Engineering]], and [[Context Engineering]], and it determines whether an AI application feels helpful or frustrating.

## System Prompts

The system prompt defines the AI's identity, capabilities, and constraints. It is the single most important piece of conversation design.

### Anatomy of a Good System Prompt

```markdown
You are a senior software engineer helping developers debug code.

## Behavior
- Be direct and concise. Lead with the answer, not the reasoning.
- When you identify a bug, explain the root cause and provide a fix.
- If you're unsure, say so explicitly rather than guessing.
- Never suggest changes to files you haven't read.

## Constraints
- Only suggest changes to the codebase in the current project.
- Do not modify files in node_modules/ or .git/.
- Always run tests after making changes.

## Format
- Use code blocks with language identifiers for all code.
- Keep explanations under 3 sentences unless the user asks for more.
```

### What Makes It Effective

| Principle | Example |
|---|---|
| **Be specific** | "Be concise" < "Keep explanations under 3 sentences" |
| **Define boundaries** | "Do not modify files in node_modules/" |
| **Set tone** | "Be direct. Lead with the answer." |
| **Handle uncertainty** | "If you're unsure, say so explicitly" |
| **Specify format** | "Use code blocks with language identifiers" |

### Common Mistakes

| Mistake | Problem |
|---|---|
| Vague instructions | "Be helpful" — the model already tries to be helpful |
| Contradictory rules | "Be thorough" + "Be concise" — which wins? |
| Too long | 2000-word system prompts dilute important instructions |
| Negative-only | "Don't do X, don't do Y" — tell it what TO do |
| Unverifiable rules | "Always be accurate" — the model can't self-verify accuracy |

## Persona Design

The AI's persona affects how users interact with it:

### Technical Assistant (Claude Code default)
- Direct, minimal filler
- Shows reasoning only when relevant
- Uses technical terminology without explanation
- Asks for clarification when the task is ambiguous

### Conversational Teacher
- Explains concepts step by step
- Checks understanding ("Does this make sense?")
- Uses analogies and examples
- Gentler with errors

### Silent Worker
- Minimal text output
- Just executes tasks and reports results
- "Done. Tests pass. 3 files modified."
- Good for [[Autonomous Coding Agents]] and [[AI in CI-CD]]

## Turn Structure

How conversations flow between user and AI:

### The Ideal Turn
```
User: Clear instruction with context
AI:   Direct response → Action → Verification → Brief summary
```

### Multi-Turn Patterns

**Clarification:**
```
User: "Fix the auth"
AI:   "I see several auth-related files. Are you referring to:
       1. The JWT validation in src/auth/jwt.ts (has a known bug)
       2. The OAuth flow in src/auth/oauth.ts
       3. Something else?"
User: "The JWT one"
AI:   *fixes it*
```

**Progressive disclosure:**
```
User: "What's wrong with this code?"
AI:   "The main issue is a null pointer on line 42.
       Want me to explain the full trace, or just fix it?"
User: "Just fix it"
AI:   *fixes it*
```

**Confirmation before destructive action:**
```
AI:   "I'm about to delete the legacy auth module (5 files, ~800 lines).
       This is irreversible. Should I proceed?"
User: "Yes"
AI:   *proceeds*
```

## Error Handling

How the AI communicates when things go wrong:

### Good Error Communication
```
"The test suite failed with 3 errors:
 1. test_login_expired_token: Expected 401, got 500 (the fix I made isn't handling expiry)
 2. test_login_missing_email: Expected 400, got 200 (existing bug, not related to my change)
 3. test_admin_access: Timeout (flaky test, not related)

I'll fix #1 now. #2 and #3 are pre-existing."
```

### Bad Error Communication
```
"Something went wrong. Let me try again."
(No context, no diagnosis, no plan)
```

### Key Principles
- **Distinguish AI errors from existing issues**
- **Explain what happened and why**
- **State the plan for recovery**
- **Don't retry the same thing silently**

## Conversation Design in [[AI Coding Harnesses]]

### CLAUDE.md as Conversation Design
The project instruction file shapes every conversation:
```markdown
# Communication Style
- When proposing changes, show the diff first, explain after
- When running commands that might fail, explain what you expect before running
- After completing a task, summarize: what changed, what was tested, what to review
```

### Custom Slash Commands
Define reusable conversation patterns:
```markdown
<!-- .claude/commands/review.md -->
Review the changes in the current branch:
1. Summarize what changed and why
2. Flag any security concerns
3. Check test coverage
4. Rate confidence: high/medium/low

$ARGUMENTS
```

### Hooks for Conversation Control
[[Claude Code]] hooks can inject context into conversations:
```json
{
  "hooks": {
    "UserPromptSubmit": [{
      "hooks": [{
        "type": "command",
        "command": "echo 'Reminder: always run tests after edits'"
      }]
    }]
  }
}
```

## The Uncanny Valley of AI Conversation

AI conversations feel wrong when:
- **Over-apologizing** — "I apologize for the confusion. I'm sorry, but..."
- **Excessive caveats** — "As an AI, I should note that while I can help with..."
- **Fake enthusiasm** — "Great question! I'd be happy to help!"
- **Unnecessary narration** — "Let me think about this... First, I'll consider..."

The best AI conversations feel like talking to a competent colleague — direct, helpful, and human without pretending to be human.

## Related

- [[Prompt Engineering]]
- [[Context Engineering]]
- [[Claude Code]]
- [[AI Coding Harnesses]]
- [[AI Pair Programming]]
- [[Responsible AI Deployment]]
- [[Hallucination and Grounding]]
