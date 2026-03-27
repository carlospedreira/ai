# Responsible AI Deployment

Responsible AI deployment covers the practical steps organizations take to ship AI systems that are safe, fair, transparent, and reliable. While [[AI Ethics and Safety]] covers principles and [[AI Regulation]] covers law, this note focuses on **what engineering teams actually do**.

## The Deployment Checklist

### Before Launch

- [ ] **Risk assessment** — What could go wrong? Who could be harmed?
- [ ] **Bias audit** — Test for disparate impact across demographics
- [ ] **Red teaming** — Adversarial testing by internal/external testers
- [ ] **[[Prompt Injection]] testing** — Can the system be hijacked via input?
- [ ] **[[Hallucination and Grounding|Hallucination]] testing** — How often does it fabricate information?
- [ ] **Human review of edge cases** — Test ambiguous, sensitive, and unusual inputs
- [ ] **Fallback plan** — How do you disable the system if something goes wrong?
- [ ] **Documentation** — Model cards, system descriptions, known limitations

### At Launch

- [ ] **Monitoring** — Real-time tracking of outputs, errors, and user feedback
- [ ] **Rate limiting** — Prevent abuse and control costs
- [ ] **Content filtering** — Block harmful outputs before they reach users
- [ ] **User disclosure** — Tell users they're interacting with AI
- [ ] **Feedback mechanism** — Let users report problems
- [ ] **Gradual rollout** — Start with a small percentage of users

### After Launch

- [ ] **Ongoing monitoring** — Track drift, new failure modes, user complaints
- [ ] **Incident response** — Process for handling AI-caused harm
- [ ] **Regular audits** — Periodic bias and safety re-evaluation
- [ ] **Model updates** — Retrain or switch models as capabilities improve
- [ ] **User education** — Help users understand AI limitations

## Model Cards

A model card documents what a model is, how it was trained, and where it should (and shouldn't) be used:

```markdown
# Model Card: CustomerSupportBot v2.1

## Intended Use
- Answer customer questions about product features and billing
- NOT for medical, legal, or financial advice

## Training Data
- 500K customer support conversations (2022-2025)
- Synthetic data for edge cases

## Known Limitations
- May hallucinate product features for products released after training cutoff
- Underperforms on queries in languages other than English and Spanish
- Cannot access real-time order status (uses cached data)

## Bias Evaluation
- Tested across gender, age, and geographic demographics
- No significant performance disparity detected in core use case
- Slightly lower accuracy on regional dialect variations
```

## Content Safety Layers

Production AI systems typically have multiple safety layers:

```
User Input
    ↓
Input Filter (block known-bad patterns, PII detection)
    ↓
System Prompt (safety instructions, role constraints)
    ↓
LLM Generation
    ↓
Output Filter (content classifier, toxicity check)
    ↓
Final Response to User
```

### Input Filtering
- Block known jailbreak patterns
- Detect and redact PII (names, emails, SSNs)
- Classify input intent (benign vs. potentially harmful)

### Output Filtering
- Toxicity classifiers (hate speech, harassment, violence)
- Content policy compliance
- Factual consistency checks (compare against known sources)
- PII leakage detection

## Human-in-the-Loop Patterns

| Pattern | When AI Handles | When Human Takes Over |
|---|---|---|
| **Full automation** | All requests | Never (high risk) |
| **AI-first** | Most requests | Edge cases, high-stakes decisions |
| **Human-first** | Never alone | AI drafts, human reviews and sends |
| **Escalation** | Simple cases | Complex cases auto-escalated |

For [[AI Coding Harnesses]], this maps to [[Approval Flows and Sandboxing]]:
- **Full auto** → full automation
- **Auto-edit** → AI-first
- **Suggest mode** → human-first
- **AskUserQuestion tool** → escalation

## Monitoring in Production

### What to Track

| Metric | Why |
|---|---|
| **Response latency** | User experience |
| **Token usage** | Cost control (see [[Tokenomics of AI]]) |
| **Error rate** | Model/API failures |
| **Refusal rate** | Too high = frustrating; too low = unsafe |
| **User feedback** | Thumbs up/down, reports |
| **Content filter triggers** | Volume and type of blocked content |
| **Hallucination rate** | Sampled and manually checked |
| **Drift** | Performance degradation over time |

### Alerting
- Spike in content filter triggers → possible new attack vector
- Spike in refusals → possible model regression
- Spike in negative feedback → investigate root cause
- Cost anomaly → runaway agent or abuse

## Incident Response

When an AI system causes harm:

1. **Detect** — Monitoring alerts, user reports, media attention
2. **Contain** — Disable the feature, increase safety filters, add manual review
3. **Assess** — Determine scope, cause, and impact
4. **Fix** — Update filters, retrain model, fix prompt, change policy
5. **Communicate** — Inform affected users, publish post-mortem if appropriate
6. **Prevent** — Add test cases, update red team playbook, improve monitoring

## AI-Specific Testing

Beyond standard software testing:

| Test Type | What It Checks |
|---|---|
| **Behavioral testing** | Does the model behave correctly on known inputs? |
| **Adversarial testing** | Can the model be manipulated? |
| **Fairness testing** | Does performance differ across demographics? |
| **Robustness testing** | How does the model handle noisy/unusual inputs? |
| **Regression testing** | Did a model update break previously correct behavior? |

## Related

- [[AI Ethics and Safety]]
- [[AI Regulation]]
- [[AI Safety and Alignment Research]]
- [[Hallucination and Grounding]]
- [[Prompt Injection]]
- [[Approval Flows and Sandboxing]]
- [[AI Benchmarks]]
