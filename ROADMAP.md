# Spectracs — Roadmap (working backlog)

> **Big-picture view:** [`roadmap_map.svg`](roadmap_map.svg) — a status + dependency graph of the
> items below (what is done, what is next/unblocked, what blocks what). Generated from
> [`roadmap_map.puml`](roadmap_map.puml); regenerate with `java -jar plantuml.jar -tsvg roadmap_map.puml`.
>
> Captured **2026-06-28** to resume next session. The **workflow object model is settled** — see
> [`SPECTRAL_WORKFLOW_CONCEPT.md`](SPECTRAL_WORKFLOW_CONCEPT.md) §8–§9 +
> [`workflow_object_model.svg`](workflow_object_model.svg). The items below are the agreed **next steps
> to build**. **Implement on explicit request only** (spec-first workflow). Roughly ordered;
> dependencies noted at the bottom.

## 1. Extract the spectrum → colour logic  *(**IMPLEMENTED 2026-06-29** — spec `spectracsPy/docs/SPEC_spectrum_processing.md`; 11 unit tests pass. Remaining: wire into `PumpkinPlugin.evaluation`, #6)*
Lift the proven `spectrasTest.py` hue pipeline into a reusable logic module / utility
(`ColorUtil.spectrumToHue(...)`; eventual home `spectracs.plugin_sdk.util.ColorUtil`). Stands alone (no
roles/workflow dependency) and becomes the body of `PumpkinPlugin.evaluation`. Trim the dependency set
(`colour-science` / `luxpy` / `spectres` / `BaselineRemoval` / `rgbxy` / `pyspectra` / `pandas` → check
what is actually needed; `colour` + `colorsys` may cover most).

## 2. Login — header UI placement  *(**IMPLEMENTED 2026-06-29** — spec `spectracsPy/docs/SPEC_login_header_placement.md`; account-icon control right of the left-aligned logo, full-width status bar, `userSessionSignal` bus refresh; login control removed from Settings. Both states render-verified.)*
- Move the **Login** control to the **right of the "spectracs" logo** — the common app-header pattern;
  **logo aligned left**. *(Spec: an account/person **icon** control, best-practice header pattern.)*
- The **status / progress bar** (below the logo) should span the **full screen width**.

## 3. SpectrometerProfile ↔ user binding + deletion  *(master user)*
- Master user can **delete** `SpectrometerProfile`(s).
- Replace the free-text **serial** with a **selection of an `AppUser`** (serial = username).
- The `SpectrometerProfile` is then **added to the `AppUser`** — the §9.5 config binding
  `AppUser → { SpectrometerProfile, Plugin }`. Needs the new `AppUser`↔`SpectrometerProfile` link.

## 4. User CRUD (master user)
- Master user can **add a user** (and edit / delete) — an `AppUser` management screen. Feeds #3's
  "select a user" and the master/end-user role gate.

## 5. Virtual spectrometer — three picture sets
Today the virtual device holds **one** stored `QImage`. Need **three**:
- a **calibration** image,
- an **acquired REFERENCE** image,
- an **acquired SAMPLE** image.

Synthesis ideas (so the virtual device produces meaningful spectra end-to-end):
- **REFERENCE** — synthesize a capture image from a **set of LED spectra** (there is a Python library
  that builds a spectrum from LEDs) → a realistic reference spectrum image.
- **SAMPLE** — compute an image that yields a **target hue** colour, so the evaluation produces a known,
  checkable result.

## 6. Pumpkin-oil workflow + plugin — first phases (in parallel)
- **Refine the GUI concept first.** The end-user view is a **wizard of phases, each phase a set of step
  tabs** → effectively a **nested wizard** (outer = phases advanced by `Next`; inner = the step tabs of
  the current phase). This needs a concrete UI design before building.
- **First milestone:** a plugin that builds the **ABSORPTION spectrum** (ACQUISITION → PROCESSING) and
  **displays it in the GUI**. Proves the static spine + the `SpectraContainer` chain + one real rendered
  step, end-to-end.

## Still-deferred design threads (pick up when their build item needs them)
- **Persistable-workflow schema** in [`DB_ENTITIES.md`](DB_ENTITIES.md): map `model/spectral/` classes to
  SQLAlchemy, add the `Plugin` entity + `AppUser` bindings, record the `DbSpectrum` + `MeasurementProfile`
  deletions, and **update `db_entities.svg`** (the one remaining stale diagram). Prereq for persistence in
  #3 / #6.
- **`EvaluationResult` GUI integration** — how the host renders the view-models (`ColorSwatch`, `Label`,
  `Verdict`, …) into the result tab.
- **PDF report** contents + personalization; **email** (transport / recipient / send-record) — postponed.

## Dependencies / suggested order
- **#1** is independent → do first (also unblocks #6's evaluation phase).
- **#4 → #3** (need users to exist before a profile can select one); both need the `AppUser`↔`Profile` link.
- **#5** unblocks realistic end-to-end runs of **#6** on the virtual device.
- **#6** needs the nested-wizard GUI design + (later) #1; its **first milestone** (absorption display) only
  needs ACQUISITION + PROCESSING and can start **in parallel** with the rest.
