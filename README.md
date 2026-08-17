# PromptLibrary

System prompts for cash reconciliation, SAP variance analysis, and enterprise AI idea evaluation.

![Type](https://img.shields.io/badge/type-prompts-blueviolet?style=flat-square)
![Last Commit](https://img.shields.io/github/last-commit/vinaygangidi/PromptLibrary?style=flat-square)
![License](https://img.shields.io/badge/license-MIT-blue?style=flat-square)

## What This Does

Three production system prompts used across finance and strategy projects. Each is a
complete role definition — persona, task, reasoning framework, output schema, and explicit
constraints — written to be pasted into a model's system slot rather than assembled at
runtime.

There is no code here. These are plain text files, model-agnostic.

## How It Works

Each prompt follows the same structure: a `ROLE` establishing expertise, a `CONTEXT`
declaring what data the model will receive, a `TASK`, a numbered `REASONING FRAMEWORK`, and
constraints on what the model must not do.

| File | Lines | Purpose |
|---|---|---|
| `CashReconciliationPrompt` | 104 | Parse raw bank statement text and ERP ledger JSON, then match, classify, allocate, and summarize transactions. Defines an explicit `parsed_bank_transactions` output schema with `date`, `description`, `amount`, `currency`, `transaction_id` |
| `VarianceAnalysisPrompt` | 41 | Senior Finance Business Partner / SAP FI/CO expert. Analyzes G/L postings, vendor and customer master data, and contract frameworks to explain variances and identify trends |
| `Idea Evaluator prompt` | 24 | Evaluates enterprise AI project ideas against business value, feasibility, data readiness, ownership, and regulatory constraints. Enforces one question per turn and withholds any verdict until enough context is gathered |

A shared design choice runs through all three: they forbid fabrication.
`VarianceAnalysisPrompt` requires the model to state when information is not present rather
than estimate or interpolate. `Idea Evaluator prompt` requires assumptions to be labeled as
assumptions.

## Quickstart

1. Clone the repository:
   ```bash
   git clone https://github.com/vinaygangidi/PromptLibrary.git
   cd PromptLibrary
   ```

2. Open the prompt you need and paste its full contents into your model's system prompt
   field — the Anthropic or OpenAI console, an API `system` parameter, or a Claude Project's
   custom instructions.

3. Supply the data the prompt expects in the user turn:
   - `CashReconciliationPrompt` — raw bank statement text plus ERP ledger JSON
   - `VarianceAnalysisPrompt` — uploaded SAP FI/CO documents or G/L extracts
   - `Idea Evaluator prompt` — a short description of the AI idea, then answer its questions

## Configuration

No environment variables, dependencies, or configuration files. Each prompt is
self-contained plain text.

| Prompt | Expected input | Output shape |
|---|---|---|
| `CashReconciliationPrompt` | Bank statement text + ledger JSON | Structured JSON, schema defined in the prompt |
| `VarianceAnalysisPrompt` | SAP FI/CO documents | Narrative analysis with document numbers, posting dates, and USD values cited |
| `Idea Evaluator prompt` | Idea description, then interactive answers | Go / delay / reject recommendation after discovery |

## Limitations

- **Untested against a fixed benchmark.** These prompts have not been evaluated on a
  labeled dataset. There is no accuracy figure and no regression suite, so behavior can
  drift when the underlying model changes.
- **No model or version pinned.** The prompts do not target a specific model. Output quality
  and instruction adherence vary meaningfully between models and between versions of the
  same model.
- **`CashReconciliationPrompt` asks the model to do arithmetic.** It requests allocation and
  matching directly, with no deterministic verification step. For production reconciliation
  the math should be computed in code and the model used only for ambiguous judgment.
- **No few-shot examples.** All three are zero-shot instructions. Adding worked examples
  would likely improve consistency on edge cases.
- **File names lack extensions.** The three prompt files have no `.md` or `.txt` suffix, so
  they do not render or syntax-highlight on GitHub, and one contains a space in its name.
- **No versioning.** Prompts are overwritten in place, so there is no record of what changed
  between revisions or why.
- **Sending real financial documents to a model is a data-egress decision.** These prompts
  are written to consume G/L postings, vendor master data, and contract references. Confirm
  your agreements before pasting real data into a hosted model.

## License

MIT — see [LICENSE](LICENSE).
