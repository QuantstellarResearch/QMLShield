# QMLShield

**Adversarial Robustness and Purification for Quantum Machine Learning**

QMLShield is a research project investigating whether input purification can improve the adversarial robustness of Variational Quantum Classifiers (VQCs). It provides a controlled experimental framework for comparing classical and quantum purification mechanisms under an explicitly defined threat model, with reproducibility and honest reporting as first-class requirements.

---

## Status

QMLShield is at research stage **v0.1**. The research design, threat model, methodology, and experiment protocol are complete. The implementation (data pipeline, encoding, victim model, attacks, purification, evaluation) is in progress.

---

## Research Question

> **To what extent can input purification improve the robustness of a variational quantum classifier against adversarial perturbations while preserving clean-input performance?**

The project does not assume that any purification mechanism is inherently superior to another. Its purpose is to establish evidence through controlled experiments.

---

## Approach

The base pipeline is:

```text
Clean input → Preprocessing → Amplitude encoding → VQC → Prediction
```

Purification is evaluated as a pluggable defense stage. Three conditions are compared under matched settings:

```text
No defense:        x_adv → Encoding → VQC
Classical (CAE):   x_adv → CAE → Encoding → VQC
Quantum (QAE):     x_adv → Encoding → QAE → VQC
```

The experimental program runs as a progressive sequence (EXP-01 … EXP-08), with explicit decision gates between stages. See [`docs/experiments.md`](docs/experiments.md).

---

## Repository Layout

```text
configs/         Experiment configuration (YAML)
docs/            Research and engineering documentation
experiments/     Per-experiment orchestration and analysis
notebooks/       Exploration and result analysis
results/         Experiment outputs (raw / processed / figures / tables)
src/qmlshield/   Core implementation
tests/           Verification
```

The core implementation is organized by research responsibility, not by external framework:

```text
src/qmlshield/
├── data/          Dataset loading, splitting, preprocessing
├── encoding/      Classical-to-quantum encoding (amplitude)
├── models/        Victim model (VQC)
├── attacks/       Adversarial attacks (FGSM, PGD)
├── purification/  Defense mechanisms (CAE, QAE)
├── noise/         Controlled quantum-noise models
├── evaluation/    Research metrics
├── experiments/   Experiment orchestration
└── utils/         Shared utilities
```

---

## Documentation

| Document | Purpose |
|---|---|
| [`docs/research_questions.md`](docs/research_questions.md) | Research questions and hypotheses |
| [`docs/threat_model.md`](docs/threat_model.md) | Attacker model and security boundaries |
| [`docs/methodology.md`](docs/methodology.md) | Experimental methodology |
| [`docs/architecture.md`](docs/architecture.md) | Software architecture |
| [`docs/rules.md`](docs/rules.md) | Engineering and research rules |
| [`docs/tech_stack.md`](docs/tech_stack.md) | Technology baseline |
| [`docs/experiments.md`](docs/experiments.md) | Experimental protocol (EXP-01 … EXP-08) |
| [`docs/findings.md`](docs/findings.md) | Living research evidence record |
| [`docs/diagrams/`](docs/diagrams/) | System context, architecture, data flow, and sequence diagrams |

---

## Technology Stack

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
| Interactive Research | Jupyter |

Hardware and cloud execution (QUDORA Cloud) is reserved for later execution stages and is not part of the v0.1 baseline.

---

## Getting Started

Requirements: Python 3.12 and [uv](https://docs.astral.sh/uv/).

From the repository root:

```bash
uv sync
```

Verify the environment:

```bash
uv run python -c "import pennylane, torch, torchvision, numpy, scipy, sklearn, pandas, matplotlib; print('QMLShield environment OK')"
```

Run the tests:

```bash
uv run pytest
```

Check code quality:

```bash
uv run ruff check .
uv run ruff format --check .
```

---

## Reproducibility

An experiment is intended to be reproducible from the source code, `pyproject.toml`, `uv.lock`, the experiment configuration, the random seed, and the dataset definition. See [`docs/methodology.md`](docs/methodology.md) and [`docs/rules.md`](docs/rules.md).

---

## Contributing

QMLShield is proprietary software. Contributions are limited to authorized contributors. Before contributing, read [`CONTRIBUTING.md`](CONTRIBUTING.md) and [`docs/rules.md`](docs/rules.md). External contributions are not accepted.

---

## License

Copyright © 2026 Quantstellar Technologies.

This project is proprietary software. All rights reserved.

See [`LICENSE`](LICENSE) for the full license terms.
