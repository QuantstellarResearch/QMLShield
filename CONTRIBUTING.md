# Contributing to QMLShield

Thank you for contributing to QMLShield.

QMLShield is a research-oriented project in Quantum Machine Learning Security, focused on adversarial robustness and purification for quantum machine learning. Contributions should prioritize research integrity, reproducibility, clarity, and a small, maintainable codebase.

## 1. Before Contributing

Before making a significant change:

1. Read the relevant documentation in `docs/`.
2. Understand the current research questions and threat model.
3. Check whether the change affects the experimental methodology.
4. Avoid introducing dependencies or abstractions that are not currently needed.
5. For research-impacting changes, explain why the change is necessary.

The core documentation is:

```text
docs/
├── research_questions.md
├── threat_model.md
├── methodology.md
├── architecture.md
├── rules.md
├── tech_stack.md
├── experiments.md
└── findings.md
```

## 2. Repository Structure

The main source of truth is:

```text
src/qmlshield/
```

Use the existing module boundaries:

```text
data/
encoding/
models/
attacks/
purification/
noise/
evaluation/
experiments/
utils/
```

Notebooks are for exploration and analysis, not for core implementation.

Do not place production research logic only inside a notebook.

## 3. Research Integrity

QMLShield is a research project. Code changes can affect scientific conclusions.

When modifying research-critical behavior:

- document the reason for the change;
- preserve reproducibility;
- update relevant documentation;
- identify experiments affected by the change;
- do not silently change scientific defaults;
- do not modify results to match an expected hypothesis.

If a change invalidates previous results, say so explicitly.

Negative or unexpected results should be preserved when scientifically meaningful.

## 4. Experiments

When adding or modifying an experiment:

- give it a clear experiment identifier;
- define its objective;
- state what is controlled and what is varied;
- record relevant configuration;
- record random seeds;
- preserve the no-defense baseline where applicable;
- use the existing metrics unless a new metric is justified;
- save reproducible result artifacts.

Follow the progression defined in `experiments.md`.

Do not introduce a more complex experiment before the required baseline is stable.

## 5. Code Changes

Keep implementations simple and aligned with the research domain.

Prefer:

```text
clear functions
small modules
explicit data flow
simple interfaces
```

Avoid premature:

```text
factories
registries
plugin systems
deep inheritance
dependency injection frameworks
unnecessary abstractions
```

Add abstraction when a real requirement emerges.

## 6. Dependencies

Use the project's existing environment and dependency workflow.

```text
uv
pyproject.toml
uv.lock
```

Before adding a dependency, ask:

- Is it actually required?
- Is the functionality already available?
- Does it materially simplify the implementation?
- Does it introduce compatibility or reproducibility concerns?

Do not add a library merely because it may be useful in the future.

## 7. Testing

Run tests before submitting a change:

```bash
uv run pytest
```

For code-quality checks:

```bash
uv run ruff check .
uv run ruff format --check .
```

For a new component, add tests when the behavior is important enough to protect.

Research-critical numerical or quantum behavior should have appropriate correctness checks.

## 8. Reproducibility

For research-impacting changes, preserve:

```text
Python version
Dependency environment
Configuration
Random seed
Experiment identifier
Git revision
```

Avoid machine-specific absolute paths and undocumented environment assumptions.

## 9. Documentation

Update documentation when a change affects:

- research questions;
- threat model;
- methodology;
- architecture;
- rules;
- technology stack;
- experiments;
- findings.

Do not duplicate the same definition across multiple documents when one document should be the source of truth.

## 10. Findings

Do not write conclusions into `findings.md` before the corresponding experiment has been performed.

When reporting a result, distinguish:

```text
Observation
Interpretation
Claim
Limitation
```

Claims must not be stronger than the evidence.

## 11. Commit Messages

Keep commit messages short and descriptive.

Examples:

```text
feat: add FGSM attack pipeline
feat: implement QAE purifier
fix: correct amplitude normalization
test: add VQC encoding tests
docs: update experiment protocol
refactor: simplify evaluation pipeline
```

Avoid vague messages such as:

```text
update
changes
fix stuff
work
```

## 12. Pull Requests

A pull request should explain:

- what changed;
- why it changed;
- which files or modules are affected;
- whether the research methodology is affected;
- whether existing experiments or results are affected;
- how the change was tested.

For research-impacting changes, include the relevant experiment or evidence impact.

## 13. Scope Discipline

Keep contributions focused.

A change should not silently expand QMLShield from:

```text
Adversarial Robustness & Purification
```

into unrelated areas such as:

```text
privacy
model extraction
poisoning
general quantum cryptography
hardware security
```

unless the research scope is intentionally changed and documented.

## 14. Security and Sensitive Data

Do not commit:

```text
API keys
credentials
tokens
private datasets
personal information
local secrets
```

Use environment variables or appropriate local configuration for secrets.

Do not include sensitive information in experiment artifacts.

## 15. Contribution Standard

A good QMLShield contribution should be:

```text
Correct
Reproducible
Research-relevant
Testable
Documented
Minimal
```

The goal is not to maximize code volume.

The goal is to build reliable research evidence with a codebase that remains understandable.

## 16. Final Principle

> **Contribute to the research, not just the code.**

Every meaningful contribution should help QMLShield become more correct, reproducible, understandable, or scientifically useful.
