# QMLShield — Sequence

## 1. Purpose

This document defines the principal runtime sequence of a QMLShield research experiment.

The sequence diagram focuses on how the main research components interact during an adversarial purification experiment. It complements:

- `system_context.md` — system boundary and external context
- `architecture.md` — internal component boundaries
- `data_flow.md` — movement and transformation of research data
- `threat_model.md` — attacker capabilities and assumptions
- `methodology.md` — experimental protocol

The sequence is intentionally compact. It describes the canonical research execution path rather than every implementation-level function call.

---

## 2. Primary Sequence

The canonical QMLShield experiment can be represented as:

```mermaid
sequenceDiagram
    actor Researcher
    participant Exp as Experiment Runner
    participant Data as Data Layer
    participant Enc as Encoding
    participant VQC as VQC / Victim Model
    participant Attack as Attack Module
    participant Purifier as Purification
    participant Noise as Noise / Backend
    participant Eval as Evaluation
    participant Results as Results / Evidence

    Researcher->>Exp: Start configured experiment
    Exp->>Data: Load and prepare dataset
    Data-->>Exp: Clean samples and labels

    Exp->>VQC: Train / load baseline model
    VQC-->>Exp: Trained VQC

    Exp->>Enc: Encode clean input
    Enc-->>Exp: Encoded state

    Exp->>VQC: Classify clean input
    VQC->>Noise: Execute quantum circuit
    Noise-->>VQC: Execution result
    VQC-->>Exp: Clean prediction

    Exp->>Attack: Generate adversarial input
    Attack->>VQC: Query victim behavior
    VQC-->>Attack: Model response
    Attack-->>Exp: Adversarial input

    Exp->>Purifier: Purify adversarial input
    Purifier-->>Exp: Purified input

    Exp->>Enc: Encode purified input
    Enc-->>Exp: Encoded state

    Exp->>VQC: Classify purified input
    VQC->>Noise: Execute quantum circuit
    Noise-->>VQC: Execution result
    VQC-->>Exp: Purified prediction

    Exp->>Eval: Evaluate clean / adversarial / purified outputs
    Eval-->>Exp: Metrics

    Exp->>Results: Record parameters, outputs, metrics
    Results-->>Researcher: Experiment evidence
```

This sequence describes the core experimental interaction. Specific experiments may omit or repeat stages depending on their purpose.

---

## 3. Sequence Responsibilities

### Researcher

The researcher:

- Defines the research question being tested.
- Selects the experiment configuration.
- Defines the relevant threat-model assumptions.
- Reviews the resulting evidence.

The researcher does not manually perform the computational pipeline during normal experiment execution.

### Experiment Runner

The experiment runner orchestrates the sequence.

It is responsible for:

- Loading configuration
- Establishing reproducibility settings
- Calling domain modules
- Maintaining experiment identity
- Collecting outputs
- Sending results to evaluation and evidence recording

It should not contain the scientific implementation of attacks, models, or purification mechanisms.

### Data Layer

The data layer:

- Loads the selected dataset.
- Applies defined preprocessing.
- Produces clean samples and labels.

### Encoding

The encoding layer converts the classical representation into the quantum representation required by the VQC.

### VQC / Victim Model

The VQC is the primary protected classifier.

It:

- Trains or loads model parameters.
- Receives encoded inputs.
- Executes the parameterized quantum circuit.
- Produces predictions.

### Attack Module

The attack module generates adversarial examples under the defined threat model.

The initial progression is:

```text
FGSM → PGD → Adaptive Attack
```

The attack module does not perform purification.

### Purification

The purification module transforms an adversarial input into a candidate purified input.

Initial mechanisms:

```text
CAE
QAE
```

Future mechanisms such as qGAN can be introduced through the same conceptual boundary.

### Noise / Backend

The noise/backend boundary represents the quantum execution environment.

It may provide:

- Ideal execution
- Controlled noisy simulation
- Optional later real-QPU execution

Noise is an execution condition, not an adversarial attack.

### Evaluation

The evaluation layer compares outputs and computes research metrics.

### Results / Evidence

The results layer records the experimental configuration, outputs, metrics, and evidence needed for later interpretation.

---

## 4. Clean Baseline Sequence

The clean baseline establishes the reference behavior before adversarial experiments.

```mermaid
sequenceDiagram
    participant Exp as Experiment Runner
    participant Data as Data Layer
    participant Enc as Encoding
    participant VQC as VQC
    participant Backend as Quantum Backend
    participant Eval as Evaluation

    Exp->>Data: Load clean sample
    Data-->>Exp: x, y

    Exp->>Enc: Encode(x)
    Enc-->>Exp: Quantum state

    Exp->>VQC: Predict(encoded x)
    VQC->>Backend: Execute circuit
    Backend-->>VQC: Measurement result
    VQC-->>Exp: Prediction

    Exp->>Eval: Evaluate prediction against y
    Eval-->>Exp: Clean metrics
```

The clean baseline should be established before interpreting adversarial robustness.

---

## 5. Non-Adaptive Adversarial Sequence

The initial attack sequence assumes the attacker operates against the VQC without optimizing specifically against the purification mechanism.

```mermaid
sequenceDiagram
    participant Exp as Experiment Runner
    participant Data as Data Layer
    participant Attack as Attack
    participant VQC as VQC
    participant Purifier as Purification
    participant Enc as Encoding
    participant Eval as Evaluation

    Exp->>Data: Obtain clean input x, label y
    Data-->>Exp: x, y

    Exp->>Attack: Generate adversarial example
    Attack->>VQC: Query victim
    VQC-->>Attack: Model response
    Attack-->>Exp: x_adv

    Exp->>VQC: Classify x_adv
    VQC-->>Exp: Adversarial prediction

    Exp->>Purifier: Purify x_adv
    Purifier-->>Exp: x_purified

    Exp->>Enc: Encode x_purified
    Enc-->>Exp: Encoded purified state

    Exp->>VQC: Classify purified input
    VQC-->>Exp: Purified prediction

    Exp->>Eval: Compare clean / adversarial / purified
    Eval-->>Exp: Robustness metrics
```

The important property is that the attacker is not initially optimized against the complete purification pipeline.

---

## 6. CAE vs QAE Comparison Sequence

CAE and QAE should receive comparable adversarial inputs under controlled experimental conditions.

```mermaid
sequenceDiagram
    participant Exp as Experiment Runner
    participant Attack as Attack
    participant CAE as CAE
    participant QAE as QAE
    participant VQC as Shared VQC
    participant Eval as Evaluation

    Exp->>Attack: Generate adversarial input
    Attack-->>Exp: x_adv

    Exp->>CAE: Purify x_adv
    CAE-->>Exp: x_cae

    Exp->>QAE: Purify x_adv
    QAE-->>Exp: x_qae

    Exp->>VQC: Classify x_cae
    VQC-->>Exp: CAE prediction

    Exp->>VQC: Classify x_qae
    VQC-->>Exp: QAE prediction

    Exp->>Eval: Compare CAE and QAE
    Eval-->>Exp: Comparative metrics
```

The shared VQC and shared evaluation definitions help isolate the effect of the purification mechanism.

---

## 7. Adaptive Attack Sequence

The adaptive sequence is a later-stage experiment.

The attacker is aware of the defense pipeline and optimizes against the complete defended model.

```mermaid
sequenceDiagram
    participant Exp as Experiment Runner
    participant Adaptive as Adaptive Attacker
    participant Purifier as Purification
    participant Enc as Encoding
    participant VQC as VQC
    participant Eval as Evaluation

    Exp->>Adaptive: Configure adaptive attack

    Adaptive->>Purifier: Forward adversarial candidate
    Purifier-->>Adaptive: Purified candidate

    Adaptive->>Enc: Encode candidate
    Enc-->>Adaptive: Encoded state

    Adaptive->>VQC: Evaluate candidate
    VQC-->>Adaptive: Model response

    Adaptive-->>Exp: Defense-aware adversarial input

    Exp->>Purifier: Purify adaptive input
    Purifier-->>Exp: Purified input

    Exp->>Enc: Encode purified input
    Enc-->>Exp: Encoded state

    Exp->>VQC: Classify defended input
    VQC-->>Exp: Prediction

    Exp->>Eval: Evaluate adaptive robustness
    Eval-->>Exp: Adaptive robustness metrics
```

This sequence tests whether the observed defense benefit survives a stronger attacker model.

---

## 8. Noise-Aware Sequence

Noise experiments introduce controlled execution conditions without redefining the attack model.

```mermaid
sequenceDiagram
    participant Exp as Experiment Runner
    participant Enc as Encoding
    participant VQC as VQC
    participant Noise as Noise Model
    participant Backend as Quantum Backend
    participant Eval as Evaluation

    Exp->>Enc: Encode input
    Enc-->>Exp: Encoded state

    Exp->>VQC: Execute classifier
    VQC->>Noise: Apply configured noise condition
    Noise->>Backend: Execute noisy circuit
    Backend-->>Noise: Measurement result
    Noise-->>VQC: Noisy execution result
    VQC-->>Exp: Prediction

    Exp->>Eval: Compare noisy and reference results
    Eval-->>Exp: Noise-aware metrics
```

The noise condition is an experimental variable.

It is not itself treated as an adversarial perturbation.

---

## 9. Result Recording Sequence

Research evidence should be recorded after an experiment produces its outputs.

```mermaid
sequenceDiagram
    participant Exp as Experiment Runner
    participant Eval as Evaluation
    participant Results as Results Store
    participant Findings as Findings

    Exp->>Eval: Submit experiment outputs
    Eval-->>Exp: Metrics

    Exp->>Results: Record configuration
    Exp->>Results: Record raw outputs
    Exp->>Results: Record processed metrics

    Results-->>Findings: Evidence available for interpretation
```

`findings.md` is an interpretation layer rather than a runtime component.

It should not be treated as part of the computational execution path.

---

## 10. Experiment Progression

The sequence architecture supports the planned experiment progression:

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
CAE vs QAE
      ↓
EXP-07
Noise Evaluation
      ↓
EXP-08
Adaptive Attack
```

Not every experiment uses the complete primary sequence.

For example:

- EXP-01 focuses on clean classification.
- EXP-02 and EXP-03 establish vulnerability.
- EXP-04 and EXP-05 evaluate individual purification mechanisms.
- EXP-06 performs controlled comparison.
- EXP-07 introduces execution noise.
- EXP-08 strengthens the attacker model.

---

## 11. Sequence-Level Research Invariants

The following interaction properties should remain stable:

1. The experiment runner orchestrates rather than reimplements domain logic.
2. The VQC remains the primary protected classifier.
3. The attacker and purifier remain separate components.
4. Clean behavior is measured before robustness claims are interpreted.
5. CAE and QAE can be evaluated through a common downstream VQC.
6. Noise remains an execution condition separate from adversarial attack generation.
7. Adaptive attacks are evaluated against the complete defense pipeline.
8. Evaluation consumes explicit experiment outputs.
9. Evidence is recorded before research conclusions are written.
10. A successful purification result is an empirical outcome, not an architectural assumption.

---

## 12. Sequence Boundary

This document intentionally stops at research-component interaction.

It does not define:

- Python method calls
- Function signatures
- Tensor-level operations
- Class constructors
- Threading or multiprocessing
- Distributed execution
- API protocols
- Cloud orchestration
- Hardware control commands

Those details should only be documented when implementation complexity makes them necessary.

---

## 13. Relationship to Other Documents

The sequence diagram connects the three preceding architectural views:

```text
System Context
      ↓
Architecture
      ↓
Data Flow
      ↓
Sequence
```

Their responsibilities are distinct:

| Document | Primary Question |
|---|---|
| `system_context.md` | What surrounds QMLShield? |
| `architecture.md` | What are the major internal components? |
| `data_flow.md` | How does research data move? |
| `sequence.md` | How do the components interact over time? |

The remaining research documents provide the scientific constraints for these interactions.

---

## 14. Core Sequence Statement

> **A QMLShield experiment is orchestrated as a controlled sequence in which clean data establishes a baseline, an attacker generates bounded perturbations, purification optionally transforms adversarial inputs, the common VQC evaluates the resulting inputs under defined execution conditions, and the evaluation layer records evidence for scientific comparison.**
