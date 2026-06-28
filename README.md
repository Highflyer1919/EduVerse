# EduVerse — Training-Free Adaptive Learning Recommendation System

> A training-free, interpretable recommendation pipeline for adaptive learning personalization in low-resource educational settings.
> 
> **Published at ICSEAIS 2026**

---

## Overview

EduVerse is a modular, multi-agent AI system that delivers personalized learning recommendations **without requiring any labeled training data**. Instead of training a model, it models each learner across four dimensions and uses a confidence-weighted ranking mechanism to recommend the most appropriate next learning action — with a plain-language explanation at every decision step.

This makes EduVerse suitable for real-world educational deployments where data is scarce, interpretability is required, and infrastructure is limited.

---

## System Architecture

The system operates as a sequential five-stage pipeline, orchestrated via **CrewAI**:

```
Learner Interaction Data (Topic · Difficulty · Correctness)
        │
        ▼
┌─────────────────────────────────────┐
│         Diagnostic Agent            │  Stage 1
│  Evaluates performance across       │
│  topics and difficulty levels       │
└──────────────────┬──────────────────┘
                   │
                   ▼
┌─────────────────────────────────────┐
│      Student Modelling Agent        │  Stage 2
│  Proficiency · Consistency          │
│  Recency · Temporal Trend           │
└──────────────────┬──────────────────┘
                   │
                   ▼
┌─────────────────────────────────────┐
│       Weak Topic Analyzer           │  Stage 3
│  Severity computation and           │
│  knowledge gap detection            │
└──────────────────┬──────────────────┘
                   │
                   ▼
┌─────────────────────────────────────┐
│       Recommendation Agent          │  Stage 4
│  Match score · Confidence scoring   │
│  Rank-and-select mechanism          │
└──────────────────┬──────────────────┘
                   │
                   ▼
     Personalized Recommendation
     + Natural-language explanation
```

Each stage outputs a justification in plain language, making the system **inherently interpretable** — no post-hoc explanation methods required.

---

## Learner State Model

The Student Modelling Agent represents each learner as a four-dimensional feature vector:

| Dimension | Description | Formula |
|-----------|-------------|---------|
| **Proficiency** `P(t)` | Topic mastery weighted by difficulty | `Σ wᵈ · Pₜ,ᵈ` |
| **Consistency** `C` | Stability of performance over attempts | `(1/n-1) Σ 1[xᵢ = xᵢ₊₁]` |
| **Recency** `R'` | Blended recent vs. historical performance | `λRₜ + (1-λ)Pₜ` |
| **Temporal Trend** `T` | Whether performance is improving or declining | `P̄_recent − P̄_past` |

---

## Recommendation Actions

The Recommendation Agent selects from seven instructional strategies based on the learner's current state:

| Action | Code | Trigger Condition |
|--------|------|------------------|
| Concept Revision | CR | Low proficiency — foundational gaps |
| Practice Easy | PE | Weak learners needing procedural practice |
| Practice Medium | PM | Mid-range proficiency |
| Practice Hard | PH | High proficiency — ready for challenge |
| Reinforcement | RR | Low consistency — needs repetition |
| Guided Learning | GL | Negative trend — declining performance |
| Advance | AD | High proficiency + improving trend |

---

## Results

Evaluated on the **ASSISTments 2012–2013** dataset: 28,834 students, 198 skills, across 5 independent runs.

| System | Alignment ↑ | Simulated Gain ↑ | Adaptivity ↑ |
|--------|-------------|------------------|--------------|
| **EduVerse** | **0.994 ± 0.0003** | **0.299 ± 0.00003** | **0.684 ± 0.019** |
| Proficiency-only | 0.996 ± 0.0002 | 0.300 ± 0.00002 | 0.685 ± 0.019 |
| BKT | 0.942 ± 0.0092 | 0.229 ± 0.002 | 0.723 ± 0.007 |
| Fixed (PM) | 0.000 ± 0.000 | 0.200 ± 0.000 | 0.479 ± 0.017 |
| Random | 0.285 ± 0.001 | 0.193 ± 0.0001 | 1.000 ± 0.000 |

> *Simulated gain is based on a predefined pedagogical progression model and does not represent measured empirical improvement.*

EduVerse achieves **0.994 alignment accuracy**, outperforming BKT by **5.5%**, while maintaining full interpretability and requiring **zero labeled training data**.

---

## Tech Stack

| Layer | Technology |
|-------|-----------|
| Agent Orchestration | [CrewAI](https://github.com/crewAIInc/crewAI) |
| LLM Integration | [LiteLLM](https://github.com/BerriAI/litellm) |
| Question Generation | Microsoft Phi-3 Medium (via HuggingFace) |
| Interface | Gradio |
| Data Processing | Pandas, NumPy |
| Backend | Python |
| Frontend | HTML, CSS, JavaScript |

---

## Project Structure

```
EduVerse/
├── agents/
│   ├── diagnostic_agent.py          # Stage 1: performance evaluation
│   ├── student_modelling_agent.py   # Stage 2: learner state computation
│   ├── weak_topic_analyzer.py       # Stage 3: severity & gap detection
│   ├── recommendation_agent.py      # Stage 4: match score & ranking
│   └── explainability_agent.py      # Natural-language justification
├── core/
│   ├── learner_state.py             # Proficiency, consistency, recency, trend
│   └── confidence_scoring.py        # Confidence-weighted rank-and-select
├── evaluation/
│   └── baselines.py                 # BKT, LR, Random, Fixed, Proficiency-only
├── interface/
│   └── app.py                       # Gradio UI + AI Calculus Tutor prototype
├── data/
│   └── README.md                    # ASSISTments dataset instructions
├── assets/
│   └── architecture.png             # System architecture diagram
├── requirements.txt
└── README.md
```

---

## Installation

```bash
# Clone the repository
git clone https://github.com/Highflyer1919/EduVerse.git
cd EduVerse

# Create and activate a virtual environment
python -m venv venv
source venv/bin/activate        # On Windows: venv\Scripts\activate

# Install dependencies
pip install -r requirements.txt
```

---

## Usage

```bash
# Launch the Gradio interface
python interface/app.py
```

To run the evaluation pipeline against baselines:

```bash
python evaluation/baselines.py
```

---

## Dataset

This project uses the **ASSISTments 2012–2013** dataset for evaluation.

- Source: [ASSISTments Data Mining Competition](https://sites.google.com/site/assistmentsdata/home/2012-13-school-data-with-affect)
- 28,834 students, 198 skills
- Download and place in the `data/` directory before running evaluation scripts

---

## Publication

If you use EduVerse in your research, please cite:

```bibtex
@inproceedings{mehta2026eduverse,
  title     = {EduVerse: A Training-Free Confidence-Weighted Framework for
               Transparent and Adaptive Learning Personalization},
  author    = {Mehta, Aarav and Taarini, Dhulipala Tripura and M, Suresh},
  booktitle = {Proceedings of ICSEAIS 2026},
  year      = {2026}
}
```

---

## Authors

- **Aarav Mehta** — Department of Computing Technologies, SRMIST
- **Dhulipala Tripura Taarini** — Department of Computing Technologies, SRMIST
- **Dr. Suresh M** *(Supervisor)* — Department of Computing Technologies, SRMIST

---

## License

This project is licensed under the MIT License — see [LICENSE](LICENSE) for details.
