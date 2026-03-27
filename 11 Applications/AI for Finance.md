# AI for Finance

The financial industry was an early adopter of [[Machine Learning]] and is now rapidly integrating [[Large Language Models]] and [[AI Agents and Sub-Agents|AI agents]]. Finance is both one of AI's highest-value applications and one of its most heavily regulated.

## Key Applications

### Algorithmic Trading

| Application | AI Approach | Status |
|---|---|---|
| **High-frequency trading** | Pattern recognition on market microstructure | Mature, deployed for decades |
| **Sentiment analysis** | LLM-powered news/social media analysis → trading signals | Growing |
| **Portfolio optimization** | [[Reinforcement Learning]] for dynamic allocation | Research → production |
| **Pairs trading** | ML identifies correlated instruments | Established |
| **Alternative data** | Satellite images, web scraping, transaction data → ML predictions | Growing |

### Risk Management

- **Credit scoring** — ML models assess borrower risk from hundreds of features
- **Fraud detection** — Real-time anomaly detection on transactions
- **Market risk** — [[Deep Learning]] for VaR (Value at Risk) estimation
- **Stress testing** — Simulating extreme scenarios with generative models
- **AML (Anti-Money Laundering)** — Graph neural networks detect suspicious transaction patterns

### LLMs in Finance

[[Large Language Models]] are transforming knowledge work in finance:

| Task | How LLMs Help |
|---|---|
| **Research synthesis** | Summarize earnings calls, SEC filings, analyst reports |
| **Document analysis** | Extract data from contracts, term sheets, prospectuses |
| **Client communication** | Draft personalized investment updates |
| **Compliance** | Review documents for regulatory requirements |
| **Code generation** | Quantitative analysts use [[AI Coding Harnesses]] for modeling code |
| **SQL generation** | Analysts query databases in natural language (see [[Text-to-SQL]]) |

### Bloomberg GPT and Finance-Specific Models

**BloombergGPT** (2023): A 50B parameter model trained on Bloomberg's proprietary financial data:
- Outperformed general LLMs on financial NLP tasks
- Demonstrated the value of domain-specific pre-training
- Sparked a wave of finance-specific model development

### Robo-Advisors

Automated investment management:
- **Betterment, Wealthfront** — ML-optimized portfolio allocation
- **Tax-loss harvesting** — Algorithmic identification of tax-saving opportunities
- **Rebalancing** — Automated portfolio maintenance

## AI Coding in Finance

Quantitative finance is one of the most active domains for [[AI Coding Harnesses]]:

### Quantitative Development
```
Quant: "Build a momentum strategy that:
        - Ranks stocks by 12-month return excluding the last month
        - Goes long top decile, short bottom decile
        - Rebalances monthly
        - Includes transaction cost modeling"

Agent: Implements the strategy, backtests, generates performance report
```

### Risk Model Development
```
Quant: "Implement a Monte Carlo simulation for portfolio VaR
        with correlated asset returns using Cholesky decomposition"

Agent: Writes simulation code, validates against known results
```

### Data Pipeline Engineering
Financial data is notoriously messy:
```
Quant: "Clean this tick data: handle gaps, detect outliers,
        adjust for corporate actions, align timestamps"

Agent: Builds the data cleaning pipeline with proper unit tests
```

## Regulatory Environment

Finance is one of the most regulated AI domains:

| Regulation | Jurisdiction | AI Impact |
|---|---|---|
| **SR 11-7** (Model Risk Management) | US (Federal Reserve) | All ML models require validation, documentation, ongoing monitoring |
| **GDPR** | EU | Right to explanation for automated decisions affecting individuals |
| **[[AI Regulation\|EU AI Act]]** | EU | Credit scoring classified as high-risk AI |
| **Fair lending laws** | US | AI must not discriminate in lending decisions |
| **MiFID II** | EU | Algorithmic trading transparency requirements |
| **Basel III/IV** | Global | Capital requirements influenced by model risk |

### Explainability Requirements

Financial regulators often require **model explainability** — you must be able to explain *why* a loan was denied or *why* a trade was flagged:

```
Regulator: "Why did your model deny this applicant?"
Black box: "The model said no." ← unacceptable
Explainable: "Income-to-debt ratio (0.45) exceeded threshold (0.40),
              and credit utilization (85%) indicates elevated risk." ← acceptable
```

This creates tension with [[Deep Learning]] models, which are inherently less interpretable than traditional models. [[Large Language Models]] can explain their reasoning in natural language, but whether that explanation is *faithful* (accurately reflects the model's actual computation) is debated.

## Challenges

### [[Hallucination and Grounding|Hallucination]]
Financial AI cannot fabricate numbers or cite non-existent regulations:
- Bloomberg data as ground truth via [[Retrieval-Augmented Generation|RAG]]
- Strict citation requirements
- Human review for all external-facing outputs

### Market Manipulation
AI trading systems must not:
- Engage in spoofing (placing and canceling orders to manipulate price)
- Trade on material non-public information
- Create artificial market activity
- Coordinate with other AI systems in anti-competitive ways

### Systemic Risk
If many firms use similar AI models, they may all react identically to market events:
- Correlated strategies amplify market movements
- Flash crashes from algorithmic feedback loops
- Regulators increasingly monitor AI model concentration

### Data Privacy
Financial data is highly sensitive:
- [[Federated Learning]] for cross-institutional model training without data sharing
- [[On-Device AI]] for client-facing applications
- Strict data governance for [[AI Coding Harnesses]] used on financial code

## Related

- [[Machine Learning]]
- [[Large Language Models]]
- [[AI Regulation]]
- [[AI Ethics and Safety]]
- [[Retrieval-Augmented Generation]]
- [[Text-to-SQL]]
- [[AI Coding Harnesses]]
- [[Responsible AI Deployment]]
- [[Hallucination and Grounding]]
