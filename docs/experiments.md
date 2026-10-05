# Experiments

> **Project:** QMLShield  
> **Domain:** Quantum Machine Learning Security  
> **Focus:** Adversarial Robustness & Purification for Quantum Machine Learning  
> **Research Stage:** QMLShield v0.1  
> **Status:** Experimental protocol defined; results pending

---

## 1. Purpose

This document defines the experimental program for QMLShield.

It translates the research questions, threat model, methodology, architecture, rules, and technology stack into a concrete sequence of experiments.

The document specifies:

- what experiments must be run;
- why each experiment exists;
- what is held constant;
- what is varied;
- what is measured;
- what artifacts must be recorded;
- how experiments depend on one another;
- and what evidence each experiment is expected to provide.

This document does **not** assume that any defense is effective.

The purpose of the experiments is to determine:

> **To what extent can input purification improve the robustness of a variational quantum classifier against adversarial perturbations while preserving clean-input performance?**

A negative result is a valid research result if the experiment is correctly designed, reproducible, and interpreted within the defined scope.

---

# 2. Experimental Philosophy

QMLShield follows a controlled, progressive experimental strategy.

The central principle is:

```text
Establish the victim
        ↓
Establish vulnerability
        ↓
Establish baseline defenses
        ↓
Evaluate quantum purification
        ↓
Compare defenses fairly
        ↓
Introduce noise
        ↓
Introduce adaptive attacks
        ↓
Study limitations and trade-offs
```

The project should not begin with the most complicated experiment.

Each experiment should establish the evidence required for the next experiment.

---

# 3. Experimental Research Question

The primary research question is:

> **To what extent can input purification improve the robustness of a variational quantum classifier against adversarial perturbations while preserving clean-input performance?**

The experiments operationalize this question through several measurable dimensions:

1. Clean performance
2. Adversarial robustness
3. Attack success
4. Reconstruction quality
5. Classification recovery
6. Defense-induced degradation
7. Attack strength
8. Quantum noise
9. Circuit complexity
10. Computational and quantum resource cost

---

# 4. Secondary Research Questions

The experimental program addresses the following secondary questions.

### RQ1 — Robustness

Does purification improve VQC robustness against adversarial perturbations?

### RQ2 — Clean Performance

Does purification preserve performance on clean inputs?

### RQ3 — Classical vs Quantum Purification

How does a classical autoencoder compare with a quantum autoencoder as a purification mechanism?

### RQ4 — Reconstruction vs Classification

Does lower reconstruction error necessarily correspond to better adversarial classification recovery?

### RQ5 — Attack Strength

How does purification performance change as attack strength increases?

### RQ6 — Noise

How does quantum execution noise affect the effectiveness and cost of purification?

### RQ7 — Adaptive Attacks

Does purification remain effective when the attacker explicitly accounts for the defense pipeline?

### RQ8 — Resource Trade-off

Does the robustness obtained from purification justify its additional computational or quantum complexity?

---

# 5. Experimental Scope

## 5.1 Included in v0.1

The core experimental scope is:

```text
Dataset:
MNIST

Initial task:
Binary 0 vs 1 classification

Input:
Full 28 × 28 MNIST representation

Flattened dimension:
784

Encoding:
Amplitude encoding

Initial quantum width:
10 qubits

Victim:
Variational Quantum Classifier

Attacks:
FGSM
PGD

Purification baselines:
No Defense
Classical Autoencoder
Quantum Autoencoder

Execution:
Ideal simulation
Noisy simulation

Later-stage threat evaluation:
Adaptive attack
```

---

## 5.2 Explicitly Outside Initial Scope

The following are not part of the initial experimental program:

- direct quantum-state adversarial attacks;
- hardware-level adversarial manipulation;
- gate-level fault injection;
- training-data poisoning;
- model extraction;
- membership inference;
- privacy attacks;
- denial-of-service attacks;
- supply-chain attacks;
- universal security claims;
- all possible QML architectures;
- all possible encodings;
- all quantum hardware platforms;
- real-QPU validation as a mandatory requirement.

These may become future research directions, but they are not required to establish the first QMLShield result.

---

# 6. Experimental Variables

## 6.1 Controlled Variables

The following should remain fixed whenever two conditions are intended to be compared directly:

- dataset;
- task definition;
- train/validation/test split;
- preprocessing;
- encoding;
- victim architecture;
- training protocol;
- evaluation set;
- random seed policy;
- attack budget when comparing defenses;
- measurement protocol;
- metric definitions.

A comparison is only meaningful when the compared systems differ in the intended experimental factor.

---

## 6.2 Independent Variables

Depending on the experiment, independent variables may include:

- defense type;
- attack type;
- perturbation budget;
- attack iteration count;
- quantum noise model;
- noise strength;
- circuit depth;
- purification architecture;
- training seed;
- dataset task;
- number of classes;
- model configuration.

---

## 6.3 Dependent Variables

Primary dependent variables include:

- clean accuracy;
- robust accuracy;
- attack success rate;
- reconstruction error;
- reconstruction fidelity where applicable;
- classification recovery;
- clean-performance degradation;
- robustness improvement;
- circuit depth;
- qubit count;
- execution cost;
- runtime;
- shot count where applicable.

---

# 7. Experimental Conditions

QMLShield uses three principal defense conditions.

## Condition A — No Defense

```text
Input
  ↓
Encoding
  ↓
VQC
  ↓
Prediction
```

Purpose:

Establish the vulnerable baseline.

---

## Condition B — Classical Autoencoder

```text
Input
  ↓
CAE
  ↓
Purified Input
  ↓
Encoding
  ↓
VQC
  ↓
Prediction
```

Purpose:

Establish a classical reconstruction-based defense baseline.

---

## Condition C — Quantum Autoencoder

```text
Input
  ↓
Encoding
  ↓
QAE
  ↓
Purified Quantum Representation
  ↓
VQC
  ↓
Prediction
```

Purpose:

Evaluate the quantum-native purification hypothesis.

---

# 8. Experimental Progression

The primary experiment sequence is:

```text
EXP-01
Clean VQC Baseline
      ↓
EXP-02
FGSM Vulnerability
      ↓
EXP-03
PGD Vulnerability
      ↓
EXP-04
CAE Baseline
      ↓
EXP-05
QAE Purification
      ↓
EXP-06
CAE vs QAE Comparison
      ↓
EXP-07
Noise Evaluation
      ↓
EXP-08
Adaptive Attack Evaluation
```

This sequence should be preserved unless an experiment reveals a methodological problem requiring revision.

---

# 9. EXP-01 — Clean VQC Baseline

## 9.1 Objective

Establish that the victim VQC can learn the initial MNIST classification task under clean conditions.

## 9.2 Research Purpose

Before measuring adversarial robustness, the project must establish that the classifier itself is functioning correctly.

An ineffective clean classifier cannot provide a meaningful robustness baseline.

## 9.3 Setup

```text
Dataset:
MNIST

Task:
0 vs 1

Representation:
784-dimensional flattened image

Encoding:
Amplitude encoding

Qubits:
10

Victim:
VQC

Execution:
Ideal simulation
```

## 9.4 Procedure

1. Load MNIST.
2. Select classes 0 and 1.
3. Apply the defined preprocessing.
4. Split data according to the methodology.
5. Normalize inputs according to the defined protocol.
6. Encode the input using amplitude encoding.
7. Initialize the VQC.
8. Train using the defined training protocol.
9. Evaluate on the clean test set.
10. Record all relevant metadata.

## 9.5 Primary Metrics

At minimum:

```text
Clean Accuracy
Clean Loss
```

Additional useful measurements:

```text
Training Time
Inference Time
Parameter Count
Circuit Depth
Qubit Count
```

## 9.6 Required Evidence

The experiment must establish:

- the VQC trains successfully;
- the evaluation pipeline works;
- the quantum encoding works;
- the model produces meaningful predictions;
- results are reproducible under the declared seed policy.

## 9.7 Failure Condition

If the clean VQC cannot achieve a meaningful baseline under the defined methodology, adversarial experiments should not proceed.

The problem should first be diagnosed in:

```text
Data
Preprocessing
Encoding
Circuit
Optimization
Training
Evaluation
```

---

# 10. EXP-02 — FGSM Vulnerability

## 10.1 Objective

Establish whether the trained VQC is vulnerable to classical input-space adversarial perturbations.

## 10.2 Threat Model

The attacker:

- has access to the classical input;
- can compute gradients under the white-box assumption;
- can perturb the input within a defined budget;
- cannot directly manipulate the quantum hardware;
- does not modify the trained model.

## 10.3 Pipeline

```text
Clean Input
    ↓
Gradient through VQC pipeline
    ↓
FGSM
    ↓
Adversarial Input
    ↓
Amplitude Encoding
    ↓
VQC
    ↓
Prediction
```

## 10.4 Variables

Primary variable:

```text
ε
```

The perturbation budget must be explicitly recorded.

Multiple attack strengths should be evaluated when computationally practical.

## 10.5 Metrics

```text
Clean Accuracy
Adversarial Accuracy
Attack Success Rate
Accuracy Drop
```

## 10.6 Required Evidence

The experiment should determine whether:

```text
Adversarial Accuracy < Clean Accuracy
```

under the defined attack conditions.

If FGSM does not produce meaningful degradation, the attack pipeline should be inspected before concluding that the VQC is robust.

---

# 11. EXP-03 — PGD Vulnerability

## 11.1 Objective

Evaluate the VQC under a stronger iterative adversarial attack.

## 11.2 Attack

```text
Projected Gradient Descent
```

PGD is used after FGSM because the project first establishes a simple attack path before introducing a stronger iterative attack.

## 11.3 Pipeline

```text
Clean Input
    ↓
Iterative Gradient Attack
    ↓
Projection into Allowed Perturbation Set
    ↓
Adversarial Input
    ↓
Encoding
    ↓
VQC
    ↓
Prediction
```

## 11.4 Variables

At minimum:

```text
Perturbation Budget
Step Size
Number of Iterations
```

All must be recorded.

## 11.5 Metrics

```text
Adversarial Accuracy
Attack Success Rate
Accuracy Drop
Robust Accuracy
```

## 11.6 Purpose

This experiment establishes the stronger attack baseline against which purification defenses will later be evaluated.

---

# 12. EXP-04 — Classical Autoencoder Baseline

## 12.1 Objective

Establish a classical reconstruction-based purification baseline.

## 12.2 Pipeline

```text
Clean / Adversarial Input
        ↓
Classical Autoencoder
        ↓
Reconstructed Input
        ↓
Amplitude Encoding
        ↓
VQC
        ↓
Prediction
```

## 12.3 Evaluation Conditions

The CAE should be evaluated on at least:

```text
Clean Input
Adversarial Input
```

The experiment must determine both:

1. whether the CAE removes useful clean information;
2. whether it recovers adversarial inputs sufficiently for classification.

## 12.4 Metrics

```text
Clean Accuracy
Purified Clean Accuracy
Adversarial Accuracy
Purified Adversarial Accuracy
Reconstruction Error
Clean Accuracy Degradation
Robustness Improvement
```

## 12.5 Research Role

The CAE is a necessary baseline because a quantum purification mechanism should not be judged only against no defense.

The comparison must determine whether quantum-native purification provides behavior beyond a classical reconstruction baseline.

---

# 13. EXP-05 — Quantum Autoencoder Purification

## 13.1 Objective

Evaluate the QAE as a quantum-native purification mechanism.

## 13.2 Pipeline

```text
Classical Input
      ↓
Amplitude Encoding
      ↓
Quantum State
      ↓
QAE
      ↓
Purified Quantum Representation
      ↓
VQC
      ↓
Prediction
```

## 13.3 Core Hypothesis

The experiment tests whether QAE-based purification can:

1. preserve useful information from clean inputs;
2. reduce the impact of adversarial perturbations;
3. recover classifier performance;
4. do so with an acceptable resource cost.

The experiment must not assume these outcomes in advance.

## 13.4 Evaluation Conditions

At minimum:

```text
Clean Input → QAE → VQC
Adversarial Input → QAE → VQC
```

## 13.5 Metrics

```text
Clean Accuracy
Purified Clean Accuracy
Adversarial Accuracy
Purified Adversarial Accuracy
Reconstruction / Fidelity Measure
Accuracy Recovery
Clean Degradation
Circuit Depth
Qubit Count
Execution Cost
```

---

# 14. EXP-06 — CAE vs QAE Comparison

## 14.1 Objective

Directly compare the classical and quantum purification mechanisms under matched experimental conditions.

## 14.2 Comparison

```text
                 Clean       Adversarial
                   ↓              ↓
                ┌─────────────────────┐
                │                     │
             No Defense            Defense
                                    ↓
                         ┌──────────┴──────────┐
                         ↓                     ↓
                       CAE                   QAE
                         ↓                     ↓
                       VQC                   VQC
```

## 14.3 Fairness Requirements

CAE and QAE comparisons must control, as far as reasonably possible:

- dataset;
- task;
- train/test split;
- victim classifier;
- attack;
- attack budget;
- evaluation data;
- random seed policy;
- metric definitions.

Differences should be attributable to the purification mechanism rather than unrelated experimental changes.

## 14.4 Primary Comparison Metrics

### Clean preservation

```text
ΔClean = Clean Accuracy after defense
          -
          Clean Accuracy without defense
```

### Robustness improvement

```text
ΔRobust = Robust Accuracy after defense
          -
          Robust Accuracy without defense
```

### Accuracy recovery

```text
Recovery =
(Purified Adversarial Accuracy
 -
Raw Adversarial Accuracy)
/
(Clean Accuracy
 -
Raw Adversarial Accuracy)
```

The exact reporting convention must remain consistent across experiments.

## 14.5 Resource Comparison

Compare:

```text
Qubit Count
Circuit Depth
Parameter Count
Runtime
Inference Cost
Training Cost
Shot Count
```

where applicable.

The objective is not to maximize robustness in isolation.

The project must study the robustness–cost trade-off.

---

# 15. EXP-07 — Noise Evaluation

## 15.1 Objective

Determine how controlled quantum noise affects:

- VQC performance;
- purification;
- robustness;
- reconstruction;
- and resource cost.

## 15.2 Important Distinction

Quantum noise is **not** the same thing as an adversarial perturbation.

They must remain separate experimental concepts.

```text
Adversarial Perturbation
        ↓
Intentional attacker-controlled input modification

Quantum Noise
        ↓
Execution-level stochastic / physical imperfection
```

## 15.3 Experimental Progression

Noise should be introduced progressively:

```text
Ideal
  ↓
Low Noise
  ↓
Medium Noise
  ↓
Higher Noise
```

The exact noise model and strength must be explicitly specified.

## 15.4 Conditions

At minimum compare:

```text
No Defense
CAE
QAE
```

under controlled noise conditions when computationally feasible.

## 15.5 Metrics

```text
Clean Accuracy
Robust Accuracy
Accuracy Degradation
Purification Recovery
Reconstruction / Fidelity
Circuit Depth
Execution Cost
```

## 15.6 Research Question

The experiment should determine whether the additional quantum circuitry required by purification introduces a cost under noisy execution that changes the defense trade-off.

A defense that performs well ideally but collapses under realistic noise should be reported as such.

---

# 16. EXP-08 — Adaptive Attack Evaluation

## 16.1 Objective

Determine whether purification remains effective when the attacker explicitly accounts for the defense.

## 16.2 Motivation

A non-adaptive attack evaluates:

```text
Attack → VQC
```

or:

```text
Attack → Purifier → VQC
```

without necessarily optimizing against the complete defended pipeline.

An adaptive attacker instead considers:

```text
Attack → Purifier → VQC
```

as the actual target.

## 16.3 Threat Model

The attacker is assumed to know the defense mechanism under a white-box setting.

The attacker can optimize perturbations against the complete differentiable pipeline where technically feasible.

## 16.4 Pipeline

```text
Clean Input
      ↓
Adaptive Attack
      ↓
Adversarial Input
      ↓
Purifier
      ↓
VQC
      ↓
Prediction
```

## 16.5 Interpretation

Possible outcomes include:

```text
Defense remains robust
Defense partially degrades
Defense fails
```

All three are scientifically meaningful.

A defense that succeeds against non-adaptive attacks but fails against adaptive attacks should not be described as universally robust.

---

# 17. Experimental Matrix

The core matrix is:

| Experiment | Model | Attack | Defense | Noise | Primary Purpose |
|---|---|---|---|---|---|
| EXP-01 | VQC | None | None | Ideal | Establish clean baseline |
| EXP-02 | VQC | FGSM | None | Ideal | Establish vulnerability |
| EXP-03 | VQC | PGD | None | Ideal | Establish stronger vulnerability |
| EXP-04 | VQC | FGSM/PGD | CAE | Ideal | Classical purification baseline |
| EXP-05 | VQC | FGSM/PGD | QAE | Ideal | Quantum purification evaluation |
| EXP-06 | VQC | FGSM/PGD | CAE vs QAE | Ideal | Fair defense comparison |
| EXP-07 | VQC | FGSM/PGD | None/CAE/QAE | Noisy | Noise sensitivity |
| EXP-08 | VQC | Adaptive | CAE/QAE | Ideal/controlled | Adaptive robustness |

The matrix can expand after the baseline program is stable.

---

# 18. Experimental Controls

The following controls are essential.

## 18.1 No-Defense Control

Always preserve:

```text
No Defense
```

as a reference condition.

Without it, defense improvement cannot be interpreted correctly.

## 18.2 Clean Control

Always evaluate:

```text
Clean → System → Prediction
```

A defense must not be considered successful merely because it improves adversarial accuracy while substantially damaging clean accuracy.

## 18.3 Attack Control

The same attack configuration must be used when comparing defenses.

For example:

```text
Same ε
Same step size
Same iteration count
Same evaluation samples
```

unless the experiment explicitly studies one of these variables.

## 18.4 Seed Control

Random seeds should be explicitly recorded.

Where practical, important comparisons should use multiple seeds.

A single seed should not be treated as definitive evidence when the result is sensitive to initialization.

---

# 19. Repeated Runs

Research-critical experiments should eventually be repeated across multiple seeds.

A suitable progression is:

```text
Development:
1 seed

Smoke experiment:
1–2 seeds

Initial research result:
multiple seeds

Final comparison:
multiple seeds + uncertainty reporting
```

The exact number of seeds is an experimental decision and should be recorded rather than silently assumed.

---

# 20. Ablation Strategy

Ablation studies should be introduced only after the primary pipeline is stable.

Potential ablations include:

```text
Attack strength
Circuit depth
Qubit count
Purifier capacity
Training data
Noise strength
Noise model
Encoding configuration
VQC architecture
```

The purpose of an ablation is to determine which component materially contributes to the observed result.

---

# 21. Attack-Strength Sweep

For both FGSM and PGD, attack strength should be studied as a variable where computationally feasible.

Conceptually:

```text
ε₁
 ↓
ε₂
 ↓
ε₃
 ↓
...
```

The result should reveal whether purification:

- works only for weak attacks;
- remains effective across a useful range;
- degrades gradually;
- or fails beyond a threshold.

---

# 22. Circuit-Depth Sweep

Circuit depth is particularly important for quantum purification.

Potential progression:

```text
Depth 1
Depth 2
Depth 3
...
```

The exact range depends on simulation cost and the selected architecture.

The objective is not simply to maximize depth.

The experiment should determine whether additional expressive capacity produces meaningful robustness gains relative to:

```text
Noise
Runtime
Optimization difficulty
Circuit complexity
```

---

# 23. Qubit-Count Consideration

The initial full-MNIST amplitude-encoding configuration uses:

```text
10 qubits
```

because:

```text
784 ≤ 1024
```

This is the primary representation for the intended full-MNIST experiment.

Smaller qubit configurations may be used for:

- debugging;
- rapid smoke tests;
- architecture development;
- controlled ablations.

A reduced-dimensional experiment must not silently replace the full-MNIST research setting.

Its role must be explicitly documented.

---

# 24. Data Splits

The dataset split must be fixed before comparative experiments.

The project should maintain a clear distinction between:

```text
Training Set
Validation Set
Test Set
```

The test set must not be used to tune:

- attack budgets;
- purifier architecture;
- hyperparameters;
- defense thresholds;
- model selection.

If a parameter is selected using validation performance, that selection process must be documented.

---

# 25. Purifier Training Protocol

Purifiers must have a clearly defined training regime.

For each purifier, document:

```text
Training data
Training objective
Architecture
Latent representation
Optimization method
Learning rate
Epochs
Batch size
Seed
Stopping rule
```

Most importantly, distinguish:

```text
Training-time information
```

from:

```text
Evaluation-time information
```

A purifier must not receive unavailable test-time labels unless the threat model explicitly allows it.

---

# 26. Clean and Adversarial Purification

The purifier must be evaluated separately on:

```text
Clean inputs
```

and:

```text
Adversarial inputs
```

This distinction is essential.

A purifier that reconstructs adversarial examples well but damages clean inputs may not be a useful defense.

Likewise, a purifier that preserves clean images perfectly but fails to recover adversarial inputs does not satisfy the defense objective.

---

# 27. Reconstruction vs Classification

QMLShield must distinguish between:

```text
Reconstruction Quality
```

and:

```text
Classification Recovery
```

A purifier can produce visually or numerically similar reconstructions without restoring the VQC decision.

Therefore:

```text
Reconstruction Metric
        ≠
Robustness Metric
```

Both should be recorded where applicable.

---

# 28. Primary Metrics

## 28.1 Clean Accuracy

```text
ACC_clean
```

Measures classification performance on clean inputs.

## 28.2 Adversarial Accuracy

```text
ACC_adv
```

Measures classification performance on adversarial inputs.

## 28.3 Robust Accuracy

For the defined attack condition:

```text
Robust Accuracy
=
Accuracy on adversarial examples
```

The attack configuration must always accompany the metric.

## 28.4 Attack Success Rate

Measures the proportion of attack attempts that achieve the defined attack objective.

The exact definition must remain consistent with the attack and task.

## 28.5 Clean Degradation

A defense-induced clean performance change:

```text
Δclean =
ACC_clean,defense
-
ACC_clean,nodefense
```

A strongly negative value indicates clean-performance damage.

## 28.6 Robustness Improvement

```text
Δrobust =
ACC_adv,defense
-
ACC_adv,nodefense
```

This measures improvement relative to the no-defense baseline.

## 28.7 Reconstruction Error

Used primarily for CAE/QAE evaluation where an appropriate reconstruction representation exists.

The exact metric must be declared per experiment.

## 28.8 Accuracy Recovery

A defense can be evaluated by how much of the attack-induced accuracy loss it recovers.

A normalized recovery metric may be used when appropriate, but the raw accuracies must also be reported.

---

# 29. Resource Metrics

Because QMLShield studies quantum-native defenses, robustness alone is insufficient.

Record, where applicable:

```text
Number of Qubits
Circuit Depth
Trainable Parameters
Training Time
Inference Time
Simulation Time
Shot Count
Memory Usage
```

For fair comparisons, clearly distinguish:

```text
Classical cost
Quantum circuit cost
Total experimental cost
```

---

# 30. Result Recording

Each experiment should produce structured metadata.

At minimum:

```text
experiment_id
timestamp
git_commit
python_version
dependency_environment
dataset
task
seed
model
encoding
attack
attack_parameters
defense
purifier_parameters
noise
noise_parameters
metrics
resource_metrics
```

The exact storage schema may evolve.

The important requirement is that results remain traceable.

---

# 31. Experiment Naming

Use stable experiment identifiers:

```text
EXP-01
EXP-02
EXP-03
...
```

Experiment runs may use a more specific identifier, for example:

```text
EXP-05_seed-01
EXP-05_seed-02
EXP-05_seed-03
```

If attack or noise sweeps are performed, the parameter values should be included in structured metadata rather than relying only on filenames.

---

# 32. Results Directory

Experiment outputs should follow the repository structure:

```text
results/
├── raw/
├── processed/
├── figures/
└── tables/
```

### `raw/`

Original experiment outputs.

### `processed/`

Cleaned and aggregated data used for analysis.

### `figures/`

Research figures.

### `tables/`

Research tables.

Raw results should not be overwritten by processed results.

---

# 33. Experiment Artifacts

A reproducible experiment should ideally leave:

```text
Configuration
Code version
Environment information
Raw metrics
Processed metrics
Figures
Tables
Notes
```

The goal is to make a result traceable rather than merely visually reproducible.

---

# 34. Experiment Configuration

Experiments should use configuration rather than scattered hard-coded scientific parameters.

Conceptual example:

```yaml
dataset:
  name: mnist
  classes: [0, 1]

encoding:
  type: amplitude
  qubits: 10

model:
  type: vqc

attack:
  type: fgsm
  epsilon: ...

purification:
  type: qae

noise:
  enabled: false
```

This is illustrative.

The actual configuration schema is defined by the implementation and should evolve with the experiments.

---

# 35. Small-to-Large Execution Strategy

Every new experiment should progress through:

```text
1. Unit-level validation
        ↓
2. Tiny smoke run
        ↓
3. Small research run
        ↓
4. Full experiment
        ↓
5. Repeated runs
```

This reduces the risk of discovering a simple implementation bug only after an expensive quantum simulation.

---

# 36. Experiment Failure Handling

A failed experiment must not be silently discarded if it provides information about the system.

Record:

```text
Failure
Cause
Affected condition
Whether the failure is implementation or scientific
Corrective action
Whether previous results are invalidated
```

If a bug invalidates previous results, the affected results must be clearly marked.

---

# 37. Experiment Reproducibility

A result should be reproducible from:

```text
Git revision
+
uv.lock
+
Configuration
+
Dataset definition
+
Seed
+
Experiment identifier
```

Where external environment differences may matter, record the relevant environment information.

---

# 38. Fair Comparison Protocol

When comparing:

```text
No Defense
CAE
QAE
```

the following principle applies:

> **Change the defense mechanism, not the rest of the experiment.**

The same:

```text
Dataset
Task
Test samples
Victim VQC
Attack
Attack strength
Evaluation metrics
```

should be used whenever the research question requires a direct comparison.

---

# 39. Statistical Reporting

For repeated runs, report more than a single number when appropriate.

Preferred reporting includes:

```text
Mean
Standard Deviation
```

or another justified uncertainty measure.

Plots should communicate variation rather than hiding it when multiple runs exist.

A small numerical difference should not automatically be interpreted as a meaningful scientific improvement.

---

# 40. Interpretation Rules

Results must be interpreted relative to the experimental condition.

Do not write:

```text
QAE is robust.
```

when the evidence only supports:

```text
QAE improved robust accuracy against the evaluated attack at the tested perturbation strengths under the tested simulation conditions.
```

Do not write:

```text
Quantum purification is better.
```

when the evidence only supports:

```text
QAE achieved higher robust accuracy under the evaluated configuration, with additional circuit and execution cost.
```

Claims must match evidence.

---

# 41. Negative Results

Negative results are explicitly valid.

Examples:

```text
QAE does not outperform CAE.
QAE damages clean accuracy.
Purification fails under stronger attacks.
Noise removes the observed robustness gain.
Adaptive attacks bypass the defense.
Higher circuit depth does not improve robustness.
```

Such results can still answer meaningful research questions.

The project should investigate why the result occurs rather than changing the methodology simply to obtain a positive result.

---

# 42. Success Criteria

QMLShield does not define success as:

```text
QAE > CAE
```

Instead, the research succeeds if the experiments can reliably determine:

1. whether purification improves robustness;
2. whether clean performance is preserved;
3. how CAE and QAE differ;
4. how attack strength affects the defenses;
5. how quantum noise affects the result;
6. whether adaptive attacks change the conclusion;
7. what resource trade-offs are involved.

---

# 43. Decision Gates

The experiment sequence contains explicit gates.

## Gate 1

Proceed beyond EXP-01 only if the VQC baseline is valid.

## Gate 2

Proceed to defense experiments only after vulnerability under the selected attack has been established or the attack limitation is understood.

## Gate 3

Compare CAE and QAE only after both pipelines are independently functional.

## Gate 4

Introduce noise only after the ideal execution pipeline is stable.

## Gate 5

Introduce adaptive attacks only after the non-adaptive defense comparison is stable.

These gates prevent later complexity from masking earlier implementation problems.

---

# 44. Experimental Evidence Chain

The intended evidence chain is:

```text
EXP-01
"Can the victim classify?"

        ↓

EXP-02 / EXP-03
"Is the victim vulnerable?"

        ↓

EXP-04
"What does a classical purifier achieve?"

        ↓

EXP-05
"What does a quantum purifier achieve?"

        ↓

EXP-06
"How do they compare?"

        ↓

EXP-07
"Does quantum noise change the result?"

        ↓

EXP-08
"Does an adaptive attacker change the result?"
```

This chain directly supports the research questions.

---

# 45. RQ-to-Experiment Mapping

| Research Question | Primary Evidence |
|---|---|
| RQ1 — Robustness | EXP-02, EXP-03, EXP-04, EXP-05, EXP-06 |
| RQ2 — Clean Performance | EXP-01, EXP-04, EXP-05, EXP-06 |
| RQ3 — CAE vs QAE | EXP-06 |
| RQ4 — Reconstruction vs Classification | EXP-04, EXP-05, EXP-06 |
| RQ5 — Attack Strength | EXP-02, EXP-03, attack sweeps |
| RQ6 — Noise | EXP-07 |
| RQ7 — Adaptive Attacks | EXP-08 |
| RQ8 — Resource Trade-off | EXP-06, EXP-07 |

---

# 46. Minimum Viable Research Result

A minimum credible QMLShield result should contain:

```text
1. Clean VQC baseline
2. Adversarial vulnerability measurement
3. No-defense control
4. CAE baseline
5. QAE evaluation
6. Matched comparison
7. Clean-performance analysis
8. Robustness analysis
9. Resource analysis
10. Reproducible configuration
```

Noise and adaptive evaluation strengthen the result substantially but belong to later stages of the progression.

---

# 47. Extended Research Result

A stronger research result should additionally include:

```text
Multiple seeds
Attack-strength sweeps
Noise experiments
Adaptive attacks
Ablation studies
Resource trade-off analysis
Uncertainty reporting
Failure analysis
```

The project should not claim this level of evidence before it has been produced.

---

# 48. Future Experimental Extensions

After v0.1, possible extensions include:

```text
Fashion-MNIST
Multi-class MNIST
Alternative encodings
Alternative VQC architectures
Alternative QAE architectures
qGAN purification
Additional attack algorithms
Additional adaptive attacks
Alternative noise models
Real QPU validation
Cross-framework validation
```

These are extensions, not requirements for the first result.

---

# 49. qGAN Extension Boundary

A qGAN-based purifier is a potential future experiment rather than part of the initial QAE baseline.

If introduced, it should become a new experimental condition rather than replacing QAE.

Conceptually:

```text
No Defense
      ↓
CAE
      ↓
QAE
      ↓
qGAN
```

The comparison should preserve the same research discipline:

```text
Same task
Same victim
Same attacks
Same evaluation protocol
Comparable resource accounting
```

The purpose would be to determine whether the additional complexity of qGAN produces a meaningful defense advantage.

---

# 50. Generalization Experiments

Generalization should only be tested after the primary MNIST pipeline is stable.

Potential progression:

```text
MNIST 0 vs 1
      ↓
MNIST multi-class
      ↓
Fashion-MNIST
```

A defense that works only under a single narrow task should be reported as such.

---

# 51. Hardware Validation Boundary

Real QPU experiments should answer a concrete question.

Examples:

```text
Does the defense survive realistic execution noise?
Does circuit depth make the defense impractical?
Does the simulated robustness survive hardware execution?
```

A real-QPU experiment should not be performed merely to make the project appear more quantum.

---

# 52. Experiment Documentation Requirements

Each completed experiment should have enough documentation to answer:

```text
What was tested?
Why was it tested?
What was held constant?
What changed?
How was it executed?
What was measured?
What happened?
What does the result support?
What does it not support?
What should happen next?
```

The final interpretation should be transferred into:

```text
docs/findings.md
```

---

# 53. Relationship to Other Documents

```text
research_questions.md
        ↓
Research objectives

threat_model.md
        ↓
Attacker and security assumptions

methodology.md
        ↓
Experimental methodology

architecture.md
        ↓
Implementation structure

rules.md
        ↓
Research and engineering constraints

tech_stack.md
        ↓
Technology environment

experiments.md
        ↓
Concrete experimental program

findings.md
        ↓
Observed evidence and conclusions
```

`experiments.md` is therefore the operational bridge between the research design and the actual execution of QMLShield.

---

# 54. Current Status

**Status:** Experimental protocol defined.

The core sequence is:

```text
EXP-01 → Clean VQC
EXP-02 → FGSM
EXP-03 → PGD
EXP-04 → CAE
EXP-05 → QAE
EXP-06 → CAE vs QAE
EXP-07 → Noise
EXP-08 → Adaptive Attack
```

No scientific conclusion should be attached to these experiments before their corresponding results are actually generated.

---

# 55. Final Experimental Principle

The purpose of QMLShield is not to prove that a particular purification mechanism is superior.

The purpose is to establish evidence about:

```text
Robustness
+
Clean Performance
+
Attack Strength
+
Purification Behavior
+
Noise Sensitivity
+
Adaptive Security
+
Quantum Resource Cost
```

The central discipline is:

> **Measure first. Compare fairly. Report failures honestly. Make claims only as strong as the evidence permits.**

That principle governs the entire QMLShield experimental program.
