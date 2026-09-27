# Notebook 1

**Kaggle Notebook:** [Zero-Shot Baseline & Frozen ModernBERT Experiments](https://www.kaggle.com/code/priyamsaha17/generalmind-ps-2-rule-engine-notebook-1)

# Zero-Shot Baseline Scores on Validation Set

## Default

**Overall Accuracy:** **24.00%**

### Per-rule Accuracy

| Rule | Accuracy |
|------|---------:|
| R01_AMENDMENT | 26.0% |
| R02_SUBSTITUTION | 30.0% |
| R03_RELEASE | 16.0% |
| R04_DOCUMENT_RELATION | 26.0% |
| R05_GOVERNING_TERMS | 22.0% |

### Per-rule AUROC

| Rule | AUROC |
|------|-------:|
| R01_AMENDMENT | 0.576 |
| R02_SUBSTITUTION | 0.442 |
| R03_RELEASE | 0.345 |
| R04_DOCUMENT_RELATION | 0.605 |
| R05_GOVERNING_TERMS | 0.583 |

**Macro AUROC:** **0.510**

---

## Typed Decisions

**Overall Accuracy:** **35.60%**

### Per-rule Accuracy

| Rule | Accuracy |
|------|---------:|
| R01_AMENDMENT | 44.0% |
| R02_SUBSTITUTION | 26.0% |
| R03_RELEASE | 22.0% |
| R04_DOCUMENT_RELATION | 38.0% |
| R05_GOVERNING_TERMS | 48.0% |

### Per-rule AUROC

| Rule | AUROC |
|------|-------:|
| R01_AMENDMENT | 0.562 |
| R02_SUBSTITUTION | 0.552 |
| R03_RELEASE | 0.366 |
| R04_DOCUMENT_RELATION | 0.610 |
| R05_GOVERNING_TERMS | 0.499 |

**Macro AUROC:** **0.518**

---

# Zero-Shot Baseline Summary

| Model | Accuracy | Macro AUROC |
|-------|---------:|------------:|
| Default | 24.00% | 0.510 |
| Typed Decisions | 35.60% | 0.518 |

---

# Experiment 1: Frozen ModernBERT Fine-Tuning

**Configuration**

- **Backbone:** ModernBERT (frozen)
- **Trainable Parameters:** Classification head only
- **Epochs:** 4
- **Maximum Sequence Length:** 512
- **Batch Size:** 2

## Results

| Model | Accuracy | Macro AUROC |
|-------|---------:|------------:|
| Zero-shot Laya | 24.00% | 0.510 |
| Fine-tuned Laya | 24.00% | 0.495 |

## Key Observation

Fine-tuning only the classification head of a frozen ModernBERT backbone did not improve validation accuracy. The accuracy remained at **24.00%**, while the Macro AUROC decreased slightly from **0.510** to **0.495**.



