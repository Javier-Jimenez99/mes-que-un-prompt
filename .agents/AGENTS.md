# Agent Guidelines & Development Rules (Spec-Driven Development)

> [!IMPORTANT]
> These rules are MANDATORY for any AI agent operating within this repository at all times.

---

## 1. Language Standard (Strict English)

* **All code, specifications, documentation, comments, docstrings, commit messages, and artifact descriptions MUST be written in English.**
* Even if the user communicates in Spanish or another language, all repository content, generated files, markdown documents, and code must remain strictly in English.

---

## 2. Spec-Driven Development (OpenSpec Workflow)

The agent **MUST NOT** create, edit, or refactor code within `solution/src/` without an approved OpenSpec change:

1. **Proposal Phase:** For any new feature, model, or refactor, the agent must create a change folder under `openspec/changes/<change-name>/` containing:
   * `proposal.md`: Problem statement, context, and scope.
   * `design.md`: Technical architecture, classes, interfaces, and mathematical foundations.
   * `specs/`: Formal requirements with RFC 2119 keywords (`MUST`, `SHOULD`, `SHALL`) and test scenarios (*Given-When-Then*).
   * `tasks.md`: Dependency-ordered atomic task checklist (`- [ ]`).

2. **User Approval Phase:** The agent must summarize the proposal and **explicitly wait for user approval** before writing or modifying any implementation code in `solution/src/`.

3. **Implementation Phase:** Once approved, the agent executes tasks sequentially, marking progress in `tasks.md` (`- [x]`) and strictly adhering to the approved specifications.

4. **Archive Phase:** Upon completion and metric verification, the change is archived to `openspec/archive/` and its requirements are consolidated into `openspec/specs/`.

---

## 3. Workspace Scoping & Isolation

* The agent operates from the repository root.
* All custom code, feature extractors, models, pipelines, and artifacts developed for the solution **MUST reside strictly inside `solution/`**.
* The original competition benchmark files (`games.py`, `CHALLENGE.md`, `DATA.md`, `EVALUATION.md`, `evaluation/`, `starter/`, etc.) **MUST NOT be modified**, preserving the integrity of the official evaluation pipeline.

---

## 4. Verification & Submission Contract

* Any model change affecting possession labels must be benchmarked against the public 5-minute ground truth via:
  ```bash
  uv run --with numpy,pandas python evaluation/run_evaluation.py
  ```
* Every generated submission CSV must pass the official validation script without errors:
  ```bash
  uv run --with numpy,pandas python evaluation/validate_submission.py <submission_path>.csv
  ```
