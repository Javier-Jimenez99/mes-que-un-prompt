# SPEC-001: Project Scope and Pipeline Architecture

- **Status:** Proposed
- **Date:** 2026-10-21
- **Domain:** Ball Possession Detection (More than a Hack 2027)

---

## 1. Objective

Define the overarching architecture and data contracts for the frame-by-frame ball possession prediction pipeline on the **FC Barcelona vs Athletic Club** (`BAR-ATH`) fixture, maximizing the primary competition ranking metric **`spellF1`** while strictly respecting delivery formats.

---

## 2. Requirements & Interface Contracts

### 2.1 Submission File Specification
* Must be a CSV file with header `frame,label`.
* Must contain exactly **199,319 rows** indexed `0` through `199318` matching the tactical video frames.
* Permitted label tokens:
  * `H`: Home team (FC Barcelona) in active possession.
  * `A`: Away team (Athletic Club) in active possession.
  * `D`: Dead ball (stoppage, ball out of bounds, fouls, VAR, restarts).
  * `X`: System abstention or ungraded periods (pre-match, halftime, post-match).
* Must strictly pass `python evaluation/validate_submission.py <submission_path>.csv`.

### 2.2 Input Assets
* **Tactical Automatic Tracking Data (ATD):** Coordinates $(x, y)$ in normalized $[0, 1]$ space and speeds at 25 fps (`data/tactical/BAR-ATH_tactical_atd.txt` and `.xml`).
* **Tactical Smart Tagging:** 638 event phases and set-piece intervals (`data/tactical/BAR-ATH_tactical_smart_tagging.xml`).
* **Homography:** $3 \times 3$ transformation matrices per frame (`data/tactical/BAR-ATH_tactical_homography.csv`).
* **External Benchmarks:** Metrica Sports Open Data (`Sample_Game_1`, `2`, `3`) for generalizable validation and training.

---

## 3. Core Architectural Challenges & Strategy

1. **Handling 40.7% Missing Ball Observations (`NaN` in ATD during play):**
   * *Domain Rule:* In football, possession remains with the last controlling team during loose balls, passes in flight, or aerial duels until an opponent establishes controlled possession or the referee halts play.
   * *Mechanism:* Temporal inertia filtering and ball trajectory interpolation between visible endpoints.

2. **Accurate Dead Ball (`D`) Segmentation:**
   * Leverage Smart Tagging `SET PIECES` (corners, goal kicks, free kicks, throw-ins) to enforce `D` during pre-restart intervals.
   * Detect out-of-bounds ball positions beyond pitch dimensions ($105\text{ m} \times 68\text{ m}$).

3. **Strategic Utilization of `X`:**
   * Abstaining with `X` during an ongoing possession is forward-filled by the official scorer, preserving continuous possession blocks in `spellF1`.
   * Forcing definitive predictions (`H` or `A`) at true possession hand-overs avoids bleed-through into opponent possessions.

---

## 4. Modular Solution Layout (`solution/src/`)

```
solution/src/
├── __init__.py
├── data/
│   ├── loaders.py          # ATD, Smart Tagging, and ground truth ingestion
│   └── external.py         # Metrica open-data adapters via kloppy
├── features/
│   ├── ball_imputation.py  # Missing ball trajectory interpolation
│   └── spatial_features.py # Relative distances, player densities, velocities
├── models/
│   ├── base.py             # Model abstract base interface
│   ├── state_machine.py    # Rule-based finite state machine with inertia
│   └── classifier.py       # Sequential or tree-based possession classifier
└── postprocessing/
    ├── temporal_filter.py  # Window smoothing and abstention optimization
    └── exporter.py         # Final submission CSV formatting and validation
```

---

## 5. Acceptance Criteria

1. **Target Baseline:**
   * Must beat the published floor: `spellF1 >= 0.2433` (`baseline_smooth50`).
   * On public 5-minute ground truth: `spellF1 > 0.45` and `macroF1 > 0.65`.
2. **Possession Realism:**
   * Avoid excessive fragmentation: predicted possessions $\approx 15$–$25$ in 5 minutes (mean duration $12$–$18$ seconds).
