# AI for Science

AI is transforming scientific discovery across disciplines — from protein structure prediction to materials design to climate modeling. While this vault focuses primarily on AI coding tools and foundational concepts, AI's impact on science represents some of the most consequential applications of [[Deep Learning]] and [[Large Language Models]].

## Landmark Achievements

### AlphaFold — Protein Structure Prediction

DeepMind's AlphaFold (2020–2024) solved a 50-year grand challenge in biology:
- Predicts the 3D structure of proteins from their amino acid sequence
- AlphaFold 2 achieved accuracy comparable to experimental methods (GDT > 90)
- AlphaFold DB: predicted structures for 200M+ proteins (nearly every known protein)
- AlphaFold 3 (2024) extends to protein-ligand, protein-DNA, and protein-RNA complexes
- Won the 2024 Nobel Prize in Chemistry (Demis Hassabis, John Jumper)

**Architecture:** Evoformer (custom [[Attention Mechanism|attention]] module) + structure module. Processes both the sequence and multiple sequence alignments (MSAs) to predict inter-residue distances and angles.

### AlphaGo / AlphaZero — Game Playing

- **AlphaGo** (2016) — Defeated world Go champion Lee Sedol
- **AlphaZero** (2017) — Learned chess, Go, and shogi from self-play alone, surpassing all previous engines
- Uses Monte Carlo Tree Search + [[Deep Learning|deep neural networks]] + [[Reinforcement Learning]]
- Demonstrated that AI could discover novel strategies humans hadn't considered in thousands of years of play

### Weather Prediction

- **GraphCast** (DeepMind, 2023) — ML weather model that outperforms traditional numerical weather prediction for 10-day forecasts
- **Pangu-Weather** (Huawei) — Achieves comparable accuracy to ECMWF operational forecasts
- 1000x faster than physics-based models
- Uses graph [[Neural Networks]] on atmospheric data

## Key Domains

### Drug Discovery

| Application | How AI Helps |
|---|---|
| Target identification | Analyze genomic data to find drug targets |
| Molecular generation | Design novel drug candidates with desired properties |
| Binding affinity prediction | Predict how well a drug binds to its target |
| ADMET prediction | Forecast absorption, distribution, metabolism, excretion, toxicity |
| Clinical trial optimization | Predict patient response, optimize trial design |

**Key tools:** AlphaFold (structure), DiffDock ([[Diffusion Models]] for molecular docking), RFdiffusion (protein design).

### Materials Science

- Discover new materials with desired properties (conductivity, strength, etc.)
- GNoME (DeepMind, 2023) — Discovered 2.2 million new crystal structures
- Battery material optimization
- Catalyst design for clean energy

### Mathematics

- **AlphaProof** (DeepMind, 2024) — Solved International Mathematical Olympiad problems at silver medal level
- **AlphaGeometry** — Solved olympiad-level geometry problems
- LLMs assist in conjecture generation and proof search
- FunSearch (DeepMind) — Used LLMs to discover new mathematical constructions

### Climate and Earth Science

- Climate model downscaling (high-resolution predictions from coarse models)
- Wildfire prediction and monitoring
- Ocean current modeling
- Carbon cycle simulation

### Genomics

- Variant calling and genome assembly
- Gene expression prediction
- Protein function annotation
- Single-cell analysis

## AI-for-Science Models

A new class of foundation models trained specifically for scientific domains:

| Model | Domain | By |
|---|---|---|
| AlphaFold 3 | Molecular biology | DeepMind |
| ESM-3 | Protein language model | Meta |
| Geneformer | Single-cell genomics | Hugging Face |
| Open Catalyst | Catalysis/materials | Meta |
| Evo | DNA/RNA/protein | Arc Institute |

These models apply the same [[Transfer Learning]] paradigm as LLMs: pre-train on large scientific datasets, then fine-tune or prompt for specific tasks.

## Scientific LLM Applications

[[Large Language Models]] assist scientists in ways beyond domain-specific models:

- **Literature review** — Summarize and synthesize thousands of papers
- **Hypothesis generation** — Propose experiments based on existing findings
- **Code generation** — Write analysis scripts, simulation code, data pipelines
- **Writing assistance** — Draft papers, grant proposals, documentation
- **Data interpretation** — Explain statistical results, visualize findings

## Challenges

### Reproducibility
- AI models can be black boxes — hard to understand *why* a prediction was made
- Reproducibility requires publishing model weights, data, and code
- Scientific claims need interpretable evidence, not just model confidence

### Data Quality
- Scientific data is often noisy, sparse, or biased
- Small datasets in specialized domains limit [[Deep Learning]]
- Experimental errors can be amplified by models

### Hallucination
See [[Hallucination and Grounding]]. In science, confident but wrong predictions can waste years of wet-lab work. Validating AI predictions experimentally remains essential.

### Dual Use
- AI for drug discovery could also design toxins
- Protein design tools could create pathogens
- These dual-use risks are central to [[AI Safety and Alignment Research]]

## Related

- [[Deep Learning]]
- [[Reinforcement Learning]]
- [[Large Language Models]]
- [[Diffusion Models]]
- [[Attention Mechanism]]
- [[Transfer Learning]]
- [[AI Ethics and Safety]]
