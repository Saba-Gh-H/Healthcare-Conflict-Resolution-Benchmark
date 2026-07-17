<div align="center">

# 🏥 How Do LLMs Handle Conflict in Healthcare?
## **Hierarchy, Collaboration Defaults, and Autonomy**

**Saba Ghanbari Haez · Claudio Giuliano · Renan Lirio De Souza · Mauro Dragoni**

[![Published Paper](https://img.shields.io/badge/paper-published%20by%20Springer-brightgreen)](https://link.springer.com/chapter/10.1007/978-3-032-30710-1_42)
[![Benchmark](https://img.shields.io/badge/benchmark-150%20vignettes-success)]()
[![Task](https://img.shields.io/badge/task-conflict%20resolution-important)]()
[![Code License](https://img.shields.io/badge/code-AGPL--3.0-blue)](LICENSE)
[![Data License](https://img.shields.io/badge/data-CC%20BY%204.0-green)](DATA_LICENSE)

</div>

---

## 📌 Overview

This repository contains the **Healthcare Conflict Resolution Benchmark (HCRB)**, introduced in the published Springer paper:

> **How Do LLMs Handle Conflict in Healthcare? Hierarchy, Collaboration Defaults, and Autonomy**

📄 **Springer chapter:** [https://link.springer.com/chapter/10.1007/978-3-032-30710-1_42](https://link.springer.com/chapter/10.1007/978-3-032-30710-1_42)  
🔗 **DOI:** [10.1007/978-3-032-30710-1_42](https://doi.org/10.1007/978-3-032-30710-1_42)

The benchmark evaluates how **Large Language Models (LLMs)** respond to healthcare conflict scenarios involving **power differences**, such as:

- 👩‍⚕️ patient vs. clinician
- 🧑‍⚕️ junior vs. senior staff
- 🏥 clinician vs. institutional constraints
- 👨‍👩‍👧 family vs. care team disagreement

It is designed to study whether LLM advice changes with **hierarchy**, whether models over-default to **collaboration**, and how these patterns compare with **human judgments**.

---

## 🧾 Abstract

> This study examines how Large Language Models (LLMs) advise users during healthcare conflicts where power differences matter, such as patient–clinician and junior–senior staff disagreements. We introduce a Thomas–Kilmann-grounded benchmark of **150 short healthcare conflict vignettes**, each paired with five response options representing common conflict styles: **accommodate, assert, compromise, collaborate, and avoid**. We evaluate multiple leading LLM families in both **single-turn (stateless)** and **history-conditioned** settings, and compare their choices with a small human-judgment study. Across models, recommendations concentrate heavily on **collaboration**, with **assertion** a distant second and very little accommodation or avoidance. Models show hierarchy-sensitive shifts in assertiveness while remaining strongly collaboration-dominant overall. Human choices are more varied, suggesting that LLMs may over-default to one supposedly “best” conflict approach. These findings motivate conflict-aware evaluation and calibration to better support autonomy in healthcare advice.

**Keywords:** `Conflict Resolution` · `Large Language Models` · `Healthcare AI` · `Hierarchy` · `Autonomy` · `Bias`

---

## ✨ What this repository includes

<table>
<tr>
<td width="50%">

### 🧪 Benchmark creation
- Scenario-generation notebooks
- Scenario validation and repair
- Role and hierarchy checks
- Conflict-style consistency checks

</td>
<td width="50%">

### 🤖 LLM evaluation
- Non-informed/stateless evaluation
- Informed/history-conditioned evaluation
- Multiple temperature conditions
- Response parsing and aggregation

</td>
</tr>
<tr>
<td width="50%">

### 👥 Human evaluation
- Human response data
- Majority-agreement analysis
- Consistency analysis
- Exploratory human baseline

</td>
<td width="50%">

### 📊 Results and analysis
- Per-model response files
- Hierarchy analysis
- Temperature sensitivity
- Aggregated tables and summaries

</td>
</tr>
</table>

---

## 🧱 Benchmark structure

The benchmark contains **150 synthetic healthcare conflict vignettes**.

Each vignette includes:

```text
number
role_who_decides
question
option_A
option_B
option_C
option_D
option_E
```

### 🏛️ Hierarchy split

```text
Q1–Q75    → higher-power focal actor
Q76–Q150  → lower-power focal actor
```

The **focal role** is the advice recipient and decision-maker in the vignette. Other actors mentioned in the scenario provide the surrounding conflict context.

> ⚠️ All scenarios are **synthetic** and are **not based on real patient records or clinician-provided cases**.

---

## ⚖️ Conflict-style options

The benchmark is grounded in the **Thomas–Kilmann conflict framework**.

| Option | Style | Meaning |
|---|---|---|
| **A** | Conform / Accommodate | Go along with the other party or existing rule |
| **B** | Assert / Compete | Stand firmly on one’s preferred action |
| **C** | Compromise | Meet in the middle or split the difference |
| **D** | Collaborate | Seek a joint solution that addresses both sides |
| **E** | Avoid / Defer | Postpone, withdraw, or sidestep the issue |

Participants and models select the option they consider most appropriate for the focal actor.

---

## 🧪 Experimental settings

### 1️⃣ Non-informed setting

Each scenario is evaluated independently.

```text
✓ one fresh prompt per scenario
✓ no access to previous scenarios
✓ no access to previous model answers
✓ no conversation history
```

### 2️⃣ Informed setting

Each model answers scenarios sequentially.

```text
✓ previous scenarios are included in context
✓ the model’s own previous answers are included
✓ the model may maintain consistency across decisions
✓ the core prompt template remains otherwise unchanged
```

Full prompt templates and response-parsing scripts are included in the repository.

---

## 🌡️ Temperature settings

The non-informed evaluation includes multiple temperature conditions:

| Temperature | Purpose |
|---|---|
| **T = 0.0** | Deterministic baseline |
| **T = 0.8** | Moderate-randomness robustness check |
| **T = 1.5** | High-randomness robustness check |

Higher-temperature runs are repeated to summarize **mean counts** and **min–max ranges**.

---

## 👥 Human evaluation

We collected responses from **9 members of a digital-health research unit** who were familiar with healthcare contexts, though they were **not necessarily clinicians or domain experts**.

This human study is used as an **exploratory baseline** rather than a representative sample of all stakeholders.

### 📋 Design

```text
18 anchor vignettes          → answered by all participants
10 additional questions     → assigned per participant
2 repeated anchor items     → used to assess consistency
```

---

## 🌳 Repository structure

```text
Healthcare-Conflict-Resolution-Benchmark/
│
├── Scenario_generation/
│   ├── Scenario-Generation.ipynb
│   ├── LLMjudge.ipynb
│   ├── Nscenarios.csv
│   ├── judged.csv
│   ├── judged_fixed.csv
│   └── README.txt
│
├── Non_informed_LLM/
│   ├── all-responses.ipynb
│   └── README_Non_informed_Test_updated.md
│
├── Informed_LLM/
│   ├── all-informed-llm-resonses.ipynb
│   ├── main.csv
│   └── README_Informed_LLM_updated.md
│
├── Human_Evaluation_Results/
│   ├── Human majority agreement.ipynb
│   └── consistency.ipynb
│
├── CITATION.cff
├── CITATION.bib
├── LICENSE
├── DATA_LICENSE
├── .gitignore
└── README.md
```

---

## 🚀 Scenario generation and validation

The benchmark was created through a synthetic generation and validation pipeline:

```text
1. Generate healthcare conflict vignettes and A–E options
2. Check conflict logic, role consistency, wording, and style purity
3. Repair unclear or inconsistent scenarios
4. Manually review the final scenarios
```

- **GPT-5.2** was used for initial scenario generation.
- **GPT-5.2-Pro** was used as an LLM-as-judge step to flag unclear or inconsistent items.
- The judge model was **not used to assign preferred answers**.

---

## 📈 Outputs

This repository includes:

```text
✓ per-model response files
✓ merged LLM response files
✓ informed and non-informed summaries
✓ hierarchy analyses
✓ temperature-sensitivity analyses
✓ human-evaluation summaries
```

---

## 🔐 Reproducibility and security

LLM inference uses **OpenRouter**.

Store your API key as an environment variable:

```bash
OPENROUTER_API_KEY
```

> 🔒 Do not hard-code API keys into notebooks or scripts before committing changes.

---

## 📚 Citations

Please cite **both the published paper and the repository** when using HCRB, its data, code, or results.

---

### 📘 1. Published paper citation

> Haez, S.G., Giuliano, C., De Souza, R.L., Dragoni, M. (2027).  
> **How Do LLMs Handle Conflict in Healthcare? Hierarchy, Collaboration Defaults, and Autonomy.**  
> In: Andreev, P., Van Woensel, W., Holmes, J., Sauré, A. (eds), *Artificial Intelligence in Medicine. AIME 2026*.  
> Lecture Notes in Computer Science, vol. 16748. Springer, Cham.  
> [https://doi.org/10.1007/978-3-032-30710-1_42](https://doi.org/10.1007/978-3-032-30710-1_42)

#### 🧾 BibTeX — paper

```bibtex
@incollection{haez2027llms,
  author    = {Haez, Saba Ghanbari and Giuliano, Claudio and De Souza, Renan Lirio and Dragoni, Mauro},
  title     = {How Do LLMs Handle Conflict in Healthcare? Hierarchy, Collaboration Defaults, and Autonomy},
  booktitle = {Artificial Intelligence in Medicine},
  editor    = {Andreev, P. and Van Woensel, W. and Holmes, J. and Sauré, A.},
  series    = {Lecture Notes in Computer Science},
  volume    = {16748},
  publisher = {Springer},
  address   = {Cham},
  year      = {2027},
  doi       = {10.1007/978-3-032-30710-1_42},
  url       = {https://doi.org/10.1007/978-3-032-30710-1_42},
  note      = {Presented at AIME 2026; published online 08 July 2026}
}
```

---

### 🗂️ 2. Repository citation

> Ghanbari Haez, S., Giuliano, C., De Souza, R.L., Dragoni, M. (2026).  
> **Healthcare Conflict Resolution Benchmark (HCRB).**  
> GitHub repository.  
> [https://github.com/Saba-Gh-H/Healthcare-Conflict-Resolution-Benchmark](https://github.com/Saba-Gh-H/Healthcare-Conflict-Resolution-Benchmark)

#### 🧾 BibTeX — repository

```bibtex
@misc{hcrb2026,
  title        = {Healthcare Conflict Resolution Benchmark (HCRB)},
  author       = {Ghanbari Haez, Saba and Giuliano, Claudio and De Souza, Renan Lirio and Dragoni, Mauro},
  year         = {2026},
  howpublished = {GitHub repository},
  url          = {https://github.com/Saba-Gh-H/Healthcare-Conflict-Resolution-Benchmark},
  note         = {Benchmark accompanying the paper: How Do LLMs Handle Conflict in Healthcare? Hierarchy, Collaboration Defaults, and Autonomy}
}
```

---

### 🔗 Publication details

| Item | Details |
|---|---|
| 📄 **Springer chapter** | [View the published paper](https://link.springer.com/chapter/10.1007/978-3-032-30710-1_42) |
| 🔗 **DOI** | [10.1007/978-3-032-30710-1_42](https://doi.org/10.1007/978-3-032-30710-1_42) |
| 📅 **Published** | 08 July 2026 |
| 🏢 **Publisher** | Springer, Cham |
| 📚 **Series** | Lecture Notes in Computer Science, vol. 16748 |
| 📕 **Print ISBN** | 978-3-032-30709-5 |
| 💻 **Online ISBN** | 978-3-032-30710-1 |

Citation metadata is also available in:

```text
CITATION.cff
CITATION.bib
```

---

## 📄 Licensing

This repository contains **two categories of material**, each with its own licence.

<table>
<tr>
<td width="50%">

### 💻 Code and notebooks

Licensed under:

```text
GNU Affero General Public License v3.0
AGPL-3.0-only
```

✅ Modifications and redistributed versions must remain under the same licence.  
✅ Covered source code must remain available under the licence terms.  
✅ Modified versions offered as network services are subject to AGPL source-sharing requirements.

See [`LICENSE`](LICENSE).

</td>
<td width="50%">

### 📊 Benchmark and data

Copyright (c) 2026 Saba Ghanbari Haez

Licensed under:

```text
Creative Commons Attribution 4.0
CC BY 4.0
```

✅ Reuse and adaptation are allowed.  
✅ Appropriate attribution is required.  
✅ Changes must be indicated.  
✅ The licence must be linked.

See [`DATA_LICENSE`](DATA_LICENSE).

</td>
</tr>
</table>

> ⚠️ This repository is **not licensed under MIT**.  
> The software licence is **AGPL-3.0-only**, and the benchmark/data licence is **CC BY 4.0**.

---

## 💬 Contact

For questions, suggestions, or collaboration:

- 📧 **FBK email:** [sghanbarihaez@fbk.eu](mailto:sghanbarihaez@fbk.eu)
- 📧 **Personal email:** [ghanbari.haez.saba@gmail.com](mailto:ghanbari.haez.saba@gmail.com)
- 💬 **GitHub:** Open an issue in this repository

<div align="center">

### ⭐ If you find this repository useful, consider starring it!

</div>
