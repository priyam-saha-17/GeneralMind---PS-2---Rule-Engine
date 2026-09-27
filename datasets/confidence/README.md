# Project 2 — Pipeline confidence

Estimate the probability that the proposed final business document is semantically acceptable for automatic processing.

## Files and usage

- `fine_tuning.jsonl`: 200 labeled training examples.
- `validation.jsonl`: 50 labeled validation examples.

JSONL means one JSON object per line: `{"case_id":"...","input":{...},"target":{...}}`. Feed only `input` to the model; `target` is the ground-truth supervision. Keep validation separate from training and use it to compare approaches and select settings. There are no test answers in this shareable folder.

All data is synthetic and requires no vision, PDF processing, external service or live pipeline. These are model-independent records, not a native training upload format. JEV does not offer customer fine-tuning: use training cases to develop typed questions or train a downstream model on JEV outputs. Laya supports its own fine-tuning workflow; adapt these records to it. Select relevant evidence for each question and check the chosen model's context budget rather than silently truncating full records.

References: https://docs.typesafe.ai/models and https://github.com/NandhaKishorM/laya.

## Input and target

Input contains `source_context`, `parser_output`, `extractor_output`, `normalization_output`, `matching_output`, `master_data`, `validation_output`, and `final_output`.

Ground truth: `{"acceptable_for_auto_processing":true}`.

Model prediction: `{"confidence":0.82}` — probability that the final output is acceptable, not the model's certainty in whichever yes/no answer it selected. For a Noul question, use P(true).

Training records also contain `training_annotations` with the expected final output, explanation, failure mechanisms and evidence references. These are optional training aids, never inference inputs. Validation records do not include those annotations. Both splits are 50% acceptable and 50% unacceptable.

## Acceptance policy

Use parsed source and master data as the available authoritative evidence. Signed amendments override only the fields they cover. Every required business field in final_output must agree with that evidence: document type/number/date, currency, parties, ship-to, payment terms, freight, all line identities/products/quantities/units/prices/price bases/discounts/taxes/dates/schedules, and derived amounts. An empty schedule means no explicit split schedule. Retain zero-quantity amendments and negative returns when instructed. Round line net amounts HALF_UP to two decimals; sum line taxes then round tax HALF_UP; total equals subtotal plus tax plus freight. Freight is untaxed in these fixtures.

Any consequential field error makes the label false. Pipeline-reported scores and validation results may be wrong. Early errors may be corrected; matching stages can propagate a wrong interpretation. Do not infer failures visible only in an unavailable original image.

## Approach and evaluation

Ask focused evidence questions (whether the current amendment is honored, whether the selected product matches the specification, whether a correction is supported), then combine answers and code-computed checks. Alternatively learn a direct acceptability decision with an appropriate Laya training workflow. The entire pipeline JSON may exceed default Laya context limits.

Primary metric: AUROC on predicted probabilities against binary labels. Secondary: Brier score (mean squared probability error), coverage at a chosen acceptance threshold, and incorrect fraction among accepted cases. An always-constant score has AUROC 0.5; these 50-case estimates are noisy.

## Difficulty and limits

24 semantic mechanisms combine revisions, split schedules, price bases, carton conversions, currency precedence, roles, variants, addresses, stacked discounts, tax exemptions, gross prices, returns, free goods, locales, substitutions, cancellations, partial postings, FX, freight, buyer codes, repeated products, net weights and payment terms. Cases have 2–5 lines and varied product vocabulary. Exact mechanism combinations and presentation families are split-separated, but synthetic language and numeric conventions recur.

Stage agreement alone is insufficient. A simple final-value statistical baseline still exploited synthetic patterns; this is a learning challenge, not a proven hard benchmark. No JEV/Laya evaluation has been run.
