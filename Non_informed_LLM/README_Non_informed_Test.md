# Non_informed_Test — LLM Response Collection (Stateless / No History)

This folder contains **non-informed (stateless)** model responses for the Healthcare Conflict Resolution Benchmark (HCRB).

“Non-informed” means **each scenario is sent to the LLM as a single, independent prompt**:
- no conversation history
- no access to other scenarios
- no access to any “previous answers”
- no retrieval or tools beyond the single request

---

## Files in this folder

- **Per-model per-run outputs**: `* responses.csv`  
  (one CSV per model and run configuration)
- **Merged output**: `all_llm_responses.csv`  
  (concatenation of all per-model CSVs)

> You mentioned the final merged file will be placed in this folder as `all_llm_responses.csv`.

---

## Runner used to generate per-model CSVs

Runner name: **`LLM_Test`**

Purpose: run **one OpenRouter call per scenario row**, log the model’s forced-choice decision (A–E), and save results to a CSV named from the model/run.

> Your implementation uses a shell-based workflow (bash/PowerShell) to execute the runs. The details below are intentionally **runner/tool-agnostic**.

---

## Input

Default input CSV:

- `main.csv`

Required columns:

- `number`
- `role_who_decides`
- `question`
- `option_A`
- `option_B`
- `option_C`
- `option_D`
- `option_E`

---

## Prompting protocol (single-turn)

For each row, `LLM_Test` sends a **fresh, stateless prompt**:

- **System instruction**: choose exactly one option A–E and output strict JSON
- **User content**: ROLE + SCENARIO + OPTIONS A–E for that single row

Expected output format (strict JSON):

```json
{"choice":"A|B|C|D|E","rationale":"..."}
```

If strict JSON parsing fails, the pipeline may fall back to extracting a letter A–E from the raw output (rationale may be missing in that case).

---

## OpenRouter configuration

Endpoint:

- `https://openrouter.ai/api/v1/chat/completions`

Common run parameters:
- `MODEL = "..."` (change per run)
- `TEMPERATURE = ...` (see below)
- `MAX_TOKENS = 250`

### API key handling

Store your OpenRouter key in an environment variable:

- `OPENROUTER_API_KEY`

**macOS / Linux (bash)**
```bash
export OPENROUTER_API_KEY="sk-or-..."
```

**Windows PowerShell**
```powershell
setx OPENROUTER_API_KEY "sk-or-..."
# then reopen the terminal
```

Do **not** hard-code keys into scripts or commit them to Git.

---

## Temperature conditions and repeated runs (non-informed)

In addition to the default greedy setting (T = 0), non-informed evaluation is commonly repeated at higher temperatures to test robustness under stochastic decoding.

Recommended temperatures:
- **T = 0.0** (greedy / near-deterministic)
- **T = 0.8** (medium stochasticity)
- **T = 1.5** (high stochasticity)

Recommended repeated runs:
- **3 runs** at **T = 0.8**
- **5 runs** at **T = 1.5**

### Practical file naming (recommended)

Save each model/temperature/run to a distinct filename, e.g.:

- `gpt52_T0.8_run1 responses.csv`
- `gpt52_T0.8_run2 responses.csv`
- `gpt52_T0.8_run3 responses.csv`

This makes later merging and analysis easier.

---

## Resume support

If resume is enabled in your runner, it should:
- detect completed `number` values in an existing output CSV
- skip completed rows
- append new rows incrementally

This helps recover from rate limits or interrupted runs.

---

## Output schema (per-model/per-run CSV)

Each per-run CSV typically includes:

- `number`
- `role_who_decides`
- `question`
- `option_A` … `option_E`
- `model`
- `temperature`
- `choice` (A–E)
- `rationale`
- `raw_response`
- `status` (`ok` or `error`)
- `error`

---

## Merging into `all_llm_responses.csv`

After producing multiple `* responses.csv` files, merge them into:

- `all_llm_responses.csv`

Tool-agnostic merge rule:
- **concatenate rows** from all per-run CSVs
- keep `model` and `temperature`
- add `run_id` (recommended) if you have repeated runs at the same temperature/model

If you prefer a shell workflow, you can also merge with CLI tools (choose one approach):

**Option A (simple, assumes identical headers):**
- keep one header row
- append the rest of rows from each file

**Option B (analysis merge):**
- use Python/R/Excel to concatenate while validating columns

---

## Notes / known limitations

- Even with `temperature=0.0`, outputs can occasionally vary across providers/infrastructure.
- Some models may output non-JSON; fallback extraction may lose rationale.
- If you change `main.csv`, treat it as a new benchmark version and keep outputs separate.

---

## Security checklist

- ✅ Store `OPENROUTER_API_KEY` in environment variables (not in code)
- ✅ Rotate keys immediately if shared publicly
