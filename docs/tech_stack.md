# Tech Stack

> **Project:** QMLShield  
> **Domain:** Quantum Machine Learning Security  
> **Focus:** Adversarial Robustness & Purification for Quantum Machine Learning  
> **Research Stage:** QMLShield v0.1  
> **Status:** Research in progress

---

## 1. Purpose

This document defines the technologies, runtime environment, libraries, and tooling used by QMLShield.

Its purpose is to establish a clear and reproducible technical baseline for the research project.

This document answers:

- What technologies does QMLShield use?
- What role does each technology play?
- Which technologies are core dependencies?
- Which technologies are development dependencies?
- Where are the boundaries between them?
- Which technical decisions are fixed for v0.1?
- Which technologies are intentionally deferred?

This document does not define the research methodology or the detailed software architecture.

Those responsibilities belong to:

- `research_questions.md`
- `threat_model.md`
- `methodology.md`
- `architecture.md`
- `rules.md`

---

## 2. Technology Philosophy

QMLShield follows a minimal research-oriented technology strategy.

The stack should be:

- sufficient for the current research questions;
- reproducible;
- widely supported;
- easy to inspect;
- appropriate for quantum machine learning experimentation;
- compatible with classical adversarial-ML tooling;
- and small enough to avoid unnecessary infrastructure.

The project does not adopt a technology merely because it is associated with quantum computing.

A technology must have a clear research or engineering purpose.

---

## 3. Technology Baseline

The current QMLShield v0.1 baseline is:

| Area | Technology |
|---|---|
| Language | Python 3.12 |
| Environment / Package Manager | uv |
| Quantum ML Framework | PennyLane |
| Classical ML / Tensor Framework | PyTorch |
| Dataset | MNIST |
| Numerical Computing | NumPy |
| Scientific Computing | SciPy |
| Classical ML Utilities | scikit-learn |
| Data Analysis | pandas |
| Visualization | Matplotlib |
| Configuration | YAML |
| Testing | pytest |
| Linting / Formatting | Ruff |
| Interactive Research | Jupyter |
| Kernel Integration | IPython / Jupyter Kernel |
| Version Control | Git |
| Dependency Locking | `uv.lock` |

This is the baseline stack for the current research stage.

---

# 4. Runtime Environment

## 4.1 Python

QMLShield currently targets:

```text
Python 3.12
```

Python is the primary implementation language for the project.

It is used for:

- dataset handling;
- preprocessing;
- quantum circuit construction;
- VQC implementation;
- adversarial attacks;
- purification models;
- evaluation;
- experiment orchestration;
- analysis;
- visualization;
- testing.

The project records the intended Python version through:

```text
.python-version
```

---

## 4.2 uv

QMLShield uses:

```text
uv
```

for Python environment and dependency management.

uv is responsible for:

- creating the project environment;
- resolving dependencies;
- installing dependencies;
- maintaining reproducible dependency resolution;
- managing the project lockfile.

The project uses:

```text
pyproject.toml
uv.lock
.python-version
```

as the primary environment-definition files.

The virtual environment itself is not part of version control.

---

# 5. Core Runtime Dependencies

## 5.1 PennyLane

**Role:** Quantum machine learning and differentiable quantum programming framework.

PennyLane is the primary quantum framework for QMLShield.

It is responsible for:

- quantum circuit construction;
- parameterized quantum circuits;
- variational quantum classifiers;
- quantum autoencoder experiments;
- quantum differentiation;
- simulator-backed quantum execution;
- noisy quantum simulation where supported.

PennyLane provides the main bridge between:

```text
Quantum Circuits
        ↕
Classical Optimization
        ↕
PyTorch
```

### QMLShield usage

PennyLane is the primary framework for:

```text
Encoding
VQC
QAE
Quantum execution
Quantum gradients
Noise experiments
```

### Boundary

Research code should depend on the QMLShield research abstractions where practical rather than spreading PennyLane-specific implementation details throughout unrelated modules.

PennyLane is an implementation framework, not the research concept itself.

---

## 5.2 PyTorch

**Role:** Classical tensor computation, optimization, and neural-network-based components.

PyTorch is used for:

- tensor operations;
- classical model training;
- optimization;
- automatic differentiation;
- adversarial attack computation;
- classical autoencoder experiments;
- integration with differentiable QML components.

PyTorch is particularly important for maintaining a common differentiable workflow between:

```text
Classical Input
        ↓
Attack
        ↓
Encoding
        ↓
Quantum Model
        ↓
Loss
        ↓
Gradient
```

and for the classical baseline:

```text
Input
   ↓
CAE
   ↓
Purified Input
```

### Boundary

PyTorch should be used where classical tensor computation or optimization is required.

It should not define QMLShield's research architecture by itself.

---

## 5.3 NumPy

**Role:** Fundamental numerical array operations.

NumPy provides:

- array manipulation;
- numerical transformations;
- interoperability with scientific Python tooling;
- lightweight numerical utilities.

NumPy may appear at interfaces between:

```text
Dataset
Quantum Framework
Classical Framework
Evaluation
```

where array-based representations are appropriate.

NumPy should not become a replacement for the primary tensor/autodiff workflow where gradients are required.

---

## 5.4 SciPy

**Role:** Scientific computing utilities.

SciPy provides supporting functionality where required for:

- numerical operations;
- scientific analysis;
- mathematical utilities;
- statistical or optimization-related utilities.

SciPy is a supporting dependency rather than a primary QML framework.

---

## 5.5 scikit-learn

**Role:** Classical machine-learning utilities and evaluation support.

scikit-learn may be used for:

- data splitting;
- preprocessing utilities;
- classical evaluation;
- supporting baselines;
- statistical utilities where appropriate.

It is not the primary framework for the victim VQC or QAE.

---

## 5.6 pandas

**Role:** Structured experiment data handling and analysis.

pandas is used for:

- experiment result tables;
- aggregation;
- comparison;
- result processing;
- exporting structured data;
- analysis preparation.

Typical flow:

```text
Raw Experiment Output
        ↓
pandas
        ↓
Processed Results
        ↓
Tables / Figures
```

pandas should not become the primary representation of high-frequency numerical tensors used inside quantum or neural computation.

---

## 5.7 Matplotlib

**Role:** Research visualization.

Matplotlib is used for:

- accuracy plots;
- robustness comparisons;
- attack-performance curves;
- reconstruction analysis;
- resource comparisons;
- experiment figures.

Figures should be generated from recorded experiment data rather than manually constructed to match expected conclusions.

---

## 5.8 YAML

**Role:** Human-readable experiment configuration.

YAML is used for experiment parameters.

Current configuration entry point:

```text
configs/baseline.yaml
```

Configuration should contain scientifically meaningful parameters rather than implementation-specific secrets.

Typical configuration categories include:

```text
dataset
task
encoding
model
training
attack
purification
noise
evaluation
reproducibility
```

The exact schema should evolve with the experiment requirements.

---

# 6. Research and Development Dependencies

## 6.1 pytest

**Role:** Automated testing.

pytest is the primary testing framework.

It is used for:

- unit tests;
- component tests;
- configuration tests;
- numerical invariant tests;
- quantum-specific correctness tests.

Tests should validate scientific and software assumptions rather than merely maximize coverage.

---

## 6.2 Ruff

**Role:** Python linting and formatting.

Ruff is used for:

- linting;
- formatting;
- maintaining consistent Python code quality.

The project intentionally avoids unnecessary combinations of multiple overlapping Python formatting and linting systems.

---

## 6.3 Jupyter

**Role:** Interactive research environment.

Jupyter is used for:

- exploratory analysis;
- visualization;
- experiment inspection;
- debugging;
- result interpretation.

Jupyter is not the source of truth for QMLShield implementation.

Core implementation belongs under:

```text
src/qmlshield/
```

---

## 6.4 IPython / Jupyter Kernel

**Role:** Interactive Python execution support.

The kernel environment allows notebooks to execute against the same project environment managed by uv.

The notebook environment should use the project's declared dependencies rather than an undocumented global Python installation.

---

# 7. Version Control

## 7.1 Git

Git is the version-control system for QMLShield.

Git tracks:

- source code;
- configuration;
- documentation;
- tests;
- experiment definitions;
- reproducibility metadata where appropriate.

Git is also part of research traceability.

Important results should be traceable to the code version that produced them.

---

## 7.2 Lockfile

The following file is committed:

```text
uv.lock
```

The lockfile records resolved dependency versions and helps reconstruct the research environment.

It should be updated when dependencies materially change.

---

# 8. Dataset Technology

## 8.1 MNIST

The primary dataset is:

```text
MNIST
```

MNIST is used as the initial controlled benchmark for QMLShield.

The current research direction uses the full MNIST representation:

```text
Image:
28 × 28

Flattened dimension:
784
```

The initial task is intentionally controlled:

```text
Binary classification:
0 vs 1
```

with later progression toward:

```text
4-class
10-class
```

as the research pipeline becomes stable.

---

## 8.2 Dataset Handling

Dataset acquisition and preprocessing should remain deterministic and explicit.

The dataset layer is responsible for:

- loading;
- selecting the research task;
- preprocessing;
- normalization;
- splitting;
- preparing data for encoding.

Dataset logic should not be duplicated across notebooks and experiments.

---

# 9. Quantum Representation

## 9.1 Amplitude Encoding

The initial encoding strategy is:

```text
Amplitude Encoding
```

For the full MNIST input:

```text
784 features
```

The required quantum state space must satisfy:

```text
2^n ≥ 784
```

Therefore:

```text
n = 10 qubits
```

because:

```text
2^9  = 512
2^10 = 1024
```

The 10-qubit requirement is a consequence of the selected representation and amplitude encoding, not an intrinsic requirement of MNIST.

The handling of unused state-space dimensions must remain consistent across experiments.

---

## 9.2 Encoding Boundary

The encoding layer is responsible for transforming the classical representation into the quantum representation expected by the quantum model.

Conceptually:

```text
Classical Input
      ↓
Preprocessing
      ↓
Amplitude Encoding
      ↓
Quantum State
```

Attack generation remains in the classical input domain for the initial threat model.

---

# 10. Quantum Model Technology

## 10.1 Variational Quantum Classifier

The primary victim model is a:

```text
Variational Quantum Classifier (VQC)
```

The VQC is implemented using PennyLane and integrated with the classical optimization workflow.

The exact circuit architecture remains an experimental parameter rather than a universal project constant.

Potential parameters include:

```text
Number of Qubits
Circuit Depth
Trainable Parameters
Entanglement Pattern
Measurement Strategy
Optimizer
Learning Rate
Training Epochs
```

These belong to experiment configuration and methodology rather than to this technology document.

---

# 11. Adversarial Machine Learning Technology

## 11.1 FGSM

The first adversarial attack is:

```text
Fast Gradient Sign Method (FGSM)
```

FGSM is used as the initial attack because it provides a relatively simple mechanism for validating the end-to-end adversarial pipeline.

The attack operates on the classical input representation.

Conceptual boundary:

```text
Classical Input
      ↓
Gradient
      ↓
FGSM Perturbation
      ↓
Adversarial Classical Input
      ↓
Quantum Encoding
      ↓
VQC
```

---

## 11.2 PGD

The next attack is:

```text
Projected Gradient Descent (PGD)
```

PGD is intended as a stronger iterative attack after the FGSM pipeline has been validated.

Its implementation should preserve the same threat-model boundary:

```text
Classical Input Space
```

The exact perturbation budget and number of iterations are experiment parameters.

---

## 11.3 Adaptive Attacks

Adaptive attacks are intentionally deferred.

They may be introduced after the basic defense comparison is stable.

The purpose of adaptive evaluation is to determine whether a defense remains effective when the attacker explicitly accounts for the purification mechanism.

No adaptive attack implementation is required for the initial baseline stage.

---

# 12. Purification Technology

## 12.1 Classical Autoencoder

The primary classical purification baseline is:

```text
Classical Autoencoder (CAE)
```

Its purpose is to establish a classical reconstruction-based reference point.

Pipeline:

```text
Adversarial Classical Input
        ↓
CAE
        ↓
Purified Classical Input
        ↓
Amplitude Encoding
        ↓
VQC
```

The CAE is a baseline, not merely an implementation convenience.

---

## 12.2 Quantum Autoencoder

The primary quantum-native purification mechanism is:

```text
Quantum Autoencoder (QAE)
```

Pipeline:

```text
Adversarial Classical Input
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
```

QAE is evaluated as a research hypothesis.

The stack does not assume that a quantum autoencoder automatically removes adversarial information.

---

## 12.3 qGAN

qGAN-based purification is not part of the initial core stack.

It is considered a potential later extension after the baseline CAE/QAE comparison is established.

This prevents the first implementation from becoming unnecessarily broad.

---

# 13. Quantum Execution Strategy

QMLShield uses a staged execution strategy.

## Stage 1 — Ideal Simulation

Purpose:

```text
Validate the research mechanism
```

The first experiments should minimize physical noise so that adversarial and purification behavior can be isolated.

---

## Stage 2 — Noisy Simulation

Purpose:

```text
Evaluate robustness under controlled quantum noise
```

Noise is treated as a separate experimental variable.

The noise model and parameters must be explicitly recorded.

---

## Stage 3 — Real QPU

Purpose:

```text
Optional hardware validation
```

Real QPU execution is not required for the initial v0.1 research baseline.

A hardware experiment should only be introduced when it answers a meaningful research question that cannot be adequately addressed by simulation.

---

# 14. Framework Boundary

The primary conceptual boundary is:

```text
QMLShield Research Logic
            ↓
    Framework Interfaces
       ↙          ↘
  PennyLane      PyTorch
       ↓             ↓
Quantum Exec     Classical Exec
```

The project should avoid coupling every module directly to every framework.

For example:

```text
Evaluation
```

should not need to know the internal details of a PennyLane circuit if it can evaluate model outputs through a defined interface.

Similarly, a data loader should not contain quantum circuit construction logic.

---

# 15. Technology-to-Module Mapping

| QMLShield Module | Primary Technologies | Responsibility |
|---|---|---|
| `data/` | torchvision, NumPy, scikit-learn | Dataset loading and preprocessing |
| `encoding/` | PennyLane, NumPy | Classical-to-quantum representation |
| `models/` | PennyLane, PyTorch | VQC and model implementations |
| `attacks/` | PyTorch, NumPy | Adversarial perturbations |
| `purification/` | PyTorch, PennyLane | CAE and QAE |
| `noise/` | PennyLane | Controlled noisy execution |
| `evaluation/` | NumPy, pandas, scikit-learn | Metrics and evaluation |
| `experiments/` | Python, YAML | Experiment orchestration |
| `utils/` | Python ecosystem | Small shared utilities |

This mapping is conceptual. A module should not import a technology merely because the table lists it.

---

# 16. Technology Boundaries

The following boundaries should remain clear:

### Data

```text
Dataset → preprocessing → experiment representation
```

### Encoding

```text
Classical representation → quantum representation
```

### Model

```text
Representation → VQC → prediction
```

### Attack

```text
Classical input → adversarial classical input
```

### Purification

```text
Classical or quantum representation
        ↓
Purified representation
```

### Noise

```text
Quantum execution → controlled noise
```

### Evaluation

```text
Predictions + targets + metadata → metrics
```

### Experiment

```text
Configuration → pipeline → results
```

---

# 17. Technologies Intentionally Not Required for v0.1

The following technologies are not part of the mandatory v0.1 stack:

```text
Qiskit
TensorFlow
JAX
D-Wave Ocean
Qiskit Machine Learning
Ray
MLflow
Weights & Biases
Docker
Kubernetes
Database systems
Cloud orchestration
Real-time serving infrastructure
```

Their absence is intentional.

QMLShield is currently a focused research project rather than a production ML platform.

A new technology should be introduced only when it solves a demonstrated research or engineering requirement.

---

# 18. Qiskit Boundary

Qiskit is not required for the current v0.1 implementation.

This does not imply that Qiskit is unsuitable for quantum research.

It simply means that QMLShield currently uses:

```text
PennyLane
```

as its primary quantum ML framework.

A second quantum framework should only be introduced if cross-framework validation becomes a meaningful research question.

---

# 19. Hardware and Backend Abstraction

The project should avoid designing a large backend abstraction before hardware execution becomes necessary.

The initial requirement is simply:

```text
Ideal simulator
        ↓
Noisy simulator
        ↓
Optional QPU
```

If multiple real backends become scientifically relevant, the architecture may introduce an explicit backend abstraction at that point.

---

# 20. Reproducibility Stack

QMLShield reproducibility depends on the combination:

```text
Python 3.12
      +
uv
      +
pyproject.toml
      +
uv.lock
      +
Configuration
      +
Seed
      +
Git commit
```

A result should ideally be reconstructable from this information.

---

# 21. Development Workflow

The intended development loop is:

```text
Edit
  ↓
Run Tests
  ↓
Run Lint / Format
  ↓
Run Small Experiment
  ↓
Inspect Results
  ↓
Update Documentation if Required
  ↓
Commit
```

Large experiments should not be the first validation step for a new component.

Use small tests and smoke experiments before expensive quantum experiments.

---

# 22. Local Development Environment

The current environment is designed for local research development.

The baseline environment is:

```text
Operating System:
Windows-compatible local development

Python:
3.12

Environment:
uv-managed virtual environment

Source Layout:
src/qmlshield/

Version Control:
Git
```

The research code should avoid depending on machine-specific absolute paths.

---

# 23. Dependency Discipline

Dependencies should be added only when they provide a clear benefit.

Before adding a dependency, consider:

1. Is the functionality already available?
2. Does the dependency materially simplify the research?
3. Is it maintained?
4. Does it introduce compatibility risks?
5. Does it increase reproducibility burden?
6. Is it needed now rather than for a hypothetical future feature?

Small research projects benefit from a small dependency surface.

---

# 24. Technology Selection Rules

A new technology should satisfy at least one of the following:

```text
Required by a research question
Required by an experiment
Required for reproducibility
Required for correctness
Required for meaningful hardware validation
Required to solve a demonstrated engineering limitation
```

"Industry standard" alone is not sufficient justification.

"Quantum" alone is not sufficient justification.

---

# 25. Technology Decision Record

When a technology materially changes the research environment, record:

```text
Technology
Reason for adoption
Problem solved
Alternatives considered
Research impact
Compatibility impact
Reproducibility impact
```

This does not require a formal architecture decision record for every package.

The level of documentation should match the significance of the decision.

---

# 26. Versioning Policy

The project should distinguish between:

```text
Research decisions
```

and:

```text
Dependency upgrades
```

A dependency upgrade that may affect numerical behavior, quantum simulation, optimization, or reproducibility should be treated carefully.

Important experiments should not be silently regenerated under a materially different environment and presented as identical results.

---

# 27. Resource Awareness

Quantum experiments can be significantly more expensive than ordinary unit tests.

The technology stack therefore follows:

```text
Unit Test
    ↓
Smoke Experiment
    ↓
Small Experiment
    ↓
Full Experiment
    ↓
Repeated Experiment
```

Use the smallest execution scale that can validate the current development step.

This is especially important for:

- full MNIST;
- 10-qubit amplitude encoding;
- QAE;
- repeated seeds;
- noisy simulation;
- large attack sets.

---

# 28. Technology and Research Integrity

Technology choices must not obscure scientific limitations.

For example:

```text
A powerful simulator
```

does not imply:

```text
Real quantum hardware robustness.
```

Likewise:

```text
PyTorch autograd
```

does not automatically guarantee:

```text
A valid adversarial attack under the intended threat model.
```

And:

```text
PennyLane execution
```

does not automatically establish:

```text
Quantum advantage.
```

Framework capability and research evidence are different things.

---

# 29. Technology Stack Invariants

For the current QMLShield v0.1 baseline:

```text
Python = 3.12
Package manager = uv
Quantum ML framework = PennyLane
Classical tensor framework = PyTorch
Primary dataset = MNIST
Primary encoding = amplitude encoding
Initial victim = VQC
Initial attack = FGSM
Next attack = PGD
Primary classical defense = CAE
Primary quantum defense = QAE
Initial execution = ideal simulation
Next execution stage = noisy simulation
Testing = pytest
Linting / formatting = Ruff
Version control = Git
Dependency lock = uv.lock
```

These are the current baseline decisions.

They may evolve as research evidence or implementation requirements justify change.

---

# 30. Future Technology Extensions

Potential future technologies may include:

```text
Qiskit
Qiskit Machine Learning
Additional quantum simulators
Additional QPU providers
Specialized adversarial ML libraries
Experiment tracking systems
Distributed experiment execution
Containerization
Hardware-specific tooling
```

These are possibilities, not commitments.

The project should not adopt them until the research requires them.

---

# 31. Technology Stack Success Criteria

The technology stack is successful when it enables QMLShield to:

- reproduce the intended experiments;
- implement the defined threat model;
- train and evaluate the VQC;
- generate controlled adversarial examples;
- compare CAE and QAE;
- evaluate ideal and noisy execution;
- record meaningful metrics;
- run repeatable experiments;
- produce traceable figures and tables;
- and support future extensions without premature infrastructure.

The stack does not need to become a general-purpose QML platform.

---

# 32. Relationship to Other Documents

```text
research_questions.md
    ↓
Defines WHAT QMLShield investigates

threat_model.md
    ↓
Defines WHAT ATTACKER and SECURITY BOUNDARIES are considered

methodology.md
    ↓
Defines HOW experiments are conducted

architecture.md
    ↓
Defines HOW the software is structured

rules.md
    ↓
Defines ENGINEERING + RESEARCH CONSTRAINTS

tech_stack.md
    ↓
Defines WHICH TECHNOLOGIES implement the system

experiments.md
    ↓
Defines CONCRETE EXPERIMENTS

findings.md
    ↓
Records OBSERVED EVIDENCE
```

The technology stack should support these documents without redefining their responsibilities.

---

# 33. Status

**Current status:** Technology baseline defined for QMLShield v0.1.

The current stack intentionally remains compact:

```text
Python
uv
PennyLane
PyTorch
NumPy
SciPy
scikit-learn
pandas
Matplotlib
PyYAML
pytest
Ruff
Jupyter
Git
```

The stack is sufficient for the current research direction:

```text
MNIST
  ↓
Classical Adversarial Attack
  ↓
Amplitude Encoding
  ↓
VQC
  ↓
CAE / QAE Purification
  ↓
Ideal / Noisy Quantum Execution
  ↓
Evaluation
```

The project should expand this stack only when a demonstrated research or engineering requirement makes the expansion worthwhile.
