# Threat Model

> **Project:** QMLShield  
> **Domain:** Quantum Machine Learning Security  
> **Focus:** Adversarial Robustness & Purification for Quantum Machine Learning  
> **Research Stage:** QMLShield v0.1  
> **Status:** Research in progress

---

## 1. Purpose

This document defines the security threat model used by QMLShield.

Its purpose is to establish, before implementation and experimentation:

- what system is being protected;
- who the attacker is;
- what the attacker can observe or control;
- where the attack takes place;
- what the attacker is trying to achieve;
- what assumptions are made;
- what is in scope;
- and what is explicitly out of scope.

The threat model is a research boundary, not a claim that QMLShield covers the complete security surface of Quantum Machine Learning.

All attack and defense experiments should be interpreted within the threat model defined here.

---

## 2. System Under Study

The primary system under study is a Variational Quantum Classifier (VQC) operating on encoded classical data.

The initial pipeline is:

```text
Classical Input
      ↓
Preprocessing
      ↓
Amplitude Encoding
      ↓
Quantum Representation
      ↓
VQC
      ↓
Prediction
```

The adversarial-defense pipeline extends this system with an attack and an optional purification stage:

```text
                         ┌───────────────┐
                         │   No Defense  │
                         └───────┬───────┘
                                 │
Classical Input → Attack → x_adv ─────────→ Encoding → VQC → Prediction
                                 │
                         ┌───────┴────────┐
                         │                │
                        CAE              QAE
                         │                │
                         └───────┬────────┘
                                 ↓
                              VQC
                                 ↓
                            Prediction
```

The system is evaluated using MNIST in the initial research stage.

---

## 3. Security Objective

The primary security objective is:

> **Reduce the effectiveness of bounded adversarial perturbations against the VQC while preserving acceptable performance on clean inputs.**

A successful defense should therefore be evaluated along multiple dimensions:

```text
Robustness
    +
Clean Performance
    +
Reconstruction Quality
    +
Resource Overhead
```

Improving one dimension alone is not sufficient to establish that a purification mechanism is an effective defense.

---

## 4. Protected Asset

The primary protected asset is the **integrity of the VQC's classification decision**.

More specifically, QMLShield seeks to protect:

1. the correctness of predictions on clean inputs;
2. the stability of predictions under bounded adversarial perturbations;
3. the useful information contained in the input representation;
4. the reliability of the complete classification pipeline.

Confidentiality and availability are not primary security objectives in QMLShield v0.1.

---

## 5. Adversary Model

### 5.1 Adversary Goal

The initial adversary attempts to cause the VQC to produce an incorrect prediction by modifying the classical input.

The attack objective is therefore:

```text
Find δ such that:

x_adv = x + δ

and

f(E(x_adv)) ≠ f(E(x))
```

where:

- `x` is the clean classical input;
- `δ` is the adversarial perturbation;
- `x_adv` is the adversarial input;
- `E(·)` is the quantum encoding operation;
- `f(·)` is the VQC classifier.

The exact loss function and optimization formulation are implementation decisions that will be specified in `methodology.md`.

---

### 5.2 Attack Domain

The primary attack domain is the **classical input space**.

The attacker modifies:

```text
x
```

before the input is encoded into a quantum state.

The initial attack flow is:

```text
Clean Classical Input
        ↓
Adversarial Perturbation
        ↓
Adversarial Classical Input
        ↓
Quantum Encoding
        ↓
VQC
```

This distinction is important:

> **The attacker operates in classical input space, while the victim model is a quantum classifier.**

QMLShield v0.1 does not assume that the attacker directly manipulates the quantum state.

---

## 6. Attacker Knowledge

The primary threat model assumes a **white-box attacker**.

The attacker is assumed to have sufficient knowledge of the victim model to construct a gradient-based adversarial perturbation.

This may include knowledge of:

- the VQC architecture;
- trainable parameters;
- the input preprocessing pipeline;
- the encoding procedure;
- the classification objective.

The exact information available to the attacker should be kept consistent across experiments.

The purpose of the white-box setting is to provide a strong and reproducible baseline for evaluating the defense.

---

## 7. Attacker Capability

For QMLShield v0.1, the attacker is assumed to be able to:

1. access or obtain a clean classical input;
2. modify the classical input within a bounded perturbation constraint;
3. generate adversarial examples using the victim model;
4. submit the resulting adversarial input to the classification pipeline.

The attacker is **not** assumed to have direct control over:

- the quantum processor;
- quantum gates during execution;
- measurement hardware;
- the quantum state after encoding;
- the simulator's internal state;
- the training process of the defense.

These capabilities may be investigated in future research, but they are outside the initial threat model.

---

## 8. Attack Constraints

The adversarial perturbation is constrained so that the attack does not become an unrestricted modification of the input.

Conceptually:

```text
x_adv = x + δ

subject to:

||δ|| ≤ ε
```

where `ε` represents the perturbation budget.

The exact norm and perturbation budgets are intentionally not fixed in this document. They belong to the experimental methodology and should be selected and justified there.

The same attack budget should be applied consistently when comparing defenses.

---

## 9. Initial Attack Progression

QMLShield uses a progressive attack strategy.

### Stage 1 — FGSM

Fast Gradient Sign Method (FGSM) is the initial adversarial attack.

Purpose:

- validate the attack pipeline;
- establish an adversarial baseline;
- measure the initial robustness degradation of the VQC.

### Stage 2 — PGD

Projected Gradient Descent (PGD) is introduced after the FGSM pipeline is stable.

Purpose:

- evaluate stronger iterative perturbations;
- test whether observed defense behavior persists beyond a single-step attack.

### Stage 3 — Adaptive Attack

An adaptive attacker may later optimize against the complete defense pipeline:

```text
Input
  ↓
Attack
  ↓
Purification
  ↓
VQC
```

This is deliberately a later stage.

A defense that succeeds against a non-adaptive attack but fails against an adaptive attacker should be characterized as such rather than treated as universally robust.

---

## 10. Defense Model

QMLShield investigates purification as a defensive mechanism.

Three primary configurations are considered:

### 10.1 No Defense

```text
x_adv
  ↓
Encoding
  ↓
VQC
```

This is the undefended baseline.

### 10.2 Classical-Domain Purification

```text
x_adv
  ↓
CAE
  ↓
Encoding
  ↓
VQC
```

The Classical Autoencoder (CAE) operates before quantum encoding.

This provides a classical-domain purification baseline.

### 10.3 Quantum-Domain Purification

```text
x_adv
  ↓
Encoding
  ↓
QAE
  ↓
VQC
```

The Quantum Autoencoder (QAE) operates on the encoded quantum representation.

This is the primary quantum-native purification direction for QMLShield v0.1.

---

## 11. Attack Domain vs. Purification Domain

A central design principle of QMLShield is:

> **Attack domain ≠ Purification domain.**

The initial threat model deliberately allows the attacker to operate in classical input space while comparing defenses operating in different representation domains.

### Classical purification

```text
Classical Input
      ↓
Adversarial Attack
      ↓
CAE
      ↓
Quantum Encoding
      ↓
VQC
```

### Quantum purification

```text
Classical Input
      ↓
Adversarial Attack
      ↓
Quantum Encoding
      ↓
QAE
      ↓
VQC
```

This distinction allows QMLShield to investigate whether the location of the purification mechanism affects robustness.

---

## 12. Defense Assumptions

For the initial non-adaptive defense experiments:

- the attacker is designed against the victim classification task;
- the attacker does not explicitly optimize through the purification module;
- the purification module is evaluated as a separate defense stage.

This is a **non-adaptive defense evaluation**.

It should not be interpreted as proof that the defense remains secure against an attacker that knows and differentiates through the complete purification pipeline.

Adaptive evaluation is a later research stage.

---

## 13. Hardware and Quantum Noise Threat Boundary

QMLShield separates adversarial perturbations from quantum execution noise.

```text
Adversarial Perturbation
    = intentional manipulation by an attacker

Quantum Noise
    = physical / execution imperfection
```

Quantum noise is therefore not automatically treated as an adversarial attack in v0.1.

Instead, noise is introduced as a separate stress condition:

```text
Attack Condition
    ×
Noise Condition
    ×
Defense Condition
```

For example:

```text
FGSM
  ×
No Noise
  ×
QAE

FGSM
  ×
Depolarizing Noise
  ×
QAE
```

The exact noise models and placement in the circuit will be defined in `methodology.md`.

---

## 14. Threat Surface

The initial threat surface is intentionally limited.

### In Scope

```text
Classical input
      ↓
Preprocessing
      ↓
Encoding
      ↓
VQC classification
```

The attacker targets the classical input.

### Not Initially in Scope

The following attack surfaces are excluded from QMLShield v0.1:

- direct quantum-state manipulation;
- malicious quantum gate injection;
- parameter tampering inside the VQC;
- measurement manipulation;
- hardware fault injection;
- quantum side-channel attacks;
- denial-of-service;
- model theft;
- training-data poisoning;
- privacy attacks;
- attacks against the classical software supply chain.

These may represent valid QML security problems, but they belong to different threat models.

---

## 15. Trust Boundaries

The initial system can be divided into three conceptual trust regions:

```text
┌─────────────────────────────────────────┐
│ Trusted Data / Defense Pipeline         │
│                                         │
│   Preprocessing → Encoding → QAE/CAE    │
│                         ↓               │
│                        VQC              │
└─────────────────────────────────────────┘
                    ▲
                    │
             Attack Boundary
                    │
┌─────────────────────────────────────────┐
│ Untrusted / Attacker-Controlled Input   │
│                                         │
│             x_adv                       │
└─────────────────────────────────────────┘
```

The exact software-level trust boundaries will be refined in `architecture.md`.

---

## 16. Threat Model Matrix

| Dimension | QMLShield v0.1 |
|---|---|
| Protected asset | VQC classification integrity |
| Primary attacker | Classical input-space adversary |
| Attacker knowledge | White-box |
| Attacker capability | Modify classical input |
| Attack objective | Cause incorrect VQC prediction |
| Attack type | Gradient-based adversarial perturbation |
| Initial attack | FGSM |
| Next attack | PGD |
| Perturbation | Bounded |
| Victim model | VQC |
| Primary dataset | MNIST |
| Encoding | Amplitude encoding |
| Quantum representation | 10 qubits for 784-dimensional input |
| Primary defense | QAE purification |
| Classical baseline | CAE purification |
| Undefended baseline | No purification |
| Initial execution | Ideal simulator |
| Later stress condition | Simulated quantum noise |
| Adaptive attacker | Future stage |
| Real QPU | Optional future stage |
| Direct quantum-state attack | Out of scope for v0.1 |
| Hardware attack | Out of scope for v0.1 |

---

## 17. Security Evaluation Dimensions

The threat model defines security evaluation as a multi-dimensional problem.

### Robustness

Measures whether the VQC maintains correct predictions under adversarial perturbation.

Candidate metrics:

- Robust Accuracy;
- Attack Success Rate;
- accuracy degradation.

### Clean Integrity

Measures whether the defense preserves correct predictions on clean inputs.

Candidate metrics:

- Clean Accuracy;
- Clean Accuracy Degradation.

### Reconstruction

Measures whether purification recovers useful information from the adversarial input.

Candidate metrics:

- reconstruction error;
- reconstruction fidelity or similarity;
- clean-to-purified distance;
- adversarial-to-purified distance.

### Resource Cost

Measures the additional computational and quantum requirements introduced by the defense.

Candidate metrics:

- qubit count;
- circuit depth;
- trainable parameter count;
- runtime;
- measurement shots;
- training cost.

The exact mandatory metric set will be fixed in `methodology.md`.

---

## 18. Threat Model Limitations

This threat model has several deliberate limitations.

### 18.1 Limited Attack Surface

The initial model focuses on classical input-space attacks and does not represent the full security surface of QML.

### 18.2 White-Box Assumption

The primary attacker is white-box. Results should not automatically be generalized to black-box settings.

### 18.3 Non-Adaptive Initial Evaluation

The initial defense experiments do not assume an attacker that explicitly optimizes through the purifier.

### 18.4 Simulator-Centered Evaluation

Initial experiments use simulation. Results from noisy simulation should not automatically be interpreted as evidence of real-hardware security.

### 18.5 Dataset Limitation

MNIST is the initial benchmark. Results on MNIST alone should not be generalized to all QML workloads.

### 18.6 Architecture Limitation

The primary victim model is a VQC. Results should not automatically be generalized to every QML architecture.

---

## 19. Future Threat Model Extensions

After the core threat model is validated, QMLShield may extend toward:

```text
Adaptive Attacker
        ↓
Attack Through Purification
        ↓
QAE / qGAN
        ↓
VQC
```

and potentially:

```text
Quantum-State Attack
        ↓
Encoded State
        ↓
Purification
        ↓
VQC
```

Other possible extensions include:

- black-box attacks;
- transfer attacks;
- stronger iterative attacks;
- direct quantum-state perturbations;
- hardware-aware attacks;
- fault injection;
- noise manipulation;
- different encodings;
- different QML architectures.

These extensions must not be added merely to increase scope. Each should be introduced only when it answers a meaningful research question.

---

## 20. Threat Model Principles

QMLShield follows these security principles:

1. **Define the attacker before evaluating the defense.**
2. **Keep attack assumptions explicit.**
3. **Separate classical adversarial perturbation from quantum hardware noise.**
4. **Distinguish attack domain from purification domain.**
5. **Do not treat non-adaptive robustness as universal robustness.**
6. **Compare defenses under controlled conditions.**
7. **Report limitations together with positive results.**
8. **Do not generalize beyond the evaluated threat model.**
9. **Introduce stronger attackers progressively.**
10. **Treat the threat model as a living research boundary that evolves with justified extensions.**

---

## 21. Relationship to Other Documents

This document defines **the security threat being studied**.

```text
research_questions.md
    ↓
Defines what we want to investigate

threat_model.md
    ↓
Defines the adversary and security boundary

methodology.md
    ↓
Defines how the threat will be tested

architecture.md
    ↓
Defines how the software implements the research system

experiments.md
    ↓
Defines the concrete experimental protocol

findings.md
    ↓
Records what the experiments actually demonstrate
```

Any future security claim should be interpreted relative to the threat model defined here.

---

## 22. Status

**Current status:** Threat model defined for QMLShield v0.1.

The core threat model is:

```text
White-box
Classical Input-Space
Bounded Adversarial Perturbation
        ↓
MNIST
        ↓
Amplitude Encoding
        ↓
10-qubit VQC
        ↓
No Defense / CAE / QAE
        ↓
Security + Reconstruction + Resource Evaluation
```

FGSM is the initial attack mechanism, PGD is the next planned attack stage, and adaptive attacks and direct quantum-state attacks remain future extensions.

Exact attack objectives, perturbation norms and budgets, VQC architecture, CAE/QAE architecture, noise models, and mandatory metrics are intentionally deferred to `methodology.md`.
