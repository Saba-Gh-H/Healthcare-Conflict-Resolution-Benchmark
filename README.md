# Evaluating Conflict-Resolution and Decision-Making in Large Language Models Using Healthcare Scenarios 
  Healthcare Conflict Resolution Benchmark (HCRB)

**Benchmark for evaluating conflict-resolution decision making in Large
Language Models and humans**

------------------------------------------------------------------------

## 📌 Overview

This repository contains the **Healthcare Conflict Resolution Benchmark
(HCRB)** and the experimental pipeline used to evaluate **Large Language
Models (LLMs)** and **human participants** on healthcare conflict
decision-making scenarios.

The benchmark evaluates how models and humans choose between different
**conflict-resolution strategies** when faced with realistic healthcare
scenarios involving:

-   clinical decision constraints\
-   hierarchical roles in healthcare teams\
-   interpersonal disagreement\
-   ethical or procedural tensions

The repository includes:

-   benchmark generation scripts\
-   validated benchmark scenarios\
-   LLM evaluation pipelines\
-   human evaluation results\
-   aggregated experiment outputs

Each major component of the repository contains its own README with
detailed documentation.

------------------------------------------------------------------------

# 📄 Associated Paper

## Title

**Evaluating Conflict-Resolution Decision-Making in Large Language Models Using Healthcare Scenarios**

------------------------------------------------------------------------

## Authors


-   Saba Ghanbari Haez
-   Claudio Giuliano
-   Renan Lirio de Suoza
-   Mauro Dragoni

------------------------------------------------------------------------

## 🧠 Abstract

Healthcare environments frequently involve situations where
professionals must resolve disagreements under time pressure,
institutional constraints, and hierarchical dynamics. Understanding how
artificial intelligence systems navigate such conflicts is important as
large language models increasingly assist with clinical communication
and decision support.

We introduce the **Healthcare Conflict Resolution Benchmark (HCRB)**, a
dataset of structured healthcare conflict scenarios designed to evaluate
how LLMs choose between alternative conflict-management strategies.

Each scenario presents a healthcare role facing a disagreement with
another party and requires selecting one of five possible response
strategies:

**conform, assert, compromise, collaborate, or avoid**

The benchmark enables comparison across two experimental settings:

-   **Stateless (non-informed)** evaluation\
-   **Sequential (informed)** evaluation

In addition to model evaluations, we collect **human participant
responses** to establish a baseline for comparison.

The benchmark provides a structured framework for studying:

-   social reasoning\
-   role-aware decision making\
-   conflict management behavior in AI systems.

------------------------------------------------------------------------

# 📂 Repository Structure

    Healthcare-Conflict-Resolution-Benchmark/
    │
    ├── benchmark_generation/      # Scenario generation and validation pipeline
    │
    ├── Non_informed_Test/         # Stateless LLM evaluation (no memory)
    │
    ├── Informed_LLM/              # Sequential LLM evaluation with history
    │
    ├── user_evaluation_results/   # Human participant responses
    │
    ├── results/                   # Aggregated experiment outputs
    │
    ├── docs/                      # Additional documentation
    │
    └── README.md                  # Project overview

Each folder contains its own README explaining the experimental
procedures and file structure.

------------------------------------------------------------------------

# 🧩 Benchmark Dataset

The **Healthcare Conflict Resolution Benchmark** consists of synthetic
healthcare scenarios designed to evaluate **conflict-resolution decision
making**.

Each scenario includes structured fields:

    role_who_decides
    power_level
    question
    option_A
    option_B
    option_C
    option_D
    option_E

Each option corresponds to a specific **conflict management strategy**.

------------------------------------------------------------------------

# ⚖️ Conflict Resolution Strategies


  Option   Strategy
  -------- -----------------------
  **A**    Conform / Accommodate
  
  **B**    Assert / Compete
  
  **C**    Compromise
  
  **D**    Collaborate
  
  **E**    Avoid / Withdraw

Participants and models select the option they consider the most
appropriate response.

------------------------------------------------------------------------

# 🧪 Experimental Conditions

## 1️⃣ Non-Informed Setting

Each scenario is presented **independently** to the model.

Characteristics:

-   single-turn prompt\
-   no memory of previous questions\
-   stateless evaluation

Documentation:

    Non_informed_Test/README

------------------------------------------------------------------------

## 2️⃣ Informed Setting

The model answers scenarios **sequentially with conversation history**.

Characteristics:

-   previous scenarios remain in context\
-   previous model answers are visible\
-   models may maintain consistency across decisions

Documentation:

    Informed_LLM/README

------------------------------------------------------------------------

# 👥 Human Evaluation

Human participants completed the same benchmark scenarios to provide a
**human baseline** for comparison with model behavior.

Documentation:

    user_evaluation_results/README

------------------------------------------------------------------------

# ⚙️ Benchmark Generation Pipeline

Scenarios were created using a **multi-stage pipeline**:

    1. Synthetic scenario generation
    2. LLM-based validation and repair
    3. Language and encoding normalization

This process ensures:

-   realistic healthcare scenarios\
-   consistent option structure\
-   correct mapping to conflict strategies

------------------------------------------------------------------------

# 📚 Citation

If you use the **Healthcare Conflict Resolution Benchmark (HCRB)** in
your research, please cite this repository and the associated paper.

Citation will be updated upon publication.

Example placeholder:

``` bibtex
@misc{hcrb2026,
  title={Healthcare Conflict Resolution Benchmark (HCRB)},
  author={Ghanbari Haez et al.},
  year={2026},
  note={GitHub repository}
}
```

The final citation and paper PDF will be added once the paper is
published.

------------------------------------------------------------------------

# 📜 License

This repository is released under the **MIT License**.

See:

    LICENSE

for full license details.

------------------------------------------------------------------------

# 🤝 Contributing

We welcome contributions that improve:

-   the benchmark\
-   documentation\
-   evaluation pipelines

See:

    CONTRIBUTING.md

for contribution guidelines.

------------------------------------------------------------------------

# ⚠️ Notes

-   Scenario data is **synthetic** and intended for research evaluation
    purposes.\
-   The benchmark simplifies complex conflict dynamics into structured
    response options.\
-   Real clinical decision-making may involve additional contextual
    factors.

------------------------------------------------------------------------

## ✉️ Contact

For questions regarding the benchmark or experiments, please open a
**GitHub issue** or contact sghanbarihaez@fbk.eu or ghanbari.haez.saba@gmail.com
