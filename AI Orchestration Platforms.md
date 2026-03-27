# AI Orchestration Platforms

AI orchestration platforms manage the end-to-end lifecycle of AI applications — from prompt management and model routing through evaluation, deployment, and monitoring. They sit above individual [[AI Agent Frameworks]] and below the final application, providing the "glue" for production AI systems.

## What Orchestration Means

A production AI application involves more than calling an LLM API:

```
User Request
    ↓
Prompt Template Selection
    ↓
Context Assembly (RAG, memory, tools)
    ↓
Model Selection (routing by complexity/cost)
    ↓
LLM Inference (with caching, retry, fallback)
    ↓
Output Processing (parsing, filtering, validation)
    ↓
Evaluation (quality check, safety check)
    ↓
Observability (logging, tracing, metrics)
    ↓
Response to User
```

Orchestration platforms manage this entire pipeline.

## Key Platforms

### LangChain / LangSmith

The most widely adopted orchestration ecosystem:

| Component | Purpose |
|---|---|
| **LangChain** | Core library — chains, agents, tools, memory, prompts |
| **LangGraph** | Graph-based agent orchestration (cycles, branching, state) |
| **LangSmith** | Observability, evaluation, prompt management, dataset management |
| **LangServe** | Deploy chains as REST APIs |

```python
from langchain_anthropic import ChatAnthropic
from langchain.prompts import ChatPromptTemplate
from langchain_core.output_parsers import StrOutputParser

chain = (
    ChatPromptTemplate.from_template("Summarize: {text}")
    | ChatAnthropic(model="claude-sonnet-4-6")
    | StrOutputParser()
)
result = chain.invoke({"text": article})
```

**Strengths:** Massive ecosystem, extensive integrations, strong community
**Weaknesses:** Abstraction overhead, rapidly changing API, can be over-engineered for simple tasks

### LlamaIndex

Focused specifically on [[Retrieval-Augmented Generation|RAG]] and data-connected AI:

| Component | Purpose |
|---|---|
| **LlamaIndex Core** | Data connectors, indexing, query engines |
| **LlamaParse** | Document parsing (PDF, DOCX, HTML) |
| **LlamaCloud** | Managed RAG infrastructure |

```python
from llama_index.core import VectorStoreIndex, SimpleDirectoryReader

documents = SimpleDirectoryReader("./data").load_data()
index = VectorStoreIndex.from_documents(documents)
query_engine = index.as_query_engine()
response = query_engine.query("What is the refund policy?")
```

**Strengths:** Best-in-class RAG, document handling, structured data
**Weaknesses:** Narrower scope than LangChain

### Haystack (deepset)

Open-source NLP/RAG framework:
- Pipeline-based architecture
- Strong document processing
- Production-ready with REST API
- Good for search-heavy applications

### Semantic Kernel (Microsoft)

Microsoft's AI orchestration SDK:
- Native C# and Python
- Deep Azure integration
- Plugin architecture for skills
- Enterprise-focused

### Vercel AI SDK

TypeScript/JavaScript-first AI toolkit:
- Streaming primitives for React/Next.js
- Multi-provider support (used by [[OpenCode]])
- Edge runtime compatible
- UI components for AI interfaces

### Instructor

Lightweight library focused on [[Structured Output]]:
```python
import instructor
from anthropic import Anthropic

client = instructor.from_anthropic(Anthropic())

class User(BaseModel):
    name: str
    age: int

user = client.messages.create(
    model="claude-sonnet-4-6",
    messages=[{"role": "user", "content": "Extract: John is 30 years old"}],
    response_model=User,
)
# user.name = "John", user.age = 30
```

**Strengths:** Simple, focused, works with any model
**Weaknesses:** Only does structured extraction (by design)

## Orchestration Patterns

### Chain Pattern
Sequential processing through multiple steps:
```
Input → Summarize → Extract entities → Generate report → Output
```

### Router Pattern
Classify input and route to specialized handlers:
```
Input → Classifier → {Code question → Code agent,
                       Data question → SQL agent,
                       General → Chat agent}
```

### Map-Reduce Pattern
Process items in parallel, then combine:
```
[Doc1, Doc2, Doc3] → Map(summarize each) → Reduce(combine summaries) → Final summary
```

### Fallback Chain
Try models in order of preference:
```
Try Opus → timeout? → Try Sonnet → error? → Try Haiku → all fail? → Static fallback
```

### Guardrail Pattern
Validate inputs and outputs:
```
Input → Input guard (PII check, injection check) →
LLM → Output guard (toxicity check, accuracy check) → Output
```

## When to Use an Orchestration Platform

| Scenario | Recommendation |
|---|---|
| Simple LLM API call | Direct API call — no framework needed |
| RAG application | LlamaIndex or LangChain |
| Complex agent with tools | LangGraph or [[AI Agent Frameworks\|Claude/OpenAI Agent SDK]] |
| Production deployment with monitoring | LangChain + LangSmith |
| TypeScript/React application | Vercel AI SDK |
| Structured extraction only | Instructor |
| Enterprise .NET application | Semantic Kernel |

### The "Do You Need a Framework?" Question

Many AI applications are simple enough that a framework adds unnecessary complexity:

```python
# No framework needed for this:
response = client.messages.create(
    model="claude-sonnet-4-6",
    messages=[{"role": "user", "content": prompt}]
)

# Framework helps when you have:
# - Multi-step pipelines
# - RAG with multiple data sources
# - Complex agent loops with tool use
# - Evaluation and observability needs
# - Multiple model providers
```

The rule of thumb: if you're building more than a chatbot, you probably benefit from a framework. If you're building a chatbot, you probably don't.

## Orchestration vs [[AI Coding Harnesses]]

Orchestration platforms build *new* AI applications. Coding harnesses *are* AI applications:

| | Orchestration Platform | Coding Harness |
|---|---|---|
| Purpose | Build AI-powered apps | Assist with coding |
| User | AI application developer | Software developer |
| Output | An application | Code changes |
| Examples | LangChain, LlamaIndex | [[Claude Code]], [[OpenCode]] |

But coding harnesses *use* orchestration internally — [[Claude Code]] has its own agent loop, tool dispatch, and context management that could be reimplemented on LangGraph.

## Related

- [[AI Agent Frameworks]]
- [[Retrieval-Augmented Generation]]
- [[AI Agents in Production]]
- [[Structured Output]]
- [[AI Development Frameworks]]
- [[Model Serving Infrastructure]]
- [[AI Observability]]
- [[Feature Flags for AI]]
