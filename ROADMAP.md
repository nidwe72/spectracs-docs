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
> **⭐ UPDATE 2026-07-31 — SERIES D RAN; the discrimination criterion is now MET ON MEASUREMENT, not projection.**
> [`SPEC_capture_quality.md`](../spectracsPy/docs/SPEC_capture_quality.md) **§16.13**. Six re-seats of one brown
> fill, post-rebuild: **brown σ = 0.131** (raw CV 1.41 %) against a pre-registered pass criterion of ≤ 3.5 % —
> **PASS**, and below the optimistic branch of §16.11.12's two-outcome prediction. Green 12.251 ± 0.354 vs brown
> 9.303 ± 0.131, **no overlap on any run**; at the shipped T = 10.6 green sits **+4.66 σ** above and brown
> **+9.88 σ** below *(those σ-margins use set C alone, matching the means quoted; against the wider B+C green
> the figure is +4.83 σ — `SPEC_capture_quality.md` §16.13.5)*. The ~10 % false-GREEN that was "all the remaining risk" is retired. The brown mean also
> survived a rig rebuild *and* a different oil (9.303 vs the archived 9.361, −0.62 %).
> **▶ What is left is σ_fill, and only σ_fill:** series D is re-seats of ONE fill and excludes sample preparation
> entirely, so **series E** (6 separate fills, time-ordered, raw + baselined side by side —
> `SPEC_capability_proof.md` §11.4f B) is the single outstanding measurement. §16.11.13's protocol inversion and
> §16.11.11's projected decision table both stay gated on it. ⚠ **T = 10.6 itself remains UNVALIDATED** — series D
> bought precision, not correctness (item 1 above is still the answer to that).
>
> **Also 2026-07-31 — the science behind the metric was corrected and written up.** The oil's pigment is
> **protochlorophyll / protopheophytin**, not chlorophyll, and its red band is at **~623–626 nm**, not ~665
> (`KB_spectroscopy_physics.md` §4.1 — sourced; the wrong molecule had propagated through seven documents). The
> green→brown difference is **speciation, not concentration** (§16.13.9, *d* = 10.26 on a scale-invariant ratio).
> A new internal document, **[*From Spectrum to Verdict*](internal/Spectracs_MetricAlgebra.pdf)**, gives the
> metric's physics, algebra and dilution-invariance proof; the two existing internal PDFs were corrected to match.
> Near-term plugin task marked: **surface the 600–630 band as a measurement and rename the metric family**
> (`SPEC_pumpkin_peak_ratio_eval.md` §15, HIGH PRIORITY, short). ⭐ **DONE 2026-08-03, differently than
> planned** — instead of surfacing 600–630, the far anchor was **moved to 620–630 nm** onto the Qy band and
> declared in `declaredEvalBands()`, which surfaces it by construction (`SPEC_capture_quality.md` §16.20;
> §15.1 marked RESOLVED).
>
> **⏸ 2026-08-01 — DECISIONS: the solvent programme is PAUSED, and "one measurement decides" is now bounded.**
> 1. **Keep isopropanol** (`SPEC_capture_quality.md` §16.12.7b). The solvent work existed to buy **precision**;
>    series D showed precision is no longer the binding constraint (*d* ≈ 11) — **the unvalidated threshold
>    is**. A solvent swap would buy what we do not need and cost a threshold re-derivation. **1-butanol is
>    rejected outright on hazard** (H318, irreversible eye damage); **2-butanol** is recorded as the surviving
>    candidate, not scheduled. ⚠ **A safety constraint nobody had written down:** the 220 V lamp sits in the
>    lower cone under a top-down beam, so **any leak runs into mains electrics** — vessel integrity is a safety
>    property, which rejects the acetone-plus-glass-hack route and constrains all future vessel work. The
>    decision is **conditional on series E**; a large σ_fill reopens it.
> 2. **The one-measurement question is bounded, and it is close to binary.** Brown is the binding class:
>    **σ_fill ≤ 3.3 % CV ⇒ one measurement decides ≥95 %**; at 5.4 % it decides 39 %. §11.4f B's pre-registered
>    3–6 % straddles that boundary exactly. **There is no failure mode, only a cost** — the fallback is the
>    three-fill protocol shipped today. Sensitivity table: §16.11.13.
> 3. **✅ Dilution invariance is RESOLVED for practical purposes** (`SPEC_capture_quality.md` §16.10.8) — the
>    other half of the gate's own pass criterion. Pooling the **three** within-oil dilution pairs already on
>    disk gives a log–log slope of **+0.033 ± 0.029**, consistent with zero: a realistic ±17 % preparation
>    error moves the metric **~0.6 %**, a *fourfold* error under 5 %, against a **33 %** class gap. Edwin's
>    point that the imprecision *is* the experiment — drops are not drops, by intent — turns out to be
>    measurable. The dedicated ≥4× sweep is **demoted**, not a gate item.
> 4. **Position adopted:** >95 % is practicable, and "measure again" is a **feature**, not a defect — provided
>    the ÜBERGANG verdict withholds its *direction* (§16.10.17c) **and** the protocol is enforced: new
>    **§16.10.17e** — an incomplete run yields **no verdict**, and the protocol, threshold and completeness are
>    **persisted with the record** (same argument as A3's `pluginVersion`). DESIGN; implement later.
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

## ▶ Plugin hardening — production-readiness gate  *(HYPER-MILESTONE / real blocker — Edwin 2026-07-24; design-first)*

**A stable plugin system is a primary concern and a real blocker: we go productive only once the plugin _code_ and
its _integration_ are stable.** This is a go/no-go gate in the class of the Capability Proof — not a feature. To be
specced; captured here so it is not lost. Scope of what "stable" must cover:
- **SDK contract stability** — settle/freeze the `plugin_sdk` surface a plugin builds against (the five phase hooks,
  the view-model vocabulary, and the new `policy()` / `WorkflowPolicy` from
  [`SPEC_simplified_plugin_navigation.md`](../spectracsPy/docs/SPEC_simplified_plugin_navigation.md)). Versioned
  already (`targetSdkVersion`); hardening = a settled, documented, back-compatible contract.
- **Execution robustness / error isolation** — a misbehaving plugin (a hook that throws, returns malformed data, or
  targets a mismatched SDK) must **degrade gracefully and never crash the host**. One uniform, defensively-wrapped
  execution surface = the `PluginExecutionView` base (that spec, §10).
- **Validation & conformance** — a plugin **conformance test-suite** + load/publish-time validation (extends the
  existing `PluginPublishUtil.inspectPluginSource` + `codeRefMatchesClass` + self-contained lint).
- **Security / trust of exec'd code** — a distributed plugin runs arbitrary Python client-side; review the trust
  model (Ed25519 sign + verify + sealedness dispatch, `SPEC_plugin_distribution.md`) as the _complete_ guard,
  including what an unsigned/dev plugin may do.
- **Diagnostics** — clear operator-facing errors + logs when a plugin fails.

**Relationship to current work:** the nav/convergence work
([`SPEC_simplified_plugin_navigation.md`](../spectracsPy/docs/SPEC_simplified_plugin_navigation.md) — `NavigationModel`
+ one `PluginExecutionView` base + two thin subclasses) is a **down-payment** on this milestone: it makes plugin
_integration_ generic, uniform, and testable, which is a precondition for hardening it. Sequence: settle that
convergence, then spec + build this hardening gate **before any productive/commercial rollout**.

> **✅ DOWN-PAYMENT SETTLED — 2026-07-25:** the simplified-plugin-navigation bundle is **IMPLEMENTED + rig-verified**
> (M1 nav-model SDK · M2 `AbstractPluginExecutionView` base + two thin subclasses · M3 DEV should-be · the §7b/§7c
> content bundle J·T1·T2·T3·C1·C2·SW). Highlights: plugin-driven `WorkflowPolicy`/`NavigationPolicy`; AUTO_ADVANCE
> with a fresh-capture-only re-jump (Option C); explicit `TabGroupView` sub-tab container across both render targets;
> DEV renames/reorder + full-frame(ROI-border)+cropped report captures; phase-chevron renames (Metadata→"Details",
> Publishing→"Verdict/Publish"); the temporary `SIMPLIFIED_NAVIGATION` toggle removed (DEV is permanently should-be).
> The `plugin_sdk` surface this settles (`policy()`/`WorkflowPolicy`, the view-model vocabulary incl. `TabGroupView`)
> is exactly the contract the SDK-stability bullet above must freeze. Suite 260 spectracsPy + 23 plugins green.

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

## Known defects (open)

- **`CameraWarmupVideoThread` reaches `CapturePanel.handleVideoThreadSignal`** *(intermittent; long-standing,
  logged 2026-08-04)* — `CapturePanel.py:413` does `if not videoSignal.isPreview:` and occasionally receives a
  **`CameraWarmupVideoThread` instance** where a `VideoSignal` is expected →
  `AttributeError: 'CameraWarmupVideoThread' object has no attribute 'isPreview'`. Seen twice in one bench
  session (`20260804A`), each time **after** the sample's `CAPTURE-SETTINGS`/`CAPTURE-LOWDN` lines, so the
  captured data itself was complete — but it kills the stream handler mid-session and is a plausible reason the
  panel does not carry a locked exposure forward between runs (see the AE-control item below).
  ⚠ **How the warm-keeper's payload reaches that slot is NOT yet explained**: `CameraWarmupService` never
  connects it to the panel, and `CameraWarmupVideoThread` does not even declare `videoThreadSignal` (only
  `DevCaptureVideoThread` and the three calibration threads do). Needs a runtime check on the rig, not a static
  read. ⛔ **Do not "fix" it with `getattr(videoSignal, "isPreview", False)`** — that silences the traceback and
  *masks* the real hazard the comment above the line warns about (§14.6: a non-capture frame becoming
  `__latestImage`, so the burst starts on a mid-ramp outlier). Root-cause it. Suspects to check first: a leaked
  worker delivering a queued signal into a discarded panel (the `__stopStream` failure mode already documented
  there), and the `CameraLease` pause/resume handover.

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
  exposure** value (dial it live in the dev view once the LED source is on hand); ~~camera **response linearity
  / gamma** check~~ **✅ DONE 2026-07-26 — measured, see the gamma item below**; **gain**
  as a v2 lever when exposure alone can't reach target; a **focus-assist dev tool** (sharpness algorithm to
  beat the eye — the instrument is already focused, this is a quality aid, matters for calibration). Detail:
  `spectracsPy/docs/SPEC_real_camera_capture.md` §9.3, `SPEC_dev_capture_view.md` §6.
- **Gamma linearization of the capture path** *(DE-RISKED DESIGN, ready to build — 2026-07-26)* — undo the
  camera's brightness curve before the spectrum is formed. **Verdict-neutral by construction**, so it moves no
  threshold and blocks nothing: measured off-line from the spectra embedded in the report PDFs, the pigment ratio
  is *bit-identical* under a pure-power decode at any exponent. Settled: decode with the **pure `x^2.2`** law
  (the official piecewise sRGB curve measured **24 % worse** on class separation and was declined), applied per
  channel as the **first** operation in `ImageSpectrumAcquisitionLogicModule.__reducedColumnValues`; raise the
  colour ceiling 3.0 → 6.6 in the same commit. Gains: **+33–40 % perceived colour chroma**, physically real
  absorbance, cross-camera comparability of absolute values — and **closure**, retiring "maybe it's the gamma"
  as a suspect for every future anomaly. Spec: `spectracsPy/docs/SPEC_capture_quality.md` §17 + §17.5;
  documentation: `spectracsPy/docs/DOC_capture_fidelity.md` → `internal/Spectracs_CaptureFidelity.pdf`.
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
- **⭐ No way to PIN the exposure from the DEV plugin — TO DISCUSS** *(NEW 2026-08-04; sibling of the item above)*
  — the AE controls exist in `CapturePanel` (auto-expose checkbox + manual slider + `__lockedExposure`) and the
  plugin-declared `CaptureView` already carries chrome flags for exactly this kind of thing (frames/exposure
  hidden by default, `SPEC_bench_pdf_export.md`), **but the DEV plugin does not surface them**, so on the bench
  there is no way to hold one exposure across a run series. **Why it now matters (measured, not theoretical):**
  in `20260804A` the AE landed **104 / 90 / 104** across three consecutive runs of one fill, and the odd run
  carried a **flat +0.024 A** artifact against the two that matched (+0.003 A between the matched pair) — enough
  to move `Verdict · baseline` from 17.5 to 15.1. `findExposure` picks the brightest exposure still **≤ 245
  channelPeak**: a hard ceiling honed by bisection, so near the boundary the control is **bistable**, not
  drifting — a hair more brightness in the blank drops it a whole bisection step. **Discussion points:** (a) do
  we expose AE via a `CaptureView` flag, or a bench-level "lock exposure for this series" control that outlives
  a single run? (b) should the AE ceiling get hysteresis so it stops flipping rungs? (c) should the applied
  exposure be **persisted into `workflow.json`** — today it exists only as a stdout `CAPTURE-SETTINGS` line, and
  reconstructing which run used which exposure needed the `CAPTURE-LOWDN` value matched back against the
  embedded spectra. **(c) is the cheapest and probably should not wait for the rest.** See the exposure-invariance
  item below — that is the *reason* this is not merely a convenience.
- **⭐⭐ Exposure-invariance of `A = −log10(S/R)` is NOT established** *(NEW 2026-08-04 — Edwin's question;
  needs one rig run to settle)* — the pipeline assumes a changed exposure cancels in `T = S/R` because both legs
  share it. `20260804A` (4 runs of one filtered fill) says it does not. **The case, 3 controls vs 1:** three runs
  at AE-landed exposure **104** give `Verdict · baseline` **17.537 / 17.596 / 17.343** (sd 0.132, **0.8 % CV**);
  the one run at exposure **90** gives **15.114** — **−13.6 %, i.e. 18 sd** off the controls. Leg-scale mismatch
  tracks it exactly: **2.5 %** (DN) on the exposure-90 run against **≤0.5 %** on all three exposure-104 runs,
  worth a **flat +0.024 A** offset versus ±0.004.
  **⛔ SETTLING IS EXCLUDED** (Edwin, from the log): in the exposure-90 block the reference reads `minDn=96.2`
  and the *next* capture at the same exposure reads `minDn=96.2` — identical to 0.1 DN, so the sensor was fully
  arrived; and the first block's three consecutive references drift only 111.1→111.7→111.8 (**+0.6 %**) against
  the 2.5 % to be explained. **Mechanism still unidentified.** The sensor transfer is *not* the pure `pow2.2` the
  decode assumes (the reference's own drop is DN-dependent, ×0.886 dimmest fifth → ×0.920 brightest), but a toe
  cannot yet be confirmed as the cause: ⚠ **`corr(excess A, sample DN)` is CONFOUNDED and must not be used** —
  dark sample bins *are* the high-absorbance bins, so the concentration drift below fakes the toe signature
  exactly (control run 004 shows a *larger* such correlation, +0.58, than the suspect run, −0.28).
  **▶ Decisive experiment (one evening, no consumables):** one fill, one seating, no reseats — capture
  reference+sample at a ladder of **pinned** exposures and plot `A` and the metric against exposure. ⭐ **The
  ladder MUST be INTERLEAVED (104, 90, 104, 90, …), not swept monotonically** — see the drift below; a monotone
  sweep aliases it straight onto the exposure axis. Flat ⇒ invariance holds; sloped ⇒ that slope *is* the
  correction and exposure-pinning becomes mandatory. ⚠ Until settled, **runs at different exposures are not
  comparable**; archived sets should be checked for exposure homogeneity before pooling. **Written up in full
  as [`SPEC_capture_quality.md`](../spectracsPy/docs/SPEC_capture_quality.md) §16.24** (§16.24.1). Diagnostic:
  `spectracsPy/diagnostics/probe_20260804A.py`.
- **⭐⭐ THE ERROR BUDGET IS 17× ASYMMETRIC — `SPEC_capture_quality.md` §16.24.2/.3/.4** *(NEW 2026-08-05 —
  Edwin's question "there is the slope of the baseline that would change between the runs; how much?")* — the
  structural finding behind the item above, and it would have bitten without any exposure problem. The
  subtracted baseline is **62 % of the raw Q band and −1.0 % of the raw Soret**, so `QB` (0.036) is a small
  difference of comparable numbers while `SoretB` ≈ raw: **a 0.001 A baseline error is worth 2.78 % on the
  metric at Q against 0.16 % at the Soret — 17×.** ⇒ the metric's precision is set almost entirely by where
  the baseline lands at 570 nm. ⛔ The *extrapolated* Soret is NOT the fragile end (the baseline is ~0 there);
  the *interpolated* Q is — reason about **baseline-as-a-fraction-of-band**, not anchor geometry. The baseline
  slope varies **39 %** run-to-run, only **23 %** of it concentration: the slope is a *difference* of two
  anchors that individually track concentration well (r² 0.87 / 0.78) but whose difference does not (r² 0.23),
  so it sheds signal and keeps noise. ⭐ **And that is why the Qy-flank anchor works:** `corr(far anchor, Q
  band)` = **0.84–0.99**, so far-anchor scatter is common-mode with the Q band and cancels — 4–18× better than
  independent noise. ⚠ **QUALIFIED 2026-08-05 (§16.24.4a):** concentration drifts within a set and both bands
  follow it, so part of that correlation is trivial. Partial correlation with `A450` removed: **Steirerkraft B
  0.996 with concentration explaining only 3 % of the far anchor** (decisive FOR genuine common-mode), but
  **Kiendler A collapses 0.843 → 0.341** (there it is mostly just the fill). ⇒ **both mechanisms, mix varies by
  set**; the 4–18 % measured benefit holds either way, but where concentration dominates it says nothing about
  rejecting instrument noise. **The precision floor is the 1–16 % that is NOT common-mode**, and the failure signature
  is *an error that moves the far anchor without moving Q* (= run 002). Also gives §16.20.9 §2 the quantitative
  argument it lacked: the pedestal metric's denominator is 50 % bigger, and measures **half the sensitivity to
  the artifact (−6.1 % vs −13.6 %) at 3× the noise on clean runs (CV 2.4 % vs 0.8 %)** — a real trade, n=3.
- **⭐⭐ SPIKE LEAKAGE — a per-run instrument-health check, ready to build** *(Edwin's idea 2026-08-05;
  `SPEC_capture_quality.md` §16.24.8, DESIGN)* — the lamp's **473 nm pump edge** and **580 nm phosphor valley**
  are sharp; the oil absorbs smoothly; so any sharp structure surviving into `A(λ)` there is pure instrument
  artifact and its size measures the failure of `S/R` to cancel **in that run, from that run's own data** — no
  second run, no ladder, no calibration constant. Statistic `k = d(hp A)/d(hp log₁₀R)` on a 14.7 nm high-pass.
  On `20260804A` it reads **0.445 for the exposure-90 run against 0.014–0.097** for the controls — and ⭐ it
  correctly **CLEARS run 006**, whose metric is badly wrong but whose fault was the *sample*. ⇒ paired with
  §16.15's `near/A450` turbidity covariate, **two numbers separate instrument faults from sample events**
  (002 fails leakage / passes turbidity; 006 the reverse). **Build:** compute both at capture, **persist into
  `workflow.json`** (a stdout-only diagnostic cannot be used on an archived run), warn above a threshold.
  ⛔ **Do not invent the threshold from one session — compute `k` across the 122-run archive first.**
  ⛔⛔ **Detector, NOT corrector — and the shift interpretation is REFUTED (§16.24.8a-bis).** Aligning the legs
  was tried on BOTH fiducials and both are worse (473: CV 0.7 %→5.0 %; 607: CV 0.7 %→**23.8 %**). **There is
  nothing to align:** `ImageSpectrumAcquisitionLogicModule` takes the ROI *and* the pixel→nm polynomial from the
  stored calibration for both legs, so they are co-registered by construction. The fitted lags (−0.9…−1.4 nm)
  are orders too large to be physical, and they reproduce the **opposite-sign estimator artifact that
  `SPEC_metric_research.md` §3.6a had already diagnosed and fixed** in `diagnostics/lamp_line_calibration.py`.
  §16.13.9's registration question stays open; nothing here advances it. ⭐ **The idea is standard practice**
  (lock mass / D₂-lamp 486·656 nm / Raman Si 520.7 / `icoshift`) — its correct level is **BETWEEN SESSIONS**,
  which is exactly where §3.6a/§3.8 already puts it, gating C14.
- **⛔⛔ THE `r_Q` DILUTION SERIES IS STILL NOT RUN — and the archive shortcut is a TRAP (`SPEC_capture_quality.md`
  §16.16.12)** *(Edwin 2026-08-05: "I think we have done the dilution test for the Steirerkraft oil now")*.
  Pooling `20260804A` with `20270729B/C` gives the 2.4× `B_Q` span §16.16.2 asks for and a tight CI excluding
  zero — **and it is void.** Collinearity check: the new cluster sits **2.0 σ off** the B+C line, and `M∞` is
  **18.544 vs 11.181 for the same oil** ⇒ two clusters, not one line; the "fit" is the chord between them.
  ⚠⚠ **The tight CI is the trap** — it is narrow *because* the clusters are far apart (max lever arm), and a
  bootstrap resamples points, not the one-line assumption. Three further disqualifications, any one sufficient:
  not serial-from-one-stock (§16.16.4), `20260804A` is **filtered** so its pedestal composition differs by
  construction (§16.21.0a), and between-session `r_Q` stability is untested (§16.16.11 2′).
  ✅ **What IS confirmed: the shipped constant reproduces exactly on the shipped anchor — Kiendler A+B+C give
  `r_Q` = −0.0184, `M∞` = 12.450, CI [−0.0292, −0.0085] excluding zero.** ▶ §16.16.5 stands unchanged (serial
  dilution, 12 runs, span ≈4×, se ≈0.002) **plus two additions**: all rungs either filtered or unfiltered,
  never mixed; and keep `A_Q` ≥ 0.19 at the most dilute rung — §16.16.3's "go DOWN" is now amended to "down,
  but not past the floor".
- **⭐⭐ TWO STANDING BENCH RULES added to the preparation recipe — `SPEC_capture_quality.md` §16.23.9**
  *(Edwin 2026-08-05)*. **RULE 1 — filter for σ_fill ONLY, expect nothing for the metric:** §16.21.0a
  *predicted* it (the filter takes the flat coarse population, not the curving nanodroplets; `B_Q` is unchanged
  by construction) and `20260804A` *measured* it — `near/A450` −31 % ✅ but metric CV **4.0 %**, inside the
  archive's unfiltered 3.6–5.0 %, and the clearing drift unchanged. ⚠ σ_fill itself is still **untested** (one
  fill) so the filter arm still needs its own experiment. **RULE 2 — measure at the specified `A_Q`, never
  thin:** ⛔ `20260804A` ran at **`A_Q` = 0.093 against the 0.19 floor — 49 % of it**, and Kiendler A at 58 %,
  so this is a recurring drift, not one slip. Thin fills **double the leverage of every Q-band error**
  (`QB` 0.036 vs 0.068–0.073 ⇒ a 0.009 A shape error costs 25 % vs 12 %) and push `r_Q` to **54 % of its own
  denominator**, outside the 23–43 % on record — which is why the pedestal gauge lost dilution invariance
  (slope +0.334 vs +0.049). ⛔ **The linked risk: a visibly cleaner filtered sample invites a thinner fill —
  the one way filtration can actively hurt.** ⚠ §16.23.6a's blue constraint pulls the other way and is
  unresolved; **Rule 2 governs until §16.23.6f decides**, since the Q-band lever is measured and the blue-end
  cost is still a hypothesis.
- **✅ DECISION 2026-08-05 (Edwin) — KEEP THE BASELINE CONSTRUCTION AS-IS** (`SPEC_capture_quality.md`
  §16.10.2a). *(a)* it discriminates — Cohen's *d* = 6.60, no overlap on the post-rebuild archive; *(b)* it is
  arguable on two independent grounds — the Morton–Stubbs construction, and §16.24.4b's two-axis physics (the
  anchor shares the *amount* axis so noise cancels, differs on the *speciation* axis so signal survives; a
  pigment-free anchor would do neither); *(c)* nothing better exists — **five** routes closed in
  `SPEC_metric_research.md` §7.8 plus §7.14's audit of the orthodox alternatives, all blocked on the same
  missing 30 nm. ⚠ **Does NOT settle:** `T` = 10.6 (still unvalidated — this is about the construction, not
  the boundary), A1/`r_Q` transfer (§16.16.5 unrun), and it is **not** an argument against the red-end
  extension. Expected to be revisited only if the capture window changes.
- **⛔⭐ EXTERNAL-LITERATURE AUDIT of the orthodox scatter corrections — `SPEC_metric_research.md` §7.14**
  *(Edwin 2026-08-05; tool `diagnostics/scatter_correction_audit.py`)*. The shipped two-window linear baseline
  uses the **Morton–Stubbs / Allen construction** — but ⛔⛔ **CORRECTED same day (§16.10.2a): we satisfy NEITHER
  of its two conditions.** (1) it requires the irrelevant absorption to be **linear** — ours is convex, which is
  `r_Q` (25 % of `B_Q` at working strength, 54 % when thin); (2) it requires anchors **free of the analyte** —
  **51–79 % of our 620–630 anchor IS pigment**, measured against the scattering law. Violation 2 is
  **deliberate and load-bearing** (§16.12.12: remove the Qy flank and the classes overlap) and does **not**
  break dilution invariance (§16.14, anchor pigment scales with `c`), but it costs comparability: **`M∞` is not
  the pigment's true Soret/Q ratio**, which is the deeper reason `T` is tied to this rig and recipe. ⛔ *"We use
  the pharmacopoeial method"* is **NOT** an available defence — the honest one is *"we use its construction
  knowingly outside its conditions; here is what each deviation costs."* Two testable alternatives were priced and **both are worse**:
  **λ⁻ⁿ turbidity baseline** (Cohen *d* 6.60 → 5.88) and **2nd derivative** (*d* = **0.38, classes overlap**).
  ⭐⭐ Route E's failure is the sharpest disproof of assumption A6 in the research: scattering must FALL toward
  the red, but our far anchor **RISES on all five sets** (far/near 1.47–2.50, brown included), so no physical
  exponent exists and the fit rails at its bound — **the far window is the Qy flank, not a turbidity window,
  and one window cannot be both.** ⇒ every orthodox method needs either an absorption-free region or a
  resolved peak, and 440–629.8 nm has **neither** — i.e. **§7.8's "30 nm of missing spectrum" reached
  independently from outside.** Raises the value of the red-end capture extension; does **not** lower its price
  (S-mount roll-off + lamp collapse past ~630 nm remain).
- **⛔ `M` IS EXACTLY INVARIANT TO ANY CONSTANT SUBTRACTION — "normalise the spectra first" is always a no-op**
  *(`SPEC_capture_quality.md` §16.24.9; Edwin's proposal 2026-08-05, a real technique — dual-wavelength
  spectrophotometry — but already subsumed here)*. Normalising `R` and `S` at a chosen feature is algebraically
  `A' = A − A(λ₀)`, a single-point baseline. **Proof:** subtracting a constant `c` shifts the fitted anchor line
  by exactly `c`, so `(A−c)−(line−c) = A−line` and every band mean is unchanged. Measured at four anchor
  wavelengths: the shipped metric is **bit-identical on every row**. ⇒ the two-window linear baseline already
  removes a constant **and a slope**; the proposal removes only the constant. Colour already does it separately
  (`BaselineOffsetOp`, which colour genuinely needs). ⛔ **Retires the whole class of "rescale/anchor before the
  metric" proposals — do not re-derive.** ⚠ `r_Q` is NOT in this class (it subtracts from ONE band, after
  baselining). ⛔ And 473 nm would be the worst anchor: a point anchor on the steepest feature turns wavelength
  jitter into value error (raw ratio degrades 7.95 % → −252 %) — a fiducial wants maximum gradient, a
  normalisation point wants minimum.
- **⛔ Three sensor models RULED OUT for the exposure effect** *(`SPEC_capture_quality.md` §16.24.1a)* — wrong
  gamma (**exactly zero effect — an algebraic result: a wrong exponent scales `A` by a constant, the baseline is
  linear in `A`, so the ratio is invariant** ⇒ `M` is already immune to every pure-power transfer error, and
  under any pure-power sensor exposure cancels exactly ⇒ **there is no metric-side change left to make**),
  black level (−13.5 % → −12.6 % at 20 DN), stray light (**worse**, −14.5 %). Remaining suspects are not static
  per-pixel transfers.
- **Two by-products of `20260804A` worth keeping** *(2026-08-04; full write-up `SPEC_capture_quality.md`
  §16.24.5, incl. a ⛔ correction — the filter's scatter reduction is ~31 %, not 37 %, once concentration is
  controlled)*: **(1) a live dilution-invariance confirmation
  on ONE fill** — across the three exposure-104 runs `A450` fell **12.3 %** (0.6214 → 0.5446 over 87 min) while
  `Verdict · baseline` moved **0.8 %**. That is the §16.10.8 claim demonstrated with everything but concentration
  held fixed — arguably cleaner than the dilution pairs, which vary the preparation too. **(2) a concentration
  drift of −0.00085 A450/min in a FILTERED fill**, shape-preserving (the metric held), so it is *not* the
  §16.11.16 demetallation signature — pigment speciation is unchanged. Candidates: continued settling despite the
  0.22 µm filter, or adsorption onto the cuvette. ⚠ Relevant to **§16.11.17 (decay-rate)** and to **§16.23
  (preparation protocol)**: it puts a clock on a fill even after filtration, and it is the reason the ladder above
  must interleave.

## ▶ RESUME POINT — 2026-08-06  *(Edwin's priorities; supersedes the ordering below, not its content)*

> ⭐ **The path below is scheduled**: [`first_presentable_state.svg`](first_presentable_state.svg) — a Gantt of
> the **FIRST PRESENTABLE STATE** milestone (12 working days, the two tail risks marked). Prose spec with the
> gates, risks and definition-of-done:
> [`spectracsPy/docs/SPEC_first_presentable_state.md`](../spectracsPy/docs/SPEC_first_presentable_state.md).
> Regenerate the diagram with `java -jar plantuml.jar -tsvg first_presentable_state.puml`.

**▶ PRIORITY ORDER SET BY EDWIN 2026-08-06:**

### ⭐⭐ NEXT TASK — trim the SORET window 440–460 → **448–460** *(Edwin 2026-08-06: "next task to do")*

`SPEC_metric_research.md` **§7.13** (ADOPTED 2026-08-04, now scheduled). One constant:
`DevSpectralPlugin.PB_SORET_BAND = (440.0, 460.0)` → `(448.0, 460.0)`.

**Why:** `DOC_pedestal_correction.md` §7 established the **440–447 bins read 2.0–2.6 DN against a reference near
88 — they are not measurements**, and they sit inside the metric's numerator. Measured on the post-rebuild
corpus (`diagnostics/soret_448_since_0729.py`, 37 runs / 8 fills):

| axis | 440–460 | **448–460** |
|---|---|---|
| class separation *d* (green vs brown) | 6.54 | **7.50** |
| within-green *d* | 1.21 | **1.34** |
| dilution spread (same oil, 2 strengths) | +8.1 % | **+3.0 %** |
| ⚠ within-fill CV | 3.6–6.2 % | +0.2…0.5 pp *worse* |

⭐ And the mechanism is measured, not assumed: `A(440–447)/A(448–460)` should be a **shape constant** under
Beer–Lambert, and it falls **11 %** across the concentration range (**r = −0.94**, n = 8 fills) — the signature
of a detector floor eating a larger share of the darker band. The trim removes **47 % of the pedestal intercept
and 28 % of `r_Q`**.

⚠ **It moves the scale** (`B_Soret` 1.03 → 0.69), so it goes into the single threshold re-derivation together
with the 1/3 reduction band and, if built, the PRIO 1 aperture. ⚠ The plugin is **signed**, so this needs a
re-sign and a publish→assign pass.

⚠ **Deliberately NOT bundled:** the right-hand edge. `diagnostics/soret_right_edge_sweep.py` swept 456→500 nm
and a paired bootstrap put every difference's 95 % CI across zero — **460 stays**, and §16.26's revisit trigger
is the capillary, not this change.



### ⭐⭐ PRIO 1 — a new SAMPLE HOLDER with a proper aperture *(slide-in-the-jar solution)*

`spectracsPy/docs/SPEC_capture_quality.md` **§16.25.2**. Edwin will build it as a **slide-in** part rather than
a fixed ring. Two apertures, both sized to the jar's **INNER** diameter:

| | position | removes |
|---|---|---|
| **lower** | lamp → jar | light **entering** the glass wall at all — ⚠ the existing ring is as thin as the jar wall, so it does not |
| **upper** | jar → camera | wall-**exit** light at the rim, plus wall scatter |

⚠ **The upper cannot do the lower's job:** light guided up the wall by total internal reflection leaks into the
liquid en route (frustrated TIR — for oil the indices nearly match), crosses a *partial* path length and
arrives through the clean aperture looking like signal. Build both.

⭐ A slide-in also fixes **tilt**, which §16.26 identified as what a cylindrical jar actually punishes — and it
is a second, independent payoff: an aperture between jar and slit **fixes the angular acceptance**, making the
instrument blind to the beam re-aiming that a reseat causes.

▶ **Measure `f` first — it costs ten minutes and sets the whole payoff.** Undiluted oil in the jar (opaque in the
blue), read 440–460; whatever is above the dark offset is the wall path. Repeat with the outer wall taped.
`f = (S_opaque − D)/(R − D)`. ⚠ At today's recipe the trimmed 448–460 window only loses **2.3 % per 1 % of `f`**,
so the aperture is mainly an **enabler for a stronger fill** (at 3× concentration the same `f` costs 14 %).

### ⭐⭐ PRIO 2 — redo GREEN-vs-BROWN and GREEN-vs-GREEN on the capillary protocol

`SPEC_capture_quality.md` **§16.23** (protocol) and **§16.23.7** (why). The capillary is now the measured
blocker, not a convenience: §16.26 put the **instrument floor at 0.42 %** and a filled-jar **reseat at 1.28 %
rms**, against an archive CV of **3–5 %** — so **~3.8 % is the preparation**, and §16.23.7 measured that as
dosing: green-green signal **0.98 units against 1.665 units of fill spread, SNR 1.8 → 18** with the capillary.

⛔ **Gates G1/G2 first** (heparin blank; weigh ten filled capillaries). ⚠ Re-run BOTH pairs — green-vs-brown to
confirm the class call survives the new recipe, green-vs-green because that is the one the capillary is *for*
and the one the capability gate needs (*d* ≈ 1.3–2.0 today against ≳ 3 required).

### ⭐⭐ PRIO 3a — THE SMALL VALIDATION *(Edwin 2026-08-06; the milestone that freezes the thresholds)*

**A scaled-down validation that removes the part of PRIO 3 that was months.** Eight shop-available oils is a
weekend's shopping, not a mill relationship; two judges is a diary entry, not a panel.

**Panel — 8 oils, three tiers:**

| tier | n |
|---|---|
| brown (over-roasted) | 2 |
| green, **non-premium** | 2 |
| green, **premium** | 4 |

⭐ **The three tiers make this a WITHIN-GREEN test in the field**, which is exactly the half of the capability
claim PRIO 2 measures in the lab. ⇒ P3a is the field confirmation of P2's number, not a separate exercise.

**Judges:** two — Edwin (developer) and the colleague (chemist).

#### ⚠ Three design conditions. The third is the one that could sink it.

**1 · BLIND, and scored before any instrument output is shown.** Both judges record their scores first,
sealed, then the measurements are revealed. ⚠ Edwin knows what the instrument says; *"we scored first"* is the
only answer that survives a knowledgeable reader.

**2 · There is NO objective ground truth here.** PRIO 3 has roast level from the mill; shop-bought oils do not
carry it. ⇒ P3a validates **agreement with expert judgement**, not *"the instrument measures roast"*. A good
claim — but it must be stated as that one.

**3 · ⚠⚠ Judge perceived ROAST / greenness, NOT "premium".** "Premium" is a market label — seed origin,
pressing, packaging, price positioning — and may not correlate with roast at all. If premium and non-premium
come out indistinguishable you could not tell whether the metric failed or whether premium simply does not
mean less-roasted. ⇒ **Score the thing the instrument claims to measure**, and record the premium/non-premium
label **separately, as a second independent variable**. Then a disagreement between the two is a *finding*
rather than a confound.

#### ⭐ Keep P2 and P3a on DIFFERENT oils

Run **P2 on the oils already held** (Kiendler, Steirerkraft, S-Budget) and **P3a on the eight new ones**.

⚠ Otherwise the thresholds are frozen on the same data they are then validated against — circular, and
visible to anyone who looks. Kept separate, **P3a is a genuine OUT-OF-SAMPLE test on oils the thresholds have
never seen**, which is a far stronger claim for the same effort.

⛔ **Write the thresholds down, with a date, before the first P3a fill.** If P3a then disagrees they must NOT
be re-fitted to make it agree — that converts a validation into a demonstration retroactively.

#### What it licenses — and what it does not

✅ *"Eight oils, two independent experts scoring blind, thresholds frozen in advance and never seen by these
oils. The instrument agrees with expert judgement, separates roasted from unroasted with no overlap, and ranks
within the green class."* ⇒ **A defensible appetizer for the lab owner.**

⛔ **Not** *"it measures roast level"* (no objective ground truth) and **not** *"it generalises"* (n = 8, one
region, one season). PRIO 3 remains the study that earns those.

#### Effort — the agreed plan

| | days |
|---|---|
| 448 trim + publish path | 1 |
| **P1** `f` test + aperture | 2 |
| **P2** gates G1/G2 (incl. settling the capillary recipe) | 2 |
| **P2** green-vs-brown + green-vs-green re-runs | 1–2 |
| **P3a** measurement, 24 fills | 2–3 |
| **P3a** blind judging | 1 |
| **P3a** analysis + threshold freeze | 1 |
| **total** | **10–12 days** ⇒ ~2–2.5 weeks at 5 sessions/week |

⚠ **Two items carry a tail.** The **448 publish path** — one constant, but publish→assign→load has never been
run end to end; ▶ do a **no-op version bump through the flow first** so a failure there is not tangled with a
metric change. And the **aperture**, which assumes the print fits first or second try. ⇒ **13–15 days** with
those carrying normally.

⇒ ⭐ **At the end of P3a the metric and its thresholds are FROZEN** — the first quasi-fixed milestone, and the
end of the research state (see §16.26.13's criteria).

### ⭐⭐ PRIO 3 — THE FULL VALIDATION STUDY *(added 2026-08-06; `SPEC_metric_research.md` §9.1 item 6)*

⚠ **PRIO 3a above comes FIRST and is not a substitute for this.** P3a freezes the thresholds and buys a
defensible demonstration; **this** is what earns the two claims P3a cannot make — *"it measures roast level"*
(objective ground truth) and *"it generalises"* (panel size, more than one region and season).

**Never done, and until now not on any milestone.** Everything measured so far shows the numbers *separate*.
Nothing shows they **mean what we say they mean**.

⇒ Run it **after** PRIO 1 + PRIO 2 and the Soret trim — otherwise it validates a configuration that is about
to change, and has to be repeated.

**Panel:** ⭐ **12+ greens, 2–3 browns.** ⚠ Today the class claim rests on **ONE brown oil** (§2.2) — a single
bottle cannot support a class. The brown arm is the weaker half of the panel and the more important one.

**Ground truth — two independent kinds, recorded separately:**

| | what | why separate |
|---|---|---|
| **objective** | **roast level** (mill/process record) | the physical variable the metric claims to track |
| **subjective** | a **jury visual sub-score** | what a buyer actually judges — and it may *disagree* with roast level, which is itself a finding |

⚠ **Record them apart and never let one inform the other.** If the jury sees the instrument's verdict, or the
scores are merged before analysis, the study measures agreement with itself.

**Design conditions, all of which decide whether it is a validation or a demonstration:**

- ⛔ **Freeze the thresholds BEFORE the first sample** — pre-registered, from the re-derivation the Soret trim
  and the 1/3 band force anyway. A threshold fitted after seeing the panel proves nothing.
- ⚠ **Spread the panel across sessions and interleave the classes.** §3.4's **session confound** is still open
  and is the one thing that could invalidate `M` itself; a study run class-by-class in one sitting inherits it
  whole.
- ⚠ **Honour the ageing rule** (§16.11.16: a 24 h-aged fill reads as a *browner oil* and misclassified 3 of 3).
  Fresh per §11.4a, timestamp every fill, log sample temperature. ▶ §16.11.17's decay-rate run should ideally
  precede this, so "within the hour" is a measured window rather than a guess.
- **Report per-oil, not pooled** — a class-level *d* hides which individual oils sit near the boundary, and
  those are the ones a customer will bring.

⇒ **What it converts.** After the Soret trim + aperture + capillary the projected run-to-run CV is **~1.4 %**
(from 3–5 %), which should lift within-green separation from *d* ≈ 1.34 to **~3.5–5** — i.e. the capability
gate's failing half becomes met. ⭐ **At that point the bottleneck stops being precision and becomes
evidence**, and this study is the only item that addresses it. Until it runs, the instrument is good and the
*claim* is untested.

### ⭐ The 660–680 nm QUIET-WINDOW test *(added to the backlog 2026-08-06)*

`SPEC_metric_research.md` **§7.14.4** + `SPEC_capture_quality.md` §16.26. **One number decides whether the whole
scatter-correction family reopens:** the ratio of raw absorbance in the far anchor to the near anchor.

```
   scattering MUST fall toward the red  =>  far/near < 1  (Rayleigh ~0.39, Mie ~0.63)
   measured today at 620-630:  1.47 - 2.50 on all five sets   <- the far anchor is the Qy FLANK, not a blank
```

⇒ If a 660–680 window comes back **below 1** it is a genuine turbidity window, and: Morton–Stubbs (which
§7.14.1 shows we already use) stops violating its own precondition; the λ⁻ⁿ baseline becomes fittable with a
physical `n`; the pedestal is **measured** instead of inferred; and ⭐⭐ **620–630 is freed from anchor duty to
become a third signal band** — §16.12.12 showed it tracks oil class at 5.1 σ, information currently spent as
background.

⚠ **Two prerequisites, in order.** (a) **Does the lamp even reach there?** NOT established — the "Sansi 24 DN at
680 nm" figure came from a screenshot whose frame stops at ~676 nm and whose wavelength scale was *transferred*,
not measured. What IS solid is that the **instrument** reaches ~680 (CFL-calibrated, Eu³⁺ 650.7 resolves).
▶ One Sansi capture in the same configuration as the CFL frame settles it. (b) **The calibration must extend
past 630** — free, software, and needed for every path here.

⚠ Even if the window is NOT quiet, the extension still delivers §7.14.4's *other* requirement — a **resolved Qy
peak with curvature** (measured 2026-08-06: every archive fill is still **convex at 626 nm**, so the maximum has
never been observed). The two requirements are independent.

### ⚠ Bundled into the next threshold re-derivation *(item 3 of the 08-03 list — the list has grown)*

| change | effect on the scale |
|---|---|
| Soret window **440–460 → 448–460** (`SPEC_metric_research.md` §7.13, ADOPTED) | `B_Soret` 1.03 → 0.69 |
| Spatial reduction **inset 0.2 → 1/3** (§16.26.9, IMPLEMENTED 2026-08-06) | raw S/Q +2.2 % |
| PRIO 1 aperture, if built | changes the optical path |

⇒ **Do them once, together.** ⭐ Edwin 2026-08-06: thresholds are work-in-progress and nothing is shipped, so
this is a bookkeeping cost, not a blocker.

### What else changed 2026-08-06

- ⭐⭐ **The null-run method** (§16.26): same empty beam as reference *and* sample, so everything returned is
  error. Eleven runs. Instrument floor **0.42 %**; reseat **1.28 % rms** filled-jar, **6.60 %** empty-jar.
- ⛔ **RETRACTED:** an earlier reading that re-seating explains the whole archive CV. It was computed over
  *empty-jar* runs — a configuration that never occurs in practice — which inflated the population rms from
  1.4 % to 4.5 %. ⚠ **Run nulls in the operating configuration.**
- ⛔ **Decoys, all measured:** wavelength shift (re-registering recovers nothing), overall level (`M` is exactly
  invariant, §16.24.9), and "do the R/S curves coincide?" (run 010 looks alarming, costs 0.46 %; run 007 looks
  mild, costs 8.83 %).
- ⛔ **The paper diffuser is rejected** (§16.26.6): 14× light loss, −32…38 % relative blue, and 83–85 % of the
  along-slit non-uniformity is a *gradient* no diffuser fixes. **Centre the lamp first.**
- ⭐ **Lamp question:** reseat sensitivity is lamp-independent (1.44 % Yuji vs 1.50 % Sansi) ⇒ choose on
  spectrum. LED-combination simulation exists (`diagnostics/led_combination_search.py`, §16.25.4a); best
  7-emitter design is **3 × 6500 K + 2 × 430 + 1 × 515 + 1 × 660**, robust across four weightings. ⚠ **Do not
  build yet** — the decisive test needs no new hardware.
- ⭐ **A capture-path crash fixed:** `VideoSignal` was a `QObject` marshalled over a queued cross-thread
  connection while its only reference was a local abandoned on the stop path — a use-after-free that
  **segfaults** when reproduced in isolation. Now a plain Python class; guarded by
  `tests/test_video_signal_queued_delivery.py`. Rig-confirmed by Edwin: the app no longer crashes.

---

## ▶ RESUME POINT — 2026-08-03  *(the metric changed; the rig work is what is outstanding)*

**Shipped and green (329 tests), NOT COMMITTED.** The DEV plugin now shows **three verdicts** on a
**620–630 nm** far baseline anchor — `baseline + pedestal` (T = 10.6), `baseline` (T = 12.5), and the raw
Soret/Q as a **value with no verdict**. All 122 archived reports were regenerated on it (verified backup at
`spectracs-references/tmp_backup_pre620_20260803/`). Spec: `SPEC_capture_quality.md` §16.20/§16.20.7.

**⚠ A live defect was fixed on the way:** the PUBLISHING verdict badge — the one screen an end user sees —
ran on the raw Soret/Q at `T` = 4.4, a threshold **below the entire brown class**, so it reported brown oil
as "good — green" on every run (`SPEC_roast_ampel.md` §2b).

**▶ Next, in order:**

1. **M3 publish + assign** the changed plugin. ⚠ **Do a no-op version bump through the flow FIRST** — the
   live publish→assign→load path has never been rig-verified, so combining it with a metric change makes any
   failure ambiguous.
2. **Rig click-through** — capture → three verdicts → PDF, on the bench.
3. **⭐ `T` re-derivation.** The scale moved (`M∞` 9.998 → 12.450). 10.6 was *retained*, not derived, and it
   is the project's binding unvalidated constant either way.
4. **⭐⭐ The DECAY-RATE run — §16.11.17.** *(new 2026-08-03; jumped ahead of the filter and acetone arms
   deliberately.)* One fill, five reads over 24 h, one evening, no consumables. Edwin's question *"why does
   29th A differ from B and C?"* turned out to have a bigger answer than expected: **the 24 h-aged fill is a
   browner OIL, not a noisier one** — 27 % below fresh on a concentration-free shape ratio, with the Qy
   flank down 17 % while the 572 nm degradation feature is up 14 %, and **3 of 3 runs misclassify**
   (§16.11.16). The pigment demetallates in the solvent. **"Measure within the hour" is therefore a VERDICT
   rule, not a precision rule — and the hour has never been tested from either side.** It runs first because
   both arms below hold a fill for an unrecorded time and inherit the confound.
   *(The archive folder was renamed `20270729A` → `20270729A_aged24h` so this cannot be read past again.)*
   ⭐ **Amended after Edwin measured the running rig 2026-08-03: nothing exceeds 40 °C after 1½ h** — so the
   fill must be stored **on the bench**, not in the instrument, and the sample temperature logged at every
   read. A conventional Q₁₀ puts 40 °C at 3–5× the 25 °C degradation rate, which raises a live hypothesis:
   part of §16.12.11 A's 30-minute "settling" drift may be **thermal degradation** — same sign, same
   timescale, and nothing in that analysis could have told them apart (§16.11.17 P5).
5. **⭐ The PREPARATION PROTOCOL — §16.23**, using hardware already owned: a **60 µL haematocrit capillary**
   (Hirschmann 9100160, €0.04) dropped into **15 mL of IPA** in a **capped graduated centrifuge tube**. That is
   1:250 against today's ~1:243 — `A_Q` 0.224, inside the window, **so no threshold is re-derived**. It removes
   drop counting, the unknown drop volume, pipette transfer loss, evaporation, batch ageing and
   cross-contamination in one move, and makes a **σ_fill triplet cost ~€0.10**. ⭐ The cuvette is unchanged, so
   §16.12.7b's leak-into-mains constraint does not apply. ⛔ **Two gates first:** G1 the additive blank (are the
   capillaries heparinised? heparin is insoluble in IPA and would add scatter to the one system whose open
   problem is a scattering pedestal), G2 weigh ten filled capillaries (the 0.9–1.0 mm ID band is a ±10 % volume
   band and "nominal 60 µL" sits at its top). Both cost one capture and one weighing.
6. **The filter arm — `SPEC_capture_quality.md` §16.21, F1 first.** PTFE 0.22 µm filters ordered. ⚠ It is
   **not** a pedestal fix (§16.21.0a: filtration cannot make the correction less important). Its target is
   **σ_fill via the aliquot step**, which gates the one-measurement protocol — three fills (~1 h) versus one
   (~20 min) per sample. ⚠ Interleave the arms and timestamp every fill (§16.21.3).
7. **The acetone arm — §16.22**, a bench probe under mitigation. ⚠⚠ **§16.12.7b's safety rejection is NOT
   withdrawn**: acetone's vapour is heavier than air and the 220 V lamp sits in the lower cone beneath it.
   ⚠ Same interleaving requirement (§16.22.4). **Narrowed 2026-08-03 (§16.22.1a):** the 40 °C measurement
   closes the *thermal* half — acetone's autoignition is 465 °C, derated ~370 °C, so hot-surface ignition is
   10× away and the LED lamp is why. What remains is **electrical**: switch contacts, socket and LED driver
   inside the cone the vapour sinks into. ⭐ **Highest-value change before any acetone reaches the bench —
   switch the lamp from a socket OUTSIDE the cone**, which removes the only spark source from the vapour
   path. Also: at 40 °C acetone's vapour pressure is ~56 kPa against ~31 kPa at 25 °C, so open-cup
   evaporation runs ~1.8× faster (§16.22.4 A5).

**Still open and unchanged:** `r_Q`'s mechanism (§16.19 ruled out both scattering and the far anchor, leaving
≥80 % unexplained); §16.10.8 dilution invariance; brown σ_fill post-rebuild, never measured.

## Dependencies / suggested order
- **#1, #2, #4 — done.** The measurement/evaluation concept + Pipeline Playground PoC — done.
- **#3 is now unblocked (#4 done)** → the natural next item; needs the new `AppUser`↔`Profile` link.
- **#5** unblocks realistic end-to-end runs of **#6** on the virtual device (synthesis already proven in the
  Playground; the three image slots + image round-trip remain).
- **#6** needs the nested-wizard GUI design; its **first milestone** (absorption display) is pipeline-proven
  in the Playground — what remains is wiring it through the real `SpectralWorkflow` spine / plugin.
