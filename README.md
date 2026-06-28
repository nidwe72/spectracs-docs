# spectracs-docs

Design notes and knowledge base for the **Spectracs** spectroscopy app (a PySide6 desktop application
for optical spectroscopy with a DIY spectroscope).

## Contents

- **[`DB_ENTITIES.md`](DB_ENTITIES.md)** — the persisted SQLAlchemy entity model (fields, FKs,
  owned-vs-shared, cascade, delete cheat-sheet) plus a runtime (non-persisted) models appendix.
  Diagrams: [`db_entities.svg`](db_entities.svg), [`runtime_models.svg`](runtime_models.svg).
- **[`SPECTRAL_WORKFLOW_CONCEPT.md`](SPECTRAL_WORKFLOW_CONCEPT.md)** — the spectral **evaluation
  workflow**: the pumpkin-seed-oil QM purpose, the two-audience (master / end-user) model, the
  one-workflow-many-plugins architecture, the `PluginState` data model and entry points, and the
  trust/security model. Diagrams: [`spectral_workflow_concept.svg`](spectral_workflow_concept.svg),
  [`plugin_model.svg`](plugin_model.svg).
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
