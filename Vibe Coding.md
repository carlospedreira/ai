# Vibe Coding

Vibe coding is a term coined by Andrej Karpathy in February 2025 to describe a new style of programming where the developer describes what they want in natural language and lets an [[AI Coding Harnesses|AI coding agent]] handle the implementation. The developer "gives in to the vibes" — accepting code they haven't fully read or understood, guided by whether the output *feels* right.

## The Original Tweet

> "There's a new kind of coding I call 'vibe coding', where you fully give in to the vibes, embrace exponentials, and forget that the code even exists."
> — Andrej Karpathy, Feb 2025

## How It Works

```
Developer intent (natural language)
        ↓
AI agent writes code
        ↓
Developer tests by running it
        ↓
"Looks right" → ship it
"Doesn't work" → describe the problem → AI fixes it
```

The developer never reads the implementation in detail. They interact with the *behavior* of the code, not its source.

## Vibe Coding vs Traditional Coding vs Agentic Coding

| Aspect | Traditional | [[Agentic Coding Paradigm|Agentic]] | Vibe Coding |
|---|---|---|---|
| Who writes code | Human | AI (human reviews) | AI (human doesn't review) |
| Code understanding | Deep | Moderate | Minimal |
| Quality assurance | Manual review + tests | AI runs tests, human verifies | "Does it work?" |
| Best for | Mission-critical systems | Professional development | Prototypes, personal tools, MVPs |
| Risk | Low | Medium | High for production |

## When Vibe Coding Works

- **Prototyping** — Build an MVP in hours instead of days
- **Personal tools** — Scripts, automations, one-off utilities
- **Learning** — Explore unfamiliar languages or frameworks
- **Hackathons** — Speed over quality
- **Front-end experimentation** — Try visual ideas quickly

## When Vibe Coding Fails

- **Production systems** — Unreviewed code harbors bugs, security holes, and tech debt
- **Complex business logic** — Subtle requirements need human understanding
- **Performance-critical code** — AI may choose simple but inefficient approaches
- **Regulated industries** — Code review is often legally required

## The Spectrum

Most real-world AI-assisted development falls somewhere between pure vibe coding and fully manual coding:

```
Manual ◄─────────────────────────────────────────► Vibe
coding   Copilot    Agent-assisted    Agent-led    coding
         suggest    (review code)     (trust AI)   (don't look)
```

## The Counter-Movement: Spec-Driven Development

AWS Kiro (see [[AI Coding Landscape]]) represents the opposite philosophy: instead of vibing, it forces a requirements spec before any code is generated. The spec is reviewed and approved before implementation begins.

## Cultural Impact

Vibe coding has become a cultural touchpoint in software engineering:
- Sparked debates about "the death of programming"
- Inspired tools optimized for the pattern (rapid generation, visual preview)
- Created tension between "move fast" and "understand your code" camps
- Benchmarks now include "vibe code bench" — testing zero-to-one app generation (~30% success rate as of early 2026)

## Related

- [[Agentic Coding Paradigm]]
- [[AI Coding Harnesses]]
- [[AI Coding Workflows]]
- [[Prompt Engineering]]
- [[AI Ethics and Safety]]
