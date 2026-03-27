# Tokens and Tokenization

Tokens are the fundamental unit of input and output for [[Large Language Models]]. Understanding tokenization is essential for working with AI effectively — it affects cost, performance, and [[Context Management|context window]] usage.

## What Is a Token?

A token is a chunk of text that the model processes as a single unit. Tokens are not words — they're subword units that balance vocabulary size with coverage.

Examples (using GPT/Claude-style tokenization):

| Text | Tokens | Count |
|---|---|---|
| `hello` | `hello` | 1 |
| `Hello, world!` | `Hello`, `,`, ` world`, `!` | 4 |
| `indescribable` | `ind`, `esc`, `rib`, `able` | 4 |
| `fn main() {` | `fn`, ` main`, `()`, ` {` | 4 |

Rules of thumb:
- ~4 characters per token in English
- ~0.75 words per token
- Code is typically more token-dense than prose
- Non-English languages often use more tokens per word

## Tokenization Algorithms

### Byte-Pair Encoding (BPE)

The dominant algorithm, used by GPT, Claude, and LLaMA.

1. Start with individual bytes/characters as the vocabulary
2. Iteratively merge the most frequent pair of adjacent tokens
3. Repeat until the desired vocabulary size is reached

Result: common words become single tokens, rare words get split into subwords.

### SentencePiece

Google's tokenizer, used by Gemini and T5. Similar to BPE but operates on raw text without pre-tokenization.

### Vocabulary Size

| Model Family | Vocab Size |
|---|---|
| GPT-4 / o-series | ~100K tokens |
| Claude | ~100K tokens |
| LLaMA 3 | ~128K tokens |
| Gemini | ~256K tokens |

Larger vocabularies mean more text per token (more efficient) but a bigger embedding table.

## Why Tokenization Matters

### Cost
API pricing is per-token:
- Input tokens (your prompt) — cheaper
- Output tokens (model's response) — more expensive
- Long [[Context Management|context windows]] with many files = more tokens = more cost

### Context Window
The context window is measured in tokens, not characters:
- 200K tokens ≈ ~150K words ≈ ~500 pages of text
- 1M tokens ≈ ~750K words ≈ ~2,500 pages

For [[AI Coding Harnesses]], this determines how much code the agent can "see."

### Speed
More tokens = slower generation. Output speed is measured in tokens per second.

### Edge Cases
Tokenization can cause unexpected behavior:
- Numbers: `123456` might become `123`, `456` (model may struggle with arithmetic)
- Whitespace: leading spaces and indentation consume tokens
- Special characters: emojis and Unicode can be token-expensive

## Tokenization in Coding Harnesses

| Concern | Impact |
|---|---|
| File contents | Every file read consumes context tokens |
| Tool definitions | Each tool's schema uses tokens |
| Conversation history | Grows with each turn |
| Extended thinking | Internal reasoning uses tokens (billed separately) |
| MCP tool definitions | External tools add to token count |

This is why [[Context Management]] strategies like compaction and selective file reading are critical.

## Counting Tokens

- **Anthropic**: `anthropic.count_tokens()` in the SDK
- **OpenAI**: `tiktoken` library
- **Rule of thumb**: Character count ÷ 4 ≈ token count (English)

## Related

- [[Large Language Models]]
- [[Context Management]]
- [[Transformers]]
- [[Training and Inference]]
