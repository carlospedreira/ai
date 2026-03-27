# AI Ethics and Safety

As [[Artificial Intelligence]] becomes more capable and widespread, ensuring it is developed and deployed responsibly is one of the most important challenges in the field.

## Key Concerns

### Bias and Fairness
- AI systems learn biases present in training data
- Can perpetuate or amplify discrimination in hiring, lending, criminal justice
- Fairness is multi-dimensional — optimizing for one definition can violate another

### Hallucination and Reliability
- [[Large Language Models]] can generate plausible but false information
- High-stakes domains (medicine, law, finance) require high reliability
- Users may over-trust AI outputs

### Privacy
- Models trained on personal data may memorize and leak it
- Facial recognition raises surveillance concerns
- Data collection practices for training can be invasive

### Safety and Alignment
The **alignment problem**: ensuring AI systems pursue goals that are beneficial to humans.

- **Reward hacking** — Models find unintended shortcuts to maximize reward
- **Goal misalignment** — The specified objective doesn't match the intended one
- **Power-seeking behavior** — Theoretical concern about advanced AI systems acquiring resources
- **Deception** — Models potentially learning to appear aligned without being so

### Deepfakes and Misinformation
- [[Generative AI]] enables creation of realistic fake media
- Erosion of trust in authentic content
- Potential for manipulation at scale

### Economic Impact
- Automation of knowledge work
- Disruption of creative industries
- Concentration of power in organizations with the most compute

## Approaches to AI Safety

### Technical
- **RLHF** — [[Reinforcement Learning|Reinforcement Learning from Human Feedback]] to align model behavior
- **Constitutional AI** — Training models with explicit principles
- **Red teaming** — Adversarial testing to find failure modes
- **Interpretability** — Understanding what models learn and why they produce certain outputs
- **Guardrails** — Runtime filters on inputs and outputs

### Governance
- **Regulation** — EU AI Act, executive orders, sector-specific rules
- **Standards** — ISO, NIST frameworks for AI risk management
- **Responsible disclosure** — Sharing safety research while managing dual-use risks
- **Open vs. closed models** — Debate about whether open-sourcing powerful models helps or hurts safety

### Organizational
- Ethics review boards
- Impact assessments before deployment
- Diverse teams to catch blind spots
- Transparent reporting of capabilities and limitations

## Principles

Most AI ethics frameworks converge on:
1. **Beneficence** — AI should benefit humanity
2. **Non-maleficence** — AI should not cause harm
3. **Autonomy** — Humans should remain in control
4. **Justice** — Benefits and risks should be distributed fairly
5. **Transparency** — AI systems should be understandable and auditable

## Related

- [[Artificial Intelligence]]
- [[Large Language Models]]
- [[Generative AI]]
- [[Reinforcement Learning]]
