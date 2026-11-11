# SCIENTIFIC OVERCLAIM AUDIT & CONSERVATIVE REVISION REPORT: IEEE AIHC 2027 (PAPER #516)

**Paper Title:** Adaptive Hybrid Quantum-Classical Neural Network for Early Medical Disease Diagnosis Using Explainable AI  
**Paper ID:** 516 | **Track:** 2 (ML in Clinical Decision Support and Predictive Analytics)  
**Conference:** IEEE Artificial Intelligence in Healthcare Conference (AIHC 2027)  
**Document Purpose:** Forensic scientific audit identifying unsupported, causal, exaggerated, or potentially misleading scientific claims across the manuscript (`paper/paper.tex`) and reviewer response documents, providing actionable conservative replacement language.

---

## 1. Executive Scientific Audit Summary

This audit examined eight sensitive scientific assertions in the research text. In scientific publishing—particularly at the intersection of Quantum Computing and Healthcare Informatics—claims must not outpace experimental boundaries. While the experimental execution across 300 repeated CV folds is empirically rigorous, several phrases in the manuscript and synthesis risk over-interpreting the data.

### Summary of Audit Verdicts:
1. **Quantum Advantage / Supremacy:** *Partially Supported / Overstated in Discussion.* Classical MLPs achieve competitive baseline F1 scores (e.g., 97.35% on WDBC vs 96.98% for M3). Must be strictly disclaimed.
2. **Genuine Quantum Expressivity vs. Low-Dimensional Collapse:** *Partially Supported.* The classical 4-D bottleneck drops F1 (-5.72% on Heart, -4.85% on Diabetes), but a 4-qubit circuit has only 16 states and is classically simulable. It shows the VQC avoids bottleneck collapse in this specific architecture, not general quantum expressivity over wider classical models.
3. **Predictive Parity vs. Non-Significance:** *Unsupported / Fallacious.* Failure to reject the null hypothesis ($p_{NB} \ge 0.05$) does not prove mathematical equivalence or parity. F1 point estimates are slightly lower for M3 than M2 on WDBC (-0.53%) and Diabetes (-0.38%).
4. **Ambiguous Case Recovery:** *Empirically Contradicted on Diabetes.* While routing helps globally, on Pima Diabetes 5-fold CV, escalated samples actually dropped in accuracy (Path 1: 56.60% prelim -> 54.72% final; Path 2: 58.33% prelim -> 52.78% final). Escalation does not universally rescue errors.
5. **Efficiency & Speedup Metrics:** *Supported with Caveats.* 98x to 155x speedup is strictly CPU state-vector simulation latency (`default.qubit`), not physical quantum hardware runtime or cryogenic energy savings.
6. **Clinical Readiness:** *Unsupported.* Small retrospective cohorts ($N=303-768$) cannot establish clinical deployment readiness or diagnostic efficacy in real practice.
7. **Completeness of Reviewer Resolution:** *Overstated in Synthesis.* Methodological requests were resolved, but fundamental study boundaries (cohort scale, simulation vs hardware, lack of tree baselines) must remain honest limitations.
8. **Exploratory Multi-Testing Status:** *Supported.* Diabetes M3 vs M4 ROC-AUC difference (+0.0359, unadjusted $p=0.0243$) becomes non-significant under Benjamini–Hochberg FDR ($p_{FDR} = 0.6561$) and must be labeled exploratory.

---

## 2. Detailed Item-by-Item Overclaim Audit

### Item 1: Quantum Advantage & Supremacy Claims

- **Locations:**
  - `paper/paper.tex`, Abstract, line 29: `...without asserting quantum supremacy.`
  - `paper/paper.tex`, Section V-B, line 382: `This study does not demonstrate quantum advantage, as classical MLPs achieve competitive baseline performance.`
  - Synthesis / Response Matrix: Multiple references to quantum utility.
- **Current Text / Context:**
  In several places, the text correctly includes disclaimers, but introductory remarks (e.g., lines 39-40) cite quantum Hilbert spaces and expressivity without contextualizing that a 4-qubit circuit is fully classically simulable.
- **Why It Is An Overclaim / Risk:**
  A 4-qubit quantum state vector has dimension $2^4 = 16$. Evaluating a 4-qubit circuit involves multiplying $16 \times 16$ unitary matrices, which a modern CPU computes in microseconds. Asserting that this Hilbert space is 'classically challenging to formulate' on tabular datasets with 8 to 30 features is physically inaccurate and invites severe reviewer skepticism.
- **Recommended Conservative Replacement Wording:**
  > *'While parameterized quantum circuits provide flexible non-linear feature maps within compact dimensional constraints, 4-qubit circuits remain classically simulable. Our focus is not asserting quantum computational advantage, but systematically investigating dynamic workload allocation within hybrid quantum-classical architectures.'*

---

### Item 2: Claims of Genuine Quantum Expressivity vs. Low-Dimensional Collapse

- **Locations:**
  - `paper/paper.tex`, Section IV-B, line 314: `Crucially, forcing feature representations through a static classical 4-D bottleneck severely degrades F1-score on Heart (-5.72%...) and Diabetes (-4.85%...). In contrast, Adaptive HQNN leverages the quantum circuit to maintain predictive discrimination.`
  - `paper/paper.tex`, Section V, line 374: `Second, ablation experiments confirm that the quantum circuit provides genuine representational utility over low-dimensional classical projections.`
- **Current Text / Context:**
  The ablation shows that an architecture with a classical linear/ReLU 4-D bottleneck degrades compared to the 4-qubit VQC.
- **Why It Is An Overclaim:**
  The degradation of the 4-D classical bottleneck demonstrates that a narrow classical dense layer with 4 units suffers informational bottlenecking in this specific pipeline. However, it does not prove that quantum circuits possess superior expressivity over wider or differently configured classical layers. The parameter-matched M4 control (which uses 16 units) performs much closer to M3. Attributing the gap exclusively to 'quantum representational utility' rather than architecture-specific nonlinear dynamics overstates the conclusion.
- **Recommended Conservative Replacement Wording:**
  > *'The ablation demonstrates that within a constrained 4-dimensional latent interface, the parameterized quantum circuit mitigates the performance degradation observed in a naive classical 4-D bottleneck. However, this reflects architectural inductive biases within compact representations rather than fundamental quantum representational superiority over unconstrained classical architectures.'*

---

### Item 3: Claims of 'Predictive Parity' vs. Statistically Non-Significant Differences

- **Locations:**
  - `paper/paper.tex`, Abstract, line 29: `Nadeau--Bengio corrected resampled t-tests across overlapping cross-validation folds reveal no statistically significant difference in predictive accuracy (p >= 0.05) between Adaptive and Fixed HQNN.`
  - `paper/paper.tex`, Section V, line 374: `...whereas Adaptive HQNN maintains strong predictive parity with full 2-layer quantum networks...`
  - Reviewer Response Matrix: Pillar 3 and Section 1 repeatedly claim 'predictive parity'.
- **Why It Is An Overclaim:**
  In classical hypothesis testing, failing to reject the null hypothesis ($H_0: \mu_{M3} - \mu_{M2} = 0$, with $p_{NB} = 0.34$ to $0.98$) strictly means the evidence is insufficient to detect a difference under the corrected variance. It **does not prove** that the models are identical or mathematically equivalent (the classical error of equating 'absence of evidence' with 'evidence of absence'). Furthermore, the point estimates for Adaptive HQNN are slightly lower than Fixed HQNN on WDBC (-0.53% F1) and Diabetes (-0.38% F1).
- **Recommended Conservative Replacement Wording:**
  > *'Adaptive HQNN achieves predictive performance with no statistically detectable difference from Fixed HQNN under the Nadeau--Bengio corrected resampled t-test (p_NB >= 0.34 across all datasets). However, failure to reject the null hypothesis does not constitute formal proof of equivalence, and point estimates reflect modest trade-offs in mean F1 score.'*

---

### Item 4: Ambiguous-Case Routing Claims (Empirical Contradiction on Diabetes)

- **Locations:**
  - Synthesis documents & Reviewer Response: Claims that *'the dynamic router accurately identifies uncertain or ambiguous samples and escalates them to quantum layers to rescue classification accuracy.'*
  - `paper/paper.tex`, Section IV-B, line 280 & Section V, line 372: Discussion of routing on Heart and Diabetes.
- **Why It Is An Overclaim / Direct Factual Contradiction:**
  According to `m3_per_path_accuracy.csv`, on Pima Diabetes 5-fold cross-validation:
  - 106 samples routed to Path 1: Preliminary classical accuracy was **56.60%**; final quantum accuracy was **54.72%** (Net drop of **-1.89%**).
  - 72 samples routed to Path 2: Preliminary classical accuracy was **58.33%**; final quantum accuracy was **52.78%** (Net drop of **-5.56%**).
  Across all 178 escalated diabetes cases, quantum escalation did **not** rescue preliminary errors; it slightly degraded accuracy on those specific subsets. The system's overall F1 stability (68.07%) is maintained because Path 0 handles 67.4% of high-confidence samples accurately (75.89% accuracy). Claiming that quantum layers 'recover ambiguous cases' is factually contradicted on Diabetes.
- **Recommended Conservative Replacement Wording:**
  > *'Empirical evaluation of per-path decisions reveals that dynamic routing recalibrates decision boundaries on uncertain instances, but does not guarantee uniform accuracy improvements across all escalated subsets. Notably, on Pima Diabetes, instances escalated to quantum pathways exhibited modest accuracy declines (-1.89% on Path 1, -5.56% on Path 2) relative to preliminary predictions, indicating that the overall system benefit stems from global threshold-based uncertainty management rather than universal error recovery on individual ambiguous cases.'*

---

### Item 5: Efficiency & Speedup Claims (CPU Simulation vs. Quantum Hardware Runtime)

- **Locations:**
  - `paper/paper.tex`, Abstract, line 29: `...reduces estimated inference-time quantum layer executions by 75.5% to 98.4%...`
  - `paper/paper.tex`, Section IV-E, line 367: `...demonstrating a 98x to 155x speedup for Path 0 in CPU simulation. We explicitly emphasize that these timings reflect classical simulation overheads rather than physical quantum hardware runtimes or energy consumption.`
  - `paper/paper.tex`, Table II & Table V: Layer counts and millisecond timing.
- **Why It Is An Overclaim If Unqualified:**
  The 98x–155x speedup is entirely due to the fact that simulating quantum state vectors on a CPU incurs matrix-multiplication overhead that classical linear layers do not. On physical quantum hardware, circuit execution involves qubit initialization, microwave pulses, and projective measurement (typically on the order of microseconds to milliseconds), plus queueing latencies. Describing simulator timing as 'speedup' without constant qualification misleads readers into assuming physical acceleration.
- **Recommended Conservative Replacement Wording:**
  > *'Because physical quantum processor execution was not available, runtime benchmarking is restricted to CPU state-vector simulation (`default.qubit`). Path 0 yields a 98x to 155x latency reduction in CPU simulation; however, this metric reflects classical matrix simulation complexity rather than physical quantum execution time or cryogenic refrigeration power.'*

---

### Item 6: Clinical Readiness and Diagnostic Efficacy Claims

- **Locations:**
  - `paper/paper.tex`, Title & Abstract, lines 28-30: `...for clinical diagnostic applications...`
  - `paper/paper.tex`, Introduction, line 37: `Early and accurate medical disease diagnosis is a cornerstone of modern clinical informatics...`
- **Why It Is An Overclaim:**
  The evaluated datasets (WDBC $N=569$, Cleveland Heart $N=303$, Pima Diabetes $N=768$) are small, historical tabular benchmarks collected decades ago (1988–1993). They lack multi-center clinical diversity, patient demographics, modern electronic health record (EHR) schemas, and prospective clinical validation. Suggesting clinical readiness or deployment viability is unwarranted.
- **Recommended Conservative Replacement Wording:**
  > *'These empirical investigations utilize established retrospective benchmark datasets to explore algorithmic properties of hybrid architectures. They serve as methodological proof-of-concept demonstrations and do not indicate clinical deployment readiness, which would require extensive prospective validation on multi-institutional clinical cohorts.'*

---

### Item 7: Reviewer Response Claims Regarding "Complete Resolution"

- **Locations:**
  - `REVIEWER_RESPONSE_MATRIX.md`, Section 1: Overview claims all concerns have been thoroughly resolved.
  - Synthesis documents: Summarizing reviewer feedback as 'fully addressed'.
- **Why It Is An Overclaim:**
  While technical and computational requests (repeated CV, Nadeau–Bengio testing, ablations, SHAP stability, parameter counts) have been rigorously completed, fundamental reviewer critiques concerning:
  - Small cohort sample size ($N=303-768$),
  - Absence of physical QPU hardware experiments,
  - Absence of GBDT/XGBoost baselines, and
  - Non-significant differences between classical MLP and quantum models
  **cannot** be 'resolved' without gathering new clinical cohorts or testing physical QPUs. Claiming they are 'fully resolved' will provoke negative reactions from reviewers. They must be characterized as addressed via empirical analysis where feasible and transparently framed as persistent study limitations otherwise.
- **Recommended Conservative Replacement Wording:**
  > *'We have addressed all empirical, statistical, and methodological requests within the computational scope of this study by executing 300 repeated cross-validation runs, 25-fold ablation suites, and corrected hypothesis tests. Intrinsic experimental boundaries—including retrospective dataset scale, state-vector simulation constraints, and the focus on neural rather than tree-based architectures—have been explicitly formalized as documented study limitations in Section V.'*

---

### Item 8: Multiple-Testing and Exploratory Significance on Diabetes ROC-AUC

- **Locations:**
  - `paper/paper.tex`, Section IV-C, line 353 & line 359: `On Diabetes, M3 achieves an unadjusted statistically significant improvement in ROC-AUC over M4 (+0.0359, p_NB = 0.0243). However, when evaluating all 27 pairwise tests under Benjamini--Hochberg False Discovery Rate (FDR) adjustment (q=0.05), the adjusted p-value (p_FDR = 0.6561) indicates this finding should be interpreted as exploratory.`
- **Why It Is An Overclaim If Highlighted Without Qualification:**
  Highlighting $p=0.0243$ with an asterisk without immediate FDR qualification would constitute p-hacking.
- **Recommended Conservative Replacement Wording:**
  > *'The manuscript correctly qualifies this finding in line 359. Ensure that any summary tables or abstract statements do not claim statistically significant superiority of M3 over M4, and maintain explicit labeling of this comparison as exploratory under the 27-test Benjamini–Hochberg family (p_FDR = 0.6561).'*
