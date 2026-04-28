<div align="center">

# 🏥 How Do LLMs Handle Conflict in Healthcare?
## **Hierarchy, Collaboration Defaults, and Autonomy**

**Saba Ghanbari Haez · Claudio Giuliano · Renan Lirio De Souza · Mauro Dragoni**

[![Paper Status](https://img.shields.io/badge/status-AIME%202026%20submission-blue)]()
[![Benchmark](https://img.shields.io/badge/benchmark-150%20vignettes-success)]()
[![Task](https://img.shields.io/badge/task-conflict%20resolution-important)]()
[![License](https://img.shields.io/badge/license-MIT-lightgrey)]()

</div>

---

## 📌 Overview

This repository contains the **Healthcare Conflict Resolution Benchmark (HCRB)**, introduced in the paper:

> **How Do LLMs Handle Conflict in Healthcare? Hierarchy, Collaboration Defaults, and Autonomy**

The benchmark evaluates how Large Language Models (LLMs) respond to **healthcare conflict scenarios** involving **power differences**, such as:

- patient vs. clinician
- junior vs. senior staff
- clinician vs. institutional constraints
- family vs. care team disagreement

It is designed to study whether LLM advice changes with **hierarchy**, whether models over-default to **collaboration**, and how these patterns compare with **human judgments**.

---

## 🧾 Abstract

> This study examines how Large Language Models (LLMs) advise users during healthcare conflicts where power differences matter (e.g., patient vs. clinician, junior vs. senior staff). We introduce a Thomas–Kilmann-grounded benchmark of **150 short healthcare conflict vignettes**, each paired with five response options representing common conflict styles (**accommodate, assert, compromise, collaborate, avoid**). We evaluate multiple leading LLM families in both **single-turn (stateless)** and **history-conditioned** settings, and compare their choices with a small human-judgment study. Across models, recommendations concentrate heavily on **collaboration**, with **assertion** a distant second and very little accommodation or avoidance. Models show **hierarchy-sensitive shifts in assertiveness**, while remaining strongly collaboration-dominant overall. Human choices are more varied, suggesting LLMs may over-default to one “best” conflict approach. These findings motivate **conflict-aware evaluation and calibration** to better support autonomy in healthcare advice.

**Keywords:** `Conflict Resolution` · `Large Language Models` · `Bias`

---

## ✨ What this repository includes

- 🧪 **Scenario generation pipeline**
- ✅ **Scenario validation and repair notebooks**
- 🤖 **LLM evaluation in non-informed/stateless settings**
- 🧠 **LLM evaluation in informed/history-conditioned settings**
- 👥 **Human evaluation data and summaries**
- 📊 **Aggregated outputs and analysis tables**

---

## 🧱 Benchmark structure

The benchmark contains **150 synthetic healthcare conflict vignettes**.

Each vignette includes:

- `number`
- `role_who_decides`
- `question`
- `option_A`
- `option_B`
- `option_C`
- `option_D`
- `option_E`

### 🏛️ Hierarchy split

- **Q1–Q75** → higher-power focal actor
- **Q76–Q150** → lower-power focal actor

The **focal role** is the advice recipient and decision-maker in the vignette. Other actors mentioned in the scenario provide the surrounding conflict context.

> All scenarios are **synthetic** and are **not based on real patient records or clinician-provided cases**.

---

## ⚖️ Conflict-style options

The benchmark is grounded in the **Thomas–Kilmann** conflict framework.

| Option | Style | Meaning |
|---|---|---|
| **A** | Conform / Accommodate | Go along with the other party or existing rule |
| **B** | Assert / Compete | Stand firmly on one’s own preferred action |
| **C** | Compromise | Meet in the middle / split the difference |
| **D** | Collaborate | Seek a joint solution that addresses both sides |
| **E** | Avoid / Defer | Postpone, withdraw, or sidestep the issue |

Participants and models select the option they consider most appropriate for the focal actor.

---

## 🧪 Experimental settings

### 1) 🤖 Non-informed setting

Each scenario is evaluated independently.

- one fresh prompt per scenario
- no access to previous scenarios
- no access to previous model answers
- no conversation history

### 2) 🧠 Informed setting

Each model answers scenarios sequentially.

- previous scenario prompts are included in context
- the model’s own prior answers are included in context
- the model can maintain consistency across scenarios when appropriate
- the core prompt template is otherwise the same as in the non-informed setting

Full prompt templates and response-parsing scripts are included in the repository.

---

## 🌡️ Temperature settings

The non-informed evaluation includes multiple temperature conditions:

- **T = 0.0** → deterministic baseline
- **T = 0.8** → moderate-randomness robustness check
- **T = 1.5** → high-randomness robustness check

Higher-temperature runs are repeated to summarize **mean counts** and **min–max ranges**.

---

## 👥 Human evaluation

We collected responses from **9 members of a digital-health research unit** who were familiar with healthcare contexts, though they were **not necessarily clinicians or domain experts**.

This human study is used as an **exploratory baseline** rather than a representative sample of all stakeholders.

### Design

- **18 anchor vignettes** answered by all participants
- **10 additional assigned questions** per participant
- **2 repeated anchor items** per participant to assess consistency

---

## 🗂️ Repository structure

```text
Healthcare-Conflict-Resolution-Benchmark/
├── Scenario_generation/      # Scenario creation, judging, repair, validated files
├── Non_informed_LLM/         # Stateless LLM evaluation
├── Informed_LLM/             # History-conditioned LLM evaluation
├── user_evaluation_results/  # Human evaluation files and outputs
├── results/                  # Aggregated results and analysis tables
├── CITATION.cff
├── CITATION.bib
├── LICENSE
└── README.md
```

---

## 🚀 Scenario generation and validation

The benchmark was created through a synthetic generation and validation pipeline:

1. generate initial healthcare conflict vignettes and A–E options
2. check conflict logic, role consistency, neutral wording, and style purity
3. repair unclear or inconsistent items
4. manually review final scenarios for plausibility and consistency

- **GPT-5.2** was used for initial scenario generation
- **GPT-5.2-Pro** was used as an LLM-as-judge step to flag unclear or inconsistent items
- the judge model was **not used to assign preferred answers**

---

## 📈 Outputs

This repository includes:

- per-model response files
- merged LLM response files
- informed and non-informed summaries
- hierarchy analyses
- temperature sensitivity analyses
- human evaluation summaries

---

## 🔐 Reproducibility and security

LLM inference uses **OpenRouter**.

Store your API key as an environment variable:

```bash
OPENROUTER_API_KEY
```

Please avoid hard-coding API keys into notebooks or scripts before committing changes.

---

## 📚 Citation

If you use this benchmark, please cite the associated paper and repository.

```bibtex
@misc{hcrb2026,
  title  = {How Do LLMs Handle Conflict in Healthcare? Hierarchy, Collaboration Defaults, and Autonomy},
  author = {Ghanbari Haez, Saba and Giuliano, Claudio and Lirio De Souza, Renan and Dragoni, Mauro},
  year   = {2026},
  note   = {Healthcare Conflict Resolution Benchmark (HCRB), GitHub repository}
}
```

---

## 📄 License

This repository is released under the **MIT License**. See [`LICENSE`](LICENSE) for details.

---

## 💬 Contact

For questions, suggestions, or collaboration, please open an issue in this repository. Or Contact sghanbarihaez@gmail.com, ghanbari.haez.saba@gmail.com

<div align="center">

### ⭐ If you find this repository useful, consider starring it!

</div>
