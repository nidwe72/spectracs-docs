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
>
> **UI conventions:** `spectracsPy/docs/DESIGN_GUIDE.md` (page layout, button variants, tables, QSS) +
> `spectracsPy/docs/DEV_WORKFLOW.md` (click-through review — drive-and-observe UI verification).
>
> **Pipeline PoC (IMPLEMENTED 2026-06-30, bridges #5+#6):** the **measurement & evaluation concept**
> (`spectracsPy/docs/SPEC_measurement_evaluation_concept.md` — sound, LED-independent
> measure→T=S/R→colour→verdict) and the **Pipeline Playground**
> (`SPEC_pipeline_playground.md` — master-only 7-tab LED→oil dev bench: CFL calibration, LED-synth
> reference, oil samples, absorption, colour, verdict) are **built and verified**, with a headless
> regression test + documentation PDF (`tests/test_pumpkin_oil_spectrum_to_color_eval.py`). It proves the
> concept end-to-end on synthetic data and de-risks #5 (synthesis) and #6 (absorption/evaluation).
> Physics/data: `KB_spectroscopy_physics.md`, `KB_led_and_oil_spectra.md`, sibling `spectracs-references/`.

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

## 3. SpectrometerProfile ↔ user binding + deletion  *(master user)*  *(**▶ NEXT — unblocked: #4 done**)*
- Master user can **delete** `SpectrometerProfile`(s).
- Replace the free-text **serial** with a **selection of an `AppUser`** (serial = username).
- The `SpectrometerProfile` is then **added to the `AppUser`** — the §9.5 config binding
  `AppUser → { SpectrometerProfile, Plugin }`. Needs the new `AppUser`↔`SpectrometerProfile` link.

## 4. User CRUD (master user)  *(**IMPLEMENTED 2026-06-30** — spec `spectracsPy/docs/SPEC_user_crud.md`; master-only `UserListViewModule` (QTableView) + `UserViewModule` editor over Pyro; new `UserAdminLogicModule` + 4 `@expose` RPCs; single role, hard delete, last-master guard, password min 8, server-unavailable banner. Façade + UI + live RPC round-trip verified.)*
- Master user can **add a user** (and edit / delete) — an `AppUser` management screen. Feeds #3's
  "select a user" and the master/end-user role gate.

## 5. Virtual spectrometer — three picture sets  *(synthesis **proven** in the Pipeline Playground; image slots remain)*
> The Playground already synthesises the REFERENCE (LED set) and SAMPLE (oil model) spectra and runs a
> fresh CFL calibration — but **spectra-only** (no image round-trip). What remains for #5: grow
> `VirtualSpectrometerSettings` from one `QImage` to **three** slots, and rasterise the synthesised SPD
> onto the ROI via the calibration polynomial so the existing acquisition reads it back.

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
  step, end-to-end. *(The absorption→colour→verdict **pipeline + display is already proven** in the
  Pipeline Playground on synthetic data; what remains here is wiring it through the real `SpectralWorkflow`
  spine / plugin hooks. The reusable ops exist: `TransmissionLogicModule`, `AbsorptionLogicModule`,
  `SpectrumUtil.transmission/absorption`, `VerdictLogicModule`.)*

## Still-deferred design threads (pick up when their build item needs them)
- **Persistable-workflow schema** in [`DB_ENTITIES.md`](DB_ENTITIES.md): map `model/spectral/` classes to
  SQLAlchemy, add the `Plugin` entity + `AppUser` bindings, record the `DbSpectrum` + `MeasurementProfile`
  deletions, and **update `db_entities.svg`** (the one remaining stale diagram). Prereq for persistence in
  #3 / #6.
- **`EvaluationResult` GUI integration** — how the host renders the view-models (`ColorSwatch`, `Label`,
  `Verdict`, …) into the result tab. *(First concrete take: the Playground's "Measured vs target" tab +
  the PDF report.)*
- **PDF report** contents + personalization; **email** (transport / recipient / send-record) — postponed.
  *(PDF prototype: the Playground eval test renders an 8-page report — a starting point for the plugin's
  PUBLISHING phase, though that will use a reusable Qt renderer over the real `EvaluationResult`.)*
- **LED-combination optimisation** *(future)* — use the Playground's LED synthesis to search LED sets for
  an even flatter, gap-free reference light source. Separate task; current LED set is fine.

## Dependencies / suggested order
- **#1, #2, #4 — done.** The measurement/evaluation concept + Pipeline Playground PoC — done.
- **#3 is now unblocked (#4 done)** → the natural next item; needs the new `AppUser`↔`Profile` link.
- **#5** unblocks realistic end-to-end runs of **#6** on the virtual device (synthesis already proven in the
  Playground; the three image slots + image round-trip remain).
- **#6** needs the nested-wizard GUI design; its **first milestone** (absorption display) is pipeline-proven
  in the Playground — what remains is wiring it through the real `SpectralWorkflow` spine / plugin.
