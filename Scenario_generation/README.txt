README
======

Repository contents for the Healthcare Conflict Resolution Benchmark.

Overview
--------
This repository contains the synthetic healthcare conflict benchmark used to study how large language models choose between conflict-resolution styles in healthcare scenarios. Each scenario gives a focal actor, a short healthcare conflict vignette, and five response options mapped to the Thomas--Kilmann conflict modes:

A = conform / accommodate
B = assert / compete
C = compromise
D = collaborate
E = avoid / defer

Correct hierarchy labels
------------------------
Use the following hierarchy mapping throughout all analysis scripts, tables, and documentation:

Questions 1--75   = higher-power focal actor
Questions 76--150 = lower-power focal actor

This is a label/documentation convention for analysis. Do not reorder the dataset rows or change the raw model choices. If older scripts or tables say that Q1--75 are lower-power and Q76--150 are higher-power, update those labels.

Files
-----
Nscenarios.csv
    Final scenario file used as input to model evaluation.
    Rows: 150
    Columns:
        number
        role_who_decides
        question
        option_A
        option_B
        option_C
        option_D
        option_E

judged.csv
    Output of the LLM-as-judge validation/repair pipeline.
    Rows: 150
    Includes the scenario text plus quality-control fields such as:
        conflict_valid
        realism_score
        question_word_count
        question_length_ok
        option_one_sentence_ok
        inferred_A ... inferred_E
        mapping_all_ok
        style_purity_score
        surface_balance_score
        neutral_tone_score
        issues
        short_rationale
        repaired
        repair_attempts
        repair_notes

judged_fixed.csv
    Corrected/final judged scenario file. In the uploaded copy, judged.csv and judged_fixed.csv are identical: True.
    To avoid confusion, use judged_fixed.csv as the canonical judged file, or keep only one of the two files in the public repository.

Scenario-Generation.ipynb
    Notebook/script used to generate the initial synthetic healthcare conflict scenarios with GPT-5.2 via OpenRouter.
    It defines conflict types, focal roles, the generation prompt, and the OpenRouter call.

LLMjudge.ipynb
    Notebook/script for the judge + repair pipeline.
    It uses GPT-5.2-Pro as an LLM-as-judge to check conflict validity, realism, option-style mapping, surface balance, neutral tone, and other constraints. If a scenario fails checks, the pipeline can repair the question/options and re-judge them.

Suggested workflow
------------------
1. Generate or inspect initial scenarios:
       Scenario-Generation.ipynb

2. Save the generated or manually finalized scenario set as:
       Nscenarios.csv

3. Run the judge + repair pipeline:
       LLMjudge.ipynb

4. Use the final validated output:
       judged_fixed.csv

5. In all downstream analysis, apply the corrected hierarchy mapping:
       number <= 75  -> Higher hierarchy
       number >= 76  -> Lower hierarchy

Quality-control summary for judged.csv
--------------------------------------
conflict_valid = True: 150/150
mapping_all_ok = True: 150/150
question_length_ok = True: 150/150
repaired = True: 16/150

Reproducibility notes
---------------------
- The benchmark scenarios and code are synthetic and are not based on real patient records or clinician-provided cases.
- GPT-5.2 was used for initial scenario generation.
- GPT-5.2-Pro was used for validation/judging, not to assign preferred answers.
- The A--E options are style labels, not correctness labels. The task is to analyze which style a model selects, not to score one option as clinically correct.

Security note
-------------
Before publishing the repository, remove any hard-coded API keys from notebooks or scripts. Use environment variables instead, for example:

    export OPENROUTER_API_KEY="your_key_here"

and in Python:

    import os
    OPENROUTER_API_KEY = os.environ["OPENROUTER_API_KEY"]

If an API key has already been committed or shared, rotate it immediately in OpenRouter.

Recommended citation/description
--------------------------------
This repository releases a synthetic benchmark of 150 healthcare conflict vignettes with five Thomas--Kilmann-inspired response options per vignette. The benchmark is intended for evaluating whether LLM advice in healthcare conflict scenarios favors accommodation, assertion, compromise, collaboration, or avoidance, and how these choices vary by hierarchy and prompting protocol.
