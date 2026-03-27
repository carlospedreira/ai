# AI Benchmarks

AI benchmarks are standardized tests used to measure and compare the capabilities of [[Large Language Models]] and [[AI Coding Harnesses|coding agents]]. They drive competition, inform model selection, and track progress in the field.

## Coding Benchmarks

### SWE-bench

The gold standard for evaluating [[AI Coding Harnesses|coding agents]].

- **What**: Real GitHub issues from open-source Python repos (Django, Flask, scikit-learn, etc.)
- **Task**: Autonomously resolve the issue by modifying the codebase
- **Evaluation**: Run the project's test suite to verify the fix
- **Variants**:
  - **SWE-bench Lite** — 300 curated issues
  - **SWE-bench Verified** — Human-verified subset with clear acceptance criteria

| Agent | SWE-bench Verified Score |
|---|---|
| Claude Code (Sonnet 4) | ~72.7% |
| JetBrains Junie | 60.8% |
| Devin (launch demo) | 13.86% |

**SWE-bench Pro** (multi-file, long-horizon) sees top models drop ~47 percentage points — real-world complexity remains hard.

### HumanEval

- **What**: 164 Python programming problems (write a function, pass test cases)
- **Task**: Generate a correct function from a docstring
- **By**: OpenAI (2021)
- **Scores**: Frontier models now score 90%+

### MBPP (Mostly Basic Python Problems)

- **What**: 974 simple Python tasks
- **Task**: Generate solutions from descriptions
- **Easier than HumanEval**, useful for testing smaller models

### Vibe Code Bench

- **What**: Tests zero-to-one application generation (see [[Vibe Coding]])
- **Task**: Build a complete app from a natural language description
- **Current best**: ~30% resolved rate — fully reliable app generation remains unsolved

### LiveCodeBench

- **What**: Competitive programming problems from recent contests (not in training data)
- **Task**: Solve novel algorithmic problems
- **Why it matters**: Tests true reasoning, not memorized solutions

## General Language Benchmarks

### MMLU (Massive Multitask Language Understanding)

- **What**: 57 subjects from STEM to humanities (multiple choice)
- **Scores**: Frontier models score 85–90%+
- **Limitation**: Saturating — top models are converging at the ceiling

### GPQA (Graduate-Level Questions)

- **What**: PhD-level science questions (physics, chemistry, biology)
- **Why**: Tests deep reasoning beyond undergraduate knowledge
- **Harder than MMLU** — even domain experts struggle

### ARC (AI2 Reasoning Challenge)

- **What**: Science questions requiring reasoning, not just recall
- **Variants**: ARC-Easy, ARC-Challenge

### HellaSwag

- **What**: Commonsense reasoning about everyday situations
- **Task**: Choose the most plausible continuation of a scenario
- **Status**: Largely saturated by frontier models (95%+)

## Math and Reasoning Benchmarks

| Benchmark | Tests | Current Best |
|---|---|---|
| GSM8K | Grade-school math word problems | 95%+ (saturated) |
| MATH | Competition-level math | 85%+ |
| AIME | American Invitational Mathematics Exam | 80%+ (frontier models) |

## Limitations of Benchmarks

### Contamination
Models may have seen benchmark questions in training data. This inflates scores and makes comparison unreliable. LiveCodeBench addresses this by using recent problems.

### Narrow Scope
SWE-bench tests single-issue bug fixes in Python repos. Real development involves:
- Multi-language codebases
- System design decisions
- Ambiguous requirements
- Long-horizon tasks spanning hours or days

### Goodhart's Law
"When a measure becomes a target, it ceases to be a good measure." Models and agents can be optimized specifically for benchmark performance without improving general capability.

### The Gap
There's a consistent gap between benchmark scores and real-world experience:
- A model scoring 90% on HumanEval still produces buggy code regularly
- SWE-bench Verified scores don't predict performance on your specific codebase
- No benchmark captures the full complexity of professional software engineering

## How to Use Benchmarks

1. **Relative comparison** — Use benchmarks to compare models to each other, not as absolute measures
2. **Task-specific selection** — If you care about coding, prioritize SWE-bench and HumanEval over MMLU
3. **Your own evaluation** — Test models on your actual codebase and tasks
4. **Trend tracking** — Watch benchmark progress over time, not single snapshots

## Related

- [[Large Language Models]]
- [[AI Coding Harnesses]]
- [[AI Coding Landscape]]
- [[Scaling Laws]]
- [[Vibe Coding]]
