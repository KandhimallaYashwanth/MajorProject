# REVIEWER-TO-EVIDENCE MAPPING REPORT: IEEE AIHC 2027 (PAPER #516)

**Paper Title:** Adaptive Hybrid Quantum-Classical Neural Network for Early Medical Disease Diagnosis Using Explainable AI  
**Paper ID:** 516 | **Track:** 2 (ML in Clinical Decision Support and Predictive Analytics)  
**Conference:** IEEE Artificial Intelligence in Healthcare Conference (AIHC 2027)  
**Document Purpose:** Exhaustive traceability mapping linking every reviewer comment across all 8 official conference reviewers (#2, #4, #6, #7, #8, #9, #10, #11) to experimental evidence, manuscript sections, or explicitly conceded study limitations.

---

## 1. Overview of Reviewer Feedback Taxonomy

Each reviewer critique has been categorized into one of four methodological resolution states:
- **Category A: Addressed Through Experiments** (New empirical computations, cross-validation runs, ablation suites, or statistical tests).
- **Category B: Addressed Through Clarification / Re-framing** (Architectural precision, loss function specification, parameter descriptions, reference updates, or formatting).
- **Category C: Partially Addressed** (Empirical progress made within computational/simulator constraints, but complete resolution bounded by project scope).
- **Category D: Cannot Be Addressed Within Current Study / Must Remain Honest Limitations** (Intrinsic constraints such as small clinical cohorts, physical QPU hardware access, or broad model families that must be transparently conceded).

---

## 2. Reviewer-by-Reviewer Evidence Mapping

### Reviewer #2 (Score: 2 - Weak Reject / Borderline)

| ID | Reviewer Concern Summary | Resolution Category | Supporting Evidence & Source File | Manuscript Section & Action |
|:---|:---|:---:|:---|:---|
| **R2.1** | Lack of demonstrated quantum benefit; Fixed HQNN (M1/M2) outperforms M3; missing essential ablations (Path-0-only, classical 4-D bottleneck, parameter-matched classical control). | **Category A** | blation_benchmarks.csv, blation_raw_folds.csv (25 folds per ablation; Path-0-only, Classical 4-D Bottleneck, and Parameter-Matched M4). | **Section IV-B, Table III**: Demonstrates that replacing the 4-qubit VQC with a classical 4-D dense bottleneck degrades F1 by -5.72% on Heart and -4.85% on Diabetes, isolating the utility of the circuit in compact 4-D latent space. |
| **R2.2** | M4 baseline is under-analyzed; architecture and parameter counts are omitted; workload statistics are absent. | **Category A** | m4_workload_metrics.csv (reports M4 refinement block invocations: 0.029 on WDBC, 0.298 on Heart, 0.428 on Diabetes); 
epeated_cv_summary.csv (M4 parameters: 691 WDBC, 419 Heart, 339 Diabetes). | **Section III-E, Table I, Table II**: Added full mathematical formulation of M4, exact parameter counts, and workload comparisons alongside M3. |
| **R2.3** | Statistical testing with 5 folds is invalid (two-sided Wilcoxon minimum p-value is 0.0625); need repeated cross-validation with confidence intervals. | **Category A** | 
epeated_cv_raw_folds.csv (300 runs across 5 repeats x 5 folds x 4 models x 3 datasets); 
epeated_cv_statistical_tests.csv (Nadeau–Bengio resampled t-test, J=25, df=24, variance correction factor 0.29); 
epeated_cv_statistical_tests_multitest.csv (Benjamini–Hochberg FDR control). | **Section IV-C, Table IV**: Replaced 5-fold Wilcoxon with Nadeau–Bengio corrected t-tests. Reported empirical 95% CIs with explicit caveat regarding CV fold overlap. |
| **R2.4** | Holdout results are over-interpreted given small sample sizes; Table I omitted test metrics; holdout model protocol was unclear. | **Category A & B** | calibration_and_detailed_holdout.csv (reports test performance across 114 WDBC, 61 Heart, 154 Diabetes samples, including confusion matrices, sensitivity, specificity, and false negatives). | **Section III-C, Table I**: Formally documented holdout selection protocol (best inner-CV checkpoint evaluated once on frozen 20% holdout). Added holdout test metrics directly into Table I. |
| **R2.5** | Quantum layer execution is a weak proxy; does not measure latency, energy, or hardware noise; Path 1 vs. 2 differs only slightly. | **Category C & D** | simulator_timing_benchmarks.csv (benchmarks single-instance latency across Path 0, 1, 2 using PennyLane default.qubit); Gate counts tabulated (12 gates/depth 4 for 1 layer, 24 gates/depth 8 for 2 layers). | **Section IV-E, Table V, Section V (Limitations)**: Added gate counts and CPU simulation latencies. Conceded in Section V-B that physical execution time, cryogenic energy, and real device noise cannot be evaluated on simulators and remain limitations. |
| **R2.6** | Under-specified training details; 4-D vs. 16-D feature capacity imbalance; threshold objective grid unstated; uncalibrated confidence. | **Category A & B** | confidence_calibration_metrics.csv (ECE and Brier score reported for all models); Preprocessing scripts model.py and exp_m3_accuracy.py specify exact grid search objective: argmax(F1 - lambda * avg_layers). | **Section III-C, Section IV-D, Table IV**: Added training hyperparameters (Adam, lr=0.01, cosine annealing, batch size 32). Fully specified threshold grid search objective and reported ECE/Brier scores. |
| **R2.7** | Limited baselines (only 1 MLP, no tree ensembles like XGBoost/LightGBM); fixed hyperparameters; small cohorts. | **Category C & D** | Added Parameter-Matched M4 and Classical Bottleneck to isolate circuit effects. | **Section V (Limitations)**: Explicitly acknowledged as a study limitation that tree-based gradient boosting models (XGBoost/LightGBM) were not benchmarked, and small clinical sample sizes (N=303-768) constrain generalizability. |
| **R2.8** | XAI component is thin; single paragraph without figures; sample counts unstated; features are classical. | **Category A & B** | shap_fold_stability.csv (evaluated Kernel SHAP across 5 outer folds; background N_bg=50, eval N_eval=50; computed pairwise Spearman rank correlations rho). | **Section IV-D, Table IV**: Detailed Kernel SHAP methodology, background/eval sample counts, and fold stability metrics (rho = 0.757 on Diabetes, 0.391 on Heart, 0.361 on WDBC). Clarified why classical input attribution explains hybrid end-to-end decisions. |
| **R2.9** | Outdated references; missing early-exit literature; undefined R(theta) and |psi_0>. | **Category B** | Added 2022–2026 citations in QML and dynamic inference; mathematically defined angle state preparation |psi(x)> and parameterized ansatz. | **Section II, Section III-B, References**: Added recent citations ([12]-[15], [20]-[23]). Defined angle embedding and variational unitary operators explicitly. |

---

### Reviewer #4 (Score: 3 - Weak Accept)

| ID | Reviewer Concern Summary | Resolution Category | Supporting Evidence & Source File | Manuscript Section & Action |
|:---|:---|:---:|:---|:---|
| **R4.1** | Citations must follow IEEE numeric appearance order. | **Category B** | paper/paper.tex bibitem ordering. | **References**: Re-indexed all 28 citations sequentially in exact order of appearance ([1] to [28]). |
| **R4.2** | Four algorithms are mentioned but not comparatively discussed. | **Category B** | 
epeated_cv_summary.csv, 
epeated_cv_statistical_tests.csv. | **Section IV-A, Section IV-C**: Added comparative performance analysis between M1 (Classical MLP), M2 (Fixed HQNN), M3 (Adaptive HQNN), and M4 (Adaptive Classical Control). |
| **R4.3** | XAI analysis needs detail and visual presentation. | **Category A & B** | shap_fold_stability.csv, 
otebooks/06_xai_and_shap_analysis.ipynb. | **Section IV-D**: Formulated detailed XAI analysis, tabulated fold stability metrics, and analyzed primary clinical biomarkers across folds. |
| **R4.4** | References are outdated (latest 2021); need 2022–2026 work. | **Category B** | Updated bibliography in paper/paper.tex. | **References**: Incorporated recent publications from 2022–2026, including Huang et al. (Science 2022), Cerezo et al. (Nat. Rev. Phys. 2021), and Xin et al. (TNNLS 2024). |
| **R4.5 & R4.6** | Define all abbreviations at first use (QL, CV, MLP, HQNN, VQC). | **Category B** | paper/paper.tex text audit. | **Abstract & Section I**: Defined all acronyms at first mention (Variational Quantum Circuit, Hybrid Quantum-Classical Neural Network, Cross-Validation, Multilayer Perceptron, Expected Calibration Error). |

---

### Reviewer #6 (Score: 2 - Weak Reject)

| ID | Reviewer Concern Summary | Resolution Category | Supporting Evidence & Source File | Manuscript Section & Action |
|:---|:---|:---:|:---|:---|
| **R6.1 & R6.2** | Clarify originality vs. classical conditional computation; validate workload beyond layer counts. | **Category B & C** | simulator_timing_benchmarks.csv; circuit gate counts (12 gates/depth 4 vs 24 gates/depth 8). | **Section I, Section III-B, Section IV-E**: Distinguished adaptive quantum routing from classical early-exits (avoiding quantum state preparation and cryogenic circuit execution on classical-confident samples). Added simulator timing and gate counts. |
| **R6.3 & R6.4** | Trade-off analysis; explain when savings justify F1 reduction; threshold optimization details. | **Category A & B** | 
epeated_cv_summary.csv, exp_m3_accuracy.py. | **Section IV-A, Section IV-B, Section V**: Articulated clinical trade-off analysis: when quantum resources are expensive or queued, 75.5%–98.4% layer reduction justifies small non-significant F1 deltas (-0.38% to -0.53%). Detailed inner-CV threshold optimization. |
| **R6.5–R6.7** | Statistical power of 5-fold Wilcoxon; M4 comparison; SHAP ranking stability across folds. | **Category A** | 
epeated_cv_statistical_tests.csv, shap_fold_stability.csv. | **Section IV-C, Section IV-D**: Expanded to 25 folds with Nadeau–Bengio resampled t-test; benchmarked M4 across all folds; audited SHAP fold stability using Spearman rank correlation. |

---

### Reviewer #7 (Score: 3 - Weak Accept)

| ID | Reviewer Concern Summary | Resolution Category | Supporting Evidence & Source File | Manuscript Section & Action |
|:---|:---|:---:|:---|:---|
| **R7.1–R7.5** | Methodological transparency, training details, simulation vs. physical hardware, dataset scale limits. | **Category B & D** | uild_paper_results.py, exp_repeated_cv.py. | **Section III-C, Section V (Limitations)**: Disclosed full training pipeline (loss functions, optimization schedules). Conceded that simulation on PennyLane does not capture real hardware noise, and dataset sizes (N=303-768) represent a limitation. |

---

### Reviewer #8 (Score: 2 - Weak Reject)

| ID | Reviewer Concern Summary | Resolution Category | Supporting Evidence & Source File | Manuscript Section & Action |
|:---|:---|:---:|:---|:---|
| **R8.1 & R8.2** | Small clinical cohorts; report sensitivity, specificity, false-negative rates, and probability calibration. | **Category A & D** | calibration_and_detailed_holdout.csv, confidence_calibration_metrics.csv. | **Section IV-C, Table I, Section V (Limitations)**: Added sensitivity, specificity, false negatives, ECE, and Brier scores on the holdout test set. Conceded small sample sizes as an inherent clinical dataset limitation. |

---

### Reviewer #9 (Score: 2 - Weak Reject)

| ID | Reviewer Concern Summary | Resolution Category | Supporting Evidence & Source File | Manuscript Section & Action |
|:---|:---|:---:|:---|:---|
| **R9.1–R9.3** | Classical bottleneck risk (overconfident early-exit errors); clinical degradation danger; simulation limits. | **Category A & B & D** | blation_benchmarks.csv (classical 4-D bottleneck drops F1 by up to -5.72%); m3_per_path_accuracy.csv (per-path preliminary vs final accuracy). | **Section IV-B, Section IV-C, Section V**: Evaluated classical bottleneck degradation. Documented routing behavior on ambiguous cases, showing mixed results on Diabetes. Conceded simulator vs hardware limitation. |

---

### Reviewer #10 (Score: 3 - Weak Accept)

| ID | Reviewer Concern Summary | Resolution Category | Supporting Evidence & Source File | Manuscript Section & Action |
|:---|:---|:---:|:---|:---|
| **R10.1–R10.6** | Router runtime overhead; trade-off criteria; repeated random seeds; gate counts and circuit depth. | **Category A & B** | simulator_timing_benchmarks.csv (Path 0 takes 0.052-0.059 ms, proving router overhead is negligible compared to quantum execution); Seeds [42, 101, 202, 303, 404] across 5 repeats. | **Section III-B, Section IV-E, Table V**: Tabulated circuit depth and gate counts. Demonstrated that router overhead is negligible (<0.06 ms) relative to quantum simulation. Verified multi-seed evaluation. |

---

### Reviewer #11 (Score: 2 - Weak Reject)

| ID | Reviewer Concern Summary | Resolution Category | Supporting Evidence & Source File | Manuscript Section & Action |
|:---|:---|:---:|:---|:---|
| **R11.1–R11.7** | Technical distinctiveness; workload proxy; parameter counts; class imbalance handling. | **Category A & B** | Model parameter counts tabulated (M1: 161-513, M2: 177-529, M3: 180-532, M4: 339-691); exp_repeated_cv.py uses stratified folds. | **Section I, Section III-D, Section IV-A**: Clarified technical contribution, detailed parameter counts for all models, and confirmed stratified cross-validation preserving class ratios. |

---

## 3. Summary of Conceded Study Limitations (Category D)

The following 4 core items **cannot** be fully resolved within the current scope and are explicitly conceded in **Section V (Limitations)**:
1. **Dataset Scale:** Benchmarks are standard clinical datasets of modest size (=303$ to $). Findings serve as proof-of-concept and require larger prospective clinical cohorts.
2. **Simulation vs. Physical QPU Hardware:** All experiments were conducted using PennyLane state-vector simulation (default.qubit). Physical NISQ devices exhibit gate infidelities, readout noise, and qubit decoherence.
3. **Absence of Tree-Based Ensembles:** Comparisons focused on neural baselines (MLP, fixed HQNN, adaptive classical control). Highly tuned gradient-boosted decision trees (GBDT) were not benchmarked.
4. **Simulator Timing vs. Hardware Runtime:** Latency reductions reflect CPU state-vector simulation speedups ((2^Q)$ overhead), not cryogenic QPU pulse durations.
