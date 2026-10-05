# Rules

> **Project:** QMLShield  
> **Domain:** Quantum Machine Learning Security  
> **Focus:** Adversarial Robustness & Purification for Quantum Machine Learning  
> **Research Stage:** QMLShield v0.1  
> **Status:** Research in progress

---

## 1. Purpose

This document defines the engineering and research rules that govern the QMLShield codebase.

The purpose of these rules is to keep the project:

- reproducible;
- testable;
- scientifically interpretable;
- maintainable;
- aligned with the research methodology;
- and resistant to unnecessary complexity.

These rules complement:

- `research_questions.md` — what QMLShield investigates;
- `threat_model.md` — what security threat is studied;
- `methodology.md` — how the threat is investigated;
- `architecture.md` — how the software implements the investigation.

This document defines the constraints that implementation and research workflow should follow.

---

## 2. Core Principle

> **Research integrity comes before implementation convenience.**

Code should make the research easier to understand and reproduce, not merely make an experiment run once.

A short implementation that produces irreproducible or uninterpretable results is not considered successful.

---

## 3. Source of Truth

The primary source of truth for implementation is:

```text
src/qmlshield/
```

The primary source of truth for experiment parameters is:

```text
configs/
```

The primary source of truth for research definitions is:

```text
docs/
```

The primary source of truth for experimental evidence is:

```text
results/
```

Notebooks are not the source of truth for core implementation.

---

## 4. Repository Boundaries

QMLShield uses the following boundaries:

```text
src/
    Core implementation

configs/
    Experiment configuration

experiments/
    Experiment-specific orchestration and analysis

notebooks/
    Interactive exploration and visualization

tests/
    Verification

results/
    Experimental outputs

docs/
    Research and engineering documentation
```

A file should be placed according to its responsibility rather than convenience.

---

## 5. No Notebook-Driven Core Implementation

Notebooks may be used for:

- exploration;
- visualization;
- debugging;
- rapid prototyping;
- result analysis.

Core logic must eventually live in:

```text
src/qmlshield/
```

Do not make a notebook the only place where:

- the VQC is defined;
- an attack is implemented;
- a purifier is implemented;
- metrics are calculated;
- or an experiment can be reproduced.

Preferred direction:

```text
src/qmlshield/
       ↓
experiment
       ↓
results
       ↓
notebook
       ↓
analysis
```

Not:

```text
notebook
       ↓
hidden implementation
```

---

## 6. Research Scope Rule

Do not implement future research stages merely because the architecture supports them.

The planned progression is:

```text
Clean VQC
    ↓
FGSM
    ↓
PGD
    ↓
CAE
    ↓
QAE
    ↓
Noise
    ↓
Adaptive Attack
```

The current stage should be completed and validated before unnecessary complexity from later stages is introduced.

For example:

- do not implement PGD before the FGSM pipeline is validated;
- do not implement QAE before the VQC and attack pipeline are stable;
- do not introduce noisy execution before the ideal pipeline is understood.

---

## 7. No Premature Generalization

Do not create abstractions solely because a future feature might exist.

Avoid introducing:

```text
large framework layers
plugin systems
dependency injection containers
complex registries
generic factories
deep inheritance hierarchies
```

unless an actual research requirement justifies them.

Prefer:

```text
small modules
small functions
small classes
explicit data flow
clear configuration
```

Generalize when repeated implementations demonstrate a real need.

---

## 8. Research Concepts Define Module Boundaries

Top-level modules should represent research responsibilities:

```text
data/
encoding/
models/
attacks/
purification/
noise/
evaluation/
experiments/
```

Do not organize the research architecture primarily around external libraries.

For example, avoid:

```text
pennylane/
torch/
qiskit/
```

as top-level research modules.

PennyLane and PyTorch are implementation dependencies, not research concepts.

---

## 9. Dependency Direction

Domain components must not depend on experiment outputs.

In particular:

```text
models/
```

must not depend on:

```text
results/
experiments/
notebooks/
```

and:

```text
attacks/
purification/
evaluation/
```

should not depend on specific result files.

The preferred direction is:

```text
Configuration
      ↓
Experiment Orchestration
      ↓
Domain Components
      ↓
Execution / Evaluation
      ↓
Results
```

Results must not become hidden inputs to the implementation unless an experiment explicitly requires a previously generated artifact.

---

## 10. Configuration Rule

Scientific parameters must not be scattered through source code.

Parameters that can materially affect an experiment should be configurable.

Examples:

```text
dataset
classes
encoding
qubit count
VQC depth
optimizer
learning rate
epochs
attack type
attack budget
PGD steps
purification type
noise model
seed
evaluation settings
```

The configuration should be version-controlled.

The current configuration entry point is:

```text
configs/baseline.yaml
```

Additional configuration files should be added only when the experiment set actually requires them.

---

## 11. No Silent Scientific Defaults

Defaults that materially affect scientific conclusions must be explicit.

Avoid code such as:

```python
epsilon = 0.1
```

inside an attack implementation if `epsilon` is intended to be an experimental variable.

Prefer:

```text
configuration
    ↓
attack constructor
    ↓
attack implementation
```

Implementation-level defaults are acceptable for non-scientific infrastructure when they do not silently alter research conclusions.

---

## 12. Experiment Reproducibility

Every meaningful experiment must be reproducible from:

```text
Code
+
Configuration
+
Seed
+
Dataset Definition
+
Dependency Lockfile
```

The experiment should record:

- experiment identifier;
- configuration;
- random seed(s);
- dataset/task;
- model configuration;
- attack configuration;
- defense configuration;
- noise configuration;
- metrics;
- result artifacts.

The project must not depend on undocumented local state.

---

## 13. Random Seed Rule

Randomness must be explicit where practical.

Record seeds for:

```text
dataset splitting
model initialization
training
attack generation
purifier training
simulator execution
```

A single seed is acceptable during early development and smoke testing.

Important comparative experiments should use multiple independent seeds once computational cost and methodology are established.

Do not claim statistical robustness from one arbitrary run.

---

## 14. Dataset Integrity

The primary dataset is MNIST.

The project should preserve a clear separation between:

```text
training
validation
test
```

The test set must not be used for:

- model training;
- purifier training;
- attack tuning;
- hyperparameter selection;
- defense selection.

Any deviation must be explicitly documented as part of a separate experiment.

---

## 15. Victim Model Integrity

The primary victim model is the VQC.

Attack generation must not silently modify model parameters.

Purification experiments should use the same trained victim model whenever the research question is a direct defense comparison.

The comparison:

```text
No Defense
vs
CAE
vs
QAE
```

should not simultaneously introduce an unrelated change to the victim VQC.

---

## 16. Attack Integrity

Attack implementations must:

1. operate within the defined threat model;
2. respect the configured perturbation constraints;
3. preserve the intended input domain;
4. record attack parameters;
5. avoid modifying persistent victim-model parameters.

Initial attacks are:

```text
FGSM
PGD
```

Adaptive attacks are a later research stage.

An attack should not be called "adaptive" unless it explicitly accounts for the defense being evaluated.

---

## 17. Attack Domain Rule

For the initial threat model:

```text
Attack Domain = Classical Input Space
```

The attacker perturbs the classical input before quantum encoding.

Do not silently introduce:

```text
direct quantum-state attacks
```

into the same experiment.

A direct quantum-state attack represents a different threat model and must be documented separately.

---

## 18. Adversarial Perturbation vs Quantum Noise

Always distinguish:

```text
Adversarial Perturbation
```

from:

```text
Quantum Noise
```

Adversarial perturbation is intentional manipulation by the attacker.

Quantum noise represents execution or physical imperfections.

Do not combine the two into a single unexplained "noise" parameter.

Experiments involving both should report them as separate controlled variables.

---

## 19. Purification Rule

Purification is a defense mechanism, not a guaranteed solution.

QMLShield must not assume:

```text
QAE automatically removes adversarial information.
```

Instead, QAE is evaluated as a research hypothesis.

The project must distinguish:

```text
Reconstruction Quality
```

from:

```text
Classification Recovery
```

Improved classification accuracy alone does not prove faithful reconstruction.

---

## 20. Baseline Rule

Every defense experiment should have an appropriate control.

The core comparison is:

```text
No Defense
vs
CAE
vs
QAE
```

where applicable.

Do not evaluate a new defense only against an unreported or inconsistent baseline.

The victim model, data split, attack, and evaluation protocol should remain controlled whenever the experiment is intended as a direct comparison.

---

## 21. Fair Comparison Rule

For direct comparisons, keep the following fixed whenever possible:

```text
Dataset
Data Split
Preprocessing
Encoding
Victim VQC
Training Protocol
Test Samples
Attack Algorithm
Attack Objective
Attack Budget
Evaluation Metrics
```

The defense should be the principal changed variable.

If another variable must change, the reason must be documented.

---

## 22. Metrics Rule

Robustness must never be reported using a single metric without context.

At minimum, relevant defense experiments should consider:

```text
Clean Accuracy
Robust Accuracy
Attack Success Rate
Clean Accuracy Degradation
Robustness Improvement
Reconstruction Quality
Resource Overhead
```

Not every metric must apply identically to every experiment.

The mandatory metric set is defined by the corresponding experimental protocol.

---

## 23. Clean Performance Rule

A defense must be evaluated on clean inputs as well as adversarial inputs.

Do not claim that a defense is successful merely because:

```text
Robust Accuracy ↑
```

if:

```text
Clean Accuracy ↓ significantly
```

Robustness and clean performance must be considered together.

---

## 24. Resource Reporting Rule

Quantum resources are part of the research result.

Where applicable, record:

```text
Qubit Count
Circuit Depth
Trainable Parameter Count
Measurement Shots
Runtime
Training Cost
```

A more complex quantum defense should not automatically be considered better because it improves one accuracy metric.

The research question includes the robustness–resource trade-off.

---

## 25. Noise Evaluation Rule

Noise must be introduced progressively.

Preferred sequence:

```text
Ideal Simulation
      ↓
Controlled Noisy Simulation
      ↓
Real QPU, if justified
```

Do not treat noisy simulation as equivalent to real hardware.

Do not claim hardware security solely from simulated noise.

Every noise experiment must record:

- noise model;
- relevant parameters;
- circuit location / affected operations;
- simulator/backend;
- shot configuration.

---

## 26. Testing Rule

Core research components must have tests.

At minimum, test:

```text
Data preprocessing
Encoding
VQC construction / execution
Attack constraints
Purification behavior
Metrics
Configuration loading
```

Tests should focus on:

- correctness;
- invariants;
- shape and dimensionality;
- parameter handling;
- deterministic behavior where expected;
- valid output ranges.

Do not write tests merely to increase coverage numbers.

---

## 27. Quantum-Specific Testing

Quantum code should be tested for both software correctness and scientific invariants.

Examples:

```text
Normalized state validity
Expected qubit count
Expected tensor / vector dimensions
Circuit construction
Parameter shape
Measurement output shape
Perturbation bound
Deterministic seed behavior where applicable
```

Tests should verify assumptions that could silently invalidate an experiment.

---

## 28. Test Isolation

Tests must not depend on:

```text
previous test execution
notebook state
local experiment artifacts
manually generated result files
specific machine paths
```

Temporary data should be generated inside the test environment.

Tests should remain fast enough to run during normal development.

Large-scale quantum experiments should not be required for ordinary unit-test execution.

---

## 29. Result Integrity

Raw experiment outputs must not be silently overwritten.

Recommended structure:

```text
results/
├── raw/
├── processed/
├── figures/
└── tables/
```

Processed results must remain traceable to their raw source.

Figures and tables should be reproducible from recorded data.

Do not manually edit numerical results inside final tables without preserving the source data and transformation.

---

## 30. Findings Rule

A finding must be supported by an actual experiment.

Do not write:

```text
"QAE improves robustness."
```

before evidence exists.

Prefer:

```text
Hypothesis:
QAE may improve robustness.

Experiment:
...

Observed result:
...

Finding:
...
```

Negative or inconclusive findings must be recorded when scientifically meaningful.

---

## 31. Claims and Evidence

Every significant research claim should be traceable to:

```text
Research Question
      ↓
Experiment
      ↓
Metric
      ↓
Result
      ↓
Finding
```

Avoid broad statements such as:

```text
"QML is secure."
"QAE solves adversarial attacks."
"Quantum purification is better."
```

unless the evidence genuinely supports the stated scope.

Claims must remain bounded by:

- dataset;
- model;
- attack;
- defense;
- noise condition;
- and experimental protocol.

---

## 32. External Literature Rule

Literature-derived facts must not be presented as experimental findings from QMLShield.

Distinguish:

```text
Literature says...
```

from:

```text
QMLShield observed...
```

The project should also distinguish:

```text
Established fact
Research hypothesis
Implementation decision
Experimental observation
Inference
```

When uncertain, the uncertainty should be made explicit.

---

## 33. Dependency Rule

Runtime and development dependencies should be managed through:

```text
pyproject.toml
uv.lock
```

Use:

```text
uv
```

for dependency management.

Do not manually install project dependencies into the environment without recording them in the project configuration.

The lockfile should be committed.

The virtual environment should not be committed.

---

## 34. Python Version Rule

The project currently targets:

```text
Python 3.12
```

The version is recorded through:

```text
.python-version
```

Changing the Python version is an environment decision and should be tested against the full dependency stack before adoption.

---

## 35. Formatting and Linting

Use:

```text
Ruff
```

for formatting and linting.

Code should pass the configured Ruff checks before a meaningful change is considered complete.

Do not introduce multiple overlapping formatting/linting systems without a clear reason.

---

## 36. Naming Rules

Names should reflect domain concepts.

Prefer:

```text
VQCClassifier
FGSMAttack
PGDAttack
QuantumAutoencoder
ClassicalAutoencoder
RobustnessMetrics
```

over vague names such as:

```text
Model1
AttackUtils
QuantumStuff
Helper
Processor
```

File and module names should remain concise and predictable.

Use `snake_case` for Python modules and functions.

Use `PascalCase` for classes.

Use `UPPER_SNAKE_CASE` only for true constants.

---

## 37. Function and Class Size

Prefer small functions with one clear responsibility.

A class should represent a meaningful domain concept or stateful behavior.

Avoid classes created only to group unrelated utility functions.

If a function becomes difficult to understand or test, split it according to domain responsibility rather than arbitrary line count.

---

## 38. Type Hints

Public functions and important internal interfaces should use type hints.

Type hints should improve:

- readability;
- editor support;
- interface clarity;
- testability.

Do not create excessively complicated generic type systems for simple research code.

---

## 39. Error Handling

Fail explicitly when an invalid scientific configuration is supplied.

Examples:

```text
invalid qubit count
invalid perturbation budget
incompatible tensor shape
invalid normalization
unsupported noise configuration
```

Do not silently correct scientific parameters.

For example, do not silently replace an invalid epsilon with a default epsilon.

A configuration error should be visible.

---

## 40. Logging

Experiments should record important execution information, including where appropriate:

```text
Experiment ID
Configuration
Seed
Dataset
Model
Attack
Defense
Noise
Runtime
Result location
```

Logging should provide enough information to understand what ran without becoming a replacement for structured experiment results.

---

## 41. Documentation Synchronization

If an implementation change affects:

- research questions;
- threat assumptions;
- methodology;
- architecture;
- experiment protocol;

the corresponding documentation must be updated.

Documentation should not describe behavior that the implementation no longer follows.

---

## 42. Architecture Change Rule

A change to:

```text
module boundaries
data flow
experiment semantics
attack assumptions
defense assumptions
```

must be treated as an architectural or research decision.

Do not silently introduce architectural changes inside an unrelated feature commit.

---

## 43. Experiment Naming

Experiments should use stable identifiers.

Current progression:

```text
01_baseline
02_fgsm
03_pgd
04_cae
05_qae
06_comparison
07_noise
08_adaptive
```

If the experimental design changes substantially, create a new experiment identifier or version rather than silently changing the meaning of an old result.

---

## 44. Git Rules

Git should preserve research history.

Meaningful commits should describe the actual change.

Prefer:

```text
feat: add MNIST data loader
feat: implement amplitude encoding
feat: add VQC baseline
test: validate amplitude normalization
feat: implement FGSM attack
docs: define threat model
```

over:

```text
update
fix stuff
changes
final
final2
```

Do not commit:

```text
.venv/
cache files
temporary files
large generated artifacts
private credentials
```

`uv.lock` should be committed.

---

## 45. Commit and Research Traceability

When a result is important, the experiment should be traceable to the code version that produced it.

Where practical, record:

```text
Git commit
Experiment ID
Configuration
Seed
```

This allows:

```text
Result
  ↓
Experiment
  ↓
Configuration
  ↓
Git Commit
```

to be reconstructed.

---

## 46. Pull Request / Review Rule

If QMLShield becomes collaborative, meaningful changes should be reviewed before merging.

Review should consider:

```text
Correctness
Research consistency
Threat-model consistency
Reproducibility
Tests
Documentation
Scope
```

A change that improves code quality but invalidates the experimental design should not be accepted without resolving the research implications.

---

## 47. Security and Secret Handling

Never commit:

```text
API keys
tokens
passwords
private credentials
personal access credentials
```

Research configurations should not contain secrets.

External service credentials, if ever required, must be supplied through environment variables or an appropriate secret-management mechanism.

---

## 48. Reproducible Environment Rule

The project environment should be reconstructable using:

```text
uv
pyproject.toml
uv.lock
.python-version
```

A fresh environment should be able to install the declared dependencies without relying on undocumented packages installed globally.

---

## 49. Scope Control

Before adding a new component, ask:

1. Does it answer an existing research question?
2. Is it required by the current experiment?
3. Does it reduce a real implementation problem?
4. Does it require a new dependency?
5. Does it change the threat model?
6. Does it change the methodology?
7. Does it create a new maintenance burden?

If the component is only useful for a hypothetical future feature, defer it.

---

## 50. Scientific Honesty Rule

The project must distinguish:

```text
What we expect
```

from:

```text
What we observe
```

and:

```text
What we can conclude
```

The following progression should be preserved:

```text
Hypothesis
    ↓
Experiment
    ↓
Observation
    ↓
Analysis
    ↓
Finding
    ↓
Bounded Claim
```

Never reverse this process by deciding the conclusion first and designing an experiment only to support it.

---

## 51. Failure-First Mindset

A failed experiment is not automatically wasted work.

Record failures when they reveal:

- attack limitations;
- purifier limitations;
- noise sensitivity;
- unstable training;
- poor reconstruction;
- resource bottlenecks;
- invalid assumptions.

The project should preserve informative negative results instead of hiding them because they do not support the preferred hypothesis.

---

## 52. Minimal-Complexity Rule

When two implementations provide the same research capability, prefer the simpler one.

For example:

```text
simple function
```

is preferred over:

```text
multi-layer abstraction framework
```

unless the additional structure provides a measurable benefit.

QMLShield is a research project.

The goal is not to maximize software architecture complexity.

---

## 53. Definition of Done

A meaningful QMLShield implementation task is considered complete when applicable:

```text
Implementation
    ✓ works

Tests
    ✓ pass

Configuration
    ✓ recorded

Documentation
    ✓ aligned

Experiment
    ✓ reproducible

Results
    ✓ traceable
```

Not every small change requires all six items, but any change affecting research behavior should satisfy the relevant parts.

---

## 54. Rule Priority

When rules conflict, use this priority:

```text
1. Research integrity
2. Threat-model correctness
3. Experimental reproducibility
4. Architecture boundaries
5. Software correctness
6. Maintainability
7. Convenience
```

Convenience must not override research validity.

---

## 55. Rule Evolution

These rules are not immutable.

A rule may be changed when:

- a research requirement changes;
- an experiment exposes a limitation;
- the architecture evolves;
- a dependency constraint requires it;
- or collaboration requirements change.

When a rule is changed, the reason should be documented rather than silently replacing the previous rule.

---

## 56. Relationship to Other Documents

```text
research_questions.md
    ↓
Defines WHAT we investigate

threat_model.md
    ↓
Defines SECURITY BOUNDARIES

methodology.md
    ↓
Defines HOW we investigate

architecture.md
    ↓
Defines SOFTWARE STRUCTURE

rules.md
    ↓
Defines ENGINEERING + RESEARCH CONSTRAINTS

tech_stack.md
    ↓
Defines TECHNOLOGIES

experiments.md
    ↓
Defines CONCRETE EXPERIMENTS

findings.md
    ↓
Records OBSERVED EVIDENCE
```

The rules in this document should support, not replace, the decisions contained in the other documents.

---

## 57. Status

**Current status:** Engineering and research rules defined for QMLShield v0.1.

The rules establish:

- source-of-truth boundaries;
- research scope discipline;
- configuration discipline;
- reproducibility;
- testing;
- scientific claim discipline;
- dependency management;
- coding conventions;
- Git practices;
- security hygiene;
- and controlled architecture evolution.

The rules should remain intentionally lightweight.

They should help QMLShield remain rigorous without turning a small research project into unnecessary process.
