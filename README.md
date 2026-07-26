# spectracs-docs

Design notes and knowledge base for the **Spectracs** spectroscopy app (a PySide6 desktop application
for optical spectroscopy with a DIY spectroscope).

## Contents

- **[`DB_ENTITIES.md`](DB_ENTITIES.md)** — the persisted SQLAlchemy entity model (fields, FKs,
  owned-vs-shared, cascade, delete cheat-sheet) plus a runtime (non-persisted) models appendix.
  Diagrams: [`db_entities.svg`](db_entities.svg), [`runtime_models.svg`](runtime_models.svg).
- **[`SPECTRAL_WORKFLOW_CONCEPT.md`](SPECTRAL_WORKFLOW_CONCEPT.md)** — the spectral **evaluation
  workflow**: the pumpkin-seed-oil QM purpose, the two-audience (master / end-user) model, the
  one-workflow-many-plugins architecture, the §9 object model (static 5-phase spine, plugin-created
  steps, the workflow-as-persisted-record), and the trust/security model. Diagrams:
  [`plugin_model.svg`](plugin_model.svg),
  [`workflow_object_model.svg`](workflow_object_model.svg).
- **[`ROADMAP.md`](ROADMAP.md)** — the working backlog / agreed next steps to build (resume point).
  Diagram: [`roadmap_map.svg`](roadmap_map.svg) — status + dependency graph (what is done, what is
  next/unblocked, what blocks what).
- **Doc-automation harness** — the "Director" screencast tooling
  ([`spectracsPy/docs/SPEC_doc_automation.md`](../spectracsPy/docs/SPEC_doc_automation.md), IMPLEMENTED).
  Diagram: [`doc_automation_architecture.svg`](doc_automation_architecture.svg) — the two-process
  (Director ↔ app `--doc-mode`) UDP + visible-mouse architecture.
- **[`internal/`](internal/)** — generated, internal-use documentation PDFs. Not hand-written and not
  hand-edited: each is produced from a markdown master in `spectracsPy/docs/` by a generator in
  `spectracsPy/docs/tools/`. Currently:
  **`Spectracs_CaptureFidelity.pdf`** — textbook-style documentation of the capture chain (how the
  camera is made to measure a spectrum, and why each step is as it is: `max(R,G,B)` grey values, the
  brightness/gamma law, auto-exposure, settling, sensor warm-up, spatial + temporal outlier
  rejection, white balance, dark frames, the wavelength window, sample dilution — plus what was
  deliberately *not* done). Source: `spectracsPy/docs/DOC_capture_fidelity.md`; regenerate with
  `python3 docs/tools/build_capture_fidelity_pdf.py`. Written for developer, chemist and lab reader
  alike; self-contained (figures embedded), so it can be sent alongside a report.
- `*.puml` — PlantUML sources for the diagrams.
- `spectracs.gaphor` — an older (2023) Gaphor UML model, kept for reference.

> **Status:** several sections describe a *to-be* design (the plugin architecture) that is not yet
> implemented. When the code is built or changed, update these docs and diagrams to match.

## Regenerating the diagrams

PlantUML itself is **not committed** (it's a ~22 MB binary). Download it once, then render:

```bash
curl -fsSL -o plantuml.jar \
  https://github.com/plantuml/plantuml/releases/download/v1.2024.7/plantuml-1.2024.7.jar
java -jar plantuml.jar -tsvg *.puml
```

Requires `java` and Graphviz (`dot`).
