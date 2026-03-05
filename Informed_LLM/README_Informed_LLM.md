# Informed_LLM — LLM Response Collection (Informed / With Conversation History)

This folder contains **informed** model responses for the Healthcare Conflict Resolution Benchmark (HCRB).

“Informed” means the LLM answers scenarios **sequentially in a single conversation**, where **previous scenarios and the model’s previous answers are included in the conversation history**. This differs from the *Non_informed_Test* setting, where each scenario is evaluated independently with no history.

---

## Folder structure / outputs

- **Per-model (and per-run) outputs**: `* informed responses.csv`  
  Example: `gpt41 informed responses.csv`
- **Merged output folder**: `all_informed_llm_responses/`  
  Contains the final merged file(s) that aggregate all informed runs.

> You mentioned: runs are executed from a runner called `LLM_Test`, and final merged outputs are saved under `all_informed_llm_responses`.

---

## Runner used: `LLM_Test` (informed mode)

The informed runner performs one OpenRouter request per scenario **while maintaining a conversation** that includes:

- an initial system instruction (consistency + ability to reference prior answers)
- each scenario prompt as a user message
- each model response as an assistant message

This allows the model to:
- maintain consistency across scenarios when appropriate
- explicitly change course if later context warrants it

> Your implementation uses a shell-based workflow (bash/PowerShell) to execute the runs. The documentation below is **runner/tool-agnostic**.

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

The scenarios should be processed in ascending `number` order so “previous answers” are well-defined.

---

## Prompting protocol (multi-turn informed)

### System prompt (once at the start)
The system message tells the model it is evaluating a **sequence** and may use prior answers for consistency.

### User prompt (per row)
Each row is formatted as:
- ROLE
- SCENARIO
- OPTIONS A–E

### Model output format
The model is instructed to output **STRICT JSON only**:

```json
{"choice":"A|B|C|D|E","rationale":"..."}
```

If parsing fails, the pipeline may fall back to extracting a letter A–E from the raw output (rationale may be missing in that case).

---

## Conversation history window (memory control)

The runner may include a configurable history window, e.g.:

- keep the initial system prompt
- keep the last **N user/assistant pairs** in the rolling context

If history trimming is enabled, document the value you used (e.g., `HISTORY_TURNS = 30`) because it affects the informed condition.

---

## OpenRouter configuration

Endpoint:

- `https://openrouter.ai/api/v1/chat/completions`

Common run parameters:
- `MODEL = "..."` (change per run)
- `TEMPERATURE = ...`
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

## Temperature settings and repeated runs (recommended)

Typical approach:
- **T = 0.0** (greedy / near-deterministic baseline)
- **T = 0.8** (medium stochasticity)
- **T = 1.5** (high stochasticity)

For repeated runs at the same model/temperature:
- run the informed evaluation multiple times
- save each output to a distinct filename, e.g.:
  - `gpt41_T0.8_run1 informed responses.csv`
  - `gpt41_T0.8_run2 informed responses.csv`

---

## Resume support (conversation reconstruction)

If resume is enabled for informed runs, the runner should:
1. load the existing `* informed responses.csv`
2. identify completed scenario numbers
3. **reconstruct the conversation** by replaying prior user prompts and prior model outputs
4. continue from the next unfinished scenario

This is important: informed results depend on the conversation history.

---

## Output schema (per informed run)

Each row typically includes:

- `number`
- `model`
- `temperature`
- `choice` (A–E)
- `rationale`
- `raw_response`
- `status` (`ok` / `error`)
- `error`

(Some implementations also store the scenario text/options; if not, `main.csv` remains the source of scenario content.)

---

## Merging informed runs into `all_informed_llm_responses`

After running multiple models (and/or multiple temperatures/runs), merge all files matching:

- `* informed responses.csv`

into a combined file stored under:

- `all_informed_llm_responses/`

If your merge step errors with:

```text
No '* informed responses.csv' files found.
```

it typically means:
- you ran the merge step in the wrong directory, or
- your files don’t match the naming pattern `* informed responses.csv`, or
- the runs haven’t produced output files yet.

Tool-agnostic merge rule:
- **concatenate rows** from all informed CSVs
- keep `model` and `temperature`
- add `run_id` if you have repeated runs at the same temperature/model

---

## Notes / limitations

- Informed runs are sensitive to context window limits and any history trimming strategy.
- Even with `temperature=0.0`, slight variation can occur depending on provider/infrastructure.
- If you change `main.csv`, treat it as a new benchmark version and keep outputs separate.

---

## Security checklist

- ✅ Store `OPENROUTER_API_KEY` in environment variables (not in code)
- ✅ Rotate keys immediately if shared publicly
