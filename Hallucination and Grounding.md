# Hallucination and Grounding

Hallucination is when a [[Large Language Models|Large Language Model]] generates text that is fluent and confident but factually incorrect. Grounding is the set of techniques used to reduce hallucination by anchoring the model's output in real data.

## What Is Hallucination?

LLMs are trained to predict plausible next tokens, not to verify truth. This means they can:

- **Fabricate facts** — "The Eiffel Tower was built in 1923" (it was 1889)
- **Invent citations** — Reference papers or URLs that don't exist
- **Confuse details** — Mix up attributes of similar entities
- **Confabulate code** — Call APIs or functions that don't exist in the codebase
- **Overstate confidence** — Present uncertain information as definitive

### Why It Happens

1. **Training objective** — Next-token prediction rewards plausibility, not accuracy
2. **Compressed knowledge** — The model stores knowledge in distributed weights, not a database it can query
3. **No source attribution** — The model doesn't "know" where its information came from
4. **Knowledge cutoff** — Training data has a fixed end date
5. **Sycophancy** — Models tend to agree with the user or generate what seems desired

## Hallucination in Coding

For [[AI Coding Harnesses]], hallucination manifests as:

| Type | Example |
|---|---|
| **Non-existent APIs** | Calling `response.getBody()` on a library that uses `.data` |
| **Wrong file paths** | Editing `src/auth/login.ts` when the actual path is `src/authentication/sign-in.ts` |
| **Fabricated imports** | `import { validateUser } from './utils'` when no such export exists |
| **Incorrect flags** | `git commit --no-edit --amend` with wrong flag behavior |
| **Phantom functions** | Referencing functions that existed in training data but not in this codebase |

This is why [[AI Coding Harnesses]] use tools (file reading, grep, LSP) rather than relying on the model's "memory" of code — tools provide **grounded** information.

## Grounding Techniques

### 1. Retrieval-Augmented Generation (RAG)

See [[Retrieval-Augmented Generation]]. Inject relevant documents into the prompt before generation:

```
Without RAG: "What's our API rate limit?" → Model guesses based on training data
With RAG:    "What's our API rate limit?" → Retrieves docs → "100 req/min per user"
```

### 2. Tool Use

See [[Tool Use in AI]]. Instead of answering from memory, the model calls tools to get real data:

```
Model: "Let me check the actual file..."
→ Calls read_file("src/config.ts")
→ Gets real content
→ Answers based on what it read
```

This is why [[Agentic Coding Paradigm|agentic coding]] is more reliable than chat-based coding — the agent verifies reality through tools.

### 3. Citation and Source Attribution

Require the model to cite its sources:
```
"Answer the question based on the retrieved documents.
For each claim, cite the document number."
```

If the model can't cite a source, it's more likely to acknowledge uncertainty.

### 4. Self-Verification

Ask the model to verify its own output:
```
"Generate the answer, then check each claim against the provided context.
Flag any claims you can't verify."
```

### 5. Constrained Generation

See [[Structured Output]]. Force the model to output valid JSON matching a schema — prevents free-form hallucination in structured contexts.

### 6. Temperature and Sampling

Lower temperature (closer to 0) makes the model more deterministic and less likely to hallucinate creative details. Higher temperature increases diversity but also hallucination risk.

## How Coding Agents Mitigate Hallucination

| Strategy | How It Works |
|---|---|
| **File reading before editing** | Agent reads the actual file instead of guessing its contents |
| **Grep/glob search** | Finds real file paths and symbol names |
| **LSP integration** | Get real type information, diagnostics |
| **Test execution** | Run tests to verify code actually works |
| **Error feedback** | When a command fails, the error message grounds the next attempt |
| **CLAUDE.md / AGENTS.md** | Project instructions prevent assumptions about conventions |

The agent loop itself is a grounding mechanism: **observe → reason → act → observe** keeps the model anchored in reality.

## Measuring Hallucination

- **Factual accuracy** — Compare claims against verified facts
- **Faithfulness** — Does the output match the provided context?
- **Self-consistency** — Does the model give the same answer when asked differently?

## Related

- [[Large Language Models]]
- [[Retrieval-Augmented Generation]]
- [[Tool Use in AI]]
- [[AI Coding Harnesses]]
- [[AI Ethics and Safety]]
- [[Context Management]]
