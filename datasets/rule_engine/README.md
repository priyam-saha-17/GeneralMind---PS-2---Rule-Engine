# Project 3 — Semantic business decisions

Interpret business correspondence to make five fixed decisions. This replaces the earlier numerical rule-engine task.

## Files and usage

- `fine_tuning.jsonl`: 200 labeled training examples.
- `validation.jsonl`: 50 labeled validation examples.

JSONL means one JSON object per line: `{"case_id":"...","input":{...},"target":{...}}`. Feed only `input` to the model; `target` is the ground-truth supervision. Keep validation separate from training and use it to compare approaches and select settings. There are no test answers in this shareable folder.

All data is synthetic and requires no vision, PDF processing, external service or live pipeline. These are model-independent records, not a native training upload format. JEV does not offer customer fine-tuning: use training cases to develop typed questions or train a downstream model on JEV outputs. Laya supports its own fine-tuning workflow; adapt these records to it. Select relevant evidence for each question and check the chosen model's context budget rather than silently truncating full records.

References: https://docs.typesafe.ai/models and https://github.com/NandhaKishorM/laya.

## Input and target

Input: `document_context`, `email_thread`, `document_text`, `reference_records`, and `decision_policy`.

Target: `{"rule_outputs":{"R01_AMENDMENT":"APPLY","R02_SUBSTITUTION":"REJECT","R03_RELEASE":"HOLD","R04_DOCUMENT_RELATION":"CORRECTION","R05_GOVERNING_TERMS":"MIXED"}}`.

| Decision | Fixed outputs | Scope |
|---|---|---|
| R01_AMENDMENT | APPLY / DO_NOT_APPLY / REVIEW / NOT_APPLICABLE | Quantity change on line A |
| R02_SUBSTITUTION | ACCEPT / REJECT / REVIEW / NOT_APPLICABLE | Production substitute for line B |
| R03_RELEASE | RELEASE / HOLD / REVIEW / NOT_APPLICABLE | Dispatch of line C |
| R04_DOCUMENT_RELATION | DUPLICATE / CORRECTION / SEPARATE_TRANSACTION / REVIEW | Current versus prior billing obligation |
| R05_GOVERNING_TERMS | ORIGINAL / REVISED / MIXED / REVIEW | Payment, freight and warranty |

All five outputs are required. The supplied decision_policy defines authority, missing evidence and precedence. The scopes are independent: a quantity amendment does not determine commercial terms, and substitution on B does not determine dispatch of C. REVISED means all three commercial terms are replaced; MIXED means only a subset changes. Quoted history is not a current instruction; silence is not approval; a later timestamp alone is not authority.

Training records also contain `training_annotations` with per-decision explanations and evidence references. Keep these out of model inputs. Each training outcome occurs 50 times per decision; validation outcomes occur 12 or 13 times each.

## Approach and evaluation

Ask one Choice question per decision with precisely its allowed options, supplying relevant correspondence and policy. Model outputs should reflect interpretation of intent, authority, scope and conditions. Deterministic preprocessing and learned combinations are allowed.

Report accuracy and four-class macro F1 for each decision; primary aggregate is the mean of the five macro F1 scores. Treat absent-class F1 as zero. Also report exact accuracy of the complete five-answer vector. Do not pool same-named labels across different decisions.

## Difficulty and limits

Authorized changes versus feasibility enquiries; current instructions versus quoted history; sample-only approval versus production approval; cleared funds versus payment advice; conditional release versus missing fulfilment evidence; resend versus corrected bill versus separate shipment; full revisions versus partial counteroffers. Eighty authored constructions recur across splits in varied combinations. This supports a semantic decision exercise but does not prove a model is necessary or establish JEV/Laya performance.
