# Architecture

> **Project:** QMLShield  
> **Domain:** Quantum Machine Learning Security  
> **Focus:** Adversarial Robustness & Purification for Quantum Machine Learning  
> **Research Stage:** QMLShield v0.1  
> **Status:** Research in progress

---

## 1. Purpose

This document defines the software and research architecture of QMLShield.

It translates the research methodology into a maintainable, testable, and reproducible Python codebase.

The architecture is designed around the principle:

> **Research concepts are the primary architectural boundaries; implementation libraries are dependencies inside those boundaries.**

QMLShield should therefore be organized around:

```text
Data
  ↓
Encoding
  ↓
Victim Model
  ↓
Attack
  ↓
Purification
  ↓
Noise
  ↓
Evaluation
  ↓
Experiment
```

rather than around individual external frameworks.

For example, PennyLane and PyTorch are implementation technologies. They should not determine the top-level research architecture.

---

## 2. Architectural Goals

The architecture has six primary goals.

### 2.1 Reproducibility

An experiment should be reproducible from:

```text
Source Code
+
Configuration
+
Seed
+
Dependency Lockfile
+
Dataset Definition
```

### 2.2 Separation of Concerns

Dataset handling, quantum encoding, model definition, adversarial attacks, purification, noise, and evaluation should remain independently testable.

### 2.3 Experimentability

The architecture must make it easy to run:

```text
Baseline
→ FGSM
→ PGD
→ CAE
→ QAE
→ Noise
→ Adaptive Attack
```

without rewriting the core model implementation.

### 2.4 Comparability

The architecture must support controlled comparisons between:

```text
No Defense
CAE
QAE
```

while keeping the victim VQC and other experimental variables fixed.

### 2.5 Extensibility

Future research should be able to introduce:

- qGAN purification;
- adaptive attacks;
- alternative encodings;
- additional noise models;
- Fashion-MNIST;
- alternative VQC architectures;
- real QPU backends;

without restructuring the entire project.

### 2.6 Research Integrity

The software architecture should make it difficult to accidentally mix:

- training data with test data;
- attack generation with evaluation;
- experimental configuration with source code;
- raw results with processed results;
- or research findings with assumptions.

---

## 3. High-Level Architecture

The QMLShield architecture is:

```text
                         ┌─────────────────────┐
                         │      Researcher      │
                         └──────────┬──────────┘
                                    │
                                    ▼
                         ┌─────────────────────┐
                         │ Experiment Config   │
                         │     + Runner        │
                         └──────────┬──────────┘
                                    │
                                    ▼
                    ┌──────────────────────────────┐
                    │       Research Pipeline      │
                    └──────────────┬───────────────┘
                                   │
         ┌─────────────────────────┼─────────────────────────┐
         │                         │                         │
         ▼                         ▼                         ▼
   ┌───────────┐            ┌────────────┐            ┌─────────────┐
   │   Data    │            │  Encoding  │            │   Models    │
   └─────┬─────┘            └─────┬──────┘            └──────┬──────┘
         │                         │                          │
         └─────────────────────────┼──────────────────────────┘
                                   │
                                   ▼
                            ┌────────────┐
                            │   Attack   │
                            └─────┬──────┘
                                  │
                                  ▼
                         ┌─────────────────┐
                         │  Purification   │
                         │ CAE / QAE / ... │
                         └────────┬────────┘
                                  │
                                  ▼
                            ┌────────────┐
                            │   Noise    │
                            └─────┬──────┘
                                  │
                                  ▼
                            ┌────────────┐
                            │ Evaluation │
                            └─────┬──────┘
                                  │
                                  ▼
                              Results
```

The exact runtime ordering of noise may vary by experiment. The diagram represents the architectural capabilities rather than forcing one physical circuit order for every experiment.

---

## 4. Research Architecture vs Software Architecture

QMLShield has two related but distinct architectural views.

### Research Architecture

```text
Dataset
  ↓
Preprocessing
  ↓
Encoding
  ↓
VQC
  ↓
Attack
  ↓
Purification
  ↓
Noise
  ↓
Evaluation
```

This describes the scientific experiment.

### Software Architecture

```text
src/qmlshield/
├── data/
├── encoding/
├── models/
├── attacks/
├── purification/
├── noise/
├── evaluation/
├── experiments/
└── utils/
```

This describes how the experiment is implemented.

The two views should remain aligned but should not be treated as identical.

---

## 5. Repository Architecture

The current repository structure is:

```text
qmlshield/
│
├── README.md
├── LICENSE
├── pyproject.toml
├── uv.lock
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
│
├── tests/
│
├── results/
│   ├── raw/
│   ├── processed/
│   ├── figures/
│   └── tables/
│
└── docs/
    ├── research_questions.md
    ├── threat_model.md
    ├── methodology.md
    ├── architecture.md
    ├── rules.md
    ├── tech_stack.md
    ├── experiments.md
    └── findings.md
```

This structure intentionally separates:

```text
Implementation
    src/

Configuration
    configs/

Experiment-specific work
    experiments/

Exploration
    notebooks/

Verification
    tests/

Evidence
    results/

Research documentation
    docs/
```

---

## 6. Component Boundaries

### 6.1 `data/`

Responsibility:

- dataset loading;
- dataset splitting;
- preprocessing;
- normalization;
- sample representation;
- deterministic data handling.

Example responsibilities:

```text
MNIST loading
Class filtering
Train/validation/test split
Normalization
Batch preparation
```

The data layer should not contain VQC-specific circuit logic.

---

### 6.2 `encoding/`

Responsibility:

- transform classical numerical representations into quantum representations;
- provide a stable encoding interface;
- expose encoding configuration.

Primary implementation:

```text
Amplitude Encoding
```

The encoding layer should not know whether the encoded state will subsequently be classified, attacked, or purified.

This separation allows future encoding experiments without rewriting the VQC or attack modules.

---

### 6.3 `models/`

Responsibility:

- define victim QML models;
- define model architectures;
- expose model inference and training interfaces.

Primary model:

```text
VQC
```

The model layer should not own adversarial attack generation.

It should provide the interfaces needed by an attack to evaluate model behavior and gradients where supported.

---

### 6.4 `attacks/`

Responsibility:

- generate adversarial examples;
- define attack configuration;
- enforce perturbation constraints;
- expose attack results.

Initial implementations:

```text
FGSM
PGD
```

Future implementations may include:

```text
Adaptive Attack
Black-box Attack
Quantum-State Attack
```

The attack module should not contain purification logic.

---

### 6.5 `purification/`

Responsibility:

- define purification mechanisms;
- transform adversarial inputs or encoded quantum states;
- expose purification configuration and outputs.

Initial implementations:

```text
CAE
QAE
```

Future:

```text
qGAN
Other purification mechanisms
```

The purifier should not own the victim classifier.

---

### 6.6 `noise/`

Responsibility:

- define controlled quantum-noise models;
- configure noise parameters;
- apply noise through the supported execution abstraction.

Noise is a separate concern from adversarial attacks.

The module should make it possible to evaluate:

```text
Attack
×
Noise
×
Defense
```

without embedding noise behavior directly inside the VQC implementation.

---

### 6.7 `evaluation/`

Responsibility:

- compute research metrics;
- compare experimental conditions;
- produce structured evaluation outputs.

Core metrics include:

```text
Clean Accuracy
Robust Accuracy
Attack Success Rate
Clean Accuracy Degradation
Robustness Improvement
Reconstruction Quality
Resource Overhead
```

The evaluation layer should not modify the model or attack.

It observes results and computes evidence.

---

### 6.8 `experiments/`

Responsibility:

- orchestrate experimental execution;
- resolve configurations;
- establish seeds;
- invoke pipeline components;
- save experiment outputs.

This layer connects:

```text
Configuration
    ↓
Data
    ↓
Model
    ↓
Attack
    ↓
Defense
    ↓
Noise
    ↓
Evaluation
    ↓
Results
```

It should orchestrate components rather than duplicate their internal logic.

---

### 6.9 `utils/`

Responsibility:

- shared non-domain-specific utilities;
- logging;
- reproducibility helpers;
- file and serialization helpers;
- common infrastructure.

`utils/` should remain small.

Domain logic must not be hidden inside a generic utilities module merely for convenience.

---

## 7. Dependency Direction

The architecture should follow a mostly one-way dependency direction:

```text
                    ┌─────────────┐
                    │   Config    │
                    └──────┬──────┘
                           │
                           ▼
                    ┌─────────────┐
                    │ Experiments │
                    └──────┬──────┘
                           │
       ┌───────────────────┼───────────────────┐
       ▼                   ▼                   ▼
     Data               Models             Attacks
       │                   │                   │
       ▼                   ▼                   │
   Encoding               │                   │
       │                   │                   │
       └───────────────────┼───────────────────┘
                           ▼
                      Purification
                           │
                           ▼
                         Noise
                           │
                           ▼
                       Evaluation
```

This is a conceptual dependency view, not a requirement that every module imports every module below it.

The key architectural rule is:

> **Domain components should not depend on experiment orchestration.**

For example:

```text
models/
```

should not import:

```text
experiments/
```

and:

```text
attacks/
```

should not import:

```text
results/
```

Experiment orchestration should depend on reusable domain components, not the reverse.

---

## 8. Data Flow

### 8.1 Clean Evaluation

```text
MNIST Sample
     ↓
Preprocessing
     ↓
Normalized Vector
     ↓
Amplitude Encoding
     ↓
Quantum Representation
     ↓
VQC
     ↓
Prediction
     ↓
Clean Metrics
```

### 8.2 Adversarial Evaluation

```text
MNIST Sample
     ↓
Preprocessing
     ↓
Attack
     ↓
Adversarial Vector
     ↓
Amplitude Encoding
     ↓
Quantum Representation
     ↓
VQC
     ↓
Prediction
     ↓
Robustness Metrics
```

### 8.3 CAE Evaluation

```text
MNIST Sample
     ↓
Preprocessing
     ↓
Attack
     ↓
Adversarial Vector
     ↓
CAE
     ↓
Purified Classical Vector
     ↓
Amplitude Encoding
     ↓
VQC
     ↓
Prediction
     ↓
Evaluation
```

### 8.4 QAE Evaluation

```text
MNIST Sample
     ↓
Preprocessing
     ↓
Attack
     ↓
Adversarial Vector
     ↓
Amplitude Encoding
     ↓
Adversarial Quantum State
     ↓
QAE
     ↓
Purified Quantum Representation
     ↓
VQC
     ↓
Prediction
     ↓
Evaluation
```

---

## 9. Core Interfaces

The initial implementation should favor small, explicit interfaces over a large framework.

Conceptually:

```python
Dataset
    load()
    split()
    preprocess()

Encoder
    encode(x)

Classifier
    train(...)
    predict(...)
    loss(...)

Attack
    generate(model, x, y, config)

Purifier
    train(...)
    purify(x)

Evaluator
    evaluate(...)
```

These are conceptual responsibilities, not requirements to introduce abstract base classes immediately.

A concrete implementation should be introduced only when an abstraction is justified by more than one implementation or by a clear testing requirement.

---

## 10. Pipeline Composition

The experiment runner should be able to compose the pipeline from configuration.

Conceptually:

```python
dataset = load_dataset(config.dataset)

model = build_model(config.model)

train(model, dataset.train)

attack = build_attack(config.attack)

purifier = build_purifier(config.purification)

result = run_experiment(
    dataset=dataset,
    model=model,
    attack=attack,
    purifier=purifier,
    noise=config.noise,
)
```

The exact implementation may differ.

The architectural principle is that experiment configuration determines composition while individual modules implement behavior.

---

## 11. Configuration Architecture

The current configuration entry point is:

```text
configs/
└── baseline.yaml
```

Configuration is responsible for experiment parameters, not source-code behavior.

Conceptual configuration sections:

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

The configuration should be serializable and version-controlled.

Parameters that affect scientific results should not be silently hard-coded inside implementation modules.

---

## 12. Configuration vs Code

The boundary is:

### Configuration

Defines:

```text
Which dataset?
Which classes?
Which encoding?
Which model configuration?
Which attack?
Which defense?
Which noise?
Which seed?
Which evaluation settings?
```

### Code

Defines:

```text
How does the dataset load?
How does amplitude encoding work?
How does the VQC execute?
How does FGSM calculate perturbations?
How does QAE purify?
How are metrics calculated?
```

This prevents experiment parameters from being scattered throughout the codebase.

---

## 13. Research Experiment Architecture

An experiment should follow:

```text
Experiment Definition
        ↓
Configuration
        ↓
Initialization
        ↓
Data Preparation
        ↓
Model Training
        ↓
Attack Generation
        ↓
Purification
        ↓
Noise / Execution
        ↓
Prediction
        ↓
Evaluation
        ↓
Result Persistence
```

Not every experiment will use every stage.

For example:

```text
Experiment 01
```

does not require an attack or purifier.

---

## 14. Baseline Architecture

### Experiment 01

```text
MNIST
  ↓
Preprocessing
  ↓
Amplitude Encoding
  ↓
10-Qubit VQC
  ↓
Prediction
  ↓
Clean Evaluation
```

This is the minimum architecture required before adversarial research begins.

The baseline must establish:

- data correctness;
- encoding correctness;
- model correctness;
- training correctness;
- evaluation correctness.

---

## 15. Attack Architecture

The attack layer sits between clean data preparation and victim execution.

```text
Clean Input
    ↓
Attack
    ↓
Adversarial Input
    ↓
Encoding
    ↓
VQC
```

The attack implementation must be able to access the model behavior required by the selected attack.

For white-box gradient-based attacks, this includes access to a differentiable loss path where supported.

The attack module should not modify the persistent model parameters.

---

## 16. Purification Architecture

Purification is modeled as a pluggable defense stage.

```text
                  ┌── No Defense ─────────────┐
                  │                            │
Adversarial Input ├── CAE → Encoding → VQC    │
                  │                            │
                  └── Encoding → QAE → VQC    │
```

This allows the experiment runner to select the defense without changing the victim model.

The purification interface should represent the transformation being studied rather than assume a specific implementation technology.

---

## 17. Noise Architecture

Noise should be applied through an execution abstraction where practical.

Conceptually:

```text
Circuit / Quantum Operation
            ↓
       Noise Model
            ↓
      Quantum Execution
            ↓
         Results
```

The VQC should not need to contain separate hard-coded branches for every noise model.

This supports controlled comparisons such as:

```text
Ideal
vs
Depolarizing
vs
Other selected model
```

when those experiments become justified.

---

## 18. Evaluation Architecture

Evaluation consumes outputs from the pipeline.

```text
                    ┌────────────────┐
                    │ Clean Outputs  │
                    └───────┬────────┘
                            │
                    ┌───────▼────────┐
                    │                │
Adversarial Outputs ─→  Evaluation   │
                    │                │
Purified Outputs ───→                │
                    │                │
Resource Data ──────→                │
                    └───────┬────────┘
                            │
             ┌──────────────┼──────────────┐
             ▼              ▼              ▼
         Accuracy       Robustness    Reconstruction
             │              │              │
             └──────────────┼──────────────┘
                            ▼
                      Result Record
```

Evaluation should be deterministic given the same recorded outputs and metric configuration.

---

## 19. Result Architecture

Results are separated into:

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

Normalized or aggregated result data.

### `figures/`

Research figures generated from processed results.

### `tables/`

Tables used for comparison and reporting.

The result directory is an output boundary.

Source modules should not depend on specific result files.

---

## 20. Notebook Boundary

Notebooks are for:

```text
Exploration
Visualization
Interactive analysis
Result interpretation
```

They are not the source of truth for core model or attack implementations.

The preferred direction is:

```text
src/qmlshield/
       ↓
Experiment
       ↓
Results
       ↓
Notebook
       ↓
Visualization / Analysis
```

rather than:

```text
Notebook
       ↓
Production implementation
```

This prevents hidden notebook state from becoming a research dependency.

---

## 21. Testing Architecture

Testing is divided into several levels.

### Unit Tests

Test isolated components:

```text
Data preprocessing
Encoding
Attack
Purification
Metrics
Configuration
```

### Integration Tests

Test component interactions:

```text
Data → Encoding → VQC
Data → Attack → VQC
Attack → CAE → Encoding → VQC
Attack → Encoding → QAE → VQC
```

### Experiment Smoke Tests

Run a minimal experiment to verify that the complete pipeline can execute.

The test architecture should prioritize correctness of transformations and invariants rather than trying to unit-test external quantum libraries themselves.

---

## 22. Architectural Invariants

The following invariants should hold unless an experiment explicitly studies a change.

### Invariant 1

The victim VQC is not modified by the attack generation process.

### Invariant 2

The test set is not used for training.

### Invariant 3

Defense comparison uses the same victim model whenever possible.

### Invariant 4

Attack parameters are explicitly recorded.

### Invariant 5

Purification does not silently change the evaluation protocol.

### Invariant 6

Noise configuration is explicitly recorded.

### Invariant 7

Experiment configuration is reproducible.

### Invariant 8

Core domain modules do not depend on experiment output files.

### Invariant 9

Research notebooks do not become the implementation source of truth.

### Invariant 10

Changing an external library should not require redesigning the research architecture unless its semantics affect the experiment.

---

## 23. Dependency Management

QMLShield uses:

```text
Python 3.12
uv
```

The environment is defined through:

```text
pyproject.toml
uv.lock
.python-version
```

The architecture should depend on stable interfaces exposed by the selected libraries rather than scattering framework-specific calls across every module.

Primary technologies currently include:

```text
PennyLane
PyTorch
TorchVision
NumPy
SciPy
scikit-learn
Pandas
Matplotlib
pytest
Ruff
Jupyter
```

Detailed technology decisions belong in `tech_stack.md`.

---

## 24. External Framework Boundary

PennyLane and PyTorch should remain infrastructure dependencies.

The preferred conceptual layering is:

```text
QMLShield Research Logic
        ↓
QMLShield Domain Modules
        ↓
PennyLane / PyTorch
        ↓
Simulator / Backend
```

This allows research logic to remain understandable even when implementation libraries change.

A direct framework call is acceptable when it is the natural implementation of a domain operation.

The architecture should not introduce unnecessary wrappers merely to hide every external API.

---

## 25. Future Extension Points

The architecture intentionally leaves extension points for:

### Additional Attacks

```text
attacks/
├── fgsm.py
├── pgd.py
└── adaptive.py
```

### Additional Purifiers

```text
purification/
├── cae.py
├── qae.py
└── qgan.py
```

### Additional Encodings

```text
encoding/
├── amplitude.py
└── ...
```

### Additional Models

```text
models/
├── vqc.py
└── ...
```

### Additional Noise Models

```text
noise/
├── channels.py
└── models.py
```

These extension points do not require implementation until a research question justifies them.

---

## 26. Architecture Evolution

The architecture should evolve according to actual research needs.

The project should **not** create:

```text
factories
registries
plugin systems
dependency injection frameworks
large abstract class hierarchies
```

unless multiple concrete implementations make them useful.

For QMLShield v0.1, explicit Python modules and small functions/classes are preferred.

The rule is:

> **Introduce an abstraction when it reduces real duplication or enables a real experiment, not because the future might need it.**

---

## 27. Architecture Decision Boundaries

The following decisions are currently established:

| Decision | Current Choice |
|---|---|
| Language | Python 3.12 |
| Environment | uv |
| Source layout | `src/` |
| Primary QML framework | PennyLane |
| Classical ML / autodiff | PyTorch |
| Primary dataset | MNIST |
| Initial task | Controlled binary classification |
| Encoding | Amplitude encoding |
| Full input representation | 784 dimensions |
| Initial qubit count | 10 |
| Victim model | VQC |
| Initial attack | FGSM |
| Next attack | PGD |
| Classical defense baseline | CAE |
| Quantum defense | QAE |
| Initial execution | Ideal simulator |
| Later execution | Noisy simulator |
| Experiment configuration | YAML |
| Testing | pytest |
| Lint / formatting | Ruff |

These decisions are implementation baselines, not permanent restrictions on future research.

---

## 28. Architecture and Research Integrity

The architecture should make it possible to answer:

> **Which component caused the observed change?**

For example, if robust accuracy improves after introducing QAE, the experiment should make it possible to distinguish:

```text
QAE effect
from
VQC architecture change
from
attack configuration change
from
data preprocessing change
from
random initialization
from
noise configuration
```

This is why component boundaries and controlled configuration are research requirements, not merely software-engineering preferences.

---

## 29. Recommended Initial Implementation Order

The architecture should be implemented progressively.

### Step 1

```text
data/
encoding/
models/
evaluation/
```

Implement the clean VQC baseline.

### Step 2

```text
attacks/
```

Add FGSM.

### Step 3

Add PGD.

### Step 4

```text
purification/
```

Add CAE.

### Step 5

Add QAE.

### Step 6

```text
noise/
```

Introduce controlled noisy execution.

### Step 7

Extend the experiment runner and configuration system as the experiment matrix grows.

This order mirrors the methodology and prevents unused architecture from accumulating.

---

## 30. Architecture Success Criteria

The architecture is considered successful when:

1. the clean VQC baseline can run reproducibly;
2. attack implementations can be added without modifying the victim model;
3. CAE and QAE can be compared under the same evaluation framework;
4. noise can be introduced without rewriting the research pipeline;
5. experiments can be configured without hard-coding scientific parameters;
6. core components can be tested independently;
7. results can be reproduced from recorded configuration and code;
8. notebooks remain analysis tools rather than hidden implementation dependencies;
9. future research extensions can be introduced incrementally;
10. the architecture remains small enough to understand.

---

## 31. Relationship to Other Documents

```text
research_questions.md
    ↓
Defines WHAT we want to learn

threat_model.md
    ↓
Defines AGAINST WHAT we evaluate

methodology.md
    ↓
Defines HOW the investigation is conducted

architecture.md
    ↓
Defines HOW THE SOFTWARE implements the investigation

rules.md
    ↓
Defines ENGINEERING CONSTRAINTS

tech_stack.md
    ↓
Defines TECHNOLOGIES

experiments.md
    ↓
Defines CONCRETE EXPERIMENTS

findings.md
    ↓
Records WHAT THE EVIDENCE SHOWS
```

Architecture must remain consistent with the research questions and threat model.

If an architectural change alters the scientific assumptions of an experiment, the corresponding methodology and experiment documentation must also be updated.

---

## 32. Status

**Current status:** Architecture defined for QMLShield v0.1.

The repository structure, component responsibilities, dependency direction, data flow, experiment composition, testing boundaries, configuration boundary, and extension points have been established.

The architecture intentionally leaves exact implementation details open where the research methodology has not yet fixed them.

The next implementation-level work should begin with the clean VQC baseline rather than implementing future attack, purification, or noise components prematurely.
