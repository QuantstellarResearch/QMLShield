# Research Questions

> **Project:** QMLShield  
> **Domain:** Quantum Machine Learning Security  
> **Focus:** Adversarial Robustness & Purification for Quantum Machine Learning  
> **Research Stage:** QMLShield v0.1  
> **Status:** Research in progress

---

## 1. Research Overview

QMLShield is a research project investigating adversarial robustness and purification in Quantum Machine Learning (QML).

The initial research setting uses a Variational Quantum Classifier (VQC) as the victim model and studies whether a purification mechanism can reduce the impact of adversarially perturbed inputs before classification.

The initial experimental pipeline is:

```text
MNIST
  ↓
Preprocessing
  ↓
Amplitude Encoding
  ↓
Variational Quantum Classifier (VQC)
  ↓
Adversarial Perturbation
  ↓
Purification
  ↓
VQC Prediction
  ↓
Evaluation
```

The project is deliberately structured as an empirical investigation. QMLShield does **not** assume that a quantum-native purification method is inherently superior to a classical defense. Instead, it evaluates where purification works, where it fails, and what trade-offs it introduces.

The initial comparison is:

```text
No Defense:
x_adv
  ↓
Encoding
  ↓
VQC

Classical Purification:
x_adv
  ↓
CAE
  ↓
Encoding
  ↓
VQC

Quantum Purification:
x_adv
  ↓
Encoding
  ↓
QAE
  ↓
VQC
```

The first quantum-native purification candidate is a Quantum Autoencoder (QAE). A quantum generative approach such as qGAN may be investigated later as an extension rather than being part of the initial minimum research scope.

---

## 2. Research Problem

Quantum Machine Learning models may be affected by adversarial perturbations in which a deliberately modified input causes a classifier to produce an incorrect prediction.

QMLShield investigates whether purification can serve as a defense mechanism for a variational quantum classifier by recovering useful structure from adversarially perturbed inputs before classification.

The central research challenge is not simply to increase post-attack accuracy. A useful defense should be evaluated across multiple dimensions:

- robustness against adversarial perturbations;
- preservation of clean-input performance;
- quality of input reconstruction;
- sensitivity to attack strength;
- and additional quantum computational cost.

The project therefore treats adversarial purification as a security problem and an empirical trade-off analysis rather than as a claim that one particular defense is universally effective.

---

## 3. Main Research Question

> **RQ0 — To what extent can input purification improve the robustness of a variational quantum classifier against adversarial perturbations while preserving clean-input performance?**

This is the primary research question of QMLShield.

The experiments should provide evidence for both positive and negative outcomes. A purification mechanism that fails under a particular threat model, noise condition, or attack strength is still a useful research result if the failure is measured and characterized rigorously.

---

## 4. Secondary Research Questions

### RQ1 — Robustness Effectiveness

> **RQ1 — Does purification improve the robustness of a VQC against adversarial perturbations compared with an undefended VQC?**

Primary measurements:

- Robust Accuracy
- Attack Success Rate
- Accuracy degradation under attack

The comparison should use the same dataset, victim architecture, attack configuration, and evaluation protocol wherever possible.

---

### RQ2 — Clean Performance Preservation

> **RQ2 — How does purification affect VQC performance on clean inputs?**

The defense should be evaluated for possible trade-offs between robustness and clean performance.

Key measurements:

- Clean Accuracy
- Clean Accuracy Degradation
- Clean-input prediction consistency

A defense should not be considered successful solely because it improves adversarial accuracy if it substantially damages performance on clean inputs.

---

### RQ3 — Classical vs. Quantum-Domain Purification

> **RQ3 — How does quantum-domain purification using a Quantum Autoencoder (QAE) compare with classical-domain purification using a Classical Autoencoder (CAE)?**

The comparison is intended to determine whether moving purification into the quantum representation domain provides measurable benefits.

The project does not assume that QAE will outperform CAE.

The comparison should consider:

- Robust Accuracy
- Clean Accuracy
- Attack Success Rate
- Reconstruction Quality
- Computational / circuit overhead

This question is important because a quantum-native defense should justify its additional complexity through measurable scientific value.

---

### RQ4 — Reconstruction vs. Classification Recovery

> **RQ4 — Does purification genuinely recover the underlying input representation, or does it primarily improve the classifier's prediction without faithfully reconstructing the clean input?**

QMLShield should distinguish:

```text
Input Reconstruction Quality
```

from:

```text
Classification Recovery
```

A defense may improve classification without accurately reconstructing the original input. These are different outcomes and should not be treated as equivalent.

Relevant measurements may include:

- reconstruction error;
- reconstruction fidelity or similarity;
- clean-to-purified distance;
- adversarial-to-purified distance;
- post-purification classification accuracy.

The exact metric set will be finalized in the methodology and experiment design documents.

---

### RQ5 — Attack Strength and Defense Stability

> **RQ5 — How does purification effectiveness change as the strength of the adversarial perturbation increases?**

The objective is to determine whether a defense only works within a narrow perturbation range or remains useful across increasing attack strength.

The initial progression is:

```text
Clean
  ↓
FGSM
  ↓
PGD
  ↓
Increasing perturbation strength
```

The exact perturbation budgets and parameter ranges should be defined in the experimental protocol rather than embedded prematurely in this document.

---

### RQ6 — Robustness Under Quantum Noise

> **RQ6 — How does purification effectiveness change when the QML pipeline is evaluated under simulated quantum noise?**

This question is intentionally separated from the adversarial threat itself.

Adversarial perturbation represents an intentional attack, while quantum noise represents physical or hardware-related imperfections.

The initial evaluation strategy is:

```text
Stage A — Ideal / noiseless simulation
Stage B — Noisy simulation
Stage C — Real QPU, only if scientifically justified
```

The project should first establish whether the defense mechanism works in an idealized setting before determining how its behavior changes under noise.

---

### RQ7 — Resource–Robustness Trade-off

> **RQ7 — Does the robustness benefit of a purification mechanism justify its additional quantum resource requirements?**

Relevant resource dimensions may include:

- qubit count;
- circuit depth;
- number of trainable parameters;
- execution time;
- number of measurement shots;
- additional training cost.

This question prevents QMLShield from evaluating a defense only by accuracy while ignoring the cost of introducing the defense.

---

## 5. Research Objectives

### Objective 1 — Establish a Reproducible VQC Baseline

Build a reproducible baseline using:

- MNIST;
- controlled preprocessing;
- amplitude encoding;
- a 10-qubit representation for the full 784-dimensional input;
- a variational quantum classifier;
- deterministic experiment configuration and seed management.

The baseline must be validated before adversarial attacks or purification are introduced.

> Note: 10 qubits arise from the chosen 784-dimensional amplitude-encoding representation, since \(2^{10} = 1024 \geq 784\). This is an implementation choice, not a claim that MNIST inherently requires 10 qubits.

---

### Objective 2 — Establish the Adversarial Threat Model

Define and implement an explicit input-space threat model.

The initial threat model should specify:

- attacker capability;
- attacker knowledge;
- attack objective;
- perturbation constraints;
- attack domain;
- victim model;
- and out-of-scope attack surfaces.

FGSM is the initial attack mechanism, followed by PGD once the baseline attack pipeline is validated.

---

### Objective 3 — Establish Purification Baselines

Evaluate three configurations under comparable conditions:

```text
No Defense
CAE Purification
QAE Purification
```

This allows QMLShield to distinguish the value of purification itself from the value of a particular quantum implementation.

---

### Objective 4 — Evaluate Robustness and Reconstruction

Measure both security and reconstruction outcomes.

The evaluation should include, as appropriate:

- Clean Accuracy;
- Robust Accuracy;
- Attack Success Rate;
- Accuracy Degradation;
- Reconstruction Error / Fidelity;
- Clean Performance Degradation;
- Resource Overhead.

---

### Objective 5 — Stress-Test the Defense

After the ideal-simulation baseline is established, investigate:

- stronger attacks;
- different attack configurations;
- quantum noise;
- and, at a later stage, adaptive attacks.

These are progressive extensions rather than requirements for the first baseline experiment.

---

## 6. Initial Hypotheses

The following are **research hypotheses**, not established conclusions.

### H1 — Purification Hypothesis

> A suitable purification mechanism can improve the robust accuracy of a VQC under bounded adversarial perturbations compared with an undefended VQC.

### H2 — Clean Preservation Hypothesis

> A suitable purification mechanism can improve robustness without causing unacceptable degradation on clean inputs.

### H3 — Domain Hypothesis

> Quantum-domain purification using a QAE may exhibit robustness or representation characteristics that differ from classical-domain purification using a CAE.

This hypothesis does **not** assume QAE will outperform CAE.

### H4 — Noise Sensitivity Hypothesis

> The effectiveness of purification may change under quantum noise because purification introduces additional quantum operations and circuit depth.

The direction and magnitude of this effect are empirical questions.

### H5 — Adaptive-Attack Hypothesis

> A purification mechanism that performs well against a non-adaptive attack may exhibit reduced effectiveness when the attacker accounts for the purification stage.

This hypothesis belongs to a later stage of the project.

---

## 7. Scope

### 7.1 In Scope for QMLShield v0.1

- Quantum Machine Learning security;
- adversarial robustness;
- input-level adversarial perturbations;
- Variational Quantum Classifiers;
- MNIST;
- controlled preprocessing of the original 28×28 MNIST images;
- 784-dimensional representation;
- amplitude encoding;
- 10-qubit quantum representation;
- FGSM;
- PGD as the next attack stage;
- classical autoencoder purification as a baseline;
- quantum autoencoder purification as the primary quantum-native defense;
- ideal quantum simulation;
- noisy simulation as a subsequent validation stage;
- reproducible evaluation.

### 7.2 Future Scope

The following may be investigated after the core pipeline is validated:

- qGAN-based purification;
- adaptive attacks against the complete purification + VQC pipeline;
- alternative data encodings;
- larger or deeper quantum circuits;
- Fashion-MNIST;
- alternative noise models;
- real quantum hardware;
- direct quantum-state attacks;
- broader QML architectures.

These are extensions, not requirements for the first successful QMLShield baseline.

---

## 8. Non-Goals

QMLShield v0.1 is **not** intended to:

1. provide a universal security solution for all QML models;
2. claim that QAE is universally superior to classical defenses;
3. study every possible QML attack surface;
4. treat all Variational Quantum Algorithms as a single security problem;
5. provide a production-ready cybersecurity product;
6. immediately target real quantum hardware as the primary execution environment;
7. claim hardware-level security from noisy simulation alone;
8. establish universal robustness from a single dataset or attack;
9. optimize for the largest possible quantum circuit;
10. introduce additional quantum frameworks without a research need.

---

## 9. Experimental Comparison Principle

The core comparisons should preserve experimental fairness.

For a given experiment, the following should remain controlled whenever possible:

- dataset and data split;
- preprocessing;
- encoding;
- victim VQC architecture;
- training procedure;
- attack configuration;
- perturbation budget;
- evaluation samples;
- random seeds.

The primary defense comparison is:

```text
                  ┌── No Defense ──→ VQC
Adversarial Input ┤
                  ├── CAE ─────────→ VQC
                  │
                  └── QAE ─────────→ VQC
```

The purpose is to isolate the contribution of the purification mechanism as much as practical.

---

## 10. Success Criteria

QMLShield should not define success as "the defense increases accuracy."

A successful research outcome requires:

### Reproducibility

The same configuration and seed should allow the experiment to be rerun with consistent methodology and comparable results.

### Security Evaluation

The project must quantify how adversarial attacks affect the victim VQC and whether purification changes that outcome.

### Clean-Performance Evaluation

The defense must be evaluated on clean inputs rather than only adversarial inputs.

### Comparative Evaluation

QAE should be evaluated against meaningful baselines, including no defense and a classical-domain purification baseline where applicable.

### Reconstruction Evaluation

Purification should be evaluated as a reconstruction mechanism as well as through downstream classification.

### Resource Evaluation

Additional quantum complexity should be reported alongside robustness results.

### Failure Characterization

The project should explicitly record conditions under which a defense fails or degrades.

A negative result is considered a valid research outcome when the experiment is properly controlled, reproducible, and informative.

---

## 11. Expected Research Contribution

The intended contribution of QMLShield is not simply another implementation of an adversarial attack or autoencoder.

The project aims to develop a reproducible experimental framework for studying:

> **when, how, and under which threat and noise conditions quantum-native purification can improve the robustness of variational quantum classifiers.**

Potential contributions include:

1. a clearly defined threat model for the studied QML pipeline;
2. a reproducible benchmark comparing undefended, classical-purification, and quantum-purification configurations;
3. empirical analysis of QAE-based purification for adversarially perturbed quantum-classifier inputs;
4. analysis of the relationship between reconstruction quality and classification robustness;
5. characterization of robustness–clean-performance–resource trade-offs;
6. analysis of how the observed behavior changes under simulated quantum noise.

The final contribution claims must be determined by experimental evidence rather than fixed in advance.

---

## 12. Research Boundaries

QMLShield should distinguish the following concepts throughout the project:

```text
Adversarial Perturbation
    ≠
Quantum Hardware Noise
```

and:

```text
Attack Domain
    ≠
Purification Domain
```

For the initial threat model:

```text
Attacker
   ↓
Classical Input Space
   ↓
Encoding
   ↓
Quantum Representation
   ↓
VQC
```

For the defense comparison:

```text
Classical Purification:
Input Space → CAE → Encoding → VQC

Quantum Purification:
Input Space → Encoding → QAE → VQC
```

This distinction is fundamental to interpreting QMLShield experiments correctly.

---

## 13. Research Progression

The intended progression is:

```text
Phase 1
Clean VQC Baseline
        ↓
Phase 2
Adversarial Attack Baseline
        ↓
Phase 3
Classical Purification Baseline
        ↓
Phase 4
Quantum Autoencoder Purification
        ↓
Phase 5
Robustness / Reconstruction / Resource Evaluation
        ↓
Phase 6
Noise Stress Testing
        ↓
Phase 7
Adaptive Attacks and Further Generalization
```

Each phase should produce a stable and interpretable result before the next layer of complexity is introduced.

---

## 14. Research Discipline

QMLShield follows these principles:

1. **Measure before claiming.**
2. **Compare against meaningful baselines.**
3. **Separate hypotheses from established results.**
4. **Keep the threat model explicit.**
5. **Keep adversarial perturbation separate from hardware noise.**
6. **Treat quantum complexity as a cost, not automatically as an advantage.**
7. **Record negative and failure results.**
8. **Prefer reproducible experiments over isolated demonstrations.**
9. **Do not claim general security from limited empirical evidence.**
10. **Expand the scope only when the previous experimental stage is understood.**

---

## 15. Open Questions for Later Methodology Design

The following questions are intentionally left open for the methodology and experimental-design stages:

- Which MNIST class configuration should be used for the first baseline: binary classification or a larger class set?
- Which exact VQC architecture provides an appropriate balance between expressivity and experimental cost?
- Which preprocessing and normalization procedure should be fixed?
- Which exact FGSM and PGD objective should define the attack?
- Which perturbation budgets should be evaluated?
- Which CAE architecture constitutes a fair classical baseline?
- What QAE architecture and latent/trash-qubit configuration should be used?
- Which reconstruction metrics are most appropriate for the chosen representation?
- Which quantum noise models should be introduced first?
- Which resource metrics should be mandatory in every defense comparison?
- At what stage should adaptive attacks be introduced?

These questions should be resolved in `methodology.md` and `experiments.md` rather than prematurely fixed here.

---

## 16. Relationship to Other Project Documents

This document defines **what QMLShield is trying to investigate and why**.

The responsibilities of the other research documents are:

```text
research_questions.md
    → What are we investigating?

threat_model.md
    → What security threat are we studying?

methodology.md
    → How will we investigate it?

architecture.md
    → How is the research software structured?

rules.md
    → What engineering constraints must the implementation follow?

tech_stack.md
    → Which technologies are used?

experiments.md
    → Which experiments will generate the evidence?

findings.md
    → What did the evidence actually show?
```

No experimental result should be treated as a finding until it has been generated and evaluated through the project methodology.

---

## 17. Status

**Current status:** Research definition in progress.

The research questions and scope are established at a high level. Exact model architecture, attack parameters, purification architecture, noise models, and detailed experimental protocols remain to be specified in subsequent project documents.

**Next document:** `threat_model.md`
