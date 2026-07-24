# Spectracs — Roadmap (working backlog)

> **Big-picture view:** [`roadmap_map.svg`](roadmap_map.svg) — the **product value-chain** the items below
> serve: the path one measurement travels (Setup → Capture → Process → Evaluate → Report → Publish → Bill)
> over the platform/foundation layer, each stage **shaded by how complete the real end-user path is**
> (status verified against the code 2026-07-13). The former real gap — CAPTURE in the end-user wizard — is now
> **CLOSED**: the wizard does real capture via the shared `CapturePanel` (M3, 2026-07-13, rig-verified). Generated
> from [`roadmap_map.puml`](roadmap_map.puml); regenerate with `java -jar plantuml.jar -tsvg roadmap_map.puml`.
>
> Captured **2026-06-28** to resume next session. The **workflow object model is settled** — see
> [`SPECTRAL_WORKFLOW_CONCEPT.md`](SPECTRAL_WORKFLOW_CONCEPT.md) §8–§9 +
> [`workflow_object_model.svg`](workflow_object_model.svg). The items below are the agreed **next steps
> to build**. **Implement on explicit request only** (spec-first workflow). Roughly ordered;
> dependencies noted at the bottom.
>
> **✅ CAPABILITY PROOF (milestone V) EFFECTIVELY MET + ROAST AMPEL GAUGE IMPLEMENTED — 2026-07-23/24:**
> [`spectracsPy/docs/SPEC_capability_proof.md`](../spectracsPy/docs/SPEC_capability_proof.md) +
> [`SPEC_roast_ampel.md`](../spectracsPy/docs/SPEC_roast_ampel.md). **Scope narrowed to a binary good-green vs
> over-roasted-brown call** (§1a — the too-green class and green-ranking are dropped: in practice oils are green-
> or brown-type, ranking greens is a matter of taste, brown is objectively worse — tastes worse, sells cheaper).
> On that claim the gate is **effectively met**: the **Pigment ratio** (Soret/Q, 440–460 / 560–580) separates the
> two clusters by **~10–13× the measurement noise, non-overlapping**, and is **dilution-invariant** (≤5% at 2 vs 3
> drops), across **4 oils / 32 runs**; colour corroborates via chroma. The **Roast Ampel verdict gauge is BUILT**
> (G0–G8, `SPEC_roast_ampel.md` §8): a plugin-driven green/brown gauge — an analytical band in EVALUATION and a
> plain verdict badge in Send-to-LIMS. Sample-clearing physics + a mandatory fresh-sample protocol are documented
> (§11.4a–d), and a colleague-facing **status-report PDF** + generator shipped (§11.8,
> `spectracsPy/docs/tools/build_capability_status_pdf.py`).
> **▶ NEXT (Edwin 2026-07-24) — two tasks:**
> 1. **Close the Capability Proof by measurement** — measure the broadened oil panel (**+1 brown, +4 green**, each
>    fresh per the §11.4a protocol; brown ×5) to lift the good/brown call from n=4 to a wider, settled panel.
>    *Confirmatory* — the core claim is already met (§11.6).
> 2. **Update the "Director" for a nice AS-IS app video** — a current-state screencast of the real end-to-end
>    (measure → process → evaluate with the Roast Ampel → report → Send-to-LIMS), extending the Doc Director
>    harness ([`SPEC_doc_automation.md`](../spectracsPy/docs/SPEC_doc_automation.md)).
>
> **✅ M3 PLUGIN DISTRIBUTION + A3 PROVENANCE — IMPLEMENTED 2026-07-18/19:**
> [`spectracsPy/docs/SPEC_plugin_distribution.md`](../spectracsPy/docs/SPEC_plugin_distribution.md). Ships plugin
> updates without an APK rebuild: `DbPlugin` identity is now **`(codeRef, version)`** (insert-never-update),
> **Ed25519**-signed source stored in the server DB and **verified + exec'd client-side** (`PluginRegistry`
> sealedness dispatch), plus the publish/assign UI (A1·A2 + B0–B6; **B6 bench listing rig-verified**). **A3 run
> provenance (this session):** every saved `SpectralWorkflow` now stamps the resolved **`pluginVersion`** beside
> `pluginCodeRef` (new nullable column + app migration `f0ac79b33dde`; NULL = shipped built-in), so a run/PDF traces
> to the exact plugin version. *Remaining: B7 Android, B8 batch-assign.*
>
> **✅ SCHEMA-MIGRATIONS HARDENING (AllEntities completeness guard) — IMPLEMENTED 2026-07-19:**
> [`spectracsPy/docs/SPEC_schema_migrations.md`](../spectracsPy/docs/SPEC_schema_migrations.md) §8. A3 surfaced that
> `AllEntities` (Alembic's `target_metadata` source) omitted the 7-table `model.spectral` workflow graph, so
> autogenerate proposed **dropping** those tables. Fixed (metadata 2→9 app tables) and made **self-enforcing** with a
> guard test (`tests/test_all_entities_complete.py`, curated-import-then-walk in a subprocess) that fails naming any
> entity missing from `AllEntities` — the invariant is now checked by CI, not by a comment.
>
> **✅ INTEGRATION MILESTONE — IMPLEMENTED 2026-07-02 (delivers #5 + #6; 37 tests green):**
> [`spectracsPy/docs/SPEC_pumpkin_integration.md`](../spectracsPy/docs/SPEC_pumpkin_integration.md).
> Three tracks, all built — **A** virtual-device 3-image folder + shared-vmax/linear encoder + round-trip
> (delivers #5); **B** plugin substrate (server-side `DbPlugin` + `AppUser` binding + seed +
> `plugin_sdk` + `EvaluationResult`); **C** `SpectralWorkflowEngine` (auto-calibrates from `calibration.png`)
> + `PumpkinOilPlugin` + **interactive** nested-wizard GUI (per-step **Measure**, real spectrum plots,
> Back/Cancel/Next→Save) (delivers #6). **Floor goal reached:** log in as `pumpkinTestUser` → load an image
> folder → wizard drives ACQUISITION(measure)→PROCESSING(absorption)→EVALUATION → colour/verdict.
> 5-phase spine kept, METADATA+PUBLISHING skipped; the `AppUser → {Plugin, device}` config binding is real +
> seeded (host runs the plugin *via the logged-in user*, device = stable `Virtuax` code-name, D15). Deferred:
> workflow-**record** persistence (Save is a no-op → Home). See the spec's "Known limits / follow-ups".
>
> **UI conventions:** `spectracsPy/docs/DESIGN_GUIDE.md` (page layout, button variants, tables, QSS) +
> `spectracsPy/docs/DEV_WORKFLOW.md` (click-through review — drive-and-observe UI verification).
>
> **GUI polish (IMPLEMENTED 2026-07-06):** `spectracsPy/docs/SPEC_gui_cosmetic_tweaks.md` — Settings moved
> from the Home footer into the logged-in account menu; the "save physically captured images" row removed;
> the virtual-spectrometer fileset made **master-only** (master authors the fileset → end user measures).
> Android account-menu handling deferred (verify the QMenu-crash assumption first).
>
> **Pipeline PoC (IMPLEMENTED 2026-06-30, bridges #5+#6):** the **measurement & evaluation concept**
> (`spectracsPy/docs/SPEC_measurement_evaluation_concept.md` — sound, LED-independent
> measure→T=S/R→colour→verdict) and the **Pipeline Playground**
> (`SPEC_pipeline_playground.md` — master-only 7-tab LED→oil dev bench: CFL calibration, LED-synth
> reference, oil samples, absorption, colour, verdict) are **built and verified**, with a headless
> regression test + documentation PDF (`tests/test_pumpkin_oil_spectrum_to_color_eval.py`). It proves the
> concept end-to-end on synthetic data and de-risks #5 (synthesis) and #6 (absorption/evaluation).
> Physics/data: `KB_spectroscopy_physics.md`, `KB_led_and_oil_spectra.md`, sibling `spectracs-references/`.
>
> **Peak-ratio evaluation + bench build-out (IMPLEMENTED 2026-07-09):**
> [`spectracsPy/docs/SPEC_pumpkin_peak_ratio_eval.md`](../spectracsPy/docs/SPEC_pumpkin_peak_ratio_eval.md)
> (see **§0 Concept & rationale**) — the physically-specific successor to the hue verdict: reads the pigment
> bands (Soret-flank blue + ~575 nm Q-band + green anchor → a *greenness* ratio) so it separates *why* an oil
> is off (browned vs faded vs turbid). **P0–P4 done, render-only first sweep** in the dev measurement bench:
> generic `SpectrumFeatureUtil` ops + `DevSpectralPlugin.evaluation` composing them with hard-coded pumpkin
> constants → a real EVALUATION phase showing greenness / pigment / browning / clarity.
> **⏭ NEXT PROMINENT TASK (Edwin, 2026-07-16) — phase `PB`, §1b: the literature-anchored bands.**
> **Blue = 440–460 nm** (the right-hand **slope** of the 430 nm Soret — the peak itself saturates at the working
> dilution, so it is not capturable) · **green Q-band = 560–580 nm** (literature peak ~570). A `DevSpectralPlugin`
> constants change; deltas + traps in §1b.1/§1b.2. **Scheduled AFTER the plugin story**
> (`SPEC_project_structure.md` → `SPEC_plugin_distribution.md`), and **before P5** — calibrating windows that are
> about to move would waste the real-oil runs. Then **P5** (save + calibrate on real known-good/bad oils → verdict
> edges — *no thresholds ship before this*), then **P6** (wire
> the independent `PumpkinOilPlugin`). Same session (`SPEC_dev_measure_bench.md` §12–18,
> `SPEC_dev_capture_view.md` §11): extended 400–700 nm analysis window + shared ROI overlay; bench acquisition
> as Reference/Sample steps with Captured-image/Spectrum tabs + per-frame progress bar + live spectra;
> metric-field rendering (gray label chip + read-only field + click-tooltip); phone-width responsiveness
> (plots shrink, images fit, word-wrapped labels — no scrollbars). Physics grounded in Fruhwirth & Hermetter
> (2007), now stored in `spectracs-references/articles/`; KB updated (`KB_led_and_oil_spectra.md` §2 pigment
> peaks + the "our bench reproduces Fig. 3A" mapping).
>
> **Bench cosmetic + UX polish (IMPLEMENTED 2026-07-10):**
> [`spectracsPy/docs/SPEC_bench_small_screen_refinements.md`](../spectracsPy/docs/SPEC_bench_small_screen_refinements.md)
> — small/phone-width refinements driven by click-through: step-tabs framing (S1), capture progress → status
> bar (S2), Evaluation *Metrics | Spectrum* split (S4), bold ratio labels via a `MetricFieldViewStyle` composite
> (S5), removed hint/status line (S7); then a **frame-hierarchy pass** — every `QTabWidget` keeps its pane
> border (S10), the WorkflowPhase `QStackedWidget` frame dropped (S11); plus Processing raster vertical-centre
> (S12), the checkbox touch-target made real-Android-only so desktop `--phone` shows the desktop icon (S13), and
> a global native combo-box **▼** (S14). **New best practice:** GUI-layout changes are now **mocked in
> [Wireloom](../spectracsPy/docs/DEV_WORKFLOW.md)** (markup → SVG, toolkit-neutral) and agreed *before* coding —
> the reference mock lives beside the spec (`docs/mock_bench_acquisition.wireloom`).
>
> **Plugin platform — M1 + M2 IMPLEMENTED (2026-07-10/11); M3 DESIGN:** the big theme — *the plugin drives the GUI of
> both the dev bench and the end-user view.* **M1 convergence** (`spectracsPy/docs/SPEC_plugin_driven_convergence.md`) is
> **built**: a shared render **visitor** seam (`QtWorkflowRenderer`) + generic `WorkflowPhaseRenderer` (steps→tabs), the
> view-model vocabulary extended (`SpectrumPlotView` traces/bands/markers, new `SpectrumCaptureView`/`CaptureView`,
> re-homed to `model.spectral.plugin.view`), EVALUATION/PROCESSING now plugin-declared, a **bench plugin selector**
> (Dev/Pumpkin), and the wizard routed through the same renderer. Two pre-existing camera bugs fixed en route (V4L2
> mid-stream exposure; tall-frame raster display). **M2 = plugin-driven PDF report — DONE**
> (`SPEC_bench_pdf_export.md`): a Qt-free `MatplotlibWorkflowRenderer` (second target of the M1 visitor) renders an
> A4 preview that **is** the PDF; a per-view-model **`isShownInReport`** flag curates the body cross-phase; the whole
> `SpectralWorkflow` is embedded as `workflow.json` + each capture as a named PNG attachment via **pypdf**; generic
> per-view-model `toJson`/`fromJson` (fixes the lossy serializer + persisted-run reload); fit-to-width `PdfPreviewWidget`
> + "Open bigger" full-window view; plugin-declared `CaptureView` chrome flags (frames/exposure hidden by default). LIS
> → **SENAITE** stays deferred. **M3 = plugin distribution** (`SPEC_plugin_distribution.md` — separate repo + DB-stored
> **signed** source, client-side load; `plugin_sdk` becomes a versioned API). M3 depends on M1; sequence M1→M2→M3.
>
> **Acquisition guidance — IMPLEMENTED (both hosts) 2026-07-13:**
> [`spectracsPy/docs/SPEC_acquisition_guidance.md`](../spectracsPy/docs/SPEC_acquisition_guidance.md) — first-time users
> are walked through the ACQUISITION phase: a **coach line** in the top status bar states the single next action (from the
> plugin's per-step `CaptureView.prompt`), and a **muted-amber cue** marks exactly one control at a time (2px border on
> the Measure/capture/Next button, an amber ● on the tab to switch to, ✓ on done tabs). Reuses the previously-orphaned
> `CaptureView.prompt` — **no new SDK**. Built + offscreen-verified in **both** the end-user wizard (`WizardViewModule`,
> 17/17) **and** the dev measurement bench (`DevMeasurementBenchViewModule`, 19/19 — **Decision B**, superseding the
> initial bench-content-only Decision A); both plugins (`PumpkinOilPlugin`, `DevSpectralPlugin`) carry role-specific
> prompts. **Cost of Decision B:** the guidance logic is **mirrored** (~90 lines) across the two still-separate
> acquisition panels — to be deduped by the convergence below.
>
> **Bench evaluation colour swatch — IMPLEMENTED 2026-07-13:** the dev measurement bench's EVALUATION now shows the
> sample's perceived **colour** as a metric-grid row — same shape as the numeric metric fields (gray label chip +
> aligned value cell), but the value cell renders a **swatch** instead of a read-only field, labelled "color", **no
> target** swatch. Done by **extending `MetricFieldView` with an optional `color`** (not a new view-model), so it shares
> the metric grid's aligned label column for free; both render targets branch inside `visitMetricField` (Qt swatch /
> matplotlib patch → also on the PDF). The plugin computes RGB from the transmission via `EvaluationColorUtil`.
> **Bench-only** — `PumpkinOilPlugin` keeps its `ColorSwatchView` measured/target comparison blocks. Specs amended
> (`SPEC_plugin_driven_convergence.md` §3 ¶, `SPEC_bench_pdf_export.md` §3/§5, `SPEC_pumpkin_peak_ratio_eval.md` §6);
> offscreen-verified 12/12, no regressions.
>
> **✅ DONE (M3, 2026-07-13, rig-verified) — capture-panel convergence (was the deferred M1 "P6"):** ACQUISITION now runs
> through a shared **`CapturePanel`** (host-owned reparented singleton, Option A) so **both** hosts share ONE
> acquisition/capture panel instead of two hand-built ones. The engine gained a headless frame-provider seam (so the
> wizard does real capture = SM3), the mirrored guidance collapsed into `AcquisitionGuidance`, and the bench shed ~460
> net lines. Commits `05bb3c6`…`e71bd80`; `SPEC_plugin_driven_convergence.md` §9. (Original plan follows.) The
> shared seam today stops at the *computed* phases; the capture panel is the last un-converged piece. When it lands, the
> **now-duplicated acquisition-guidance logic collapses into that one shared path.** Edwin flagged a "tight discussion"
> to precede this. **This is the next plugin-platform item to handle.**
>
> **Documentation / screencast automation — IMPLEMENTED (M1, 2026-07-12):**
> [`spectracsPy/docs/SPEC_doc_automation.md`](../spectracsPy/docs/SPEC_doc_automation.md) +
> [`doc_automation_architecture.svg`](doc_automation_architecture.svg). A **"Director"** harness so the AI
> can generate per-scenario scripts that drive Spectracs for tutorial **videos + screenshots** with a
> **visible cursor** and human-in-the-loop pauses. App side (`--doc-mode`, all gated): a right-side hint
> panel + a local **UDP** service (`DocModeUdpService` :5555 — set_hint/locate/nav/wait/**activate**/dismiss/
> ping). Director side (external `automation/`): non-modal Prompter + `QThread` scenario runner + a real
> mouse (PyAutoGUI). Robust clicks = **glide the visible cursor, then trigger the widget programmatically**
> over UDP (never a missed pixel-click); nav via `NavigationSignal` (menu entries are `QAction`s, not
> widgets). **Driven live end-to-end:** smoke gate, the **virtual pumpkin wizard** (real computed absorption
> curve, no hardware), and the **dev measurement bench on the real device** (all 8 beats, live reference
> spectrum off the real camera). One-command launchers (`wizard.sh`, `bench.sh`) with **auto screen
> recording** (ffmpeg x11grab → `automation/recordings/*.mp4`, app-window-only; **videos gitignored, not
> versioned**). **Open issues → to specify later** (spec §15): login automation, bench calibration/serial
> prerequisite, record-after-prep trimming, Tier A feature-tour chapters, unattended/CI for no-hardware runs.

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

## 3. SpectrometerProfile ↔ user binding + deletion  *(master user)*  *(**✅ SUPERSEDED + IMPLEMENTED as CX / milestone 5a, 2026-07-05** — re-keyed on SERIAL (not `AppUser`); see the "Real-hardware capture & connection/calibration" section below + `SPEC_connection_and_calibration_ux.md` §10)*
- The **`AppUser → {Plugin, device}` config binding is DONE + seeded** (integration Track B.1a): `AppUser`
  gained `pluginId` (FK) + `spectrometerDevice`; the host runs the plugin via the logged-in user. The device
  is bound by stable code-name (`Virtuax`), not a profile id (D15).
- **Still to do (the master-facing UI half):** master can **delete** `SpectrometerProfile`(s); replace the
  free-text **serial** with a **selection of an `AppUser`**; a master screen to pick user↔profile. These
  remain #3's own work.
- **⚠ DIRECTION CHANGED 2026-07-05 (Edwin):** key on **SERIAL, not username/`AppUser`**. The master authors,
  *per serial*, the `{ device, calibration, plugin }` bundle; the end user registers by entering the label
  serial. This **reverses** the "replace serial with an `AppUser` selection" line above and supersedes the
  "username = future serial" idea. The serial is a stable natural key that also resolves D15's dangling-id
  workaround. Full object model → new `SPEC_connection_and_calibration_ux` (see the real-hardware-capture
  section below + `spectracsPy/docs/SPEC_real_camera_capture.md` §9.4).

## 4. User CRUD (master user)  *(**IMPLEMENTED 2026-06-30** — spec `spectracsPy/docs/SPEC_user_crud.md`; master-only `UserListViewModule` (QTableView) + `UserViewModule` editor over Pyro; new `UserAdminLogicModule` + 4 `@expose` RPCs; single role, hard delete, last-master guard, password min 8, server-unavailable banner. Façade + UI + live RPC round-trip verified.)*
- Master user can **add a user** (and edit / delete) — an `AppUser` management screen. Feeds #3's
  "select a user" and the master/end-user role gate.

## 5. Virtual spectrometer — three picture sets  *(**✅ IMPLEMENTED 2026-07-02** — integration Track A; see `SPEC_pumpkin_integration.md`)*
> **Done:** `VirtualSpectrometerSettings` now holds three role-keyed images (CALIBRATION/REFERENCE/SAMPLE)
> + `activeRole`; a folder picker loads a named set; `SpectrumToVirtualImageUtil` rasterises the synthesised
> SPD onto the ROI via the calibration polynomial (shared vmax, linear), and the existing acquisition reads
> it back (round-trip test within ±3° hue). Baked demo sets live in
> `spectracs-references/pumpkin_oil/virtual_captures/`.

Today the virtual device holds **one** stored `QImage`. Need **three**:
- a **calibration** image,
- an **acquired REFERENCE** image,
- an **acquired SAMPLE** image.

Synthesis ideas (so the virtual device produces meaningful spectra end-to-end):
- **REFERENCE** — synthesize a capture image from a **set of LED spectra** (there is a Python library
  that builds a spectrum from LEDs) → a realistic reference spectrum image.
- **SAMPLE** — compute an image that yields a **target hue** colour, so the evaluation produces a known,
  checkable result.

## 6. Pumpkin-oil workflow + plugin  *(**✅ IMPLEMENTED 2026-07-02** — integration Tracks B + C; see `SPEC_pumpkin_integration.md`)*
> **Done:** the nested-wizard GUI (outer phase rail + inner step tabs), `SpectralWorkflowEngine` running the
> 5-phase spine (METADATA+PUBLISHING skipped), `PumpkinOilPlugin` (5 hooks over `plugin_sdk`), interactive
> ACQUISITION (per-step **Measure**), PROCESSING absorption plot, EVALUATION colour swatches + verdict —
> all through the real spine on the virtual device. Absorption *display* (the original "first milestone")
> and the full colour/verdict flow are both live.
> **Follow-ups (not this milestone):** workflow-record persistence (Save→save), METADATA form + PUBLISHING
> (PDF/email), plugin signature verification, real-hardware re-measure invalidation.

## Real-hardware capture & connection/calibration  *(NEW 2026-07-05 — spec-first, design only)*

Real USB-camera capture is **verified on hardware** (ELP `32e4:8830` captured a real CFL mercury-line
spectrum; resolver picks the camera by USB VID:PID). Design lives in
[`spectracsPy/docs/SPEC_real_camera_capture.md`](../spectracsPy/docs/SPEC_real_camera_capture.md) (+ hardware
construction in `KB_spectroscopy_physics.md` §7). Two tracks below feed one milestone. **Implement on
explicit request only.**

> **✅ MILESTONE 5a IMPLEMENTED + pushed 2026-07-05** — the setup track (CX Phases A/B/C) is built and
> verified on the virtual device: serial-keyed object model (server-DB migration), master authoring GUIs
> (Plugin / Spectrometer profiles + auto-calibrate / Spectrometer setups under Settings → Administration),
> end-user self-registration (Register on the login dialog → auto-login), and a header connection indicator.
> Seeded test users: `masterUser/masterUser`, `pumpkinTestUser/pumpkinTestUser` (→ `TEST-0001`). As-built +
> deviations: `spectracsPy/docs/SPEC_connection_and_calibration_ux.md` §10. **Next: 5b (real device, RC track).**
>
> **✅ SM1 — capture track resumed + IMPLEMENTED 2026-07-06/07 (first step toward real-live usage).**
> **RC-R0 + RC-R1 are DONE**, delivered as a new **"Capture images" dev view** (Settings → Development,
> master-only): `SensorCaptureIndexResolver` (VID:PID→cv2 index via Linux sysfs), `CaptureBackend` owns cv2
> (no forced MJPG, tolerant reads), live stream + save-PNG (feeds building virtual filesets). Plus two things
> that came out of using it on the ELP: **per-camera exposure** seeded at the sensor catalog
> (`SpectrometerSensorUtil`, ELP CFL=78) — fixing the over-exposure that clipped the mercury green and
> **merged the doublet**, so calibration **detect-peaks now resolves the green on real hardware** — and
> **our own auto-exposure** (a bisection to just-below-clip: checkbox default-on, one-shot on stream start,
> manual slider, status-bar progress; explicitly **not** the camera's broken built-in). Decided for
> measurement (details later): auto-expose the **reference**, reuse that exposure on the **sample** (same
> exposure → clean T=S/R, preserves absorption). Windows auto-resolve = own deferred milestone. Specs:
> `SPEC_dev_capture_view.md`, `SPEC_real_camera_capture.md` §4/§9.3, `KB_spectroscopy_physics.md` §7.
> **Next: SM2 = real calibration (RC-R2)** — wire the resolver into ROI/Hough **and** detect-peaks for a
> complete real `SpectrometerSetup`.
>
> **✅ SM2 — real calibration off the ELP DONE 2026-07-08 (RC-R2), via the DEV MEASUREMENT BENCH work.**
> Calibrating a real `SpectrometerSetup` on the ELP now works end-to-end. Fixes that landed:
> **auto-exposure in calibration authoring** (`AutoExposureCaptureHelper` pre-pass before the ROI/Hough +
> detect-peaks bursts — they previously opened the camera bloomed at index 0 with no AE); **green doublet
> resolved** (parameterized `SmoothSpectrumLogicModule` + light calibration smoothing so the 50-frame mean
> isn't over-smoothed, plus a best-effort 6th anchor `MERCURY_MANGO_GREEN_LEFT` 542.4 via a local search left
> of the dominant green); **ROI left-bound** now uses the brightest *channel* (max r,g,b) not `qGray`, so a
> visible **blue** line is no longer clipped out of the ROI.
>
> **✅ DEV MEASUREMENT BENCH — IMPLEMENTED + click-through verified 2026-07-08.** A master "Swiss-knife"
> real-camera bench (Settings → Development): a generic sibling of the end-user plugin wizard that runs the
> *same* pipeline — acquire REFERENCE + SAMPLE (live preview + auto-exposure, exposure locked on ref and
> reused for sample) → transmission `T=S/R` → absorption `A=−log10(S/R)` — **without** a use-case verdict.
> Backed by a transiently-injected **`DevSpectralPlugin`**; capture owned by the view, extraction shared
> (`ImageSpectrumAcquisitionLogicModule`). Seeded **`masterUserExakta`** bound to the real ELP serial;
> login/bench re-install the calibrated profile into `ApplicationSettings` via the server RPC
> (`ActiveSpectrometerProfileLogicModule`). Transmission geometry only (sample between bulb and camera);
> reflectance is future. Spec: [`spectracsPy/docs/SPEC_dev_measure_bench.md`](../spectracsPy/docs/SPEC_dev_measure_bench.md).
> **Next: SM3 = real measurement in the end-user workflow** (live-burst → graph; reuse the bench's capture).
>
> **PRIORITY (Edwin, 2026-07-05, now partly overtaken): SETUP track (CX) FIRST, against the VIRTUAL
> spectrometer; CAPTURE track (RC) was postponed — RESUMED 2026-07-06, RC-R0/R1 now done (see SM1 above).**
> The virtual-spectrometer functionality already exists, so the master
> setup + end-user registration flows can be built and **GUI-tested without any real hardware**. The
> milestone is therefore first reached **on the virtual device** (master authors a serial-object → end user
> self-registers with the serial → **connect** → measurement on the virtual spectrometer); the **real-device**
> measurement follows once RC-R0…R3 land.
>
> **First-sweep build order (Edwin): (1) full DB migration `CX-DB` → (2) master-user GUIs → (3) end-user GUIs;
> connect-spectrometer is part of this sweep** (can auto-connect; details TBD). Master = **three GUIs**: Plugin
> (`DbPlugin` CRUD), SpectrometerProfile (device + **auto-calibrate on file-set assignment**), SpectrometerSetup
> (bind profile + plugin). End-user = **Register** link on login → self-register (username/password/email/first/
> last + serial) → auto-login. Full detail: `spectracsPy/docs/SPEC_connection_and_calibration_ux.md` §4–§5.

```
 ID      Step                                                    Delivers                         Depends on     Kind
 ─────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
 RC      SPEC_real_camera_capture (resolver, backend, params)    the design (HW-verified)         —              DESIGN ✔
 SM1     "Capture images" dev view (Settings>Development):       real live capture + the shared   RC             DONE ✓
         resolver + backend(no MJPG) + save-PNG + per-camera     capture foundation (RC-R0+R1)
         exposure (ELP=78) + OUR auto-exposure (bisect/slider)
 RC-R0   SensorCaptureIndexResolver (VID:PID→cv2 index, sysfs)   right camera, not hardcoded 0    RC             DONE ✓
 RC-R1   CaptureBackend owns cv2; VideoThread routes through it  one capture path; drop MJPG-force RC-R0         DONE ✓
 RC-R2   Real device selection: ROI/Hough + detect-peaks (=SM2)  complete calib off the real cam  RC-R1          DONE ✓
         (+ calibration auto-exposure, green doublet, blue ROI)  (via dev measurement bench)
 RC-R3   Real capture in workflow "Measure" + live-burst→graph   real measurement UX (=SM3)       RC-R2          DONE ✓
         (shipped M3: shared CapturePanel in the wizard; rig-verified)
 ─────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
 CX      SPEC_connection_and_calibration_ux (serial-as-key)      the object model + UX design     RC (context)   DESIGN ✔ + BUILT
 CX-M    Master setup UI: serial → {device, calibration(CFL),    master authors a unit            CX, #4         DONE ✓
         plugin}  (+ strip AppUser.pluginId/spectrometerDevice)
 CX-E    End-user register-by-serial → resolves bundle → session end user activates a unit        CX, CX-M       DONE ✓
 ─────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
 ★ MS    MILESTONE: real measurement after user setup           see below                        RC-R3+CX-M+CX-E goal
```

**★ Next milestone — a real measurement after the user has been set up.** End-to-end on the **real** device:
1. **Master** logs in and sets up a spectrometer unit: assigns a **serial**, links its device/sensor,
   produces a **calibration profile** (calibrate against the **CFL** lamp), and assigns the **plugin**.
2. A **new end user is registered** and **enters the serial** (from the unit's label) → the app resolves
   the unit's { device, calibration, plugin } bundle.
3. The end user runs a **real measurement** on the ELP camera (live-during-burst → spectrum graph) → the
   plugin evaluation renders. No virtual device involved.

`CX` (the `SPEC_connection_and_calibration_ux` design) is the **natural next step** (Edwin). Note it
**reverses Roadmap #3's** user-selection direction → key on **serial**, not username (see #3's ⚠ note).

## Test-suite hygiene debt  *(✅ test items CLEARED 2026-07-19 — was NEW 2026-07-10)*
Catalogued in [`spectracsPy/docs/SPEC_test_hygiene_debt.md`](../spectracsPy/docs/SPEC_test_hygiene_debt.md).
**All test items T1–T4 are now resolved** and `pytest tests/` runs as one invocation for the first time
(**192 passed in ~25s, no hang**). T1 (stale calib-DB fixture) & T3 (stale nav-glyph) cleared incidentally;
**T2** (the suite-staller — the delete test patched the *wrong* dialog, `QMessageBox.question`, while the delete
path opens `InWindowDialog.confirm`) fixed by patching the right target **plus** a dependency-free per-test
faulthandler **hang-watchdog** (`tests/conftest.py`) so a future modal-in-test fails fast instead of hanging;
**T4** (`test_lims_submission_assembly` — LIMS manufacturer default: born inconsistent in the M1 LIMS work) fixed
test-side to expect the `"Spectracs"` house label (vendor is a hard invariant; a blank Manufacturer is the
corrupt-data exposure to avoid). **Remaining debt is runtime/doc only** (R1 LAN-IP interface names, R2 missing
`-core` on `runServer.sh` PYTHONPATH, D1 `luxpy` doc drift) — no open test items.

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
- **Exposure / capture follow-ups (from SM1, 2026-07-07)** — auto-exposure is proven in the dev view;
  **✅ now also wired into real calibration (SM2, 2026-07-08)** via `AutoExposureCaptureHelper`; still
  open: **wire it into the measurement (SM3) flow**; the **LED-array measurement
  exposure** value (dial it live in the dev view once the LED source is on hand); camera **response linearity
  / gamma** check for quantitative T (same-exposure ref/sample keeps the ratio robust regardless); **gain**
  as a v2 lever when exposure alone can't reach target; a **focus-assist dev tool** (sharpness algorithm to
  beat the eye — the instrument is already focused, this is a quality aid, matters for calibration). Detail:
  `spectracsPy/docs/SPEC_real_camera_capture.md` §9.3, `SPEC_dev_capture_view.md` §6.
- **Pumpkin-oil evaluation → peak-ratio algorithm** *(later task, 2026-07-05)* — switch the
  `PumpkinOilPlugin` evaluation (currently spectrum→hue/colour) to a **peak-ratio** method. Design discussion
  (in German) captured externally: `https://share.google/x0Ij7iuZQR8Q` (Gemini thread). To be specced in
  English when picked up; affects `PumpkinOilPlugin.evaluation` (plugin-local, does not touch capture).
- **Pumpkin-oil authenticity / genuineness check** *(later task, 2026-07-05)* — a mechanism that verifies the
  measured oil is **really genuine pumpkin oil** (authenticity / adulteration detection), not just a colour/
  verdict. Plugin-side evaluation feature, postponed; relates to the peak-ratio algorithm switch above.
- **LIMS integration** *(**IMPLEMENTED + click-through verified 2026-07-11** — spec
  [`spectracsPy/docs/SPEC_lims_integration.md`](../spectracsPy/docs/SPEC_lims_integration.md); driving the bench to
  Publishing + Publish created `OIL-0006` in SENAITE with the PDF attached)* — the **field-to-lab**
  handoff: a plugin-declared **PUBLISHING** phase + "Send to LIMS" step (Publish button) creates a **Sample**
  (`AnalysisRequest`) in **SENAITE LIMS** (local Docker, `6090:8080`) and **attaches the M2 PDF** (whose embedded
  `workflow.json` + `capture_*.png` are the payload). **Everything runs over the spectracsPy-server API** — the client
  builds the PDF and calls **one Pyro `@expose`** (`publishSampleToLims(pluginLimsInfo, pdfBytes)`); the **server** holds
  the LIMS creds (never leaves the server) and assembles a **LIMS-neutral submission** from the authenticated AppUser +
  the **real spectrometer graph** (instrument = the registered spectrometer's vendor/model/style, analyses = the
  plugin's real metrics). A thin **`LimsGateway` seam** with an adapter registry keeps it **LIMS-agnostic** (no
  cross-LIMS library exists) — the **plugin selects the LIMS** via a `LimsTarget`; `SenaiteLimsGateway` (+ a
  `MockLimsGateway` for tests) is the first adapter and runs an **idempotent bottom-up ensure-or-create** of the whole
  `senaite.jsonapi` graph (Department→Category→Service, SampleType, InstrumentType/Manufacturer/Supplier→**Instrument**
  by serial, Client→Contact) then the Sample + PDF attach. The **`spectracs_app_service` is set up manually** (Lab Manager, so
  the bootstrap can create setup objects). Creds via `ServerConfig` `.env` (`LIMS_SENAITE_*`). **Deferred:** LIMS-side
  Plone add-on, numeric results push + analysis-level Instrument link, sample state machine, offline queue, second
  adapter (OpenELIS/FHIR). Implies the client is **online-required**. Implement on explicit request only.
- **Rental-fee / licensing gate** *(product — payment MILESTONE 1 IMPLEMENTED 2026-07-06)* — a **monthly
  rental-fee / license check** gates use; another reason operation is online-required. Ties to the
  connection/registration flow (`SPEC_real_camera_capture.md` §9.4-a2) — the serial-bundle resolve is a
  natural place to also verify licence. Spec:
  [`spectracsPy/docs/SPEC_paypal_payment.md`](../spectracsPy/docs/SPEC_paypal_payment.md). **✅ Milestone 1
  DONE + click-through verified:** single one-off **€1 sandbox** PayPal payment end-to-end — server-DB
  `Transaction` entity, PayPal Orders v2 (sandbox, stdlib-only), per-user **Account settings** screen
  (Profile + Payment tabs) off the header account menu (desktop), server-side secret in an un-versioned
  `.env`. Product model = monthly rental fee (pay even without use). **Next milestones:** M2 recurring
  €200/month (Subscriptions API), M3 live go-live, M4 Android. Implement on explicit request only.
- **Per-camera exposure range** *(capture fidelity — **POSTPONED** 2026-07-19)* — the auto-exposure search range is a
  **hardcoded `[1, 500]`** in `CapturePanel`, not per-camera. V4L2 `exposure_time_absolute` units differ wildly between
  cameras (and the driver clamps to its own min/max/step), so a fixed cap can top out **dark** on a different camera — a
  real gap now that **multiple cameras are in use**. Fix: read the camera's actual exposure range at open, else **seed
  per-sensor in `SpectrometerSensorUtil`** (the home for per-camera capture constants — WB-Kelvin, exposure default,
  range). Cheap diagnostic first: log the exposure AE lands on (near 500 ⇒ cap-limited; well below ⇒ the deliberate
  no-clip AE metric, leave it). Not urgent — the ELP works and the ~110–120 spectrum peak is mostly the no-clip metric,
  not the cap. Spec: [`spectracsPy/docs/SPEC_capture_quality.md`](../spectracsPy/docs/SPEC_capture_quality.md) §14.9.
  Emerged from the capture-fidelity arc (C1–C3 + WB split + full-frame settle, all done 2026-07-19). Implement on
  explicit request only.

## Dependencies / suggested order
- **#1, #2, #4 — done.** The measurement/evaluation concept + Pipeline Playground PoC — done.
- **#3 is now unblocked (#4 done)** → the natural next item; needs the new `AppUser`↔`Profile` link.
- **#5** unblocks realistic end-to-end runs of **#6** on the virtual device (synthesis already proven in the
  Playground; the three image slots + image round-trip remain).
- **#6** needs the nested-wizard GUI design; its **first milestone** (absorption display) is pipeline-proven
  in the Playground — what remains is wiring it through the real `SpectralWorkflow` spine / plugin.
