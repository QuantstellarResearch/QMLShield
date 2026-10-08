# AGENTS.md

QMLShield — a research project on adversarial robustness and purification for Variational Quantum Classifiers (VQCs). See `README.md` for the overview.

## Current state (important)

- `docs/` is complete and is the **specification**.
- `src/qmlshield/` is a **skeleton**: every module file is currently empty (0 bytes). Do not assume any implementation exists.
- The project is at the start of EXP-01 (clean VQC baseline).

## Commands (uv, Python 3.12)

- `uv sync` — install dependencies (`uv.lock` is committed; keep it in sync when dependencies change).
- `uv run pytest` — run tests (currently placeholder files; reports "no tests ran").
- `uv run ruff check .` and `uv run ruff format --check .` — lint/format (Ruff defaults; no config in `pyproject.toml`).
- Environment check: `uv run python -c "import pennylane, torch, torchvision, numpy, scipy, sklearn, pandas, matplotlib; print('QMLShield environment OK')"`

## Package setup

- `qmlshield` is installed as an editable package via Hatchling (`[build-system]` in `pyproject.toml`); after `uv sync`, `import qmlshield` resolves to `src/qmlshield/`.
- Pytest is configured with `pythonpath = ["src"]` (`[tool.pytest.ini_options]`), so tests import the package without depending on the editable install.
- `__init__.py` exists for `qmlshield` and every subpackage.

## Sources of truth (read before changing behavior)

- `docs/rules.md` — binding engineering and research rules.
- `docs/architecture.md` — module boundaries and dependency direction.
- `docs/experiments.md` — experiment protocol and decision gates (EXP-01 … EXP-08).
- `docs/research_questions.md`, `docs/threat_model.md`, `docs/methodology.md`, `docs/tech_stack.md`.
- If a change alters research assumptions, methodology, or architecture, update the matching doc (`rules.md` §41). Docs must not describe behavior the code no longer follows.

## Conventions most likely to be violated

- **Config-driven.** No hard-coded scientific parameters (epsilon, qubits, depth, learning rate, split, seeds). They belong in `configs/*.yaml`; fail loudly on invalid config instead of silently defaulting.
- **Results boundary.** Experiment outputs go to `results/{raw,processed,figures,tables}` only — never into `src/`, `experiments/`, or notebooks.
- **Module boundaries.** `src/qmlshield/` is organized by research concept (data, encoding, models, attacks, purification, noise, evaluation, experiments, utils), not by library — do not create `pennylane/` or `torch/` modules. Domain modules must not import `experiments/`, `results/`, or notebooks.
- **Two `experiments` directories.** Top-level `experiments/0X_*/` holds per-experiment drivers and analysis; `src/qmlshield/experiments/` holds shared orchestration code (runner, registry, seeds).
- **Notebooks are not the source of truth.** Core logic lives in `src/qmlshield/`.
- **Findings discipline.** Never write conclusions into `docs/findings.md` before the corresponding experiment exists; keep observation, interpretation, and claim separate.
- **Minimal complexity.** Do not add dependencies or abstractions without a research need. PennyLane is the only quantum framework for v0.1; QUDORA Cloud is a later-stage hardware backend and is not wired in.

## Repo quirks

- `notebooks/01_mnist_exploration.ipynb`, `02_vqc_baseline.ipynb`, `03_fgsm_attack.ipynb`, `04_qae_analysis.ipynb` are currently empty **directories**, not notebook files.
- `experiments/` currently has `01_baseline` … `06_noise`, while `docs/experiments.md` defines eight stages (… `06_comparison`, `07_noise`, `08_adaptive`). The naming is not yet reconciled.
- `.agents/` and `skills-lock.json` are agent-skill tooling checked in on purpose — not project code.
- Proprietary license; contributions are limited to authorized contributors (see `CONTRIBUTING.md`, `LICENSE`).
