# Findings

> **Project:** QMLShield  
> **Domain:** Quantum Machine Learning Security  
> **Focus:** Adversarial Robustness & Purification for Quantum Machine Learning  
> **Research Stage:** QMLShield v0.1  
> **Document Type:** Living Research Evidence Record  
> **Status:** Results pending

---

## 1. Purpose

This document records the empirical findings produced by QMLShield experiments.

Unlike the other project documents, this document must not define expected scientific outcomes in advance.

It is a **living evidence record**.

The purpose of this document is to answer, based on actual experimental evidence:

- What happened?
- Under which conditions did it happen?
- How reproducible was the result?
- What does the evidence support?
- What does the evidence not support?
- What limitations were observed?
- What should be investigated next?

The document must be updated as experiments are completed.

---

# 2. Core Research Question

The central research question is:

> **To what extent can input purification improve the robustness of a variational quantum classifier against adversarial perturbations while preserving clean-input performance?**

No conclusion is assumed.

The final answer must emerge from the experimental evidence.

---

# 3. Finding Principles

QMLShield follows these principles when recording findings.

## 3.1 Evidence Before Claims

A claim must be supported by recorded experimental evidence.

Do not write:

```text
QAE improves robustness.
```

before the corresponding experiments establish this.

Instead record:

```text
Observed:
QAE achieved X% robust accuracy under condition Y.

Comparison:
The no-defense condition achieved Z%.

Interpretation:
Under this evaluated condition, QAE improved robust accuracy by X-Z percentage points.

Boundary:
This does not establish robustness against attacks or conditions that were not evaluated.
```

---

## 3.2 Observation vs Interpretation

Every important finding should distinguish:

```text
Observation
```

from:

```text
Interpretation
```

and:

```text
Claim
```

These are not interchangeable.

### Observation

What the experiment directly produced.

### Interpretation

What the observed result may mean.

### Claim

A bounded scientific statement supported by the available evidence.

---

## 3.3 Negative Results Are Valid

The project must record negative results with the same care as positive results.

Examples:

```text
QAE did not outperform CAE.
QAE reduced clean accuracy.
Purification failed against stronger attacks.
Noise removed the observed robustness gain.
Adaptive attacks substantially reduced defense effectiveness.
```

These are findings, not failures of the research process.

---

## 3.4 No Result Shopping

Results must not be selectively reported merely because they support the preferred hypothesis.

If an experiment produces:

```text
Positive result
Negative result
Mixed result
Unexpected result
```

the relevant result should be recorded.

---

## 3.5 No Silent Method Changes

If the experimental methodology changes after results are observed, the change must be documented.

Do not silently alter:

- attack budgets;
- dataset splits;
- evaluation samples;
- model architecture;
- purification architecture;
- noise settings;
- metrics;
- seed policy;
- training protocol.

A methodological change may invalidate direct comparison with earlier results.

---

# 4. Current Research Status

At the beginning of the project:

```text
Experimental status:
Results not yet established.

Scientific conclusion:
Not yet available.
```

The following sections are therefore intentionally incomplete until the corresponding experiments are run.

---

# 5. Evidence Lifecycle

The intended evidence lifecycle is:

```text
Experiment
    ↓
Raw Result
    ↓
Validation
    ↓
Processed Result
    ↓
Comparison
    ↓
Observation
    ↓
Interpretation
    ↓
Bounded Finding
    ↓
Research Conclusion
```

No step should be skipped for research-critical findings.

---

# 6. Evidence Hierarchy

QMLShield distinguishes several levels of evidence.

## Level 1 — Raw Measurement

Examples:

```text
Accuracy
Loss
Attack Success Rate
Reconstruction Error
Circuit Depth
Runtime
```

---

## Level 2 — Processed Measurement

Examples:

```text
Mean accuracy
Standard deviation
Accuracy drop
Robustness improvement
Recovery rate
```

---

## Level 3 — Comparison

Examples:

```text
QAE vs CAE
Defense vs No Defense
Clean vs Adversarial
Ideal vs Noisy
Non-adaptive vs Adaptive
```

---

## Level 4 — Finding

A bounded statement derived from the comparison.

---

## Level 5 — Research Conclusion

A broader answer to a research question supported by multiple findings.

The project should not jump directly from a single raw measurement to a broad conclusion.

---

# 7. Experiment Evidence Registry

The core experiment registry is:

| Experiment | Status | Evidence Available | Finding |
|---|---|---|---|
| EXP-01 | Pending | No | — |
| EXP-02 | Pending | No | — |
| EXP-03 | Pending | No | — |
| EXP-04 | Pending | No | — |
| EXP-05 | Pending | No | — |
| EXP-06 | Pending | No | — |
| EXP-07 | Pending | No | — |
| EXP-08 | Pending | No | — |

The status should evolve as experiments are executed.

Suggested states:

```text
Pending
In Progress
Completed
Blocked
Invalidated
Repeated
Finalized
```

---

# 8. EXP-01 — Clean VQC Baseline

## Objective

Establish whether the VQC can successfully perform the initial clean MNIST classification task.

## Expected Evidence

The experiment should produce:

```text
Clean Accuracy
Clean Loss
Training Information
Inference Information
Circuit Information
Seed
Configuration
Git Revision
```

## Finding

```text
Status:
Pending

Observation:
Not yet available.

Interpretation:
Not yet available.

Conclusion:
Not yet available.
```

## Required Interpretation

The final finding should answer:

> Can the selected VQC establish a sufficiently meaningful clean classification baseline for the subsequent security experiments?

## Failure Interpretation

If the baseline is weak, record the cause if identifiable.

Potential categories:

```text
Data
Preprocessing
Encoding
Circuit
Optimization
Training
Evaluation
Implementation
```

Do not proceed to security conclusions using an invalid victim baseline.

---

# 9. EXP-02 — FGSM Vulnerability

## Objective

Determine whether the clean VQC is vulnerable to classical input-space FGSM perturbations.

## Required Evidence

Record:

```text
Clean Accuracy
Adversarial Accuracy
Attack Success Rate
Accuracy Drop
Perturbation Budget
Evaluation Sample Count
Seed
Configuration
```

## Finding

```text
Status:
Pending

Observation:
Not yet available.

Interpretation:
Not yet available.

Conclusion:
Not yet available.
```

## Required Interpretation

The final finding should answer:

> Does FGSM meaningfully degrade VQC classification under the defined threat model?

---

# 10. EXP-03 — PGD Vulnerability

## Objective

Determine the behavior of the VQC under the stronger iterative PGD attack.

## Required Evidence

Record:

```text
Clean Accuracy
Adversarial Accuracy
Attack Success Rate
Perturbation Budget
Step Size
Number of Iterations
Accuracy Drop
Seed
Configuration
```

## Finding

```text
Status:
Pending

Observation:
Not yet available.

Interpretation:
Not yet available.

Conclusion:
Not yet available.
```

## Required Interpretation

The final finding should answer:

> Does PGD produce stronger or materially different degradation than the initial FGSM evaluation?

The answer must be based on matched experimental conditions.

---

# 11. EXP-04 — Classical Autoencoder Baseline

## Objective

Evaluate classical reconstruction-based purification as a baseline.

## Conditions

At minimum:

```text
Clean → CAE → VQC
Adversarial → CAE → VQC
```

## Required Evidence

Record:

```text
Clean Accuracy
Purified Clean Accuracy
Raw Adversarial Accuracy
Purified Adversarial Accuracy
Reconstruction Error
Clean Degradation
Robustness Improvement
CAE Configuration
Seed
```

## Finding

```text
Status:
Pending

Observation:
Not yet available.

Interpretation:
Not yet available.

Conclusion:
Not yet available.
```

## Required Interpretation

The final finding should answer:

> Can a classical autoencoder recover useful classification performance after adversarial perturbation while preserving clean performance?

---

# 12. EXP-05 — Quantum Autoencoder Purification

## Objective

Evaluate the QAE as a quantum-native purification mechanism.

## Conditions

At minimum:

```text
Clean → Encoding → QAE → VQC
Adversarial → Encoding → QAE → VQC
```

## Required Evidence

Record:

```text
Clean Accuracy
Purified Clean Accuracy
Raw Adversarial Accuracy
Purified Adversarial Accuracy
Reconstruction / Fidelity Metric
Accuracy Recovery
Clean Degradation
Qubit Count
Circuit Depth
QAE Configuration
Seed
```

## Finding

```text
Status:
Pending

Observation:
Not yet available.

Interpretation:
Not yet available.

Conclusion:
Not yet available.
```

## Required Interpretation

The final finding should answer:

> Under the evaluated conditions, does QAE-based purification improve adversarial classification recovery while preserving clean performance?

The answer must not be generalized beyond the tested conditions.

---

# 13. EXP-06 — CAE vs QAE

## Objective

Compare classical and quantum purification under matched conditions.

## Primary Comparison

```text
No Defense
      ↓
CAE
      ↓
QAE
```

The no-defense condition remains necessary as the reference point.

## Required Evidence

Compare:

```text
Clean Accuracy
Purified Clean Accuracy
Adversarial Accuracy
Robust Accuracy
Accuracy Recovery
Reconstruction Metric
Circuit Depth
Qubit Count
Runtime
Training Cost
Inference Cost
```

where applicable.

## Finding

```text
Status:
Pending

Observation:
Not yet available.

Interpretation:
Not yet available.

Conclusion:
Not yet available.
```

## Required Interpretation

The final finding should answer:

> Under matched conditions, how do CAE and QAE differ in robustness, clean preservation, reconstruction behavior, and resource cost?

The result does not need to favor QAE.

---

# 14. EXP-07 — Noise Evaluation

## Objective

Determine how quantum execution noise changes the observed behavior.

## Important Distinction

The project must not combine:

```text
Adversarial Perturbation
```

and:

```text
Quantum Noise
```

into a single undifferentiated concept.

Adversarial perturbation represents attacker-controlled input modification.

Quantum noise represents execution-level imperfection.

---

## Required Evidence

Record:

```text
Noise Model
Noise Strength
Clean Accuracy
Robust Accuracy
Purification Recovery
Reconstruction / Fidelity
Circuit Depth
Execution Cost
```

for the evaluated defense conditions.

## Finding

```text
Status:
Pending

Observation:
Not yet available.

Interpretation:
Not yet available.

Conclusion:
Not yet available.
```

## Required Interpretation

The final finding should answer:

> Does quantum noise materially change the effectiveness or cost trade-off of purification?

---

# 15. EXP-08 — Adaptive Attack

## Objective

Determine whether the purification mechanism remains effective when the attacker explicitly accounts for the defense pipeline.

## Required Evidence

Record:

```text
Attack Definition
Attack Budget
Adaptive Objective
Defense Configuration
Adversarial Accuracy
Attack Success Rate
Robust Accuracy
Clean Accuracy
Comparison with Non-Adaptive Attack
```

## Finding

```text
Status:
Pending

Observation:
Not yet available.

Interpretation:
Not yet available.

Conclusion:
Not yet available.
```

## Required Interpretation

The final finding should answer:

> Does adaptive knowledge of the purification mechanism materially reduce the observed robustness?

---

# 16. Primary Findings Table

This table should eventually summarize the strongest validated findings.

| Finding ID | Research Question | Condition | Observation | Interpretation | Confidence / Limitation |
|---|---|---|---|---|---|
| F-01 | RQ1 | — | Pending | Pending | — |
| F-02 | RQ2 | — | Pending | Pending | — |
| F-03 | RQ3 | — | Pending | Pending | — |
| F-04 | RQ4 | — | Pending | Pending | — |
| F-05 | RQ5 | — | Pending | Pending | — |
| F-06 | RQ6 | — | Pending | Pending | — |
| F-07 | RQ7 | — | Pending | Pending | — |
| F-08 | RQ8 | — | Pending | Pending | — |

Finding IDs should remain stable once assigned.

---

# 17. Finding Record Template

Every significant finding should use a consistent structure.

```text
Finding ID:
F-XX

Research Question:
RQ-X

Experiment:
EXP-XX

Status:
Validated / Preliminary / Inconclusive / Invalidated

Condition:
[Describe evaluated condition]

Observation:
[Direct empirical observation]

Comparison:
[Reference condition]

Interpretation:
[Reasoned interpretation]

Supported Claim:
[Bounded scientific claim]

Not Supported:
[Claims the evidence does not justify]

Limitations:
[Known limitations]

Reproducibility:
[Seeds / runs / environment]

Evidence:
[Result artifact references]

Next Action:
[Next experiment or analysis]
```

---

# 18. Observation Template

Use this format for raw empirical observations:

```text
Observation:
Under [condition], the system achieved [metric] = [value].

Reference:
The comparison condition achieved [metric] = [value].

Difference:
[value]

Experimental configuration:
[configuration identifier]

Runs:
[number]

Seed(s):
[list]

Artifact:
[result path / identifier]
```

The observation should contain measurable facts before interpretation.

---

# 19. Interpretation Template

After recording the observation:

```text
Interpretation:
The observed difference indicates [bounded interpretation].

The result is consistent / inconsistent with [research hypothesis].

However, the result is limited to [condition].

The evidence does not establish [broader claim].
```

This prevents interpretation from becoming stronger than the measurement.

---

# 20. Claim Strength

QMLShield should use a hierarchy of claim strength.

## Weak Claim

> Under the tested configuration, QAE achieved higher robust accuracy than the no-defense condition.

## Moderate Claim

> Across the evaluated seeds and attack strengths, QAE consistently improved robust accuracy relative to the no-defense condition.

## Strong Claim

> QAE is robust against adversarial attacks.

The third statement requires substantially broader evidence and should not be made from a narrow experiment.

---

# 21. Evidence Confidence

Findings may be classified as:

```text
Preliminary
Validated
Strongly Supported
Inconclusive
Invalidated
```

### Preliminary

Observed once or under limited conditions.

### Validated

Repeated under the intended protocol.

### Strongly Supported

Consistent across multiple relevant conditions and analyses.

### Inconclusive

Evidence is insufficient to distinguish competing explanations.

### Invalidated

A methodological or implementation issue makes the previous finding unreliable.

---

# 22. Reproducibility Record

Every major finding should record:

```text
Experiment ID
Run ID
Git commit
Python version
Dependency environment
Configuration
Dataset version / definition
Seed
Execution backend
Noise configuration
```

A finding without traceable experimental provenance should not be treated as a final research result.

---

# 23. Cross-Seed Findings

When multiple seeds are available, record:

```text
Mean
Standard Deviation
Minimum
Maximum
Number of Runs
```

where appropriate.

A result that appears under one seed but disappears across repeated runs should be described as seed-sensitive rather than universally observed.

---

# 24. Cross-Attack Findings

When both FGSM and PGD have been evaluated, findings should distinguish them.

Example structure:

```text
FGSM:
[Finding]

PGD:
[Finding]

Comparison:
[Finding]

Limitation:
[Scope of evaluated attacks]
```

Do not combine attack results into one statement when their threat strength or behavior differs materially.

---

# 25. Attack-Strength Findings

For attack-strength sweeps, record:

```text
Perturbation Budget
Clean Accuracy
Adversarial Accuracy
Defense Accuracy
Attack Success Rate
Recovery
```

The key question is whether the defense:

```text
Fails immediately
Degrades gradually
Maintains a useful range
Fails beyond a threshold
```

The threshold itself should be reported as an experimental observation, not treated as a universal security boundary.

---

# 26. Reconstruction Findings

For CAE and QAE, reconstruction quality should be analyzed separately from classification recovery.

A useful record is:

```text
Reconstruction Metric:
[value]

Classification Metric:
[value]

Relationship:
[Observed relationship]

Interpretation:
[Does better reconstruction correspond to better classification recovery?]
```

The project must not assume:

```text
Lower reconstruction error
        =
Higher robustness
```

unless the evidence supports the relationship.

---

# 27. Clean-Performance Findings

Every defense finding must consider clean performance.

Record:

```text
No Defense Clean Accuracy
Defense Clean Accuracy
Difference
Relative Change
```

Then state:

```text
Clean performance preserved
or
Clean performance degraded
```

within the evaluated tolerance.

The tolerance itself must be defined before final interpretation where possible.

---

# 28. Robustness Findings

Robustness findings should always identify:

```text
Attack
Attack Budget
Defense
Model
Dataset
Noise Condition
Metric
```

Avoid statements such as:

```text
The model is robust.
```

Prefer:

```text
The defended VQC maintained X% accuracy under PGD with ε = Y in the evaluated simulation condition.
```

---

# 29. Resource Findings

Robustness must be considered together with resource requirements.

Record:

```text
Qubits
Circuit Depth
Trainable Parameters
Training Time
Inference Time
Simulation Time
Shots
Memory
```

A defense with better robustness but substantially higher quantum complexity may represent a trade-off rather than an unconditional improvement.

---

# 30. Noise Findings

Noise findings should identify:

```text
Noise Model
Noise Strength
Circuit
Defense
Attack
Metric
```

The analysis should determine whether noise:

```text
Has little effect
Moderately degrades performance
Strongly degrades performance
Changes the ranking of defenses
```

The final interpretation should remain tied to the tested noise model.

---

# 31. Adaptive-Attack Findings

Adaptive results should be compared with non-adaptive results.

Record:

```text
Non-Adaptive Robust Accuracy
Adaptive Robust Accuracy
Difference
Attack Success Rate
```

A large difference may indicate that the defense relies partly on the attacker not accounting for the purifier.

This should be described as an empirical observation rather than an assumption about all purification methods.

---

# 32. Comparative Findings

The central comparative table should eventually take the form:

| Condition | Clean Accuracy | Robust Accuracy | Recovery | Reconstruction | Qubits | Depth | Cost |
|---|---:|---:|---:|---:|---:|---:|---:|
| No Defense | — | — | — | — | — | — | — |
| CAE | — | — | — | — | — | — | — |
| QAE | — | — | — | — | — | — | — |

This table must only contain actual measured values.

---

# 33. Defense Ranking

QMLShield should avoid creating a single ranking by default.

Instead, compare defenses across dimensions:

```text
Robustness
Clean Preservation
Reconstruction
Adaptive Robustness
Noise Sensitivity
Circuit Complexity
Runtime
```

A defense may be better in one dimension and worse in another.

If an overall ranking is eventually required, its weighting methodology must be explicitly defined.

---

# 34. Unexpected Findings

Unexpected observations should be explicitly recorded.

Examples:

```text
QAE performs worse than expected.
CAE unexpectedly outperforms QAE.
Higher circuit depth reduces robustness.
Noise improves one metric while damaging another.
Reconstruction improves without classification recovery.
FGSM is unexpectedly weak.
PGD behaves differently from expectation.
```

Unexpected results should trigger investigation, not automatic rejection.

---

# 35. Anomaly Investigation

When a result appears anomalous, investigate:

```text
Data
Preprocessing
Labels
Random Seed
Gradient Flow
Attack Implementation
Encoding
Circuit
Measurement
Optimizer
Purifier Training
Noise Configuration
Metric Calculation
```

If the anomaly is caused by an implementation error, mark the affected result as invalidated.

---

# 36. Invalidated Results

An invalidated result must remain traceable.

Use:

```text
Status:
Invalidated

Reason:
[reason]

Affected Experiments:
[list]

Affected Findings:
[list]

Replacement Run:
[run ID if available]
```

Do not silently delete scientific history when the project has already relied on the result.

---

# 37. Methodology Changes

If an experiment reveals that the methodology must change, record:

```text
Previous Method:
[...]

Problem:
[...]

Change:
[...]

Reason:
[...]

Affected Results:
[...]

Compatibility with Previous Results:
[compatible / partially compatible / incompatible]
```

A methodology change may require rerunning earlier experiments.

---

# 38. Findings and Literature

Literature-derived expectations must remain separate from QMLShield observations.

Use explicit labels such as:

```text
Literature expectation:
[...]

QMLShield observation:
[...]

Agreement:
[...]

Difference:
[...]

Possible explanation:
[...]
```

Do not present literature results as if they were produced by QMLShield.

---

# 39. External Validation

If QMLShield results are later compared with published work, record:

```text
Reference
Dataset
Attack
Defense
Encoding
Noise
Metrics
Observed QMLShield Result
Published Result
Comparability
Differences
```

Differences in methodology must be identified before comparing numerical results.

---

# 40. Research Question Mapping

The final findings should answer the research questions explicitly.

| Research Question | Evidence | Final Answer |
|---|---|---|
| RQ1 — Robustness | EXP-02–06 | Pending |
| RQ2 — Clean Performance | EXP-01, EXP-04–06 | Pending |
| RQ3 — CAE vs QAE | EXP-06 | Pending |
| RQ4 — Reconstruction vs Classification | EXP-04–06 | Pending |
| RQ5 — Attack Strength | EXP-02, EXP-03 + sweeps | Pending |
| RQ6 — Noise | EXP-07 | Pending |
| RQ7 — Adaptive Attacks | EXP-08 | Pending |
| RQ8 — Resource Trade-off | EXP-06, EXP-07 | Pending |

---

# 41. Hypothesis Tracking

If hypotheses are defined in `research_questions.md`, they should be tracked without assuming that they are correct.

Use:

```text
Hypothesis:
H-X

Status:
Supported / Partially Supported / Not Supported / Inconclusive

Evidence:
[...]

Counter-evidence:
[...]

Conditions:
[...]

Interpretation:
[...]
```

A hypothesis being unsupported is a legitimate outcome.

---

# 42. Evidence Gaps

The project should maintain an explicit list of unanswered questions.

Current evidence gaps:

```text
[ ] Clean VQC baseline
[ ] FGSM vulnerability
[ ] PGD vulnerability
[ ] CAE baseline
[ ] QAE evaluation
[ ] CAE vs QAE comparison
[ ] Attack-strength sweep
[ ] Multi-seed validation
[ ] Noise evaluation
[ ] Adaptive attack evaluation
[ ] Resource trade-off analysis
[ ] Generalization evaluation
```

This list should be updated as evidence becomes available.

---

# 43. Findings Roadmap

The intended evolution is:

```text
No Results
    ↓
Baseline Findings
    ↓
Vulnerability Findings
    ↓
Purification Findings
    ↓
Comparative Findings
    ↓
Noise Findings
    ↓
Adaptive Findings
    ↓
Generalization Findings
    ↓
Final Research Conclusions
```

The document should evolve with the project rather than being completed artificially at the beginning.

---

# 44. Minimum Evidence for a Final Defense Claim

Before making a meaningful defense claim, QMLShield should ideally have:

```text
Valid clean baseline
+
Valid attack baseline
+
No-defense control
+
Defense condition
+
Matched evaluation
+
Clean-performance analysis
+
Robustness analysis
+
Reproducible configuration
```

For stronger claims, additional evidence should include:

```text
Multiple seeds
Multiple attack strengths
PGD
Noise
Adaptive attacks
Resource analysis
```

The exact claim strength must match the available evidence.

---

# 45. Minimum Evidence for a Comparative Claim

To claim:

> "QAE outperforms CAE"

the project should not rely on a single accuracy number.

At minimum, compare:

```text
Clean Performance
Robust Performance
Attack Conditions
Evaluation Set
Seed Policy
Resource Cost
```

If QAE wins robustness but loses substantially in clean performance or cost, the correct conclusion may instead be:

```text
QAE provides a robustness advantage under the evaluated condition with an associated performance/resource trade-off.
```

---

# 46. Minimum Evidence for a Security Claim

Security claims require particular caution.

The following statement:

```text
The system is secure.
```

is not justified by a narrow experiment.

A bounded statement such as:

```text
The evaluated purification mechanism reduced the success of the tested white-box classical input-space attacks under the specified experimental conditions.
```

is substantially more defensible.

The threat model defines the boundary of the claim.

---

# 47. Finding-to-Artifact Traceability

Every final finding should point to evidence artifacts.

Conceptually:

```text
Finding
  ↓
Experiment ID
  ↓
Run ID
  ↓
Configuration
  ↓
Raw Result
  ↓
Processed Result
  ↓
Figure / Table
```

This creates an evidence chain from conclusion back to measurement.

---

# 48. Figures

Research figures should be generated from recorded result data.

Potential figures include:

```text
Clean vs Adversarial Accuracy
Robust Accuracy vs Attack Strength
CAE vs QAE Comparison
Reconstruction vs Classification Recovery
Robustness vs Circuit Depth
Robustness vs Noise Strength
Clean Accuracy vs Noise Strength
Adaptive vs Non-Adaptive Robustness
Robustness vs Resource Cost
```

A figure should have:

```text
Title
Axis Labels
Units
Legend where necessary
Experimental condition
Caption
```

---

# 49. Tables

Important tables should preserve the experimental context.

At minimum, a table should make clear:

```text
Dataset
Task
Model
Attack
Defense
Noise
Metric
```

Do not present isolated percentages without their conditions.

---

# 50. Result Reproduction Checklist

Before treating a major finding as validated:

```text
[ ] Experiment configuration is saved
[ ] Git revision is recorded
[ ] Environment is reproducible
[ ] Dataset definition is known
[ ] Seed is recorded
[ ] Attack parameters are recorded
[ ] Defense parameters are recorded
[ ] Noise parameters are recorded if applicable
[ ] Raw result exists
[ ] Processed result exists
[ ] Metric calculation is validated
[ ] Comparison baseline exists
[ ] Limitations are recorded
```

---

# 51. Finding Review Checklist

Before adding a final finding:

```text
[ ] Is the observation directly supported by data?
[ ] Is the comparison fair?
[ ] Are the experimental conditions stated?
[ ] Is the claim narrower than or equal to the evidence?
[ ] Are negative or contradictory results included?
[ ] Are limitations stated?
[ ] Is the result reproducible?
[ ] Is the result distinguishable from literature evidence?
[ ] Is the corresponding experiment identified?
[ ] Is the next research action clear?
```

---

# 52. Final Research Synthesis

This section should remain empty until the primary experimental program is complete.

## Overall Finding

```text
Pending.
```

## Answer to the Primary Research Question

```text
Pending.
```

## Strongest Positive Evidence

```text
Pending.
```

## Strongest Negative Evidence

```text
Pending.
```

## Most Important Limitation

```text
Pending.
```

## Most Important Trade-off

```text
Pending.
```

## Most Important Unexpected Result

```text
Pending.
```

## Recommended Next Research Direction

```text
Pending.
```

---

# 53. Final Conclusions

This section must only be populated after the evidence chain is sufficiently complete.

The final conclusion should answer:

```text
1. What did QMLShield demonstrate?
2. Under what conditions?
3. How strong is the evidence?
4. What did it fail to demonstrate?
5. What are the main limitations?
6. What should be investigated next?
```

No conclusion should be added merely because the project has completed its implementation.

The conclusion must be earned by evidence.

---

# 54. Document Status

**Current status:** Evidence framework established; experimental results pending.

Current experiment status:

```text
EXP-01  Pending
EXP-02  Pending
EXP-03  Pending
EXP-04  Pending
EXP-05  Pending
EXP-06  Pending
EXP-07  Pending
EXP-08  Pending
```

Current scientific conclusion:

```text
Not yet established.
```

---

# 55. Final Principle

`findings.md` is not a place to defend the project hypothesis.

It is a place to record what the project discovered.

The governing principle is:

> **Do not write the result before running the experiment. Do not strengthen the claim beyond the evidence. Do not hide the result when the evidence contradicts the hypothesis.**

QMLShield should be allowed to discover that:

```text
QAE works,
```

or:

```text
CAE works better,
```

or:

```text
Neither works reliably,
```

or:

```text
The answer depends on attack strength,
```

or:

```text
The answer changes under quantum noise,
```

or:

```text
Adaptive attacks substantially reduce the observed benefit.
```

Any of these can become a meaningful research result when supported by rigorous evidence.

