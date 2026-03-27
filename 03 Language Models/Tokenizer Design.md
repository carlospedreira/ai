# Tokenizer Design

Tokenizer design determines how raw text is split into [[Tokens and Tokenization|tokens]] before being processed by a [[Large Language Models|Large Language Model]]. The tokenizer is the first and last component in the pipeline — it shapes what the model sees and directly impacts cost, performance, and multilingual capability.

## The Design Space

A tokenizer must balance three competing objectives:

| Objective | Wants | Trade-off |
|---|---|---|
| **Compression** | Fewer tokens per text → cheaper, faster | Needs larger vocabulary |
| **Coverage** | Handle any input (all languages, code, emoji) | Rare items get poor tokenization |
| **Learnability** | Meaningful, consistent token boundaries | Hard to guarantee with statistical methods |

## Byte-Pair Encoding (BPE)

The dominant algorithm, used by GPT, Claude, and LLaMA.

### Training Algorithm

1. Start with a vocabulary of individual bytes (256 entries)
2. Count all adjacent token pairs in the training corpus
3. Merge the most frequent pair into a new token
4. Repeat steps 2–3 until the desired vocabulary size is reached

### Example

```
Corpus: "low low low low lowest lowest newer newer"

Initial: l o w l o w l o w l o w l o w e s t ...
Step 1:  merge (l, o) → lo:  lo w lo w lo w lo w lo w e s t ...
Step 2:  merge (lo, w) → low: low low low low low e s t ...
Step 3:  merge (e, s) → es:  low low low low low es t ...
Step 4:  merge (es, t) → est: low low low low low est ...
Step 5:  merge (low, est) → lowest: low low low low lowest ...
```

Common words become single tokens. Rare words are split into subword pieces.

### Properties

- **No unknown tokens** — Can always fall back to individual bytes
- **Efficient** — Common words use one token; rare words use multiple
- **Language-agnostic** — Works on any byte sequence
- **Deterministic** — Same text always produces the same tokens

## SentencePiece

Google's tokenizer framework, used by Gemini and T5.

- Treats the input as a raw byte stream (no pre-tokenization)
- Supports both BPE and Unigram algorithms
- Language-agnostic — doesn't assume spaces separate words (critical for Chinese, Japanese, Thai)
- Adds a special character (▁) for word boundaries

## Unigram Language Model

Alternative to BPE, used by SentencePiece:

1. Start with a large vocabulary (all substrings up to some length)
2. Score each token by how much removing it would increase the corpus loss
3. Remove the least useful tokens
4. Repeat until the desired vocabulary size

Works top-down (pruning) rather than bottom-up (merging like BPE).

## Vocabulary Size Trade-offs

| Vocab Size | Tokens/Word | Memory | Example |
|---|---|---|---|
| 256 (bytes) | ~4.5 | Tiny | Character-level models |
| 32K | ~1.3 | Moderate | GPT-2 |
| 100K | ~1.1 | Large | GPT-4, Claude |
| 128K | ~1.0 | Larger | LLaMA 3 |
| 256K | ~0.9 | Very large | Gemini |

Larger vocabulary = more text per token (cheaper) but bigger embedding table (more parameters).

## Tokenizer Artifacts

Tokenizers create surprising behaviors in LLMs:

### Arithmetic Difficulty
```
"123456" might tokenize as ["123", "456"]
The model sees two separate tokens, not the number 123,456
This is why LLMs struggle with arithmetic on large numbers
```

### Leading Space Sensitivity
```
"hello" → ["hello"]           (1 token)
" hello" → [" hello"]         (1 different token)
"  hello" → [" ", " hello"]   (2 tokens)
```

The presence or absence of a leading space changes the token entirely.

### Language Inequality
Tokenizers trained primarily on English text are inefficient for other languages:
```
English: "Hello, how are you?" → 6 tokens
Chinese: "你好，你怎么样？" → 11 tokens (same meaning, nearly 2x cost)
Hindi:   "नमस्ते, आप कैसे हैं?" → 17 tokens
```

This means non-English users pay more per API call and fill their [[Context Management|context window]] faster.

### Code Tokenization
```
"    " (4 spaces) → might be 1 token or 4 tokens depending on the tokenizer
"def " → typically 1 token
"self.variable_name" → might be ["self", ".", "variable", "_", "name"]
```

Indentation-heavy languages (Python) are affected by whitespace tokenization.

## Special Tokens

Every tokenizer includes special tokens with reserved meaning:

| Token | Purpose |
|---|---|
| `<BOS>` / `<s>` | Beginning of sequence |
| `<EOS>` / `</s>` | End of sequence |
| `<PAD>` | Padding for batch alignment |
| `<UNK>` | Unknown token (rare in modern tokenizers) |
| `<|im_start|>` | Chat template markers (varies by model) |
| `<tool_call>` | Tool use delimiters |

Chat templates wrap messages with special tokens:
```
<|im_start|>system
You are a helpful assistant.<|im_end|>
<|im_start|>user
Hello!<|im_end|>
<|im_start|>assistant
```

## Tokenizer and Model Must Match

A tokenizer is trained alongside (or before) the model. You cannot swap tokenizers between models — the embedding table maps specific token IDs to learned vectors. Using the wrong tokenizer produces garbage.

This is why [[Open Source AI Models]] always distribute the tokenizer alongside the model weights.

## Related

- [[Tokens and Tokenization]]
- [[Large Language Models]]
- [[Word Embeddings]]
- [[Transformers]]
- [[Tokenomics of AI]]
