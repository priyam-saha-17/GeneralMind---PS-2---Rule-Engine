# Project 1 — Document classifier

Determine the business purpose of the supplied document text.

## Files and usage

- `fine_tuning.jsonl`: 200 labeled training examples.
- `validation.jsonl`: 50 labeled validation examples.

JSONL means one JSON object per line: `{"case_id":"...","input":{...},"target":{...}}`. Feed only `input` to the model; `target` is the ground-truth supervision. Keep validation separate from training and use it to compare approaches and select settings. There are no test answers in this shareable folder.

All data is synthetic and requires no vision, PDF processing, external service or live pipeline. These are model-independent records, not a native training upload format. JEV does not offer customer fine-tuning: use training cases to develop typed questions or train a downstream model on JEV outputs. Laya supports its own fine-tuning workflow; adapt these records to it. Select relevant evidence for each question and check the chosen model's context budget rather than silently truncating full records.

References: https://docs.typesafe.ai/models and https://github.com/NandhaKishorM/laya.

## Input and expected output

Input: `email_context` (subject/body) and `document_text`.

Target: `{"document_type":"SO","review_required":false}`.

| Output | Meaning |
|---|---|
| PO | Authorized buyer order, binding call-off or order revision. |
| SO | Seller acceptance or acknowledgment committing to supply. |
| INVOICE | Actual bill, including corrected or already-paid invoices. |
| OTHER | Quote, RFQ, pro forma, credit note, delivery note, statement, remittance, forecast, unapproved draft or standalone cancellation. |
| null | Insufficient evidence or mixed standalone documents with no primary document. |

`review_required` must be true exactly when `document_type` is null. Referenced historical documents do not determine the current document's class. Email subjects can be misleading. Train has 40 cases per output state; validation has 10 each.

## Approach and evaluation

Use a Choice decision with five options, mapping the review option back to null plus review_required=true. Report four-class macro F1 on determinate cases, review F1 on all cases, and exact target accuracy. A review prediction on a determinate case is incorrect. F1 for an absent class is zero.

## Difficulty and limits

Misleading headings, untitled documents, quoted history, pro forma versus actual invoices, English/German text, fragments and mixed packets. Synthetic constructions recur across splits; no real-world or JEV/Laya performance is claimed.
