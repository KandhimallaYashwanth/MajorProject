# INSTRUCTION MANUAL & MASTER BRIEFING FOR CLAUDE
## IEEE AIHC 2027 Camera-Ready Revision Package — Paper #516
### "Adaptive Hybrid Quantum-Classical Neural Network for Medical Diagnosis"

---

## 1. Package Overview & Contents

Welcome, Claude. This package is an authoritative, forensically audited, and completely verified evidence bundle prepared to enable you to perform the final revision of IEEE Paper #516 for the **IEEE Artificial Intelligence in Healthcare Conference (AIHC 2027)**, Track 2: ML in Clinical Decision Support and Predictive Analytics.

### Directory Structure:
```
CLAUDE_PAPER_REVISION_PACKAGE/
├── manuscript/
│   ├── latest_camera_ready.pdf        # Current compiled camera-ready PDF (6 pages)
│   ├── latest_camera_ready.tex        # Current camera-ready LaTeX source (paper/paper.tex)
│   ├── original_main.tex              # Original accepted LaTeX submission (for diff reference)
│   └── Architecture.png               # High-resolution system architecture figure
├── reviewer_materials/
│   ├── official_reviewer_comments.pdf # CMT official review PDF (13 pages, 8 reviewers)
│   ├── REVIEWER_RESPONSE_MATRIX.md    # 8-pillar thematic response & reviewer-by-reviewer matrix
│   ├── CAMERA_READY_AUDIT.md          # Comprehensive workspace audit & baseline comparison
│   ├── Antigravity_Audit.txt          # Antigravity forensic scientific audit report
│   └── CHANGELOG.md                   # Chronological ledger of all modifications
├── experimental_results/
│   ├── repeated_cv_summary.csv        # 12 summary rows across 4 models & 3 datasets (25 folds)
│   ├── repeated_cv_raw_folds.csv      # 300 raw fold records (5 repeats x 5 folds x 4 models x 3 datasets)
│   ├── repeated_cv_statistical_tests.csv          # 27 Nadeau-Bengio corrected resampled t-tests
│   ├── repeated_cv_statistical_tests_multitest.csv# Benjamini-Hochberg FDR multi-comparison tests
│   ├── m3_per_path_accuracy.csv       # Routing allocation and preliminary vs final accuracy
│   ├── ablation_benchmarks.csv        # 9 summary rows for 3 ablations across 3 datasets
│   ├── ablation_raw_folds.csv         # 225 raw ablation fold runs (25 folds x 3 ablations x 3 datasets)
│   ├── simulator_timing_benchmarks.csv# CPU state-vector simulation latencies (N=1 and N=32)
│   ├── shap_fold_stability.csv        # Kernel SHAP fold stability metrics (Spearman rho across folds)
│   ├── calibration_and_detailed_holdout.csv      # Holdout confusion matrices, sensitivity, specificity, FN
│   ├── confidence_calibration_metrics.csv        # Expected Calibration Error (ECE) and Brier scores
│   ├── m4_workload_metrics.csv        # Adaptive classical control (M4) refinement invocations
│   └── initial_submission_baseline/   # Older 5-fold baseline CSVs preserved for audit provenance
├── experiment_scripts/
│   ├── build_paper_results.py         # Master pipeline orchestrating all benchmark executions
│   ├── exp_repeated_cv.py             # 5x5 repeated cross-validation engine (seeds 42, 101, 202, 303, 404)
│   ├── exp_ablations.py               # 25-fold ablation suite (Path 0, Bottleneck, Matched M4)
│   ├── exp_m3_accuracy.py             # Per-path routing accuracy evaluation script
│   ├── exp_m4_workload.py             # Classical control workload benchmark
│   ├── exp_simulation_timing.py       # PennyLane default.qubit timing benchmarks
│   ├── exp_shap_stability.py          # Kernel SHAP fold stability evaluator
│   ├── exp_calibration_holdout.py     # Holdout calibration, ECE, Brier, sensitivity/specificity
│   ├── extract_circuit_specs.py       # Quantum circuit gate count and depth analysis
│   ├── configs/                       # Hyperparameter YAML configuration files
│   ├── preprocessing/                 # Data loading, z-score scaling, median imputation scripts
│   └── splits/                        # Deterministic train/dev/test split index files
├── reproducibility/
│   ├── README_REPRODUCIBILITY.md      # Reproduction instructions, seed documentation, hardware specs
│   ├── requirements.txt               # Exact Python dependencies (torch, pennylane, shap, etc.)
│   └── dataset_checksums.csv          # SHA-256 integrity hashes for raw clinical datasets
├── reports/
│   ├── FILE_PROVENANCE_MANIFEST.md    # Metadata, size, timestamps, and row counts of all CSVs
│   ├── NUMERICAL_VERIFICATION_REPORT.md# Complete mathematical verification of all empirical claims
│   ├── MANUSCRIPT_DISCREPANCY_REPORT.md# Exact discrepancy mapping between LaTeX and CSV files
│   ├── REVIEWER_EVIDENCE_MAPPING.md   # Exhaustive mapping of all 8 reviewers to evidence/limitations
│   └── SCIENTIFIC_OVERCLAIM_AUDIT.md  # Forensic audit of overclaims with conservative replacements
└── SHA256_MANIFEST.txt                # Cryptographic checksums of all package contents
```

---

## 2. Project History & Revision Context

1. **Initial Submission:** Paper #516 was submitted to IEEE AIHC 2027 reporting preliminary results from a single 5-fold cross-validation run comparing Adaptive HQNN against Fixed HQNN and classical baselines.
2. **Reviewer Evaluation:** Eight reviewers (#2, #4, #6, #7, #8, #9, #10, #11) evaluated the submission. While finding the dynamic routing concept novel and intriguing, they raised substantial concerns regarding:
   - Low statistical power of 5-fold Wilcoxon testing ($p \ge 0.0625$ floor).
   - Lack of quantum-isolating ablations (classical 4-D bottleneck and parameter-matched classical controls).
   - Weakness of layer counts as a computational workload proxy.
   - Ambiguity in routing threshold selection and potential data leakage.
   - Thin XAI evaluation lacking fold stability analysis.
   - Outdated references (pre-2022) and IEEE formatting issues.
3. **Camera-Ready Re-Engineering:** The authors expanded the empirical framework to **300 newly completed model runs** (5 repeats x 5 folds x 4 models x 3 datasets), implemented the Nadeau–Bengio resampled $t$-test, ran a 25-fold ablation suite, conducted CPU timing benchmarks, and audited SHAP fold stability.
4. **Antigravity Scientific Audit:** A forensic audit revealed critical nuances:
   - Escalation on Pima Diabetes does **not** rescue preliminary errors; escalated instances dropped in accuracy.
   - 98x–155x speedups represent classical CPU simulation artifacts, not physical QPU speedups.
   - Statistical non-significance ($p_{NB} \ge 0.05$) does not prove mathematical equivalence or predictive parity.
   - Four reviewer points (small cohorts, simulator vs. QPU hardware, lack of GBDTs, non-significant MLP comparison) cannot be eliminated experimentally and must be explicitly embraced as study limitations.
5. **Claude's Mission:** Execute the final, conservative refinement of `paper/paper.tex` and `REVIEWER_RESPONSE_MATRIX.md` to ensure complete scientific integrity, 100% numerical concordance with raw CSVs, full reviewer alignment, and strict adherence to the 6-page IEEE conference page limit.

---

## 3. Absolute Ground Rules for Revision

Claude, you must adhere strictly to the following five scientific ground rules:

1. **DO NOT INVENT DATA OR ALTER NUMBERS:** Every number in the text, tables, and response documents must match the verified CSV values reported in `reports/NUMERICAL_VERIFICATION_REPORT.md`.
2. **DO NOT CLAIM QUANTUM ADVANTAGE OR SUPREMACY:** Classical MLPs achieve competitive baseline F1 scores (WDBC: 97.35%, Heart: 79.41%, Diabetes: 68.61%). 4-qubit quantum circuits are classically simulable ($2^4=16$ states). Explicitly disclaim quantum advantage.
3. **DO NOT CONFLATE SIMULATOR LATENCY WITH PHYSICAL QPU SPEEDUP:** The 98x–155x latency reduction in Path 0 reflects matrix multiplication overhead in PennyLane's `default.qubit` CPU simulator. It does not reflect physical quantum gate execution times or cryogenic power reduction. Always qualify timing as CPU simulation benchmarks.
4. **DO NOT CLAIM AMBIGUOUS CASES ARE UNIVERSALLY IMPROVED:** On Pima Diabetes 5-fold CV, escalated samples dropped from 56.60% to 54.72% on Path 1 and 58.33% to 52.78% on Path 2 (`m3_per_path_accuracy.csv`). Frame routing as uncertainty-guided global resource allocation, not an infallible error-correction mechanism.
5. **DO NOT EQUATE NON-SIGNIFICANT DIFFERENCES WITH EQUIVALENCE ("PARITY"):** Failing to reject the null hypothesis ($p_{NB} \ge 0.05$) under the Nadeau–Bengio test indicates that no difference was statistically detected. It does not prove mathematical parity. Note that Adaptive HQNN has slightly lower F1 point estimates than Fixed HQNN on WDBC (-0.53%) and Diabetes (-0.38%).

---

## 4. Authoritative Verified Experimental Values (Citable)

Use **only** these verified figures in your revisions:

### Table I: Repeated Cross-Validation (25 Folds) & Holdout Performance
| Dataset | Model | Params | CV Accuracy (%) | CV F1-Score (%) | CV ROC-AUC | Test Acc (%) | Test F1 (%) | Test AUC |
|:---|:---|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
| **WDBC** | M1 (Classical MLP) | 513 | 96.91 ± 2.05 | 97.35 ± 1.76 | 0.9904 ± 0.0125 | 97.37 | 97.87 | 0.9953 |
| | M2 (Fixed HQNN) | 529 | 96.94 ± 1.83 | 97.51 ± 1.54 | 0.9956 ± 0.0055 | 96.49 | 97.10 | 0.9926 |
| | **M3 (Adaptive HQNN)** | **532** | **96.55 ± 2.08** | **96.98 ± 2.10** | **0.9898 ± 0.0145** | **96.49** | **97.10** | **0.9926** |
| | M4 (Adaptive Class.) | 691 | 95.89 ± 2.22 | 96.11 ± 2.07 | 0.9904 ± 0.0125 | 95.61 | 96.35 | 0.9906 |
| **Heart** | M1 (Classical MLP) | 241 | 79.46 ± 4.90 | 79.41 ± 5.08 | 0.8842 ± 0.0381 | 80.33 | 82.54 | 0.9197 |
| | M2 (Fixed HQNN) | 257 | 78.02 ± 4.88 | 78.36 ± 5.10 | 0.8656 ± 0.0392 | 80.33 | 81.97 | 0.9109 |
| | **M3 (Adaptive HQNN)** | **260** | **78.35 ± 4.67** | **78.51 ± 4.32** | **0.8745 ± 0.0387** | **81.97** | **83.87** | **0.9163** |
| | M4 (Adaptive Class.) | 419 | 75.21 ± 5.56 | 75.38 ± 5.49 | 0.8524 ± 0.0401 | 75.41 | 77.42 | 0.8797 |
| **Diabetes**| M1 (Classical MLP) | 161 | 74.96 ± 4.14 | 68.61 ± 4.41 | 0.8174 ± 0.0366 | 75.32 | 68.09 | 0.8351 |
| | M2 (Fixed HQNN) | 177 | 74.45 ± 4.29 | 68.45 ± 4.80 | 0.8289 ± 0.0382 | 75.97 | 70.00 | 0.8327 |
| | **M3 (Adaptive HQNN)** | **180** | **74.42 ± 4.11** | **68.07 ± 5.07** | **0.8378 ± 0.0379** | **75.97** | **70.00** | **0.8327** |
| | M4 (Adaptive Class.) | 339 | 73.08 ± 4.19 | 65.25 ± 5.17 | 0.8019 ± 0.0387 | 74.03 | 66.67 | 0.8105 |

### Table II: Adaptive Routing Distribution & Workload Savings
- **WDBC:** Path 0 = $98.4 \pm 0.8\%$, Path 1 = $1.4 \pm 0.8\%$, Path 2 = $0.2 \pm 0.3\%$. Avg Q-layers: **$0.032 \pm 0.012$** (**98.4% reduction** vs. 2.000). M4 refinement blocks: 0.029.
- **Heart:** Path 0 = $79.8 \pm 4.9\%$, Path 1 = $14.8 \pm 4.7\%$, Path 2 = $5.4 \pm 3.1\%$. Avg Q-layers: **$0.305 \pm 0.078$** (**84.8% reduction** vs. 2.000). M4 refinement blocks: 0.298.
- **Diabetes:** Path 0 = $67.4 \pm 6.1\%$, Path 1 = $18.2 \pm 5.4\%$, Path 2 = $14.4 \pm 5.2\%$. Avg Q-layers: **$0.490 \pm 0.114$** (**75.5% reduction** vs. 2.000). M4 refinement blocks: 0.428.

### Table III: 25-Fold Ablation Suite Performance
- **WDBC:** M3 F1: $96.98 \pm 2.10\%$, Path-0-only: $95.14 \pm 2.14\%$, Classical 4-D Bottleneck: $96.41 \pm 2.42\%$, Parameter-Matched M4: $95.81 \pm 1.93\%$.
- **Heart:** M3 F1: $78.51 \pm 4.32\%$, Path-0-only: $75.15 \pm 4.83\%$, Classical 4-D Bottleneck: $72.79 \pm 4.02\%$ (**-5.72% drop**), Parameter-Matched M4: $74.21 \pm 5.13\%$.
- **Diabetes:** M3 F1: $68.07 \pm 5.07\%$, Path-0-only: $66.36 \pm 4.93\%$, Classical 4-D Bottleneck: $63.22 \pm 6.81\%$ (**-4.85% drop**), Parameter-Matched M4: $66.25 \pm 5.20\%$.

### Table IV: Pairwise Nadeau–Bengio Resampled $t$-Tests ($J=25, df=24$)
- **WDBC (M3 vs M2):** Accuracy diff $-0.396\%$ ($t=-0.955, p=0.3491$); F1 diff $-0.534\%$ ($t=-0.970, p=0.3416$); AUC diff $-0.0058$ ($t=-1.209, p=0.2386$).
- **Heart (M3 vs M2):** Accuracy diff $+0.330\%$ ($t=+0.157, p=0.8769$); F1 diff $+0.154\%$ ($t=+0.072, p=0.9435$); AUC diff $+0.0089$ ($t=+0.532, p=0.6000$).
- **Diabetes (M3 vs M2):** Accuracy diff $-0.033\%$ ($t=-0.021, p=0.9838$); F1 diff $-0.384\%$ ($t=-0.196, p=0.8461$); AUC diff $+0.0089$ ($t=+0.903, p=0.3756$).
- **Diabetes (M3 vs M4):** ROC-AUC diff $+0.0359$ ($t=+2.405$, unadjusted $p=0.0243$, Benjamini–Hochberg FDR adjusted $p_{FDR} = 0.6561$ $\to$ **exploratory**).

### Table V: CPU Simulation Latencies & Quantum Specifications
- Single sample ($N=1$): Path 0: $0.151-0.232$ ms; Path 1: $11.45-12.01$ ms; Path 2: $22.69-24.21$ ms. Speedup: $98\times$ to $155\times$ on CPU simulation.
- Circuit specifications: 4 qubits; Path 1 has 12 gates ($R_y, R_z$, CNOT), depth 4; Path 2 has 24 gates, depth 8.

---

## 5. Known Manuscript Discrepancies to Fix

See `reports/MANUSCRIPT_DISCREPANCY_REPORT.md` for full context:

1. **Ablation Parameter Counts (Table III):**
   `paper/paper.tex` currently prints M3 params as 659, 387, 307. These numbers include auxiliary training heads. The standard inference parameter counts in Table I are 532, 260, 180. Add a footnote or clarify that ablation parameter counts include auxiliary projection tensors.
2. **Ambiguous Case Routing Claim (Section IV-B & V):**
   Remove any claim that dynamic routing universally 'rescues' or 'recovers' errors on escalated samples. Explicitly disclose that on Diabetes, escalated samples exhibited modest accuracy declines (-1.89% on Path 1, -5.56% on Path 2) and that routing acts as global uncertainty-guided resource regulation.
3. **FDR Adjustment Citation & Qualification (Section IV-C):**
   Ensure the Diabetes M3 vs M4 ROC-AUC difference ($p_{NB} = 0.0243$) is explicitly denoted with an exploratory qualifier and references the 27-test Benjamini–Hochberg FDR value ($p_{FDR} = 0.6561$).
4. **Predictive Parity Terminology:**
   Replace assertions of 'predictive parity' with 'statistically non-significant differences under the Nadeau–Bengio test ($p_{NB} \ge 0.34$)'.

---

## 6. How to Address Reviewer Concerns

Consult `reports/REVIEWER_EVIDENCE_MAPPING.md`. Use this strategy:

- **Defend with Verified Experiments:**
  - Repeated CV statistical validity: Cite 300 runs, $J=25, df=24$, and Nadeau–Bengio overlap correction factor 0.29.
  - Quantum utility in compact space: Cite the 4-D classical bottleneck ablation (-5.72% Heart, -4.85% Diabetes).
  - Workload validation: Cite gate counts (12-24 gates) and CPU simulation timings (Table V).
  - XAI robustness: Cite Kernel SHAP fold stability Spearman correlations ($\rho = 0.757$ on Diabetes).
  - Holdout evaluation: Cite frozen 20% test metrics (Table I & Table IV).
- **Concede Honestly as Limitations:**
  - Small cohort sizes ($N=303-768$): Concede in Section V-A as standard benchmark constraints.
  - Simulation vs. QPU hardware: Concede in Section V-B that real device noise and physical runtimes require physical QPU hardware.
  - Absence of GBDTs: Concede in Section V-C that tree ensembles were not evaluated.
  - Non-significant differences vs Classical MLP: Acknowledge that classical MLPs achieve competitive baseline performance on these small tabular datasets.

---

## 7. Manuscript Structure & 6-Page IEEE Limit Management

The IEEE AIHC 2027 conference enforces a **strict 6-page maximum** including references. The current `camera_ready/CAMERA_READY_REVISED.pdf` renders at exactly 6 pages.

### Space Management Recommendations:
- Keep `IEEEtran` formatting pristine (`\documentclass[conference]{IEEEtran}`).
- Do not adjust margins or reduce font size below template standards.
- Table formatting: Tables I, II, III, IV, and V are already compact. If space is tight, combine Table III (Ablations) or adjust table column spacing (`\setlength{\tabcolsep}{3pt}`).
- Citations: All 28 references are ordered numerically. Do not add superfluous citations that create orphan reference lines on page 7.

---

## 8. Reviewer Response Matrix Refinement Guidelines

In `REVIEWER_RESPONSE_MATRIX.md`:
1. **Tone:** Humble, rigorous, scientifically objective, and appreciative.
2. **Avoid "Fully Resolved":** For Reviewers #2, #7, #8, #9, and #11 on dataset scale and hardware noise, write:
   *'We have addressed the empirical and methodological aspects of this critique by providing... while formally acknowledging the intrinsic boundaries of retrospective tabular cohorts and state-vector simulation as study limitations in Section V.'*
3. **Maintain Comment Traceability:** Do not alter reviewer comment IDs (R2.1, R2.2, etc.). Keep every reviewer's points clearly demarcated.

---

## 9. Claude's Step-by-Step Revision Checklist

- [ ] **Step 1:** Review `reports/SCIENTIFIC_OVERCLAIM_AUDIT.md` and replace any overclaimed phrasing in `paper/paper.tex`.
- [ ] **Step 2:** Review `reports/MANUSCRIPT_DISCREPANCY_REPORT.md` and ensure all table entries match verified CSVs.
- [ ] **Step 3:** Verify that the 27-test Benjamini–Hochberg FDR correction ($p_{FDR} = 0.6561$) for Diabetes M3 vs M4 ROC-AUC is clearly labeled exploratory.
- [ ] **Step 4:** Verify that per-path accuracy on Diabetes is accurately characterized as global uncertainty management rather than universal error recovery.
- [ ] **Step 5:** Compile `paper/paper.tex` using `pdflatex` or `latexmk`.
- [ ] **Step 6:** Inspect the compiled PDF to ensure page count does not exceed 6 pages (including references).
- [ ] **Step 7:** Synchronize `REVIEWER_RESPONSE_MATRIX.md` with the finalized text and honest limitation framing.
- [ ] **Step 8:** Verify that all file hashes match `SHA256_MANIFEST.txt`.

---
*End of Master Briefing for Claude.*
