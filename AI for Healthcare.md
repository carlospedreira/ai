# AI for Healthcare

Healthcare is one of the highest-impact application domains for [[Artificial Intelligence]]. AI is transforming diagnosis, drug discovery, clinical workflows, and patient care — while also raising unique ethical, regulatory, and safety challenges.

## Key Applications

### Medical Imaging

AI matches or exceeds radiologist performance on specific imaging tasks:

| Application | AI Approach | Status |
|---|---|---|
| **Chest X-ray** | [[Convolutional Neural Networks\|CNN]]/Transformer classifiers | FDA-approved devices in clinical use |
| **Mammography** | Detection + triage models | Reduces missed cancers by 20%+ in studies |
| **Retinal scans** | Diabetic retinopathy screening | Deployed in primary care (no specialist needed) |
| **CT/MRI** | 3D segmentation models | Assists tumor detection, organ measurement |
| **Pathology** | Whole slide image analysis | Cancer grading, biomarker detection |
| **Dermatology** | Skin lesion classification | Consumer apps + clinical decision support |

### Drug Discovery

See [[AI for Science#Drug Discovery]] for the technical details:
- **Target identification** — Genomic analysis to find drug targets
- **Molecular design** — [[Generative AI]] creates novel drug candidates
- **AlphaFold** — Predicts protein structures, accelerating target understanding
- **Clinical trial optimization** — AI predicts patient response, optimizes enrollment
- **Timeline impact** — AI can compress early discovery from 4–5 years to 1–2 years

### Clinical Decision Support

[[Large Language Models]] assisting clinicians:
- **Differential diagnosis** — Given symptoms and history, suggest likely conditions
- **Treatment recommendations** — Evidence-based suggestions considering patient specifics
- **Lab result interpretation** — Flag abnormal results, suggest follow-up
- **Clinical note generation** — Transcribe and summarize doctor-patient conversations
- **Prior authorization** — Automate insurance documentation

### Administrative AI

Much of healthcare cost is administrative:
- **Medical coding** — Assign ICD/CPT codes from clinical notes
- **Revenue cycle** — Automate billing, claims processing
- **Scheduling** — Optimize appointments, reduce no-shows
- **Documentation** — Generate referral letters, discharge summaries

### Patient-Facing AI

- **Symptom checkers** — Triage before doctor visit
- **Mental health** — AI-assisted therapy (Woebot, companion apps)
- **Chronic disease management** — Personalized coaching
- **Drug interaction checking** — LLM-powered medication review

## Notable Healthcare AI Systems

| System | Developer | What It Does |
|---|---|---|
| **Med-PaLM 2** | Google | Medical Q&A, expert-level on licensing exams |
| **AlphaFold** | DeepMind | Protein structure prediction (Nobel Prize 2024) |
| **PathAI** | PathAI | Pathology image analysis |
| **Paige** | Paige AI | FDA-approved cancer detection in pathology |
| **Ambient Clinical Intelligence** | Nuance/Microsoft | Auto-generates clinical notes from conversations |
| **BioGPT** | Microsoft | Biomedical text mining and generation |

## The LLM Impact

General-purpose [[Large Language Models]] are increasingly used in healthcare:

### What LLMs Do Well
- **Medical exam performance** — GPT-4 passes USMLE (US Medical Licensing Exam) at expert level
- **Literature synthesis** — Summarize hundreds of research papers
- **Patient communication** — Draft empathetic, accessible explanations
- **Documentation** — Generate structured clinical notes from free-text

### What LLMs Do Poorly
- **[[Hallucination and Grounding|Hallucination]]** — Fabricating medical facts is potentially lethal
- **Rare conditions** — Training data biases toward common conditions
- **Up-to-date guidelines** — Knowledge cutoff means outdated treatment recommendations
- **Individual patient context** — LLMs lack access to the patient's full history

### Mitigation
- **[[Retrieval-Augmented Generation|RAG]]** over medical databases (UpToDate, PubMed, clinical guidelines)
- **Human-in-the-loop** — AI assists, clinician decides
- **Specialized fine-tuning** on medical corpora
- **Citation requirements** — Force the model to cite evidence

## Regulatory Landscape

Healthcare AI faces the strictest regulatory environment:

| Jurisdiction | Framework |
|---|---|
| **US (FDA)** | Software as a Medical Device (SaMD) — requires 510(k) or De Novo approval for clinical use |
| **EU** | Medical Device Regulation (MDR) + [[AI Regulation\|AI Act]] high-risk classification |
| **UK** | MHRA software and AI regulation |

### FDA AI/ML-Based SaMD
- 950+ AI/ML-enabled medical devices approved (as of 2025)
- Majority in radiology and cardiology
- "Predetermined change control plan" allows models to update without re-approval
- Locked vs adaptive algorithms: locked (frozen weights) is easier to approve

## Ethical Challenges

### Bias
Medical AI can perpetuate or amplify healthcare disparities:
- **Training data bias** — Models trained on data from major academic centers underperform on underserved populations
- **Skin tone bias** — Dermatology AI historically less accurate on darker skin
- **Gender/age bias** — Cardiovascular AI may miss atypical presentations in women
- **Socioeconomic bias** — Algorithms trained on insurance data may disadvantage the uninsured

### Privacy
- **HIPAA** (US) strictly regulates health data
- **[[Federated Learning]]** enables training without centralizing patient data
- De-identification is imperfect — LLMs may re-identify patients from context
- Cloud AI APIs raise data sovereignty concerns

### Liability
When AI contributes to a medical error:
- Is the developer liable? The hospital? The clinician who followed the recommendation?
- Current legal frameworks hold clinicians responsible (AI is a "tool")
- This may evolve as AI autonomy increases

### Over-Reliance
Automation bias — clinicians may trust AI over their own judgment:
- "The AI said it's not cancer" → clinician skips closer inspection
- Particularly dangerous for rare conditions where AI underperforms
- Training and interface design must encourage critical evaluation

## AI Coding in Healthcare

[[AI Coding Harnesses]] are used to develop healthcare software:
- **HIPAA compliance** — CLAUDE.md can enforce PHI (Protected Health Information) handling rules
- **Regulatory documentation** — AI generates compliance documentation
- **Test generation** — [[AI-Powered Testing]] for medical device software
- **Code review** — [[AI Code Review]] with security focus for patient safety

But extra caution is needed:
- AI-generated code in medical devices faces regulatory scrutiny
- [[AI and Security|Security]] is paramount when handling patient data
- [[Responsible AI Deployment]] practices are legally required, not optional

## Related

- [[AI for Science]]
- [[Artificial Intelligence]]
- [[AI Ethics and Safety]]
- [[AI Regulation]]
- [[Federated Learning]]
- [[Retrieval-Augmented Generation]]
- [[Hallucination and Grounding]]
- [[Responsible AI Deployment]]
- [[Computer Vision]]
