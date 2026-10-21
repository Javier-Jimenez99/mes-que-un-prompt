# 📖 OpenSpec — Spec-Driven Development Framework

This directory implements the **OpenSpec** framework for AI-assisted, specification-driven development across this project.

---

## 🤝 Roles & Responsibilities

### 🧑‍💻 User Role (Lead Architect / Product Owner)
* Sets objectives, architectural priorities, and high-level requirements in natural language.
* Reviews and approves change proposals, designs, and task checklists before code execution begins.
* Has final decision authority on design trade-offs.

### 🤖 AI Agent Role (Implementation Engineer)
1. **Draft Specifications:** Creates proposed changes under `openspec/changes/<change-name>/` with:
   * `proposal.md`: Context, rationale, and scope.
   * `design.md`: Technical architecture, data contracts, and algorithmic details.
   * `specs/`: Formal RFC 2119 requirements (`MUST`, `SHOULD`, `SHALL`) and scenarios.
   * `tasks.md`: Structured, dependency-ordered task checklist.
2. **Seek Approval:** Presents proposal summaries and waits for explicit user confirmation.
3. **Execute Implementation:** Implements code inside `solution/src/`, checking off items in `tasks.md`.
4. **Evaluate & Verify:** Runs verification scripts and possession metrics (`run_evaluation.py`).
5. **Archive:** Consolidates finalized requirements into `openspec/specs/` and moves the change to `openspec/archive/`.

---

## 📁 Directory Structure

```
openspec/
├── specs/       # Living system specifications (source of truth)
├── changes/     # Active change proposals under review or development
│   └── <change-name>/
│       ├── proposal.md
│       ├── design.md
│       ├── specs/
│       └── tasks.md
└── archive/     # Completed and historical changes
```
