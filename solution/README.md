# Solution — More than a Hack 2027 (Ball Possession)

This directory contains the implementation, documentation, and dependencies for our frame-by-frame ball possession prediction pipeline.

> [!NOTE]
> All custom engineering work is encapsulated inside `solution/` to keep the challenge's original files pristine and guarantee full reproducibility.

---

## 📁 Directory Structure

```
solution/
├── src/            # Source code for data pipelines, feature engineering, and models
├── docs/           # Technical design records, experiment logs, and submission report
├── pyproject.toml  # Isolated environment definition and dependencies (uv)
├── uv.lock         # Deterministic dependency lockfile
└── README.md       # This file
```

* **[`src/`](src/)**: Modular Python implementation (`data loaders`, `feature extraction`, `models`, `post-processing`, `export`).
* **[`docs/`](docs/)**: Architecture notes, experiment tracking, and drafts for the official technical submission report ([`SUBMISSION.md`](../SUBMISSION.md)).
* Specifications and changes are tracked globally at the repository root in **[`openspec/`](../openspec/)** following the guidelines in **[`.agents/AGENTS.md`](../.agents/AGENTS.md)**.

---

## 🛠️ Environment & Dependency Management (`uv`)

The project uses **[`uv`](https://docs.astral.sh/uv/)** for fast, reproducible environment and dependency management.

### Environment Synchronization
From the project root:
```bash
uv sync --project solution
```
Or from inside `solution/`:
```bash
uv sync
```

### Running Modules
```bash
uv run --project solution python -m solution.src.<module_name>
```

### Adding New Dependencies
```bash
uv add --project solution <package_name>
```
