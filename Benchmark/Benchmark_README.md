# Healthcare Conflict Resolution Benchmark (HCRB)

## Overview

The **Healthcare Conflict Resolution Benchmark (HCRB)** is a synthetic evaluation dataset designed to measure how large language models (LLMs) reason about **interpersonal conflict in healthcare decision-making contexts**.

Each item is a short healthcare vignette followed by five response options (A–E). Each option is intended to represent a distinct conflict-handling style.

### Conflict style mapping

| Option | Style | Meaning |
|---|---|---|
| A | Conform (accommodate) | Go along to reduce tension |
| B | Assert (compete) | Insist on your preferred plan |
| C | Compromise | Seek a middle-ground plan |
| D | Collaborate | Jointly co-create a shared plan |
| E | Avoid | Defer/withdraw from the decision for now |

---

## Dataset format

The generated CSVs contain (at minimum) these scenario columns:

- `number`
- `role_who_decides`
- `power_level` (high/low; present in the initial generator output)
- `question` (EXACTLY **40 words**)
- `option_A` … `option_E` (each intended to be **one sentence**)

After judging/repair, additional judge metadata columns are appended (see below).

---

## Generation pipeline

The benchmark is produced in **three stages**:

1. **Offline scenario generation (no API)**
2. **LLM-as-judge validation + repair (OpenRouter)**
3. **Encoding / language cleanup (spell/character abnormalities)**

### Stage 1 — Offline scenario generation

Script: `generate_scenarios.py`

This script procedurally constructs scenarios by sampling:

- a *decider role* (high-power or low-power)
- a *counterpart role*
- a *clinical setting*
- a *domain hint* (e.g., diagnosis, staffing, policy constraint)
- a *conflict trigger* (e.g., challenged in front of others)

#### Key constraints enforced

- Generates exactly **N** scenarios (default: 150)
- Splits scenarios approximately 50/50 into:
  - **high-power** deciders (first half)
  - **low-power** deciders (second half; receives the extra item if N is odd)
- The `question` text is forced to be **exactly 40 words**
- Options A–E are fixed templates mapped to the five conflict styles above
- Uniqueness check prevents near-duplicate items

#### Run

```bash
python generate_scenarios.py --num 150 --seed 42 --out Nscenarios.csv
```

Output: `Nscenarios.csv`

---

### Stage 2 — LLM-as-judge + repair (OpenRouter)

Script: `judge_and_repair.py`

This stage uses OpenRouter with an OpenAI GPT-5.x model as an **LLM judge** (and optionally as a **repair editor**) to validate each scenario.

Model used (example): `openai/gpt-5.2-pro`

#### What the judge checks

- **Conflict validity**: two interdependent parties with incompatible goals/constraints and a decision point
- **Realism**: plausible healthcare setting, roles, and tension
- **Question length**: question word count is exactly **40**
- **Option design**:
  - Style purity (each option reflects only one style)
  - Surface balance (similar length/specificity)
  - Neutral tone (does not imply a “correct” answer)
- **Style inference**: classifies each option into one of:
  - `conform`, `assert`, `compromise`, `collaborate`, `avoid`

#### Automatic repair loop

If a scenario fails checks, the script requests a rewrite of the **question and options** to meet all constraints, then re-judges. This is repeated up to `--max-repair-attempts` times (default: 2).

#### Run

```bash
python judge_and_repair.py --in Nscenarios.csv --out judged.csv
```

Output: `judged.csv` (contains updated scenario text + judge metadata such as `conflict_valid`, `realism_score`, `mapping_all_ok`, `issues`, etc.)

> **Security note:** Do **not** hard-code or commit API keys. If a key was shared publicly, rotate it immediately.

---

### Stage 3 — Encoding / abnormal character cleanup

Script: `fix_encoding.py`

This stage corrects common encoding artifacts (mojibake) that can appear in CSV exports, such as:

- `â€™` → `’`
- `â€œ` → `“`
- `â€` → `”`
- `â€“` → `–`

It also supports optional ASCII punctuation normalization (smart quotes → straight quotes).

#### Run

```bash
python fix_encoding.py
```

Output: `judged_fixed.csv` (final cleaned dataset)

---

## Outputs

Typical outputs produced by the pipeline:

- `Nscenarios.csv` — raw generated scenarios
- `judged.csv` — judged + repaired scenarios (includes judge metadata)
- `judged_fixed.csv` — final cleaned CSV with encoding/language abnormalities corrected

---

## Intended use

HCRB is intended to evaluate LLM behavior on:

- conflict handling in healthcare-like settings
- sensitivity to power dynamics (high vs low power deciders)
- consistent mapping between response options and conflict styles
- neutral and balanced decision option framing

---

## Limitations

- Scenarios are synthetic, though designed for realism
- Conflict styles are simplified into five discrete strategies
- Real clinical decision-making may require additional context beyond a 40-word vignette

---

## License

Add your license here (e.g., MIT / CC-BY 4.0 / custom terms).
