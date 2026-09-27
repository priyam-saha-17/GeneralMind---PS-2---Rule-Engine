Zero Shot Baseline Scores on Validation Set


======================================================================
Default
======================================================================
Overall Accuracy: 24.00%

Per-rule Accuracy
rule
R01_AMENDMENT            26.0%
R02_SUBSTITUTION         30.0%
R03_RELEASE              16.0%
R04_DOCUMENT_RELATION    26.0%
R05_GOVERNING_TERMS      22.0%
Name: correct, dtype: object

Per-rule AUROC
R01_AMENDMENT            0.576
R02_SUBSTITUTION         0.442
R03_RELEASE              0.345
R04_DOCUMENT_RELATION    0.605
R05_GOVERNING_TERMS      0.583
dtype: float64

Macro AUROC: 0.510
======================================================================
Typed Decisions
======================================================================
Overall Accuracy: 35.60%

Per-rule Accuracy
rule
R01_AMENDMENT            44.0%
R02_SUBSTITUTION         26.0%
R03_RELEASE              22.0%
R04_DOCUMENT_RELATION    38.0%
R05_GOVERNING_TERMS      48.0%
Name: correct, dtype: object

Per-rule AUROC
R01_AMENDMENT            0.562
R02_SUBSTITUTION         0.552
R03_RELEASE              0.366
R04_DOCUMENT_RELATION    0.610
R05_GOVERNING_TERMS      0.499
dtype: float64

Macro AUROC: 0.518

======================================================================
Summary
======================================================================
 	Model	Accuracy	Macro AUROC
0	Default	24.00%	0.510
1	Typed Decisions	35.60%	0.518





Experiment 1: Kept the ModernBERT frozen, trained only the head. Epochs = 4, Max_len = 512, batch_size = 2, 
Results:
 	Model	Accuracy	Macro AUROC
0	Zero-shot Laya	24.00%	0.510
1	Fine-tuned Laya	24.00%	0.495





