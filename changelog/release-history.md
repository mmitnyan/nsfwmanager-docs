# NSFW Manager — Release History  
## Public Version & Milestone Index

This document summarizes the evolution of NSFW Manager across all major public releases.  
Each version has its own dedicated page (`v<version>.md`) containing full changelog details, screenshots, and technical notes.

## Version Timeline (Mermaid)

```mermaid
gantt
    title NSFW Manager — Version Timeline
    dateFormat  YYYY-MM-DD
    axisFormat  %d/%m

    section v1.0.0
    v1 — Initial product foundation (photo scan, ifnude, base README) :done, v1, 2024-12-08, 2026-05-23

    section v2.0.0
    v2.0 — Full architecture refactor (MVC, config, quarantine, multi-engines, i18n, licence, packaging) :done, v20, 2026-05-24, 2026-06-01
    Licence system introduced (public milestone) :milestone, licv2, 2026-05-27, 0d

    section v2.0.1
    v2.0.1 — Video detection, preview, player, GPU d3d11va + fallback CPU, MSI/tests :done, v201, 2026-06-13, 2026-06-15

    section v2.0.2
    v2.0.2 — SQLite cache, system tray prep, public documentation, consolidation release :done, v202, 2026-06-15, 2026-07-07

    section v2.0.3
    v2.0.3 — UX improvements, hardened licence, silent per-user MSI, few bug fix :done, v203, 2026-07-08, 2026-07-23

---

# 🟩 Stable Releases

## v2.0.3 — Latest Stable Release  
**July 2026**  
Second official public release. Introduces instant scan caching, improved UX, hardened licence security, and a silent per-user MSI installer.

➡️ Full details: [v2.0.3.md](v2.0.3.md)

---

## v2.0.2 — Previous Stable Release  
**June 2026**  
Adds the fp16 engine (“Le Juste Parfait”), dark mode, multilingual UI, licence management, and video support.

➡️ Full details: [v2.0.2.md](v2.0.2.md)

---

## v2.0.1 — Video Expansion Release  
**June 2026**  
First major multimedia extension: video detection, video preview, GPU acceleration, and improved MSI packaging.

➡️ Full details: [v2.0.1.md](v2.0.1.md)

---

## v2.0.0 — Architecture Refactor Release  
**June 2026**  
Complete redesign of the application: new UI, new configuration system, multi-engine support, licence system, i18n, dark/light themes, and modern packaging.

➡️ Full details: [v2.0.0.md](v2.0.0.md)

---

## v1.0.0 — First Public Release  
**January 2026**  
Initial public version with core scanning, quarantine, delete, and the first MSI installer.

➡️ Full details: [v1.0.0.md](v1.0.0.md)

---

# 🟦 Pre‑Release Series (Public Beta)

## v0.9.x — Public Beta Builds  
**Late 2025**  
Community testing of engines, early video experiments, UI refinements.

➡️ Full details: [v0.9.x.md](v0.9.x.md)

---

## v0.8.x — Engine Experiments  
**Mid 2025**  
Testing ONNX models, GPU acceleration, threshold tuning.

➡️ Full details: [v0.8.x.md](v0.8.x.md)

---

## v0.7.x — Prototype UI  
**Early 2025**  
First functional UI: list view, preview panel, delete/quarantine actions.

➡️ Full details: [v0.7.x.md](v0.7.x.md)

---

# 🟫 Internal Milestones (Private Development)

## v0.6.x — Proof of Concept  
**Late 2024**  
First working NSFW detection pipeline using ONNX. Console-only.

➡️ Full details: [v0.6.x.md](v0.6.x.md)

---

## v0.5.x — Research Builds  
**2024**  
Model research, dataset evaluation, feasibility studies.

➡️ Full details: [v0.5.x.md](v0.5.x.md)

---

## v0.4.x — Early Experiments  
**2023**  
Python prototypes, early classifiers, basic image tests.

➡️ Full details: [v0.4.x.md](v0.4.x.md)

---

## v0.3.x — Concept Phase  
**2022**  
Exploration of a privacy-focused NSFW detection tool.

➡️ Full details: [v0.3.x.md](v0.3.x.md)

---

## v0.2.x — Pre‑Concept  
**2021**  
Initial ideas, early notes, first drafts.

➡️ Full details: [v0.2.x.md](v0.2.x.md)

---

## v0.1.x — Genesis  
**2020**  
The very first idea: “A tool to safely clean old folders.”

➡️ Full details: [v0.1.x.md](v0.1.x.md)

---

# 📌 Notes

- Each version page (`vX.Y.Z.md`) contains:
  - Full changelog  
  - Screenshots  
  - Technical notes  
  - Engine changes  
  - UI changes  
  - MSI installer notes  
  - Known issues  
  - Migration notes  

- This index is updated whenever a new version is released.

---
