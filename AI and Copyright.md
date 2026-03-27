# AI and Copyright

The intersection of [[Artificial Intelligence]] and copyright law is one of the most contested legal frontiers of the 2020s. It touches every aspect of AI: the data used for [[Training and Inference|training]], the content models generate, and the tools developers use.

## Three Core Questions

### 1. Is Training on Copyrighted Data Legal?

AI models are trained on vast corpora that include copyrighted works — books, articles, code, images, music. Is this:
- **Fair use** (transformative, doesn't substitute for the original)?
- **Copyright infringement** (unauthorized copying)?

**Arguments for fair use:**
- Models learn patterns, not memorize specific works
- Training is transformative — the output is entirely new
- No single work is reproduced in the output
- Similar to a human learning from reading books

**Arguments against:**
- Entire works are copied into training datasets
- Models can sometimes reproduce near-verbatim excerpts
- Commercial benefit from copyrighted material without compensation
- Scale: billions of works used without permission

**Key lawsuits (ongoing as of 2026):**
- NYT v. OpenAI — New York Times suing over article reproduction
- Getty v. Stability AI — Photo agency suing over image training data
- Authors Guild v. OpenAI — Book authors challenging training practices
- Multiple class actions from artists, musicians, and programmers

### 2. Who Owns AI-Generated Content?

If an AI generates an image, poem, or piece of code — who holds the copyright?

| Position | Argument |
|---|---|
| The user (prompter) | They directed the creation, chose the output |
| The AI company | They built and trained the model |
| Nobody (public domain) | Copyright requires human authorship |
| The training data creators | The output derives from their work |

**Current legal status (US):**
- The US Copyright Office has ruled that purely AI-generated content is **not copyrightable**
- Works with "sufficient human authorship" (selection, arrangement, modification) may qualify
- The threshold for "sufficient" human involvement is being litigated

### 3. Can AI-Generated Code Infringe?

Particularly relevant for [[AI Coding Harnesses]]:
- Models trained on open-source code may reproduce GPL-licensed code
- Generated code could match existing copyrighted implementations
- GitHub Copilot class action lawsuit alleges violation of open-source licenses

## The Open Source Code Question

### The GitHub Copilot Case
GitHub Copilot was trained on public GitHub repositories, including code under restrictive licenses (GPL, AGPL). The lawsuit alleges:
- Copilot reproduces copyrighted code without attribution
- GPL requires derivative works to be open-sourced — does AI-generated code that resembles GPL code inherit the license?
- Copilot strips license headers and attribution

### License Implications for Developers

| Scenario | Risk Level |
|---|---|
| AI generates generic boilerplate | Very low |
| AI generates common algorithm implementation | Low |
| AI reproduces a distinctive, copyrighted implementation | High |
| AI generates code resembling GPL-licensed code | Uncertain |

**Practical advice:**
- Review AI-generated code for unusual similarity to known projects
- Some tools (Copilot) offer filters to block suggestions matching public code
- Consider indemnification policies (GitHub, Anthropic offer some protection)

## AI Company Positions

| Company | Training Data Position | Output Ownership | Indemnification |
|---|---|---|---|
| **OpenAI** | Fair use | User owns outputs | Yes (for business users) |
| **Anthropic** | Fair use | User owns outputs | Yes (for API customers) |
| **Meta** | Open training data | Open model outputs | N/A (open source) |
| **Stability AI** | Opt-out available | User owns outputs | Limited |
| **GitHub** | Fair use | User owns outputs | Yes (Copilot Business) |

## The "Opt-Out" Model

Some companies now offer data owners the ability to opt out of training:
- `robots.txt` and `ai.txt` for web crawling
- DeviantArt, ArtStation added opt-out for artists
- Stability AI offers opt-out from future training
- But: retroactive opt-out (removing already-learned knowledge) is technically impossible

## Implications for Developers

### Using AI Coding Tools
1. **Review generated code** — Don't blindly ship AI output
2. **Check for license compliance** — Especially if using open-source-trained models
3. **Use indemnified products** — Enterprise plans often include legal protection
4. **Document AI usage** — Track which code was AI-generated
5. **Don't train on private data without rights** — Fine-tuning on proprietary code you don't own is risky

### Building AI Applications
1. **Training data audit** — Know what's in your training data
2. **Content filtering** — Prevent verbatim reproduction of copyrighted material
3. **Attribution systems** — Track provenance where possible
4. **Terms of service** — Clear ownership terms for generated content

## Regulatory Landscape

| Jurisdiction | Approach |
|---|---|
| **EU AI Act** | Requires transparency about training data; copyright obligations |
| **US** | Litigation-driven; no comprehensive AI copyright law yet |
| **UK** | Proposed text and data mining exception for AI training |
| **Japan** | Broad exception for AI training (most permissive) |
| **China** | Requires consent for training on personal data |

## Related

- [[AI Ethics and Safety]]
- [[Large Language Models]]
- [[Generative AI]]
- [[AI Coding Harnesses]]
- [[Open Source AI Models]]
- [[Diffusion Models]]
