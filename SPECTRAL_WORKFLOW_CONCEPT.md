# Spectracs — Spectral evaluation workflow (concept & recovery)

> Recovered 2026-06-28 by reading the code three years after it was written. Captures the **original
> intent** behind `SpectralWorkflow` / `SpectraContainer` / the evaluation idea, and reconciles it
> against what is **actually implemented**. Companion diagrams:
> [`spectral_workflow_concept.svg`](spectral_workflow_concept.svg) (the pipeline) and
> [`plugin_model.svg`](plugin_model.svg) (the two-audience / plugin model, §7–8).
> See also [`DB_ENTITIES.md`](DB_ENTITIES.md) and [`runtime_models.svg`](runtime_models.svg).
>
> **⚠ Maintenance rule:** §§7–11 are the **to-be design**, not yet built. When the object model or the
> plugin structure is actually implemented (or changed), **update these docs and the diagrams to match** —
> they are the source of truth for the design and must not drift from the code.

## 1. Why the app exists (the root requirement)

Quality management for **pumpkin-seed-oil mills** (*Kürbiskern-Ölmühlen*). The roasting heat of the
seeds determines oil quality, and that shows up as **color**: too **brown** ⇒ over-roasted, too
**green/fresh** ⇒ under-roasted, a **"perfect green"** ⇒ best. The idea: derive that color objectively
from a spectrum instead of by eye.

Measurement protocol:
1. **REFERENCE** — pure isopropanol in a transparent lidded cup; ~50 captures, averaged.
2. **SAMPLE** — same cup with a few drops of pumpkin oil added; ~50 captures, averaged.
3. Compute the **absorption spectrum** from the two.
4. **Evaluate** it — originally two ideas: (a) reduce to one **color/hue** and compare to a reference
   "perfect green"; (b) classify with **LDA** (Linear Discriminant Analysis / Fisher's method, the
   Iris-flower classifier). LDA was abandoned — oil spectra have **no discriminating peaks**.

The need for *two* evaluation strategies (color-sample and LDA) is what motivated a **generic engine**:
source spectra → operations → pluggable evaluation → render. That engine is `SpectralWorkflow`.

## 2. Intended pipeline

```
 REFERENCE (isopropanol)      SAMPLE (isopropanol + oil)
   ~50 frames → median          ~50 frames → median
        └─────────────┬───────────────┘
                      ▼
            ABSORPTION  A(λ) = −log10(SAMPLE / REFERENCE)
                      ▼
            PROCESSING  baseline · smooth · rebin · normalize
                      ▼
            EVALUATION  (pluggable "evaluation profile")
               ├─ color-sample:  spectrum → HUE → judge vs "perfect green"
               └─ LDA classifier (abandoned)
                      ▼
            RENDER + signal result to the frontend
```

(Colored status version: `spectral_workflow_concept.svg`.)

## 3. Concept → implementation status

| Concept | Status | Where it lives / note |
|---|---|---|
| REFERENCE capture (isopropanol), ~50 frames | ✅ | `SpectrumSampleType.REFERENCE`; burst `setFrameCount(50)` |
| SAMPLE capture (+ oil drops) | ✅ | `SpectrumSampleType.SAMPLE`; `SpectralJob.spectraBySampleTypes` |
| Frame reduction = **median** | 🟡 | implemented as **mean** (`SpectrumUtil.mean` = `numpy.mean`; graph `PLOT_SPECTRA_MEAN`). **Median was intended** |
| **Absorption** A(λ) = −log10(sample/reference) | ❌ | not implemented anywhere |
| Processing operations | 🟡 | app: `SpectrumUtil.removeBaseline` (morphological opening), `smooth` (SavGol×7), `mean` — each with a `clone` flag. Prototype used **Zhang** baseline + `spectres` rebin. Not composed into a pipeline |
| Drop a Spectrum (e.g. REFERENCE) optionally | 🟡 | modeled by `SpectraContainer.sourceSpectraContainer` chaining — but unwired |
| **Spectrum → color → HUE** | 🟡 **prototyped & working** | **`spectracsPy/spectrasTest.py`** (standalone, NOT integrated): `colour.sd_to_XYZ` (CIE 1931 2°, D65) → `XYZ_to_xy` → RGB → `colorsys.rgb_to_hls` → **hue/lightness/saturation**, + a fixed-lightness swatch. Validated on real oil `.dx` data |
| Judge hue vs "perfect green" (brown/green/perfect) | ❌ | only a ΔE primitive exists (`SpectralColorUtil.getColorDifference`, CIE2000); no threshold/decision, no stored reference color |
| LDA classification | ❌ | abandoned; no `sklearn`/discriminant anywhere (test2/3 are RANSAC *calibration*, not LDA) |
| Generic evaluation via **plugin / evaluation profile** | ❌ | no plugin system; the Settings **"Evaluation profiles"** button is dead (no handler, no entity/view) |
| `SpectralWorkflow` phase engine (acquire→process→evaluate→render, plugin entry points) | 🟡 scaffold, **unwired** | classes exist; nothing instantiates them; phase types only `ACQUIREMENT`, `ACQUIREMENT_VIEW` (no PROCESS/EVALUATE) |

## 4. The working color prototype (`spectracsPy/spectrasTest.py`)

This is the script you remembered — it **works** and is the reference implementation for the evaluation
color math. It is a **standalone matplotlib experiment**, separate from the app's `SpectralColorUtil`
(which is a different, simpler `wavelength→RGB` + ΔE path). Steps:

1. Read an oil spectrum from a **Foss `.dx`** file (`pyspectra.read_dx`).
2. **Smooth** — Savitzky-Golay ×3 (`pyspectra ... sav_gol`).
3. **Rebin** to 380–780 nm @ 1 nm (`spectres`).
4. **Baseline** — `BaselineRemoval.ZhangFit()` (adaptive, not the app's morphological method).
5. **Normalize** to max.
6. Build a `colour.SpectralDistribution`.
7. **Spectrum → color** — `sd_to_XYZ(sd, CMFS "CIE 1931 2°", illuminant D65, method="Integration")`
   → `XYZ_to_xy` → `xy_to_rgb` → `colorsys.rgb_to_hls`.
8. Extract **hue** (the green↔brown discriminator), lightness, saturation; render a swatch at fixed
   lightness 0.20.
9. Saves step1–step6 PNGs to `spectracs-evaluations/<date>/`.

**Libraries it relies on:** `colour-science`, `luxpy`, `spectres`, `BaselineRemoval`, `pyspectra`,
`rgbxy`, `colorsys`, `pandas`. (This is **why `colour-science` is a project dependency** — and why
numpy is pinned `<2`.)

**Validation data:** `spectracs-evaluations/` holds real reference-instrument oil spectra
(`oil_spar_mean.dx`, `oil_lugitsch_mean.dx`, `oil_weinhandel_mean.dx`, `oil_hofer…`, etc., across
2022–2023 dated folders) plus the step1–6 evaluation images.

## 5. How the scaffolding maps to the concept

```
SpectralWorkflow                      ← the generic phase engine          (UNWIRED)
  phases : { SpectralWorkflowPhaseType → SpectralWorkflowPhase }
                                         types defined: ACQUIREMENT, ACQUIREMENT_VIEW only
     SpectralWorkflowPhase
        steps : { id → SpectralWorkflowStep }
           SpectraContainer            ← a step's data: source spectra + derived spectra
             spectra                : { key → Spectrum }
             sourceSpectraContainer : SpectraContainer   (the "operation input" chain)
             workflowStep           : SpectralWorkflowStep
```

`SpectraContainer` is exactly the "**source spectra → operation → derived spectra**" node the generic
pipeline needs (a calibration/processing/evaluation step would read `sourceSpectraContainer`, run an
operation, and store results in `spectra`). It is unwired and has a duplicate-`__init__` bug.

> **Superseded:** the settled, simplified data model is in **§9** — named bags + `inputs[]` +
> `producedBy`, with the recursive `spectraContainers` tree dropped and `sourceSpectraContainer`
> replaced by a list `inputs`.

## 6. Gaps to realize the concept (no code yet — for planning)

1. **Absorption** step: `A(λ) = −log10(sample/reference)` from the two sample-typed spectra.
2. **Median** reduction (replace/augment `SpectrumUtil.mean`).
3. **Integrate the color/hue evaluation** from `spectrasTest.py` into a real `SpectralColorUtil`/logic
   method (spectrum → hue), reusing the colour-science path that already works.
4. **Judgment**: a stored "perfect green" reference + thresholds → brown/green/perfect verdict.
5. **Evaluation-profile + plugin** mechanism (wire the dead Settings button; define an entity/interface).
6. **PROCESS / EVALUATE phase types** + actually **instantiate `SpectralWorkflow`** and fix
   `SpectraContainer`.
7. (Dropped) **LDA** — left out by design; revisit only if a peak-bearing feature is found.

---

# 7. Two audiences (personas) & roles

The app is really **two apps wearing one GUI**, joined by the **serial number**. Diagram:
[`plugin_model.svg`](plugin_model.svg).

| | **Master user** (project owner) | **End user** (e.g. pumpkin-oil producer) |
|---|---|---|
| Goal | Author & publish | Run *one* thing, see a verdict |
| Works with | Spectrometer profiles, calibration, plugin modules | Only **their module** |
| Sees | All of Settings (device, calibration, plugin authoring, publish) | Run-the-workflow screen; maybe a **read-only** SpectrometerProfile. Nothing else |
| Identity | — | **The serial number** (eventually a unique username) |

**Role gate** — same screens, **role-filtered (show/hide)**, *not* a separate UI. The `Login` button
(today a dead stub, no handler/view, no role concept in code) will flip **master mode** ↔ **end-user
mode**. End-user mode hides all authoring/setup; it exposes the workflow-run screen (+ optional
read-only profile).

**End-user journey:** `enter serial / login → (download my profile + plugin) → run my module →
guided measure REFERENCE then SAMPLE → RESULT (color swatch + verdict) → done`. They never see
spectra, profiles, calibration, or evaluation setup.

# 8. The plugin model — one workflow, many Plugin Modules

**Corrected understanding (supersedes any "module = a separate workflow" reading):**

- **`SpectralWorkflow` is single and generic** — one fixed phase pipeline
  (ACQUISITION → PROCESSING → EVALUATION → RENDER) shared by *every* scenario. Each phase exposes an
  **entry point**.
- A **Plugin Module** is the per-scenario implementation that **fills those entry points** (how to
  acquire / process / **evaluate**). Same skeleton, different organs:
  - **Pumpkin module** → evaluation = spectrum → **hue** → green/brown verdict (the `spectrasTest.py`
    math).
  - **Olive module** → evaluation = **LDA** classification.
- The **serial** is the key to **(SpectrometerProfile calibration + the Plugin Module(s), typically
  one)**. Final intent: the **serial becomes a unique username**, and **login downloads both** the
  profile and the plugin.
- The **result** is whatever the evaluation plugin emits (color swatch + verdict for pumpkin; a class
  label for olive).

This is what `SpectralWorkflow` / `SpectraContainer` were scaffolded for; the **Plugin Module concept
does not exist in code yet** (no plugin system — the "Evaluation profiles" Settings button is the
nearest dead placeholder).

# 9. Settled data model & plugin entry points (design — supersedes the as-built §5 `SpectraContainer`)

## 9.1 Container — one stage of a run

A **`SpectraContainer`** is a dict of **named bags** of spectra that remembers its lineage:

```
SpectraContainer:
    spectra    : { role(str) → [Spectrum] }   # named bags: "reference","sample","absorption",…
    inputs     : [SpectraContainer]            # provenance = operands (a LIST — absorption needs 2)
    producedBy : Operation / Step              # who/what produced this container
```

This is the **same shape as today's `SpectralJob.spectraBySampleTypes`**, generalized so the role can
be a *derived* label (not just a captured sample type) and the container records its lineage. The
as-built recursive `spectraContainers` **tree is dropped** (never needed) and the singular
`sourceSpectraContainer` **becomes `inputs: [ ]`** (a list).

Example run (pumpkin), each container linking back to the last:

```
C0 { reference:[50], sample:[50] }  inputs=—   producedBy=Acquisition
C1 { reference:[1],  sample:[1] }    inputs=[C0] producedBy=MedianOp
C2 { absorption:[A] }                inputs=[C1] producedBy=AbsorptionOp   (A=−log10(sample/reference))
C3 { absorption:[A'] }               inputs=[C2] producedBy=BaselineOp
```

`inputs` (operands) + `producedBy` (operator) give full **provenance** back to the raw frames; a
plugin can walk it to reach the physical measurements.

## 9.2 Two spectra sources for a plugin

1. the **live measurement** — the container chain above; and
2. the plugin's **own reference data** — pumpkin: the "perfect-green" color; olive: the LDA
   **training-set spectra** — **bundled/downloaded with the plugin**, *not* part of the measurement.

## 9.3 The entry points — all threaded through `PluginState`

Every phase has the **same uniform signature** `(PluginState) → PluginState`: each reads earlier slots
and fills its own. `PluginState` is the single object that accumulates the whole run:

```
PluginState:
    device      : Device
    acquisition : AcquisitionSpec           # filled by acquire (the measurement steps; §9.4)
    metadata    : dict | None                # filled by the METADATA phase (declarative form; §9.5)
    container   : SpectraContainer | None    # current spectra (+ provenance via .inputs)
    result      : Result | None              # filled by evaluate (DATA, not a widget)
    view        : ViewSpec | None            # filled by render (DECLARATIVE, not a QWidget)
    widget      : QWidget | None             # filled by the HOST from `view` (plugins ship no Qt)
```

| Phase | Plugin fills | Pumpkin | Olive |
|---|---|---|---|
| **acquire** | `state.acquisition` = ordered `[MeasurementStep]` (§9.4) | REFERENCE + SAMPLE ×50 | SAMPLE ×50 |
| **process** | `state.container` via an ordered **recipe** of Operations | median → absorption → baseline | median → baseline → normalize |
| **evaluate** | `state.result` (a `Result`, using `self.referenceData()`) | hue vs perfect-green | LDA vs training set |
| **render** | `state.view` (a declarative `ViewSpec`) | `ColorSwatchView` | `ClassLabelView` |
| **metadata** | `state.metadata` from a plugin-declared form (§9.5) | remark + roasting temp | scenario fields |

- **Two layers.** Phases speak `PluginState → PluginState` (outer); the **Operations** inside `process`
  speak `SpectraContainer → SpectraContainer` (inner, reusable). The engine runs the recipe and stamps
  `inputs`/`producedBy` automatically → free provenance. Operations (median, absorption, baseline,
  normalize, smooth) are a **shared library**; the plugin only *lists* them.
- **Qt-free plugins.** `evaluate` produces **data** (`Result`); `render` produces a **declarative**
  `ViewSpec`; the **host** turns `view` → real `QWidget` (`state.widget`). Plugins never import Qt.
- **Discipline:** each phase fills *its own* slot and only reads earlier ones (keeps `PluginState` from
  degrading into a god-object).

## 9.4 Acquisition spec — declarative, drives the master GUI

`acquire` declares an **ordered list of measurement steps**; the host renders one tab per step (order +
label + mandatory enforced). The plugin never pokes the GUI.

```python
def acquire(self, state):
    state.acquisition = AcquisitionSpec([
        MeasurementStep(role=REFERENCE, label="Isopropanol (reference)", frames=50, mandatory=True),
        MeasurementStep(role=SAMPLE,    label="Pumpkin oil",            frames=50, mandatory=True),
    ])
    return state
# olive: a single SAMPLE step; a 3-step case can prepend a BLANK step. The label is the tab title.
```

## 9.5 The run as a phase sequence (screens + headless + host persistence)

The entry points generalize into an **ordered list of phases** the host runs — owning navigation (the
main-buttons-bar **Next**) and persistence. Phases alternate **screens** (the plugin declares a
view/form; the host renders it) with **headless compute** (the host runs it on `Next`). This matches the
existing `SpectralWorkflowPhase` scaffolding — we just grow the phase types beyond today's
`ACQUIREMENT` / `ACQUIREMENT_VIEW`.

Pumpkin run:

```
[ACQUIREMENT]      capture REFERENCE + SAMPLE                screen: measurement tabs (§9.4)
   │ Next → PROCESSING (median→absorption→baseline)          headless
[ACQUIREMENT_VIEW] show the ABSORPTION spectrum              screen: spectrum plot (ViewSpec)
   │ Next
[METADATA]         remark + roasting temp (both mandatory)   screen: plugin-declared FORM
   │ Next → EVALUATION (hue → verdict)                       headless
[RESULT]           color swatch + verdict                    screen: ColorSwatchView
   │ Finish → PERSIST (spectra + metadata + result)          headless — HOST, never the plugin
```

- **METADATA entry point** — declarative, like acquisition: the plugin declares fields; the host builds
  the controls, enforces `mandatory`, and writes values into `state.metadata`:

  ```python
  def metadataSpec(self):
      return MetadataForm([
          Field("remark",              kind=TEXT,   label="Remark",             mandatory=True),
          Field("roastingTemperature", kind=NUMBER, label="Roasting temp (°C)", mandatory=True),
      ])
  ```

- **Persistence = host, not the plugin** (plugins are DB-free). At `Finish` the host saves the whole
  `PluginState` (spectra + `metadata` + `result`) against the serial/profile + a timestamp. ⚠️ This is
  **unbuilt today** — `DbSpectrum` is an empty stub and `MeasurementProfile` is unused — so it needs a
  **measurement-record schema** (this is where `MeasurementProfile` finally earns its purpose; see
  `DB_ENTITIES.md`).

**Open — decide before building this part:**
1. **Phase order** — METADATA before RESULT/evaluation (as drawn, so the saved record is complete), or RESULT first?
2. **Who owns the phase sequence** — **plugin-declared** (olive can differ: no absorption view, other metadata) *(leaning)*, or a fixed host sequence the plugin only fills?
3. **Persistence scope** — store the raw 50+50 frames, or only the derived spectra (medians, absorption)? And revive `MeasurementProfile`/`DbSpectrum`, or model a fresh "measurement run" entity?

# 10. Plugin architecture — the SDK and the host/plugin split

A Plugin Module is **one class** (`SpectralPlugin`) with the four `PluginState`-threaded hooks plus
`referenceData()`. It imports from **exactly one** curated namespace — the **Plugin SDK** — which is both
the convenient toolbox and the boundary of what a plugin can reach:

```
spectracs.plugin_sdk
  ├─ data   : Spectrum, SpectraContainer, PluginState, SpectrumSampleType, Result types
  ├─ ops    : MedianOp, AbsorptionOp, BaselineOp, NormalizeOp, SmoothOp   (container → container)
  ├─ util   : ColorUtil (spectrum → hue), …            ← the step-1 color utility lands here
  └─ views  : ColorSwatchView, ClassLabelView, SpectrumPlotView   (declarative ViewSpecs)
```

```python
from spectracs.plugin_sdk import SpectralPlugin, MeasurementStep, AcquisitionSpec, \
    MedianOp, AbsorptionOp, BaselineOp, ColorUtil, ColorResult, ColorSwatchView, REFERENCE, SAMPLE
```

Reference data (the plugin's *second* spectra source — pumpkin's perfect-green, olive's training set) is
**bundled inside the plugin** and returned by `referenceData()`.

Layering:

```
HOST  (trusted — full access to Qt, DB, filesystem, device)
  ├─ Workflow engine     runs phases, threads PluginState
  ├─ Plugin SDK          the ONLY surface a plugin imports
  ├─ View renderer       ViewSpec → real QWidget → state.widget
  └─ Plugin loader       loads + SIGNATURE-VERIFIES downloaded plugins
PLUGIN  (trusted, but Qt-free & DB-free by construction)
  imports only spectracs.plugin_sdk · pure logic + declarative specs
```

# 11. Trust & security model

- **No Python sandbox.** Untrusted Python cannot be safely sandboxed in-process (introspection escapes).
  The SDK is a **convenience + decoupling boundary, NOT a security boundary** — do not attempt to sandbox
  Python (a known rabbit hole; note kept here so future-us doesn't try).
- **First-party trust (current model).** The **master authors and signs** all plugins; end users run
  **only the master's signed plugins** → plugins are *trusted code*. No third-party plugins for now.
- **The one real control:** the download/login path must **verify the plugin's signature** (authentic +
  unmodified) before loading it.
- **Deferred** (only if third-party plugins ever appear): either **declarative-only** plugins (no code —
  spec + named ops/evaluator/view, all logic in the host) or **process/OS isolation**.

# 12. Agreed build sequence (discussion-locked; implement on explicit request only)

1. **Extract the color utility** — lift the proven `spectrasTest.py` hue pipeline into a utility/logic
   class (`ColorUtil.spectrumToHue(...)`; eventual home `spectracs.plugin_sdk.util.ColorUtil`, §10) so
   the essential, hard-won color math is **safe and reusable**. Stands alone (no dependency on roles or
   workflow wiring); becomes the body of `PumpkinPlugin.evaluate`.
   *Step-1 heads-up:* trim the dependency set — `spectrasTest.py` uses `colour-science`, `luxpy`,
   `spectres`, `BaselineRemoval`, `rgbxy`, `pyspectra`, `pandas`; check what's installed and whether
   `colour` + `colorsys` can cover most of it.
2. **Login + user roles** — the master ↔ end-user role gate (show/hide).
3. **Actually run the "workflow"** — wire `SpectralWorkflow` with its **phase sequence** (§9.5) + the
   plugin entry points, end-to-end, with **pumpkin color** as the first concrete plugin. Includes
   host-owned **Next** navigation, the **METADATA** form, and host **persistence** — which needs a new
   **measurement-record schema** (the currently-unbuilt part; `DbSpectrum`/`MeasurementProfile`).
