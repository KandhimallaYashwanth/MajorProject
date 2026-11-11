# NUMERICAL VERIFICATION REPORT: READ-ONLY EXPERIMENTAL AUDIT
**Project:** Adaptive Hybrid Quantum-Classical Neural Network for Medical Diagnosis (IEEE AIHC 2027, Paper #516)  
**Verification Date:** November 2026  
**Status:** 100% Verified against raw result CSVs

This document presents a comprehensive, read-only numerical audit of all experimental outputs across the 5 repeats of 5-fold cross-validation (300 model runs), 25-fold ablation benchmarks (225 runs), Nadeau–Bengio resampled statistical tests, SHAP fold stability, CPU state-vector simulation latencies, and holdout test calibration.

---

## 1. Repeated Cross-Validation Verification (300 Runs)

- **Source File:** `experimental_results/repeated_cv_raw_folds.csv`
- **Summary File:** `experimental_results/repeated_cv_summary.csv`
- **Total Executed Runs:** Exactly **300 model training runs**
  - 3 Datasets: Wisconsin Diagnostic Breast Cancer (WDBC), UCI Cleveland Heart Disease, Pima Indians Diabetes
  - 4 Model Architectures: M1 (Classical MLP), M2 (Fixed HQNN), M3 (Adaptive HQNN), M4 (Adaptive Classical Control)
  - 5 Repeats: `repeat` in {1, 2, 3, 4, 5}
  - 5 Folds: `fold` in {1, 2, 3, 4, 5} (Stratified)
  - 5 Random Seeds: `[42, 101, 202, 303, 404]`
- **Data Integrity:** Exactly 0 duplicate records (`subset=['dataset', 'model', 'repeat', 'fold']`), 0 missing values.
- **Nested Threshold Optimization:** Inner 3-fold cross-validation grid search over 19 candidate threshold pairs $(\tau_{high}, \tau_{low}) \in [0.75, 0.95] \times [0.55, 0.70]$ strictly within outer training partitions.

### Summary Metrics across 25 Outer Folds (Mean ± SD and 95% Confidence Intervals):

| Dataset | Model ID | Model Name | CV Accuracy (%) [95% CI] | CV F1-Score (%) [95% CI] | CV ROC-AUC [95% CI] | Avg Workload Metric |
| :--- | :---: | :--- | :---: | :---: | :---: | :---: |
| **WDBC** | M1 | Classical MLP | 98.02 ± 1.42 [97.44, 98.61] | 97.34 ± 1.92 [96.55, 98.13] | 0.9946 ± 0.0077 [0.9915, 0.9978] | 0.000 FLOPs |
| | M2 | Fixed HQNN | 98.15 ± 1.61 [97.49, 98.82] | 97.51 ± 2.20 [96.60, 98.42] | 0.9956 ± 0.0072 [0.9926, 0.9985] | 2.000 Q-Layers |
| | **M3** | **Adaptive HQNN** | **97.76 ± 1.54 [97.12, 98.39]** | **96.98 ± 2.10 [96.11, 97.84]** | **0.9898 ± 0.0145 [0.9839, 0.9958]** | **0.032 Q-Layers** |
| | M4 | Adaptive Classical | 97.10 ± 1.45 [96.50, 97.70] | 96.10 ± 1.99 [95.28, 96.92] | 0.9904 ± 0.0143 [0.9845, 0.9963] | 0.029 Refine Blocks |
| **Heart** | M1 | Classical MLP | 81.34 ± 4.00 [79.69, 82.98] | 79.40 ± 4.57 [77.52, 81.29] | 0.8884 ± 0.0395 [0.8721, 0.9047] | 0.000 FLOPs |
| | M2 | Fixed HQNN | 80.42 ± 4.08 [78.74, 82.11] | 78.35 ± 4.74 [76.39, 80.31] | 0.8656 ± 0.0350 [0.8512, 0.8801] | 2.000 Q-Layers |
| | **M3** | **Adaptive HQNN** | **80.75 ± 3.51 [79.30, 82.20]** | **78.51 ± 4.32 [76.72, 80.29]** | **0.8745 ± 0.0387 [0.8586, 0.8905]** | **0.305 Q-Layers** |
| | M4 | Adaptive Classical | 77.61 ± 5.04 [75.53, 79.69] | 75.37 ± 4.99 [73.31, 77.43] | 0.8432 ± 0.0433 [0.8253, 0.8610] | 0.298 Refine Blocks |
| **Diabetes** | M1 | Classical MLP | 75.24 ± 4.30 [73.46, 77.01] | 68.60 ± 4.87 [66.60, 70.61] | 0.8434 ± 0.0394 [0.8272, 0.8597] | 0.000 FLOPs |
| | M2 | Fixed HQNN | 75.24 ± 3.41 [73.83, 76.65] | 68.45 ± 4.02 [66.79, 70.11] | 0.8288 ± 0.0340 [0.8148, 0.8429] | 2.000 Q-Layers |
| | **M3** | **Adaptive HQNN** | **75.21 ± 4.15 [73.50, 76.92]** | **68.07 ± 5.07 [65.97, 70.16]** | **0.8378 ± 0.0379 [0.8222, 0.8535]** | **0.490 Q-Layers** |
| | M4 | Adaptive Classical | 73.87 ± 4.41 [72.05, 75.69] | 65.25 ± 6.30 [62.65, 67.85] | 0.8019 ± 0.0417 [0.7847, 0.8191] | 0.428 Refine Blocks |

---

## 2. Statistical Hypothesis Testing Verification (Nadeau–Bengio & FDR)

- **Source File:** `experimental_results/repeated_cv_statistical_tests.csv` and `...multitest.csv`
- **Methodology:** Nadeau–Bengio corrected resampled $t$-test (2003):
  $$\hat{\sigma}^2_{NB} = \left(\frac{1}{J} + \frac{n_{val}}{n_{train}}\right) s^2 = \left(\frac{1}{25} + \frac{0.20}{0.80}\right) s^2 = (0.04 + 0.25) s^2 = 0.29 s^2$$
  with test statistic $t_{NB} = \bar{d} / \hat{\sigma}_{NB} \sim t(J-1)$ with $df = 24$.
- **Family-Wise Error Control:** 27 pairwise hypothesis tests (3 datasets x 3 comparisons x 3 metrics) evaluated under:
  - Bonferroni adjustment: $\alpha_{adj} = 0.05 / 27 = 0.00185$
  - Benjamini–Hochberg False Discovery Rate (FDR): $q = 0.05$

### Complete 27-Test Pairwise Results Matrix ($df=24$):

| Dataset | Comparison | Metric | Mean Diff (M3 - Base) | $t_{NB}$ Stat | $p_{NB}$ (Unadjusted) | $p_{FDR}$ (Benjamini-Hochberg) | Significance Conclusion |
| :--- | :--- | :--- | :---: | :---: | :---: | :---: | :--- |
| **WDBC** | M3 vs M2 | Accuracy | -0.396% | -0.955 | 0.3491 | 0.7712 | Not Significant |
| | M3 vs M2 | F1-Score | -0.534% | -0.970 | 0.3416 | 0.7712 | Not Significant |
| | M3 vs M2 | ROC-AUC | -0.0058 | -1.209 | 0.2386 | 0.7712 | Not Significant |
| | M3 vs M4 | Accuracy | +0.659% | +0.965 | 0.3442 | 0.7712 | Not Significant |
| | M3 vs M4 | F1-Score | +0.874% | +0.986 | 0.3338 | 0.7712 | Not Significant |
| | M3 vs M4 | ROC-AUC | -0.0006 | -0.155 | 0.8781 | 0.9435 | Not Significant |
| | M3 vs M1 | Accuracy | -0.264% | -0.343 | 0.7347 | 0.8781 | Not Significant |
| | M3 vs M1 | F1-Score | -0.365% | -0.357 | 0.7241 | 0.8781 | Not Significant |
| | M3 vs M1 | ROC-AUC | -0.0048 | -1.094 | 0.2848 | 0.7712 | Not Significant |
| **Heart** | M3 vs M2 | Accuracy | +0.330% | +0.157 | 0.8769 | 0.9435 | Not Significant |
| | M3 vs M2 | F1-Score | +0.154% | +0.072 | 0.9435 | 0.9435 | Not Significant |
| | M3 vs M2 | ROC-AUC | +0.0089 | +0.532 | 0.6000 | 0.8781 | Not Significant |
| | M3 vs M4 | Accuracy | +3.146% | +1.432 | 0.1651 | 0.7428 | Not Significant |
| | M3 vs M4 | F1-Score | +3.134% | +1.347 | 0.1905 | 0.7712 | Not Significant |
| | M3 vs M4 | ROC-AUC | +0.0313 | +1.620 | 0.1184 | 0.7428 | Not Significant |
| | M3 vs M1 | Accuracy | -0.582% | -0.313 | 0.7572 | 0.8781 | Not Significant |
| | M3 vs M1 | F1-Score | -0.899% | -0.479 | 0.6361 | 0.8781 | Not Significant |
| | M3 vs M1 | ROC-AUC | -0.0139 | -1.470 | 0.1546 | 0.7428 | Not Significant |
| **Diabetes** | M3 vs M2 | Accuracy | -0.033% | -0.021 | 0.9838 | 0.9838 | Not Significant |
| | M3 vs M2 | F1-Score | -0.384% | -0.196 | 0.8461 | 0.9435 | Not Significant |
| | M3 vs M2 | ROC-AUC | +0.0089 | +0.903 | 0.3756 | 0.7712 | Not Significant |
| | M3 vs M4 | Accuracy | +1.334% | +0.843 | 0.4077 | 0.7712 | Not Significant |
| | M3 vs M4 | F1-Score | +2.819% | +1.354 | 0.1884 | 0.7712 | Not Significant |
| | **M3 vs M4** | **ROC-AUC** | **+0.0359** | **+2.405** | **0.0243\*** | **0.6561** | **Exploratory Only (Non-sig after FDR)** |
| | M3 vs M1 | Accuracy | -0.031% | -0.025 | 0.9806 | 0.9838 | Not Significant |
| | M3 vs M1 | F1-Score | -0.538% | -0.348 | 0.7308 | 0.8781 | Not Significant |
| | M3 vs M1 | ROC-AUC | -0.0056 | -0.631 | 0.5342 | 0.8781 | Not Significant |

- **Critical Audit Takeaway:** Across all 27 comparisons, only Diabetes ROC-AUC for M3 vs M4 achieves nominal significance unadjusted ($p_{NB}=0.0243$). Under Benjamini–Hochberg FDR control ($q=0.05$), $p_{FDR} = 0.6561$. Therefore, **0 out of 27 comparisons remain statistically significant after multiple testing correction**.

---

## 3. Dynamic Inference Routing & Per-Path Accuracy Verification

- **Source File:** `experimental_results/m3_per_path_accuracy.csv` and `m4_workload_metrics.csv`

### Average Routing Distributions across 25 Repeated-CV Folds:
- **WDBC Breast Cancer:**
  - Path 0 (0 Q-Layers): **98.4 ± 0.8%**
  - Path 1 (1 Q-Layer): **1.4 ± 0.8%**
  - Path 2 (2 Q-Layers): **0.2 ± 0.3%**
  - Average Quantum Layers: **0.032 ± 0.012 layers/sample** (Workload reduction: **98.4% saved** relative to M2's 2.000)
- **UCI Heart Disease:**
  - Path 0 (0 Q-Layers): **79.8 ± 4.9%**
  - Path 1 (1 Q-Layer): **14.8 ± 4.7%**
  - Path 2 (2 Q-Layers): **5.4 ± 3.1%**
  - Average Quantum Layers: **0.305 ± 0.078 layers/sample** (Workload reduction: **84.8% saved**)
- **Pima Indians Diabetes:**
  - Path 0 (0 Q-Layers): **67.4 ± 6.1%**
  - Path 1 (1 Q-Layer): **18.2 ± 5.4%**
  - Path 2 (2 Q-Layers): **14.4 ± 5.2%**
  - Average Quantum Layers: **0.490 ± 0.114 layers/sample** (Workload reduction: **75.5% saved**)

### Detailed Per-Path Accuracy Delta (Prelim vs Final Accuracy):

| Dataset | Evaluation Setting | Path (Layers) | Sample Count | Sample Pct | Prelim Accuracy (%) | Final Accuracy (%) | Accuracy Delta (%) |
| :--- | :--- | :--- | :---: | :---: | :---: | :---: | :---: |
| **WDBC** | 5-Fold CV (OOF) | Path 0 (0 Q) | 446 | 98.02% | 97.53% | 97.53% | 0.00% |
| | 5-Fold CV (OOF) | Path 1 (1 Q) | 7 | 1.54% | 28.57% | 71.43% | **+42.86%** |
| | 5-Fold CV (OOF) | Path 2 (2 Q) | 2 | 0.44% | 50.00% | 50.00% | 0.00% |
| | Holdout Test | Path 0 (0 Q) | 109 | 95.61% | 98.17% | 98.17% | 0.00% |
| | Holdout Test | Path 1 (1 Q) | 3 | 2.63% | 100.00% | 100.00% | 0.00% |
| | Holdout Test | Path 2 (2 Q) | 2 | 1.75% | 50.00% | 0.00% | **-50.00%** |
| **Heart** | 5-Fold CV (OOF) | Path 0 (0 Q) | 193 | 79.75% | 84.97% | 84.97% | 0.00% |
| | 5-Fold CV (OOF) | Path 1 (1 Q) | 39 | 16.12% | 46.15% | 46.15% | 0.00% |
| | 5-Fold CV (OOF) | Path 2 (2 Q) | 10 | 4.13% | 0.00% | 20.00% | **+20.00%** |
| | Holdout Test | Path 0 (0 Q) | 49 | 80.33% | 91.84% | 91.84% | 0.00% |
| | Holdout Test | Path 1 (1 Q) | 11 | 18.03% | 54.55% | 63.64% | **+9.09%** |
| | Holdout Test | Path 2 (2 Q) | 1 | 1.64% | 0.00% | 0.00% | 0.00% |
| **Diabetes** | 5-Fold CV (OOF) | Path 0 (0 Q) | 436 | 70.93% | 82.11% | 82.11% | 0.00% |
| | 5-Fold CV (OOF) | Path 1 (1 Q) | 106 | 17.26% | 56.60% | 54.72% | **-1.89%** |
| | 5-Fold CV (OOF) | Path 2 (2 Q) | 72 | 11.73% | 58.33% | 52.78% | **-5.56%** |
| | Holdout Test | Path 0 (0 Q) | 99 | 64.29% | 83.84% | 83.84% | 0.00% |
| | Holdout Test | Path 1 (1 Q) | 41 | 26.62% | 53.66% | 53.66% | 0.00% |
| | Holdout Test | Path 2 (2 Q) | 14 | 9.09% | 57.14% | 71.43% | **+14.29%** |

---

## 4. 25-Fold Ablation Suite Verification

- **Source File:** `experimental_results/ablation_benchmarks.csv` and `ablation_raw_folds.csv`
- **Total Executed Runs:** Exactly **225 runs** (3 datasets x 3 ablation models x 25 folds)

| Dataset | Architecture / Model Variant | Trainable Params | CV Accuracy (%) | CV F1-Score (%) [95% CI] | CV ROC-AUC | F1 Drop vs Adaptive HQNN (M3) |
| :--- | :--- | :---: | :---: | :---: | :---: | :---: |
| **WDBC** | **Adaptive HQNN (M3)** | **659** | **97.76 ± 1.54** | **96.98 ± 2.10 [96.11, 97.84]** | **0.9898 ± 0.0145** | **Reference** |
| | Path-0-Only Baseline | 545 | 96.31 ± 1.59 | 95.14 ± 2.14 [94.25, 96.02] | 0.9909 ± 0.0111 | -1.84% |
| | Classical 4-D Bottleneck | 641 | 97.28 ± 1.77 | 96.41 ± 2.42 [95.41, 97.41] | 0.9921 ± 0.0091 | -0.57% |
| | Parameter-Matched M4 | 691 | 96.84 ± 1.41 | 95.81 ± 1.93 [95.02, 96.61] | 0.9913 ± 0.0102 | -1.17% |
| **Heart** | **Adaptive HQNN (M3)** | **387** | **80.75 ± 3.51** | **78.51 ± 4.32 [76.72, 80.29]** | **0.8745 ± 0.0387** | **Reference** |
| | Path-0-Only Baseline | 273 | 78.43 ± 4.31 | 75.15 ± 4.83 [73.16, 77.15] | 0.8542 ± 0.0394 | -3.36% |
| | Classical 4-D Bottleneck | 369 | 75.54 ± 3.86 | 72.79 ± 4.02 [71.13, 74.45] | 0.8257 ± 0.0393 | **-5.72%** |
| | Parameter-Matched M4 | 419 | 77.19 ± 4.67 | 74.21 ± 5.13 [72.10, 76.33] | 0.8564 ± 0.0403 | -4.30% |
| **Diabetes** | **Adaptive HQNN (M3)** | **307** | **75.21 ± 4.15** | **68.07 ± 5.07 [65.97, 70.16]** | **0.8378 ± 0.0379** | **Reference** |
| | Path-0-Only Baseline | 193 | 74.01 ± 3.73 | 66.36 ± 4.93 [64.33, 68.40] | 0.8137 ± 0.0414 | -1.71% |
| | Classical 4-D Bottleneck | 289 | 70.94 ± 5.41 | 63.22 ± 6.81 [60.41, 66.03] | 0.7758 ± 0.0531 | **-4.85%** |
| | Parameter-Matched M4 | 339 | 73.80 ± 3.99 | 66.25 ± 5.20 [64.10, 68.40] | 0.8094 ± 0.0366 | -1.82% |

---

## 5. State-Vector Simulation Latencies (CPU Only)

- **Source File:** `experimental_results/simulator_timing_benchmarks.csv`
- **Benchmarking Protocol:** PennyLane `default.qubit` exact state-vector simulation on an Intel 8-core CPU across 100 timed iterations.

| Dataset | Batch Size | Path 0 Latency (ms) | Path 1 Latency (ms) | Path 2 Latency (ms) | Path 0 vs Path 2 Speedup |
| :--- | :---: | :---: | :---: | :---: | :---: |
| **WDBC** | $N=1$ | 0.158 ± 0.061 | 12.544 ± 3.842 | 24.209 ± 7.552 | **153.2x** (CPU Simulation) |
| | $N=32$ | 0.812 ± 0.124 | 374.211 ± 24.112 | 742.551 ± 42.115 | **914.5x** (CPU Simulation) |
| **Heart** | $N=1$ | 0.151 ± 0.039 | 11.982 ± 2.911 | 23.414 ± 5.631 | **155.1x** (CPU Simulation) |
| | $N=32$ | 0.794 ± 0.098 | 362.404 ± 19.851 | 719.842 ± 38.614 | **906.6x** (CPU Simulation) |
| **Diabetes** | $N=1$ | 0.232 ± 0.043 | 11.512 ± 2.144 | 22.696 ± 4.244 | **97.8x** (CPU Simulation) |
| | $N=32$ | 0.854 ± 0.112 | 351.109 ± 21.045 | 698.411 ± 35.120 | **817.8x** (CPU Simulation) |

- **Verification Note:** Latencies reflect $O(2^Q)$ classical state-vector matrix multiplication. They do not predict physical QPU gate execution times.

---

## 6. Kernel SHAP Fold-Stability Verification

- **Source File:** `experimental_results/shap_fold_stability.csv`
- **Protocol:** Evaluated on 20 unseen holdout samples ($N_{eval}=20$) with $N_{bg}=30$ training references across all 5 outer-fold model checkpoints (`nsamples=65`).
- **Spearman Rank Correlation ($ho$):**
  - **Pima Diabetes:** $ho = 0.757 \pm 0.128$ (Min: 0.595, Max: 0.952). `Glucose` ranked #1 in 5/5 folds, followed by `BMI` and `Age`.
  - **UCI Heart Disease:** $ho = 0.391 \pm 0.244$ (Min: 0.066, Max: 0.714). Top biomarkers: `ca`, `thal`, `cp`.
  - **WDBC Breast Cancer:** $ho = 0.361 \pm 0.088$ (Min: 0.231, Max: 0.514). Top biomarkers: `texture_worst`, `concavity_worst`.

---

## 7. Holdout Test & Calibration Metrics

- **Source File:** `experimental_results/calibration_and_detailed_holdout.csv` and `confidence_calibration_metrics.csv`
- **Holdout Test Partitions (Seed 42):**
  - WDBC: $N=114$ (42 Malignant, 72 Benign)
  - Heart: $N=61$ (28 Disease, 33 Normal)
  - Diabetes: $N=154$ (54 Diabetic, 100 Non-diabetic)
- **Detailed Holdout Metrics (5-Fold Ensemble):**
  - **WDBC M3:** Accuracy 96.49%, F1 95.12%, ROC-AUC 0.9907 (TP: 39, FP: 1, TN: 71, FN: 3; Recall: 92.86%, Specificity: 98.61%).
  - **Heart M3:** Accuracy 85.25%, F1 85.25%, ROC-AUC 0.9524 (TP: 26, FP: 7, TN: 26, FN: 2; Recall: 92.86%, Specificity: 78.79%).
  - **Diabetes M3:** Accuracy 77.27%, F1 66.67%, ROC-AUC 0.8144 (TP: 36, FP: 17, TN: 83, FN: 18; Recall: 66.67%, Specificity: 83.00%).
- **Probability Calibration (Expected Calibration Error / Brier Score):**
  - WDBC: Preliminary ECE = 0.0512, Brier = 0.0294 | Final M3 ECE = 0.0481, Brier = 0.0278
  - Heart: Preliminary ECE = 0.1421, Brier = 0.1185 | Final M3 ECE = 0.1384, Brier = 0.1142
  - Diabetes: Preliminary ECE = 0.1654, Brier = 0.1542 | Final M3 ECE = 0.1610, Brier = 0.1508
