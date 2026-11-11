# MANUSCRIPT DISCREPANCY REPORT: SOURCE VS DATA AUDIT
**Project:** Adaptive Hybrid Quantum-Classical Neural Network for Medical Diagnosis (IEEE AIHC 2027, Paper #516)  
**Comparison Target:** `latest_camera_ready.tex` (`paper/paper.tex`) and `latest_camera_ready.pdf` vs. Verified Experimental CSVs and Reviewer Response Matrix  
**Date:** November 2026  

This audit systematically checks every reported number, equation, p-value, workload metric, and narrative claim in the revised manuscript against the authoritative raw CSV results.

---

## 1. Structured Manuscript-to-CSV Discrepancy Table

| Manuscript Section / Table | Reported Manuscript Value | Current CSV Value | Source File | Status | Audit Findings & Notes |
| :--- | :--- | :--- | :--- | :---: | :--- |
| **Table I (Datasets)** | WDBC: $N=569, D=30$ | $N=569, D=30$ | `splits_config.json` | **VERIFIED** | Matches dataset metadata exactly. |
| **Table I (Datasets)** | Heart: $N=303, D=13$ | $N=303, D=13$ | `splits_config.json` | **VERIFIED** | Matches dataset metadata exactly. |
| **Table I (Datasets)** | Diabetes: $N=768, D=8$ | $N=768, D=8$ | `splits_config.json` | **VERIFIED** | Matches dataset metadata exactly. |
| **Sec. IV-C (Parameters)** | M1: 1,601 (WDBC), 1,057 (Heart), 897 (Diabetes) | 1,601 / 1,057 / 897 | PyTorch graph audit | **VERIFIED** | Corrected from initial submission ($D \to 32 \to 16 \to 1$). |
| **Sec. IV-C (Parameters)** | M2: 625 (WDBC), 353 (Heart), 273 (Diabetes) | 625 / 353 / 273 | PyTorch graph audit | **VERIFIED** | Matches classical encoder + 24 quantum params. |
| **Sec. IV-C (Parameters)** | M3: 659 (WDBC), 387 (Heart), 307 (Diabetes) | 659 / 387 / 307 | PyTorch graph audit | **VERIFIED** | Matches classical encoder + 36 quantum params. |
| **Sec. IV-C (Parameters)** | M4: 1,923 (WDBC), 1,651 (Heart), 1,571 (Diabetes) | 1,923 / 1,651 / 1,571 | PyTorch graph audit | **VERIFIED** | Matches full M4 residual dense blocks. |
| **Table II (WDBC M1)** | Acc: $98.02 \pm 1.42$, F1: $97.34 \pm 1.92$, AUC: $0.9946 \pm 0.0077$ | Acc: 98.02, F1: 97.34, AUC: 0.9946 | `repeated_cv_summary.csv` | **VERIFIED** | Exact numerical match across 25 folds. |
| **Table II (WDBC M2)** | Acc: $98.15 \pm 1.61$, F1: $97.51 \pm 2.20$, AUC: $0.9956 \pm 0.0072$ | Acc: 98.15, F1: 97.51, AUC: 0.9956 | `repeated_cv_summary.csv` | **VERIFIED** | Exact numerical match across 25 folds. |
| **Table II (WDBC M3)** | Acc: $97.76 \pm 1.54$, F1: $96.98 \pm 2.10$, AUC: $0.9898 \pm 0.0145$ | Acc: 97.76, F1: 96.98, AUC: 0.9898 | `repeated_cv_summary.csv` | **VERIFIED** | Exact numerical match across 25 folds. |
| **Table II (WDBC M4)** | Acc: $97.10 \pm 1.45$, F1: $96.10 \pm 1.99$, AUC: $0.9904 \pm 0.0143$ | Acc: 97.10, F1: 96.10, AUC: 0.9904 | `repeated_cv_summary.csv` | **VERIFIED** | Exact numerical match across 25 folds. |
| **Table II (Heart M1)** | Acc: $81.34 \pm 4.00$, F1: $79.40 \pm 4.57$, AUC: $0.8884 \pm 0.0395$ | Acc: 81.34, F1: 79.40, AUC: 0.8884 | `repeated_cv_summary.csv` | **VERIFIED** | Exact numerical match across 25 folds. |
| **Table II (Heart M2)** | Acc: $80.42 \pm 4.08$, F1: $78.35 \pm 4.74$, AUC: $0.8656 \pm 0.0350$ | Acc: 80.42, F1: 78.35, AUC: 0.8656 | `repeated_cv_summary.csv` | **VERIFIED** | Exact numerical match across 25 folds. |
| **Table II (Heart M3)** | Acc: $80.75 \pm 3.51$, F1: $78.51 \pm 4.32$, AUC: $0.8745 \pm 0.0387$ | Acc: 80.75, F1: 78.51, AUC: 0.8745 | `repeated_cv_summary.csv` | **VERIFIED** | Exact numerical match across 25 folds. |
| **Table II (Heart M4)** | Acc: $77.61 \pm 5.04$, F1: $75.37 \pm 4.99$, AUC: $0.8432 \pm 0.0433$ | Acc: 77.61, F1: 75.37, AUC: 0.8432 | `repeated_cv_summary.csv` | **VERIFIED** | Exact numerical match across 25 folds. |
| **Table II (Diabetes M1)** | Acc: $75.24 \pm 4.30$, F1: $68.60 \pm 4.87$, AUC: $0.8434 \pm 0.0394$ | Acc: 75.24, F1: 68.60, AUC: 0.8434 | `repeated_cv_summary.csv` | **VERIFIED** | Exact numerical match across 25 folds. |
| **Table II (Diabetes M2)** | Acc: $75.24 \pm 3.41$, F1: $68.45 \pm 4.02$, AUC: $0.8288 \pm 0.0340$ | Acc: 75.24, F1: 68.45, AUC: 0.8288 | `repeated_cv_summary.csv` | **VERIFIED** | Exact numerical match across 25 folds. |
| **Table II (Diabetes M3)** | Acc: $75.21 \pm 4.15$, F1: $68.07 \pm 5.07$, AUC: $0.8378 \pm 0.0379$ | Acc: 75.21, F1: 68.07, AUC: 0.8378 | `repeated_cv_summary.csv` | **VERIFIED** | Exact numerical match across 25 folds. |
| **Table II (Diabetes M4)** | Acc: $73.87 \pm 4.41$, F1: $65.25 \pm 6.30$, AUC: $0.8019 \pm 0.0417$ | Acc: 73.87, F1: 65.25, AUC: 0.8019 | `repeated_cv_summary.csv` | **VERIFIED** | Exact numerical match across 25 folds. |
| **Table II (Holdout F1)** | WDBC: M1 98.80%, M2 96.30%, M3 95.12%, M4 96.30% | 98.80 / 96.30 / 95.12 / 96.30 | `calibration_and_detailed_holdout.csv` | **VERIFIED** | Exact match with holdout test ensemble. |
| **Table II (Holdout F1)** | Heart: M1 86.67%, M2 85.25%, M3 85.25%, M4 85.25% | 86.67 / 85.25 / 85.25 / 85.25 | `calibration_and_detailed_holdout.csv` | **VERIFIED** | Exact match with holdout test ensemble. |
| **Table II (Holdout F1)** | Diabetes: M1 67.72%, M2 65.04%, M3 66.67%, M4 59.83% | 67.72 / 65.04 / 66.67 / 59.83 | `calibration_and_detailed_holdout.csv` | **VERIFIED** | Exact match with holdout test ensemble. |
| **Table III (Workload)** | WDBC: Path 0 98.4%, Path 1 1.4%, Path 2 0.2%, Avg QL 0.032 | 98.4 / 1.4 / 0.2 / 0.032 | `repeated_cv_summary.csv` | **VERIFIED** | Exact match (98.4% layer reduction). |
| **Table III (Workload)** | Heart: Path 0 79.8%, Path 1 14.8%, Path 2 5.4%, Avg QL 0.305 | 79.8 / 14.8 / 5.4 / 0.305 | `repeated_cv_summary.csv` | **VERIFIED** | Exact match (84.8% layer reduction). |
| **Table III (Workload)** | Diabetes: Path 0 67.4%, Path 1 18.2%, Path 2 14.4%, Avg QL 0.490 | 67.4 / 18.2 / 14.4 / 0.490 | `repeated_cv_summary.csv` | **VERIFIED** | Exact match (75.5% layer reduction). |
| **Table IV (Ablations)** | Path-0-Only F1: 95.14% (WDBC), 75.15% (Heart), 66.36% (Diabetes) | 95.14 / 75.15 / 66.36 | `ablation_benchmarks.csv` | **VERIFIED** | Exact match across 25 folds. |
| **Table IV (Ablations)** | 4-D Bottleneck F1: 96.41% (WDBC), 72.79% (Heart), 63.22% (Diabetes) | 96.41 / 72.79 / 63.22 | `ablation_benchmarks.csv` | **VERIFIED** | Exact match across 25 folds. |
| **Table IV (Ablations)** | Param-Matched M4 F1: 95.81% (WDBC), 74.21% (Heart), 66.25% (Diabetes) | 95.81 / 74.21 / 66.25 | `ablation_benchmarks.csv` | **VERIFIED** | Exact match across 25 folds. |
| **Table V (Stats)** | M3 vs M2 Acc $p_{NB}$: 0.3491 (WDBC), 0.8769 (Heart), 0.9838 (Diabetes) | 0.3491 / 0.8769 / 0.9838 | `repeated_cv_statistical_tests.csv` | **VERIFIED** | All $p_{NB} \ge 0.05$. |
| **Table V (Stats)** | M3 vs M2 F1 $p_{NB}$: 0.3416 (WDBC), 0.9435 (Heart), 0.8461 (Diabetes) | 0.3416 / 0.9435 / 0.8461 | `repeated_cv_statistical_tests.csv` | **VERIFIED** | All $p_{NB} \ge 0.05$. |
| **Table V (Stats)** | M3 vs M4 Diabetes AUC: $+0.0359, t=2.405, p_{NB}=0.0243^*$ | $+0.0359, t=2.405, p=0.0243$ | `repeated_cv_statistical_tests.csv` | **VERIFIED** | Qualified with footnote and FDR in text. |
| **Sec. V-D (Multitest)** | Diabetes AUC $p_{FDR} = 0.6561$ under Benjamini–Hochberg ($q=0.05$) | $p_{FDR} = 0.6561$ | `...statistical_tests_multitest.csv` | **VERIFIED** | Explicitly framed as exploratory. |
| **Sec. V-E (SHAP $\rho$)** | Diabetes $\rho = 0.757 \pm 0.128$, Heart $0.391 \pm 0.244$, WDBC $0.361 \pm 0.088$ | 0.757 / 0.391 / 0.361 | `shap_fold_stability.csv` | **VERIFIED** | Matches Spearman correlation output. |
| **Sec. V-F (Timing)** | Single-instance Path 0: ~0.15–0.23 ms; Path 2: ~22.7–24.2 ms | 0.151–0.232 ms / 22.70–24.21 ms | `simulator_timing_benchmarks.csv` | **VERIFIED** | Correctly labeled as CPU simulation. |

---

## 2. Identified Discrepancies and Narrative Tone Mismatches

### Discrepancy 1: Ambiguous Case Recovery vs. Ground Truth
- **Location:** Reviewer Response Matrix (R2.1, Pillar 2)
- **Reported Claim:** *"demonstrating that quantum escalation actively recovers ambiguous cases that fail classical preliminary classification."*
- **Actual Experimental Finding (`m3_per_path_accuracy.csv`):**
  On Pima Diabetes 5-fold CV (178 escalated samples), preliminary accuracy was 56.60% on Path 1 and 58.33% on Path 2. Final quantum accuracy was **lower**: 54.72% on Path 1 ($-1.89\%$) and 52.78% on Path 2 ($-5.56\%$).
- **Discrepancy Severity:** **HIGH (Scientific Tone / Factual Nuance)**. Claude must ensure the revised narrative clarifies that routing adjusts global sensitivity/thresholds rather than guaranteeing individual error recovery.

### Discrepancy 2: Statistical "Parity" Phrasing
- **Location:** `paper/paper.tex` Section VI (Discussion) and Response Matrix Pillar 2/3
- **Reported Claim:** *"maintains strong predictive parity with full 2-layer quantum networks"*
- **Actual Statistical Finding:**
  The Nadeau–Bengio test yields $p_{NB} = 0.3416$ (WDBC F1) and $0.8461$ (Diabetes F1). Point estimates are actually 0.53% lower on WDBC and 0.38% lower on Diabetes. Non-significance does not prove parity.
- **Discrepancy Severity:** **MEDIUM (Statistical Precision)**. Claude should replace "predictive parity" with "predictive performance with no statistically significant difference detected under the Nadeau–Bengio resampled test ($p_{NB} \ge 0.34$)."

### Discrepancy 3: Resolution vs. Limitation Scope in Reviewer Responses
- **Location:** `REVIEWER_RESPONSE_MATRIX.md` Overview and R6.5–R6.7 response
- **Reported Claim:** *"All feedback from all 8 peer reviewers was addressed / resolved."*
- **Actual Status:**
  Small clinical cohort sizes ($N=303$–$768$), state-vector CPU simulation vs. real QPU hardware, lack of GBDT baselines, and prospective clinical deployment are **permanent study limitations**, not resolved problems.
- **Discrepancy Severity:** **MEDIUM (Reviewer Diplomacy)**. Claude must ensure the response matrix explicitly separates resolved empirical requests from acknowledged study boundaries.
