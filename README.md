# QMLShield

**Adversarial Robustness & Purification for Quantum Machine Learning**

QMLShield is a research-oriented framework for studying adversarial robustness and input purification in Quantum Machine Learning (QML), with a primary focus on Variational Quantum Classifiers (VQCs).

The project investigates whether purification mechanisms can recover adversarially perturbed inputs before quantum classification while preserving clean-input performance.

> **Research status:** Early-stage research prototype. Results and conclusions are experimental and should not be interpreted as claims of universal QML security.

---

## Research Focus

QMLShield studies the following core question:

> **To what extent can input purification improve the robustness of a variational quantum classifier against adversarial perturbations while preserving clean-input performance?**

The initial research direction evaluates:

- Adversarial attacks against VQC-based classification
- Classical Autoencoder (CAE) purification
- Quantum Autoencoder (QAE) purification
- Clean versus adversarial versus purified classification
- Controlled quantum-noise conditions
- Adaptive attacks against the complete defense pipeline
- Robustness, reconstruction, accuracy, and resource trade-offs

The project does not assume that a particular purification mechanism will outperform another. Its purpose is to establish evidence through controlled experiments.

---

## Research Pipeline

```text
Clean Dataset
      │
      ▼
Preprocessing
      │
      ├──────────────► Adversarial Attack
      │                       │
      │                       ▼
      │                Adversarial Input
      │                       │
      │                       ▼
      │                  Purification
      │                  ┌────┴────┐
      │                  ▼         ▼
      │                 CAE       QAE
      │                  └────┬────┘
      │                       │
      └───────────────────────┤
                              ▼
                       Quantum Encoding
                              │
                              ▼
                    Variational Quantum
                       Classifier
                              │
                              ▼
                         Evaluation
                              │
                              ▼
                    Research Evidence
```

The initial experimental progression is:

```text
EXP-01  Clean VQC Baseline
   ↓
EXP-02  FGSM Vulnerability
   ↓
EXP-03  PGD Vulnerability
   ↓
EXP-04  CAE Baseline
   ↓
EXP-05  QAE Purification
   ↓
EXP-06  CAE vs QAE
   ↓
EXP-07  Noise Evaluation
   ↓
EXP-08  Adaptive Attack
```

---

## Project Structure

```text
qmlshield/
├── README.md
├── LICENSE
├── pyproject.toml
├── .python-version
├── .gitignore
│
├── configs/
│   └── baseline.yaml
│
├── src/
│   └── qmlshield/
│       ├── data/
│       ├── encoding/
│       ├── models/
│       ├── attacks/
│       ├── purification/
│       ├── noise/
│       ├── evaluation/
│       ├── experiments/
│       └── utils/
│
├── experiments/
│   └── 01_baseline/
│
├── notebooks/
├── tests/
│
├── results/
│   ├── raw/
│   ├── processed/
│   ├── figures/
│   └── tables/
│
└── docs/
    ├── system_context.md
    ├── architecture.md
    ├── data_flow.md
    ├── sequence.md
    ├── research_questions.md
    ├── threat_model.md
    ├── methodology.md
    ├── rules.md
    ├── tech_stack.md
    ├── experiments.md
    └── findings.md
```

The main source of truth for implementation is `src/qmlshield/`. Research definitions and experimental protocols are maintained in `docs/`, while experiment outputs are kept under `results/`.

---

## Technology Stack

QMLShield uses a deliberately lightweight research stack:

| Area | Technology |
|---|---|
| Language | Python 3.12 |
| Environment | uv |
| Quantum ML | PennyLane |
| Classical ML | PyTorch |
| Numerical Computing | NumPy, SciPy |
| Data / Metrics | pandas, scikit-learn |
| Visualization | Matplotlib |
| Configuration | YAML |
| Testing | pytest |
| Linting / Formatting | Ruff |
| Exploration | Jupyter |

The initial implementation does not require multiple quantum frameworks. Additional dependencies should be introduced only when there is a clear research or engineering need.

---

## Dataset and Initial Setting

The initial experiments use **MNIST**.

The first research stage focuses on binary classification, with the broader dataset and classification scope expanded only after the baseline pipeline is established.

For the initial full-resolution MNIST setting:

- Image size: `28 × 28`
- Input dimension: `784`
- Amplitude encoding: `10 qubits`, since `2^10 = 1024 ≥ 784`

The qubit count is therefore a consequence of the selected encoding and input representation, not an intrinsic requirement of MNIST.

---

## Threat Model

The initial threat model is intentionally narrow.

The attacker:

- Operates in the classical input space
- Is initially modeled as a white-box adversary
- Generates bounded adversarial perturbations
- Uses FGSM and PGD as the initial attack mechanisms
- Does not directly manipulate quantum states, gates, measurements, or hardware

Direct quantum-state attacks, hardware attacks, training-data poisoning, model extraction, and related threats are outside the initial scope.

Quantum noise is treated separately as an experimental execution condition rather than as an adversarial perturbation.

See [`docs/threat_model.md`](docs/threat_model.md) for the complete definition.

---

## Evaluation

QMLShield evaluates purification using a common downstream VQC and shared research metrics.

Primary metrics include:

- Clean accuracy
- Robust accuracy
- Attack success rate
- Reconstruction error or fidelity
- Clean performance degradation
- Circuit depth
- Qubit count
- Runtime
- Shot / execution cost where applicable

A successful defense is not defined solely by robustness. Clean-input performance and computational/quantum resource cost are also part of the evaluation.

---

## Documentation

The project documentation is organized around the research lifecycle:

| Document | Purpose |
|---|---|
| [`system_context.md`](docs/system_context.md) | System boundary and external context |
| [`architecture.md`](docs/architecture.md) | Internal software architecture |
| [`data_flow.md`](docs/data_flow.md) | Research data transformations |
| [`sequence.md`](docs/sequence.md) | Component interaction over time |
| [`research_questions.md`](docs/research_questions.md) | Research questions and hypotheses |
| [`threat_model.md`](docs/threat_model.md) | Attacker model and security boundaries |
| [`methodology.md`](docs/methodology.md) | Experimental methodology |
| [`rules.md`](docs/rules.md) | Engineering and research rules |
| [`tech_stack.md`](docs/tech_stack.md) | Technology baseline |
| [`experiments.md`](docs/experiments.md) | Operational experiment protocol |
| [`findings.md`](docs/findings.md) | Living research evidence record |

The documentation is intentionally modular. Not every implementation detail requires a separate document.

---

## Getting Started

### 1. Clone the repository

```bash
git clone <repository-url>
cd qmlshield
```

### 2. Create the environment

```bash
uv python install 3.12
uv python pin 3.12
uv venv
```

### 3. Install dependencies

```bash
uv sync
```

### 4. Verify the environment

```bash
uv run python -c "import pennylane, torch, torchvision, numpy, scipy, sklearn, pandas, matplotlib; print('QMLShield environment OK')"
```

### 5. Run tests

```bash
uv run pytest
```

### 6. Check code quality

```bash
uv run ruff check .
uv run ruff format --check .
```

---

## Research Principles

QMLShield follows a small set of principles throughout development:

**Research first.** Software structure should support the research question rather than become the research question.

**Controlled comparison.** CAE, QAE, and future purification mechanisms should be compared under controlled and reproducible conditions.

**No assumed winner.** The project does not assume that QAE, qGAN, or any other mechanism is inherently superior.

**Reproducibility.** Experimental configurations, seeds, attack settings, purification settings, and noise conditions should be recoverable.

**Evidence before conclusions.** Observations, interpretations, and research claims should remain distinguishable.

**Minimal complexity.** Avoid premature abstractions, unnecessary frameworks, and infrastructure that does not serve the current research stage.

**Failure is evidence.** A defense that fails under a particular threat or noise regime can still produce a meaningful research result.

---

## Current Scope

### In scope

- VQC-based classification
- Classical input-space adversarial attacks
- FGSM and PGD
- CAE purification
- QAE purification
- Clean / adversarial / purified comparison
- Ideal and controlled noisy execution
- Adaptive attacks as a later research stage
- Reproducible experimental evaluation

### Out of scope for the initial version

- Universal QML security
- All QML model families
- Direct quantum-state attacks
- Hardware fault injection
- Training-data poisoning
- Model theft
- Privacy-preserving QML
- Production deployment security

The scope may expand only when justified by the research direction and available evidence.

---

## Research Status

QMLShield is an evolving research project.

The current priority is to establish a reliable baseline:

```text
VQC
  ↓
FGSM / PGD
  ↓
CAE / QAE Purification
  ↓
Controlled Evaluation
  ↓
Evidence
```

Later stages may investigate adaptive attacks, noise sensitivity, additional datasets, alternative encodings, additional purification mechanisms, and real quantum hardware.

No future direction is considered a result until it is experimentally evaluated.

---

## Contributing

Contributions should preserve both software quality and research integrity.

Before contributing, please read:

- `CONTRIBUTING.md`
- `docs/research_questions.md`
- `docs/threat_model.md`
- `docs/methodology.md`
- `docs/rules.md`

The core principle is:

> **Contribute to the research, not just the code.**

---

## License

Copyright © 2026 Quantstellar Technologies.

This project is proprietary software. All rights reserved.

See [LICENSE](./LICENSE) for the full license terms.
