# Healthcare Conflict Resolution Benchmark (HCRB)

**Benchmark for evaluating conflict-resolution decision making in Large Language Models and humans**

---

## Overview

This repository contains the **Healthcare Conflict Resolution Benchmark (HCRB)** and the experimental pipeline used to evaluate how **Large Language Models (LLMs)** and human respondents choose between conflict-resolution strategies in healthcare scenarios.

The benchmark focuses on healthcare conflicts involving:

- clinical decision constraints;
- hierarchical roles in healthcare teams;
- interpersonal disagreement;
- ethical, organizational, or procedural tensions.

The repository includes:

- benchmark generation and validation scripts;
- the validated synthetic scenario set;
- non-informed and informed LLM evaluation pipelines;
- human evaluation outputs;
- aggregated experiment results.

Each major folder contains its own README with more detailed documentation.

---

## Associated paper

**How Do LLMs Handle Conflict in Healthcare? Hierarchy, Collaboration Defaults, and Autonomy**

Authors: Saba Ghanbari Haez, Claudio Giuliano, Renan Lirio De Souza, and Mauro Dragoni.

Citation information will be updated after publication.

---

## Benchmark dataset

HCRB contains **150 synthetic healthcare conflict scenarios**. Each scenario presents a focal actor facing a healthcare-related disagreement and asks which response that actor should take.

Required scenario fields include:

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

The focal role is the **advice recipient and decision-maker** in the vignette, not a persona assigned to the LLM. Other actors mentioned in the scenario form the conflict context.

All scenarios are synthetic and are **not** based on real patient records or clinician-provided cases.

---

## Correct hierarchy mapping

The benchmark is balanced by hierarchy. The correct mapping is:

```text
Q1--Q75    = higher-power focal actor
Q76--Q150  = lower-power focal actor
```

Examples of higher-power focal actors include attending physicians, supervisors, or institutions. Examples of lower-power focal actors include patients, junior staff, nurses, caregivers, or family members.

If regenerating hierarchy analyses, use this mapping. Raw scenario text and raw model choices are unchanged; only hierarchy labels and derived hierarchy summaries depend on this mapping.

---

## Conflict-resolution strategies

Each scenario has five response options, mapped to Thomas--Kilmann-style conflict modes:

| Option | Strategy |
|---|---|
| A | Conform / Accommodate |
| B | Assert / Compete |
| C | Compromise |
| D | Collaborate |
| E | Avoid / Defer |

Models and human respondents select the option they consider most appropriate for the focal actor.

---

## Experimental conditions

### 1. Non-informed setting

Each scenario is presented independently to the model.

Characteristics:

- single-turn prompt;
- no memory of previous questions;
- no access to previous model answers;
- stateless evaluation.

See:

```text
Non_informed_Test/README.md
```

### 2. Informed setting

The model answers scenarios sequentially with conversation history.

Characteristics:

- previous scenarios and the model's previous answers are included in context;
- the model may maintain consistency across decisions;
- outputs can depend on the history window and prompt construction.

See:

```text
Informed_LLM/README.md
```

---

## Human evaluation

Human responses are used as an exploratory baseline. The study used responses from **9 members of a digital-health research unit** who were familiar with healthcare contexts but were not necessarily clinicians or domain experts. The sample is not intended to represent patients, clinicians, or all conflict stakeholders.

See:

```text
user_evaluation_results/README.md
```

---

## Benchmark generation pipeline

Scenarios were created through a multi-stage pipeline:

1. synthetic scenario and option generation;
2. validation and repair;
3. language and encoding normalization;
4. manual checking for clinical plausibility, role consistency, neutral wording, and conflict-style purity.

GPT-5.2 was used to generate initial scenario templates and options. GPT-5.2-Pro was used as an LLM-as-judge validation step to flag unclear or inconsistent items; it did not assign preferred answers.

See:

```text
benchmark_generation/README.md
```

---

## Repository structure

```text
Healthcare-Conflict-Resolution-Benchmark/
│
├── benchmark_generation/      # Scenario generation and validation pipeline
├── Non_informed_Test/         # Stateless LLM evaluation, no history
├── Informed_LLM/              # Sequential LLM evaluation with history
├── user_evaluation_results/   # Human evaluation responses and summaries
├── results/                   # Aggregated experiment outputs
├── docs/                      # Additional documentation
├── CITATION.bib
├── CITATION.cff
├── LICENSE
└── README.md
```

---

## Reproducibility notes

- Use the corrected hierarchy mapping above for all hierarchy analyses.
- Keep outputs from different benchmark versions separate if `main.csv` or scenario text changes.
- The exact prompt templates and response-parsing scripts are included in the relevant evaluation folders.
- Temperature conditions and repeated-run settings are documented in the non-informed README.
- Informed runs depend on conversation history; if resume is used, the conversation should be reconstructed before continuing.

---

## Security checklist

- Store `OPENROUTER_API_KEY` in an environment variable.
- Do not hard-code API keys in notebooks or scripts.
- Rotate any key that was committed, shared, or shown in screenshots.
- Do not commit raw secrets or local environment files.

---

## License

This repository is released under the **MIT License**. See:

```text
LICENSE
```

---

## Citation

If you use HCRB, please cite the associated paper and this repository. The final citation will be updated after publication.

```bibtex
@misc{hcrb2026,
  title = {Healthcare Conflict Resolution Benchmark (HCRB)},
  author = {Ghanbari Haez, Saba and Giuliano, Claudio and De Souza, Renan Lirio and Dragoni, Mauro},
  year = {2026},
  note = {GitHub repository}
}
```

---

## Contact

For questions, please open a GitHub issue or contact the authors.
