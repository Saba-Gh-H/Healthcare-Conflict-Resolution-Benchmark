
# Healthcare Conflict Resolution Benchmark (HCRB)

## Overview
This repository contains the **Healthcare Conflict Resolution Benchmark (HCRB)** and the experimental pipeline used to evaluate **large language models (LLMs)** and **human participants** on healthcare conflict decision‑making scenarios.

The benchmark evaluates how models and humans choose between different **conflict‑resolution strategies** when faced with realistic healthcare scenarios involving:

- clinical decision constraints
- hierarchical roles in healthcare teams
- interpersonal disagreement
- ethical or procedural tensions

The repository includes:

- benchmark generation scripts
- validated benchmark scenarios
- LLM evaluation pipelines
- human evaluation results
- aggregated experiment outputs

Each major component of the repository contains its own README with detailed documentation.

---

# Associated Paper

## Title
**Evaluating Conflict‑Resolution Decision‑Making in Large Language Models Using Healthcare Scenarios**

## Authors
*(Author list will be finalized upon publication)*

Example placeholder:

- First Author  
- Second Author  
- Third Author  
- ...

---

## Abstract

Healthcare environments frequently involve situations where professionals must resolve disagreements under time pressure, institutional constraints, and hierarchical dynamics. Understanding how artificial intelligence systems navigate such conflicts is important as large language models (LLMs) increasingly assist with clinical communication and decision support.

We introduce the **Healthcare Conflict Resolution Benchmark (HCRB)**, a dataset of structured healthcare conflict scenarios designed to evaluate how LLMs choose between alternative conflict‑management strategies. Each scenario presents a healthcare role facing a disagreement with another party and requires selecting one of five possible response strategies: **conform, assert, compromise, collaborate, or avoid**.

The benchmark enables comparison across two experimental settings: a **stateless (non‑informed) setting**, where models evaluate scenarios independently, and a **sequential (informed) setting**, where models can reference previous decisions within a conversation context. In addition to model evaluations, we collect **human participant responses** to establish a baseline for comparison.

Our experiments analyze how different LLMs respond to conflict scenarios, how decision patterns vary across models and decoding conditions, and how model behavior compares to human choices. The benchmark provides a structured framework for studying **social reasoning, role‑aware decision making, and conflict management behavior in AI systems**.

---

# Repository Structure

benchmark_generation/  
Scenario generation and validation pipeline

Non_informed_Test/  
Stateless LLM evaluation (no memory between scenarios)

Informed_LLM/  
Sequential LLM evaluation with conversation history

user_evaluation_results/  
Human participant responses

results/  
Aggregated experiment outputs

docs/  
Additional documentation

Each folder contains its own README explaining the experimental procedures and file structure.

---

# Benchmark Dataset

The **Healthcare Conflict Resolution Benchmark** consists of synthetic healthcare scenarios designed to evaluate conflict‑resolution decision making.

Each scenario includes:

- role_who_decides
- power_level
- question (40‑word scenario description)
- option_A
- option_B
- option_C
- option_D
- option_E

Each option corresponds to a specific **conflict management strategy**.

---

# Conflict Resolution Strategies

| Option | Strategy |
|------|------|
| A | Conform / Accommodate |
| B | Assert / Compete |
| C | Compromise |
| D | Collaborate |
| E | Avoid / Withdraw |

Participants and models select the option they consider the most appropriate response.

---

# Experimental Conditions

## Non‑Informed Setting

Each scenario is presented independently to the model.

Characteristics:

- single‑turn prompt
- no memory of previous questions
- stateless evaluation

Documentation:

Non_informed_Test/README

---

## Informed Setting

The model answers scenarios sequentially with conversation history.

Characteristics:

- previous scenarios remain in context
- previous model answers are visible
- models may maintain consistency across decisions

Documentation:

Informed_LLM/README

---

# Human Evaluation

Human participants completed the same benchmark scenarios to provide a **human baseline** for comparison with model behavior.

Documentation:

user_evaluation_results/README

---

# Benchmark Generation Pipeline

Scenarios were created using a multi‑stage pipeline:

1. Synthetic scenario generation  
2. LLM‑based validation and repair  
3. Language and encoding normalization  

This process ensures:

- realistic healthcare scenarios
- consistent option structure
- correct mapping to conflict strategies

---

# Citation

If you use this benchmark or dataset, please cite the associated paper.

Citation information will be updated once the paper is officially published.

The final citation and the **paper PDF will be added to this repository upon publication**.

---

# License

This repository is released under the **MIT License**.

See the `LICENSE` file for full license details.

---

# Contributing

We welcome contributions to improve the benchmark, documentation, and analysis tools.

See:

CONTRIBUTING.md

for contribution guidelines.

---

# Notes

- Scenario data is **synthetic** and intended for research evaluation purposes.
- The benchmark simplifies complex conflict dynamics into structured response options.
- Real clinical decision‑making may involve additional contextual factors.
