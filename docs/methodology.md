# Methodology

> **Project:** QMLShield  
> **Domain:** Quantum Machine Learning Security  
> **Focus:** Adversarial Robustness & Purification for Quantum Machine Learning  
> **Research Stage:** QMLShield v0.1  
> **Status:** Research in progress

---

## 1. Purpose

This document defines how QMLShield will investigate the research questions established in `research_questions.md` under the threat model defined in `threat_model.md`.

It translates the research questions into:

- an experimental pipeline;
- controlled variables;
- baseline configurations;
- attack procedures;
- purification procedures;
- noise evaluation;
- metrics;
- reproducibility requirements;
- and a staged research protocol.

The methodology is intentionally progressive.

The project should establish a clean and reproducible VQC baseline before introducing adversarial attacks, purification, or quantum noise.

---

## 2. Methodological Principle

QMLShield follows the principle:

> **First establish the mechanism. Then stress the mechanism.**

The research progression is:

```text
Clean VQC
   ↓
Adversarial Attack
   ↓
Classical Purification Baseline
   ↓
Quantum Purification
   ↓
Robustness Evaluation
   ↓
Noise Stress Testing
   ↓
Adaptive Attack
```

Each stage should be validated before the next stage is introduced.

The purpose is to prevent multiple sources of failure from being introduced simultaneously.

---

## 3. Research Pipeline

The core QMLShield pipeline is:

```text
MNIST
  ↓
Preprocessing
  ↓
Normalization
  ↓
Amplitude Encoding
  ↓
10-Qubit Quantum Representation
  ↓
VQC
  ↓
Prediction
```

The adversarial pipeline is:

```text
MNIST
  ↓
Preprocessing
  ↓
Adversarial Attack
  ↓
Adversarial Input
  ↓
Amplitude Encoding
  ↓
VQC
  ↓
Prediction
```

The purification experiments introduce two alternative defense locations:

### Classical-domain purification

```text
MNIST
  ↓
Adversarial Attack
  ↓
CAE
  ↓
Amplitude Encoding
  ↓
VQC
  ↓
Prediction
```

### Quantum-domain purification

```text
MNIST
  ↓
Adversarial Attack
  ↓
Amplitude Encoding
  ↓
QAE
  ↓
VQC
  ↓
Prediction
```

The undefended adversarial pipeline remains the primary control condition.

---

## 4. Dataset

### 4.1 Primary Dataset

The primary dataset is **MNIST**.

MNIST contains grayscale handwritten digit images with a native spatial resolution of:

```text
28 × 28 pixels
```

The native flattened representation therefore contains:

```text
28 × 28 = 784 features
```

QMLShield retains MNIST as the primary research dataset rather than defining the project around a reduced 8×8 dataset.

---

### 4.2 Initial Classification Task

The first implementation stage should use a controlled binary classification task.

Recommended initial task:

```text
Class 0 vs Class 1
```

The reason is experimental isolation rather than a claim that binary classification represents the full MNIST problem.

After the complete pipeline is stable, the research can progressively expand to:

```text
Binary classification
      ↓
Four-class classification
      ↓
Ten-class MNIST
```

The class configuration used in each experiment must be explicitly recorded.

---

### 4.3 Future Dataset

**Fashion-MNIST** is reserved as a later generalization dataset.

It should not be introduced until the primary MNIST pipeline is stable.

The purpose of the later dataset is to test whether observed behavior is specific to MNIST or persists across a related image-classification domain.

---

## 5. Preprocessing

The preprocessing pipeline must be deterministic and explicitly documented.

Initial conceptual pipeline:

```text
28 × 28 MNIST image
        ↓
Flatten
        ↓
784-dimensional vector
        ↓
Normalization
        ↓
Amplitude encoding
```

Preprocessing must preserve the semantic identity of the image while producing a valid numerical representation for the selected encoding.

The exact normalization operation must be fixed before attack evaluation because changing normalization after attack generation can invalidate comparisons.

The preprocessing implementation must be shared across:

- clean evaluation;
- adversarial evaluation;
- CAE experiments;
- QAE experiments.

---

## 6. Quantum Encoding

### 6.1 Primary Encoding

The initial encoding method is **amplitude encoding**.

For a normalized input vector:

```text
x ∈ R^784
```

the vector is mapped to a quantum state whose amplitudes represent the normalized input values.

The number of qubits is determined by the dimension of the Hilbert space:

```text
2^10 = 1024 ≥ 784
```

Therefore, the initial full-MNIST representation uses:

```text
10 qubits
```

The unused state-space dimensions must be handled consistently by the encoding implementation.

The choice of 10 qubits is a consequence of the selected 784-dimensional representation and amplitude encoding. It is not a property of MNIST itself.

---

### 6.2 Encoding Invariant

The same encoding procedure must be used when comparing:

- no defense;
- CAE;
- QAE;
- different attack strengths;
- and noise conditions,

unless an experiment explicitly studies encoding variation.

Encoding changes should therefore be treated as an experimental variable rather than introduced implicitly.

---

## 7. Victim Model

### 7.1 Model Type

The primary victim model is a **Variational Quantum Classifier (VQC)**.

The VQC is the system whose classification integrity is being protected.

The methodology does not treat VQA as a separate model family. Variational optimization is the training/optimization paradigm used to train the parameterized quantum circuit.

---

### 7.2 Initial VQC Design

The initial VQC should prioritize:

- reproducibility;
- manageable simulation cost;
- sufficient trainable capacity for the selected task;
- explicit circuit structure;
- compatibility with gradient-based training;
- compatibility with the selected quantum simulator.

A small hardware-efficient parameterized circuit is preferred for the first implementation.

The initial circuit configuration should explicitly record:

```text
Number of qubits
Number of variational layers
Parameterized gates
Entanglement pattern
Measurement scheme
Parameter initialization
Training optimizer
Learning rate
Number of training epochs
Batch strategy
Random seed
```

The exact architecture should be fixed before the main adversarial comparison.

---

### 7.3 Victim Model Invariance

When comparing defense mechanisms, the victim VQC should remain unchanged whenever possible.

The purpose is to compare:

```text
No Defense
vs
CAE
vs
QAE
```

rather than simultaneously comparing different classifiers.

---

## 8. Training Protocol

The VQC training process should be separated from adversarial evaluation.

The initial procedure is:

```text
Training Dataset
      ↓
VQC Training
      ↓
Best / Final Model
      ↓
Clean Validation
      ↓
Adversarial Evaluation
```

The default research principle is:

> **Do not retrain the victim VQC on adversarial examples unless the experiment explicitly studies adversarial training.**

This is important because QMLShield v0.1 is primarily investigating purification rather than adversarial retraining.

If adversarial training is introduced later, it must be treated as a separate baseline.

---

## 9. Data Splitting

The dataset should be separated into distinct roles:

```text
Training Set
Validation Set
Test Set
```

The training set is used to train the VQC and purification models according to the experiment.

The validation set is used for model-selection decisions where applicable.

The test set is reserved for final evaluation.

Test data should not be used to tune attack budgets, purification hyperparameters, or model-selection decisions.

The exact split must be fixed and reproducible.

---

## 10. Randomness and Reproducibility

Quantum ML experiments can be sensitive to initialization and stochastic training behavior.

QMLShield must explicitly control and record randomness where supported.

The experiment configuration should record:

```text
Global seed
Dataset split seed
Model initialization seed
Attack seed, if applicable
Purification training seed
Simulator seed, if applicable
```

A single seed should not be treated as sufficient evidence of general robustness.

For important final experiments, multiple independent seeds should be used to estimate variability.

The number of seeds becomes part of the experimental protocol once the baseline pipeline is stable.

---

## 11. Adversarial Attack Methodology

### 11.1 Attack Objective

The initial attacker modifies the classical input before quantum encoding.

Conceptually:

```text
x_adv = x + δ
```

subject to a bounded perturbation constraint.

The attack objective is to cause the VQC to produce an incorrect prediction.

The attack is evaluated through the complete victim path:

```text
x
 ↓
preprocessing
 ↓
encoding
 ↓
VQC
 ↓
loss / prediction
```

The attack must therefore account for the actual model pipeline rather than treating the quantum circuit as an isolated black box.

---

### 11.2 FGSM

FGSM is the first attack to implement.

Conceptually:

```text
x_adv = x + ε · sign(∇x L(θ, x, y))
```

with the result projected or clipped to the valid input domain as required by the chosen input representation.

The exact loss function, epsilon values, clipping rule, and preprocessing interaction must be recorded in the experiment configuration.

FGSM serves primarily as the first validation attack because it allows the attack pipeline to be tested before introducing an iterative attack.

---

### 11.3 PGD

PGD is introduced after FGSM is validated.

Conceptually:

```text
x_0 = x

x_(t+1) =
    Projection(x_t + α · update(∇x L))
```

where the perturbation remains inside the selected attack budget.

The experimental configuration must record:

- perturbation budget;
- step size;
- number of iterations;
- initialization strategy;
- clipping / projection rule;
- loss function.

---

### 11.4 Attack Strength

Attack strength must be treated as an experimental variable.

A defense should not be evaluated at only one arbitrary perturbation strength.

The experiment should examine a controlled range of perturbation budgets after the attack implementation has been validated.

The exact values should be chosen based on:

- numerical stability;
- validity of the input domain;
- attack success;
- comparability across experiments.

---

## 12. Purification Methodology

QMLShield evaluates purification as a preprocessing defense against adversarial inputs.

The central comparison is:

```text
No Defense
CAE
QAE
```

---

### 12.1 Classical Autoencoder Baseline

The CAE operates in the classical input domain:

```text
x_adv
  ↓
CAE
  ↓
x_hat
  ↓
Amplitude Encoding
  ↓
VQC
```

The CAE serves as a baseline for determining whether purification itself provides value independently of quantum-domain processing.

The CAE should be trained using clean data according to the selected reconstruction objective.

Its architecture and parameter count should be documented sufficiently to make the comparison reproducible.

---

### 12.2 Quantum Autoencoder

The QAE operates after amplitude encoding:

```text
x_adv
  ↓
Amplitude Encoding
  ↓
|ψ_adv>
  ↓
QAE
  ↓
|ψ_hat>
  ↓
VQC
```

The QAE is the primary quantum-native purification mechanism in QMLShield v0.1.

The QAE should be trained to learn useful structure from clean encoded data.

The methodology must not assume that adversarial information is automatically eliminated by a discarded subsystem.

Instead, QAE purification is treated as an empirical hypothesis:

> A QAE may learn a representation in which useful clean-data structure is retained while some components associated with adversarial perturbations are reduced or discarded.

Whether this occurs must be measured experimentally.

---

### 12.3 Purifier Training

Unless a specific experiment states otherwise, purification models should be trained independently from the adversarial test examples.

The primary training principle is:

```text
Clean Training Data
       ↓
Train Purifier
       ↓
Freeze / Select Purifier
       ↓
Generate Adversarial Test Inputs
       ↓
Purify
       ↓
Evaluate VQC
```

This prevents direct leakage of the adversarial test set into purifier training.

If adversarial training of the purifier is later investigated, it must be defined as a separate experimental condition.

---

## 13. Non-Adaptive Defense Evaluation

The initial purification experiments use a non-adaptive attacker.

The conceptual flow is:

```text
Clean Input
    ↓
Attack optimized for VQC
    ↓
Adversarial Input
    ↓
Purifier
    ↓
VQC
```

The attacker does not explicitly optimize through the purifier.

This condition establishes the first evidence about purification effectiveness.

It is not considered evidence of robustness against an attacker who knows the complete defense pipeline.

---

## 14. Adaptive Attack Evaluation

Adaptive attacks are a later-stage experiment.

The adaptive attacker targets:

```text
Purifier + VQC
```

rather than only:

```text
VQC
```

The conceptual pipeline becomes:

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
```

The purpose is to determine whether the defense provides robustness beyond obscurity or non-adaptive assumptions.

A defense that fails under adaptive attack should be reported as such.

---

## 15. Quantum Noise Methodology

Quantum noise is introduced only after the ideal-simulation defense pipeline is validated.

The progression is:

```text
Stage A
Ideal / noiseless simulation

        ↓

Stage B
Noisy simulation

        ↓

Stage C
Real QPU, if justified
```

The first noisy experiments should introduce one controlled noise condition at a time.

The exact noise model must be documented together with:

- affected gates;
- error parameters;
- measurement effects;
- noise placement;
- simulator/backend;
- shot count.

Noise should be treated as a separate experimental axis rather than silently mixed into adversarial attack results.

---

## 16. Core Experimental Matrix

The first controlled matrix is:

| Attack | Noise | Defense |
|---|---|---|
| Clean | None | No Defense |
| None | None | CAE |
| None | None | QAE |
| FGSM | None | No Defense |
| FGSM | None | CAE |
| FGSM | None | QAE |
| PGD | None | No Defense |
| PGD | None | CAE |
| PGD | None | QAE |
| FGSM | Noise | No Defense |
| FGSM | Noise | CAE |
| FGSM | Noise | QAE |
| PGD | Noise | No Defense |
| PGD | Noise | CAE |
| PGD | Noise | QAE |

Not every cell needs to be executed immediately.

The matrix is a conceptual experimental design. Execution should proceed progressively.

---

## 17. Baseline Hierarchy

QMLShield should maintain three baseline levels.

### Baseline 0 — Clean VQC

```text
MNIST
 ↓
Encoding
 ↓
VQC
```

Purpose:

- validate dataset;
- validate encoding;
- validate VQC;
- establish clean performance.

### Baseline 1 — Attacked VQC

```text
MNIST
 ↓
Attack
 ↓
Encoding
 ↓
VQC
```

Purpose:

- establish attack effectiveness;
- measure baseline robustness degradation.

### Baseline 2 — Purification Comparison

```text
Attack
 ↓
┌───────────────┬───────────────┬───────────────┐
│ No Defense    │     CAE       │      QAE      │
└───────────────┴───────────────┴───────────────┘
                        ↓
                       VQC
```

Purpose:

- isolate purification effectiveness;
- compare classical and quantum purification.

---

## 18. Evaluation Metrics

### 18.1 Clean Accuracy

The proportion of clean test samples classified correctly.

```text
Clean Accuracy =
Correct Clean Predictions / Total Clean Samples
```

This is required for every defense condition.

---

### 18.2 Robust Accuracy

The proportion of adversarial test samples that remain correctly classified.

```text
Robust Accuracy =
Correct Adversarial Predictions / Total Adversarial Samples
```

This is the primary robustness metric.

---

### 18.3 Attack Success Rate

The proportion of eligible adversarial samples for which the attack causes the intended failure.

The exact definition must account for the baseline correctness of the original sample.

This metric should not be interpreted without reporting the clean accuracy of the victim.

---

### 18.4 Clean Accuracy Degradation

Measures the performance cost introduced by purification:

```text
ΔClean =
Clean Accuracy_without_defense
-
Clean Accuracy_with_defense
```

The sign convention should remain consistent across reports.

---

### 18.5 Robustness Improvement

Measures the improvement over the undefended attacked baseline:

```text
ΔRobust =
Robust Accuracy_defense
-
Robust Accuracy_no_defense
```

This provides a direct comparison between defense and control.

---

### 18.6 Reconstruction Quality

Depending on the representation and defense, evaluate:

- reconstruction error;
- fidelity or similarity;
- clean-to-purified distance;
- adversarial-to-purified distance.

The exact metric should be selected according to whether the purifier operates in classical or quantum representation space.

A reconstruction metric must not be assumed to be interchangeable between CAE and QAE without considering the representation domain.

---

### 18.7 Resource Overhead

At minimum, the project should record:

- qubit count;
- circuit depth;
- number of trainable parameters;
- execution time;
- measurement shots where applicable.

For training-based purification, training cost should also be reported when practical.

---

## 19. Fair Comparison Protocol

Defense comparisons should control:

```text
Dataset
Data split
Preprocessing
Encoding
Victim VQC
Training protocol
Test samples
Attack algorithm
Attack objective
Attack budget
Random seed
Evaluation metrics
```

The defense itself should be the principal changed variable whenever the experiment is intended as a direct defense comparison.

If a variable must change, the reason must be recorded.

---

## 20. Statistical and Repeated-Run Evaluation

Quantum ML training and simulation may exhibit variability due to initialization and stochastic procedures.

For exploratory experiments:

```text
Single seed
```

may be sufficient to validate the implementation.

For important comparative results:

```text
Multiple independent seeds
```

should be used.

Final reported results should include an appropriate measure of variability, such as:

- mean;
- standard deviation;
- confidence interval where justified.

The exact number of final seeds will be fixed once the computational cost of the baseline is established.

---

## 21. Ablation Strategy

QMLShield should use ablation studies to determine which components actually contribute to observed robustness.

Potential ablations include:

### Encoding Ablation

```text
Different input representations
```

This is a later-stage experiment.

### Purification Ablation

```text
No Defense
vs
CAE
vs
QAE
```

This is part of the core comparison.

### Attack Ablation

```text
FGSM
vs
PGD
```

### Noise Ablation

```text
Ideal
vs
Noisy
```

### Circuit Ablation

```text
Different circuit depth
Different QAE capacity
```

These should be introduced only after the core experiment is stable.

---

## 22. Experimental Execution Order

The recommended implementation order is:

### Experiment 01 — Clean VQC Baseline

```text
MNIST
→ preprocessing
→ amplitude encoding
→ 10-qubit VQC
→ clean evaluation
```

Success condition:

- pipeline runs reproducibly;
- model trains successfully;
- clean metrics are recorded.

---

### Experiment 02 — FGSM Attack

```text
Clean VQC
→ FGSM
→ adversarial evaluation
```

Success condition:

- adversarial examples are generated correctly;
- attack causes measurable degradation;
- perturbation constraints are respected.

---

### Experiment 03 — PGD Attack

```text
Clean VQC
→ PGD
→ adversarial evaluation
```

Success condition:

- iterative attack behaves as expected;
- results are reproducible;
- attack configuration is fully recorded.

---

### Experiment 04 — CAE Purification

```text
FGSM / PGD
→ CAE
→ encoding
→ VQC
```

Purpose:

- establish classical purification baseline.

---

### Experiment 05 — QAE Purification

```text
FGSM / PGD
→ encoding
→ QAE
→ VQC
```

Purpose:

- evaluate the primary quantum-native purification mechanism.

---

### Experiment 06 — Comparative Evaluation

Direct comparison:

```text
No Defense
vs
CAE
vs
QAE
```

under matched conditions.

---

### Experiment 07 — Noise Stress Test

Repeat selected experiments under controlled quantum noise.

---

### Experiment 08 — Adaptive Attack

Attack the complete purification + VQC pipeline.

This experiment is intentionally later because it requires the defense pipeline to be stable first.

---

## 23. Experiment Configuration

Experiment-specific parameters should be stored in configuration files rather than hard-coded in source code.

The current project uses:

```text
configs/
└── baseline.yaml
```

As the experiment set grows, additional configuration files may be introduced only when necessary.

Configurations should contain, as applicable:

```yaml
dataset:
encoding:
model:
training:
attack:
purification:
noise:
evaluation:
seed:
```

The configuration must be sufficient to reproduce the experiment setup.

---

## 24. Result Recording

Every completed experiment should record:

```text
Experiment ID
Configuration
Git commit / code version
Dataset
Data split
Random seed(s)
Model configuration
Attack configuration
Defense configuration
Noise configuration
Metrics
Runtime / resource information
Result artifacts
Notes
```

Raw outputs should be kept separate from processed tables and publication-quality figures.

Recommended project structure:

```text
results/
├── raw/
├── processed/
├── figures/
└── tables/
```

---

## 25. Reproducibility Protocol

A final experiment should be reproducible from:

```text
Source Code
+
pyproject.toml
+
uv.lock
+
Configuration
+
Seed
+
Dataset Definition
```

The experiment should not depend on undocumented notebook state.

Notebooks may be used for:

- exploration;
- visualization;
- result analysis.

The source implementation and configuration remain the source of truth.

---

## 26. Threat-to-Experiment Mapping

| Research Question | Primary Evidence |
|---|---|
| RQ1 — Robustness | Robust Accuracy, Attack Success Rate |
| RQ2 — Clean Performance | Clean Accuracy, Clean Accuracy Degradation |
| RQ3 — CAE vs QAE | Matched defense comparison |
| RQ4 — Reconstruction | Reconstruction metrics + classification recovery |
| RQ5 — Attack Strength | Performance across perturbation budgets |
| RQ6 — Noise | Ideal vs noisy evaluation |
| RQ7 — Resource Trade-off | Robustness vs quantum/resource overhead |

This mapping should be maintained as the methodology evolves.

---

## 27. Methodological Limitations

The initial methodology has deliberate limitations:

1. The first experiments use MNIST.
2. The first victim model is a VQC.
3. The initial attack is classical input-space based.
4. The initial attacker is white-box.
5. Initial purification experiments are non-adaptive.
6. Initial development uses simulation.
7. Real hardware is optional and not a critical-path requirement.
8. QAE is not assumed to remove all adversarial information.
9. A single successful attack-defense configuration is insufficient to establish general robustness.
10. Results should not be generalized beyond the evaluated dataset, architecture, attack, and noise conditions.

---

## 28. Methodological Decision Rules

The following rules govern experimental progression:

### Rule 1 — Do not add complexity before validating the previous stage.

```text
Baseline
→ Attack
→ Defense
→ Noise
→ Adaptive Attack
```

### Rule 2 — Do not change multiple major variables in a direct comparison.

### Rule 3 — Do not tune the defense using the final test set.

### Rule 4 — Do not report robustness without reporting clean performance.

### Rule 5 — Do not report reconstruction quality as equivalent to classification robustness.

### Rule 6 — Do not treat noisy simulation as proof of real-hardware behavior.

### Rule 7 — Do not claim universal security from a limited threat model.

### Rule 8 — Record failure cases, not only successful examples.

---

## 29. Relationship to Other Documents

```text
research_questions.md
    ↓
Defines what QMLShield wants to learn

threat_model.md
    ↓
Defines who attacks, what they can do, and what is protected

methodology.md
    ↓
Defines how the research questions will be tested

architecture.md
    ↓
Defines how the software implements the methodology

rules.md
    ↓
Defines engineering constraints

tech_stack.md
    ↓
Defines the technologies used

experiments.md
    ↓
Defines the concrete experiment registry and execution plan

findings.md
    ↓
Records the evidence and conclusions
```

This document should remain aligned with both `research_questions.md` and `threat_model.md`.

---

## 30. Status

**Current status:** Methodology defined at the initial research-design level.

The overall experimental pipeline, controlled variables, baseline hierarchy, evaluation dimensions, and progression have been established.

The following items remain implementation-level decisions to be finalized before their corresponding experiments:

- exact VQC architecture;
- exact optimizer and training hyperparameters;
- exact dataset split;
- exact preprocessing normalization;
- exact FGSM and PGD budgets;
- exact CAE architecture;
- exact QAE architecture;
- exact reconstruction metrics;
- exact noise models;
- final number of independent seeds.

These values must be fixed in experiment configurations before final comparative runs.

The methodology should be updated when a justified research decision changes the experimental design, while preserving a record of the previous protocol where reproducibility requires it.
