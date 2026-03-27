# AI and Security

AI intersects with cybersecurity in two directions: AI systems *as targets* (attacks against AI) and AI *as a tool* for security work. For [[AI Coding Harnesses]], both dimensions are critical — agents that write code must write *secure* code, and agents with tool access are themselves attack surfaces.

## AI Systems as Targets

### [[Prompt Injection]]
The #1 vulnerability. Malicious instructions embedded in data hijack the model's behavior. See the dedicated note for full coverage.

### Model Extraction
Attackers query an API model repeatedly to reconstruct a functional copy:
- Systematically probe the model's behavior
- Train a "stolen" model that mimics the original
- Circumvents licensing, usage limits, and safety guardrails
- Defense: rate limiting, output perturbation, watermarking

### Data Poisoning
Inject malicious data into the training pipeline:
- Modify open-source training datasets
- Submit poisoned code to public repositories the model trains on
- Plant "sleeper" behaviors that activate under specific conditions
- Defense: data curation, anomaly detection, training data auditing (see [[Data Preprocessing for AI]])

### Adversarial Examples
Inputs crafted to cause specific model failures:
- Imperceptible image modifications that change classification
- Text that causes models to produce specific outputs
- Relevant for [[Computer Vision]] and content safety classifiers
- Defense: adversarial training, input sanitization

### Membership Inference
Determine whether specific data was in the training set:
- Privacy concern: "Was my medical record used for training?"
- Legal concern: "Was my copyrighted work used?" (see [[AI and Copyright]])
- Defense: differential privacy, [[Federated Learning]]

### Model Inversion
Extract training data from model outputs:
- LLMs can sometimes reproduce training text verbatim
- Extracting PII, API keys, or proprietary code from model memory
- Defense: deduplication, memorization detection, output filtering

## AI as a Security Tool

### AI-Powered Code Scanning

[[AI Coding Harnesses]] can find security vulnerabilities during development:

```
Developer: "Scan src/ for security vulnerabilities"

Agent:
  1. Reads all source files
  2. Identifies: SQL injection, XSS, hardcoded secrets, insecure defaults,
     missing input validation, SSRF, path traversal
  3. For each finding: severity, location, explanation, fix
  4. Implements fixes if requested
  5. Verifies fixes don't break functionality
```

### Codex Security
[[OpenAI Codex]] launched a specialized **Codex Security** agent (March 2026):
- Generates project-specific threat models
- Scans for vulnerabilities with context awareness
- Pressure-tests findings in sandboxed environments (reduces false positives)
- Scanned 1.2M+ commits, 84% noise reduction vs traditional scanners

### AI vs Traditional SAST/DAST

| Aspect | Traditional Scanners | AI-Powered Analysis |
|---|---|---|
| Pattern matching | Regex/AST rules | Semantic understanding |
| False positive rate | High (30–60%) | Lower (contextual filtering) |
| Business logic flaws | Cannot detect | Can reason about intent |
| Novel vulnerabilities | Only known patterns | Can identify new patterns |
| Configuration | Rule sets, tuning | Natural language description |
| Explanation quality | Error code + docs link | "This is vulnerable because..." |

### Penetration Testing
AI agents can assist in authorized security testing:
- Reconnaissance: enumerate attack surfaces from codebase
- Exploit identification: match discovered patterns to known CVEs
- Report generation: document findings with severity and remediation

### Incident Response
AI helps analyze and respond to security incidents:
- Log analysis: process massive log volumes to identify anomalies
- Root cause analysis: trace an attack chain across systems
- Remediation: suggest and implement fixes

## Secure AI-Generated Code

AI-generated code can introduce vulnerabilities:

| Vulnerability | How AI Introduces It | Mitigation |
|---|---|---|
| **SQL injection** | Generates string concatenation instead of parameterized queries | CLAUDE.md rule: "Always use parameterized queries" |
| **XSS** | Doesn't sanitize user input in templates | Lint rules, template auto-escaping |
| **Hardcoded secrets** | Generates example API keys that look real | Secret scanning in CI |
| **Insecure dependencies** | Suggests outdated packages with known CVEs | Dependency scanning (Dependabot, Snyk) |
| **SSRF** | Generates URL fetch without validation | Input validation rules |
| **Path traversal** | Doesn't sanitize file paths | Sandbox restrictions ([[Approval Flows and Sandboxing]]) |

### Defense-in-Depth for AI-Generated Code

```
Layer 1: CLAUDE.md / AGENTS.md security rules
Layer 2: AI generates code
Layer 3: Linter catches common patterns (ESLint security rules)
Layer 4: AI Code Review (see [[AI Code Review]])
Layer 5: SAST scanner in CI
Layer 6: Human security review for sensitive changes
Layer 7: Runtime protection (WAF, RASP)
```

## Securing [[AI Coding Harnesses]]

### Agent Permissions
- **Principle of least privilege** — Only grant tools the agent needs
- [[Claude Code]]: Deny bash commands that access secrets (`Read(.env)` → deny)
- [[OpenAI Codex]]: Default sandbox disables network → prevents exfiltration

### Secret Management
- Never put secrets in CLAUDE.md / AGENTS.md
- Use environment variables, not hardcoded values
- [[Claude Code]] hooks can strip secrets from tool results
- CI/CD: use `--bare` mode to avoid loading local secrets

### Supply Chain
- AI agents install packages — verify they're legitimate
- Lock file changes should be reviewed carefully
- Typosquatting: AI might install `reqeusts` instead of `requests`

### Audit Trail
- Log all agent actions (see [[AI Observability]])
- Track which code was AI-generated ([[AI and Version Control|co-author tags]])
- Review AI-generated dependency changes with extra scrutiny

## Related

- [[Prompt Injection]]
- [[AI Ethics and Safety]]
- [[AI Safety and Alignment Research]]
- [[Approval Flows and Sandboxing]]
- [[AI Code Review]]
- [[AI Coding Harnesses]]
- [[OpenAI Codex]]
- [[Responsible AI Deployment]]
- [[AI and Copyright]]
