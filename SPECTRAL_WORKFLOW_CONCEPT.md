# Spectracs — Spectral evaluation workflow (concept & recovery)

> Recovered 2026-06-28 by reading the code three years after it was written. Captures the **original
> intent** behind `SpectralWorkflow` / `SpectraContainer` / the evaluation idea, and reconciles it
> against what is **actually implemented**. Companion diagrams:
> [`plugin_model.svg`](plugin_model.svg) (two-audience / plugin model, §7–8) and
> [`workflow_object_model.svg`](workflow_object_model.svg) (the §9 object model).
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
3. Compute **transmittance** `T = SAMPLE / REFERENCE` (and **absorbance** `A = −log10 T` for the plot).
4. **Evaluate** it — originally two ideas: (a) reduce to one **color/hue** and compare to a reference
   "perfect green"; (b) classify with **LDA** (Linear Discriminant Analysis / Fisher's method, the
   Iris-flower classifier). LDA was abandoned — oil spectra have **no discriminating peaks**.

> **⚠ Correction (see `spectracsPy/docs/SPEC_measurement_evaluation_concept.md`):** the colour is computed
> from the **transmission `T`** (under a fixed D65), **not** from the absorbance — colouring `A` would give
> the complement, and colouring the raw `SAMPLE` would depend on the LEDs. The reference-normalisation
> (`T = S/R`) is also what makes the verdict **LED-independent**. Phrasings below that say
> "hue(absorption)" should read **hue of the transmission**; absorbance stays the right object for the
> absorption *plot* and peak analysis.

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

(Per-stage build status: see the table in §3.)

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
                                         types defined: ACQUIREMENT, ACQUIREMENT_VIEW only (dead;
                                         ACQUIREMENT_VIEW dropped in §9 — viewing is a step property)
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

> **Superseded:** the settled object model is in **§9** (revised 2026-06-28) — a static 5-phase spine
> with plugin-created steps; `SpectraContainer` = named bags + `inputs[]` + `producedBy(step)`;
> `SpectralJob` folded into the acquisition container and **deleted**; `SpectralWorkflow` itself is the
> persisted record. The recursive `spectraContainers` tree is dropped and `sourceSpectraContainer`
> becomes the list `inputs`.

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

- **`SpectralWorkflow` is single and generic** — one fixed phase **spine**, the same for *every*
  scenario: **ACQUISITION → PROCESSING → EVALUATION → METADATA → PUBLISHING**. The spine (the set of
  phase *types*) is **static**; what varies per scenario is the **steps inside each phase**, which the
  plugin creates (§9). Each phase exposes one **entry point**; *rendering is no longer a phase* — it is a
  property of a step (a step may carry a view).
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

# 9. Settled object model & plugin entry points (design — supersedes the as-built §5)

> **Revised 2026-06-28.** Replaces the earlier `PluginState`-threaded draft. The model now rests on a
> **static phase spine** with **plugin-created steps**, and the `SpectralWorkflow` *itself* is the
> persisted record. `SpectralJob`, `PluginState`, and the separate "measurement-record" entity are all
> gone (§9.1, §9.4, §9.5). Class diagram: [`workflow_object_model.svg`](workflow_object_model.svg).
> Illustrative plugin code: **Appendix A**.

## 9.1 The static spine, the dynamic steps

The model turns on one split:

- **Static — the phase *types*.** Every scenario walks the same fixed spine of five phases. This is the
  `SpectralWorkflowPhaseType` enum and it never changes:

  ```
  ACQUISITION → PROCESSING → EVALUATION → METADATA → PUBLISHING
  ```

- **Dynamic — the *steps* inside a phase.** The plugin decides, per scenario, how many steps a phase has
  and what each does. A phase is a fixed slot; its `steps` are created at runtime by the plugin's hook
  for that phase (§9.4). A phase whose hook creates zero steps is **auto-skipped** by the host (no tab, no
  `Next` stop) — this is the general opt-out: any phase, METADATA included, is skipped via an empty/no-op hook.

So `SpectralWorkflowPhase` is "static" only in that the *set of phases* is fixed — its **`steps` are
plugin-controlled**. Rendering is **not** a phase: the old `ACQUIREMENT_VIEW` and `RESULT` were really
*views*, so here "does this show a tab?" is a property of a **step** (§9.3). The absorption plot is a
PROCESSING step with a view; the colour swatch is the EVALUATION step's view.

```
SpectralWorkflow                       ← the run AND the persisted record (§9.5); has a User
  phases : { phaseType → SpectralWorkflowPhase }    # fixed 5 — the static spine
     SpectralWorkflowPhase
        entryPoint : the plugin hook for this phase (§9.4)
        steps      : [ SpectralWorkflowStep ]        # plugin-created; one tab each if it has a view
           SpectralWorkflowStep (§9.3)
              container        : SpectraContainer?    # spectral data (§9.2)
              evaluationResult : EvaluationResult?
              document / email : Pdf? / Email?
              persist          : bool                 # per-step save mark (§9.5)
              view / widget    : transient — host-built, not saved
```

## 9.2 Container — one stage of a run

A **`SpectraContainer`** is a dict of **named bags** of spectra that remembers its lineage:

```
SpectraContainer:
    spectra    : { role(str) → [Spectrum] }   # named bags: "reference","sample","absorption",…
    inputs     : [SpectraContainer]            # provenance = operands (a LIST — absorption needs 2)
    producedBy : SpectralWorkflowStep          # the step that produced this container
```

This is the **same shape as the now-retired `SpectralJob.spectraBySampleTypes`** — which is why
**`SpectralJob` is folded into `SpectraContainer` and deleted**. `SpectralJob` was nothing but *the
container an ACQUISITION step produces*: bags keyed by `SpectrumSampleType`, each holding the burst of
~50 raw captures. The "many captures → mean/median" reduction it implied is **not** acquisition's job —
it is the first PROCESSING step (the acquisition tab *previews* that mean live, drawn over the ~50 raw
captures, but the median step is its canonical, persisted producer). The as-built recursive
`spectraContainers` tree is dropped; the singular
`sourceSpectraContainer` becomes the list `inputs`.

Example run (pumpkin); `producedBy` is now the *step*:

```
C0  { reference:[50] }            inputs=—           producedBy=Acq/REFERENCE step  ┐ two acquisition
C0' { sample:[50] }               inputs=—           producedBy=Acq/SAMPLE step     ┘ steps, one bag each
C1  { reference:[1], sample:[1] } inputs=[C0,C0']    producedBy=Proc/median step
C2  { absorption:[A] }            inputs=[C1]        producedBy=Proc/absorption step (A=−log10(sample/reference))
C3  { absorption:[A'] }           inputs=[C2]        producedBy=Proc/baseline step
```

Each ACQUISITION step owns a **single-bag** container; the absorption step pulls
`inputs=[referenceStep.container, sampleStep.container]`. `inputs` + `producedBy` give full
**provenance** back to the raw frames.

## 9.3 The Step — carrier of data + view

A `SpectralWorkflowStep` has **no entry point of its own** (that lives on the phase, §9.4). It carries one
unit of work and at most one tab:

```
SpectralWorkflowStep:
    # durable — saved as part of the workflow (§9.5)
    container        : SpectraContainer?    # spectra (saved per the `persist` mark)
    evaluationResult : EvaluationResult?     # a CONTAINER of renderable view-models (ColorSwatch, Label, Verdict, …)
    document         : Pdf?                   # PUBLISHING — file-on-disk + DB reference (built by a host report util)
    email            : Email?                 # PUBLISHING — POSTPONED (transport/recipient TBD)
    persist          : bool                   # per-step save mark
    # transient — host-built, never saved
    view             : ViewSpec?              # declarative; the host turns it into a tab
    widget           : QWidget?               # the realised tab; rebuilt from durable data on reopen
```

A step is either **interactive** (its view *collects* input — a live-video acquisition tab, a metadata
form) or **computed** (its data is produced by the phase hook — absorption, evaluation, PDF). A purely
headless step (e.g. median) may carry no view at all.

## 9.4 Phase entry points — one per phase; they *create the steps*

Each phase has exactly **one plugin entry point**. Running it **creates that phase's `steps`**, fills
their durable data — or, for interactive phases, *declares* the input steps the host will fill — and the
host then renders one tab per viewable step. The hook is handed the **`SpectralWorkflow`** itself and
reaches every earlier phase's steps through it, so there is **no separate `PluginState`**: the workflow
*is* the accumulating state.

A plugin is **one class with five hooks**, one per phase type, plus its bundled reference data:

```python
class SpectralPlugin:
    def acquisition(self, workflow): ...   # DECLARE the ACQUISITION steps (interactive video tabs)
    def processing(self, workflow):  ...   # CREATE+FILL the PROCESSING steps (median, absorption, …)
    def evaluation(self, workflow):  ...   # CREATE+FILL the EVALUATION step (criteria live in this code)
    def metadata(self, workflow):    ...   # DECLARE the METADATA step (a form; empty hook = phase skipped)
    def publishing(self, workflow):  ...   # CREATE+FILL the PUBLISHING steps (PDF; email postponed)
```

- **Interactive phases** (ACQUISITION, METADATA): the hook *declares* steps and their view kind (video,
  form); the **host fills them from the user** — captures frames into the container, writes the form
  values.
- **Computed phases** (PROCESSING, EVALUATION, PUBLISHING): the hook *creates and fills* the steps when
  the host runs it on **Next**. The reusable **Operations** (median, absorption, baseline, normalize,
  smooth — each `SpectraContainer → SpectraContainer`) remain a shared library the plugin composes.

The host owns navigation (the main-buttons-bar **Next**) and runs each phase's hook at the right moment.

**Acquisition declaration** — example of an interactive hook; the host renders one tab per step:

```python
def acquisition(self, workflow):
    phase = workflow.getPhase(ACQUISITION)
    phase.addStep(MeasurementStep(role=REFERENCE, label="Isopropanol (reference)", frames=50, mandatory=True))
    phase.addStep(MeasurementStep(role=SAMPLE,    label="Pumpkin oil",            frames=50, mandatory=True))
# olive: a single SAMPLE step; a 3-step case can prepend a BLANK step. The label is the tab title.
```

**The plugin's criteria & reference data:** beyond the **live measurement** (the container chain it
builds), each plugin needs its own decision criteria — pumpkin's "perfect-green" hue + threshold bands,
olive's LDA training set. **The plugin code itself is the source of truth**: criteria are constants/logic
in the plugin, and larger assets (the LDA model) are bundled with it. There is **no separate
`referenceData()` hook** — the `evaluation` hook just uses what the plugin carries.

## 9.5 Persistence — the workflow *is* the record

What gets saved is the **`SpectralWorkflow` itself**: simultaneously the live runtime object and the
stored measurement record. It gains a **`User`** (the username / serial). There is **no** separate
"measurement-run" entity, and **no** `MeasurementProfile` / `DbSpectrum`.

- **Durable** (saved): the phase/step structure, each step's `container` spectra **subject to its
  `persist` mark**, `evaluationResult`, `document` (PDF), `email`, the METADATA values, the `User`, and a
  timestamp.
- **Transient** (dropped on save, **rebuilt on reopen**): each step's `view` / `widget`. Reopening a
  saved workflow re-renders the tabs *from* the durable data — exactly "view the result and how it was
  obtained."

Save rules:
- **Only the reduced MEAN spectra are persisted** (one per sample type). The raw acquisition bursts
  (~50 each) are **transient** — the acquisition tab plots all 50 captures with their **mean overlaid
  above them** for live feedback, but the raw frames are **never saved**.
- **Later derived spectra → opt-in**: the plugin sets `persist=True` on the ones worth keeping — notably
  the **absorption** spectrum, the basis of the evaluation.
- **`evaluationResult` → persisted** (the renderable view-models that make up the result). *How the host
  turns those view-models into the result tab — the GUI integration — is **deferred**.*
- **`document` (PDF) → built by a host report utility** from the completed workflow, **stored in the
  filesystem** with only a **reference kept in the DB** (no BLOB). Contents/personalization TBD.
- **Email → postponed** (transport, recipient, send-record — section C, deferred).

**Two deletions this locks in** (dead placeholders, superseded by the above):
- **`DbSpectrum`** — literally `class DbSpectrum: pass`, never a table. Deleted.
- **`MeasurementProfile`** — an `id` + FK-to-`SpectrometerProfile` stub behind a dead Settings button.
  Its meaning ("the configured thing a user runs") is now the **config binding** below. Deleted.

**Plugin boundary (refined).** Plugins stay Qt-free and **never call persistence** — the **host (the
client invoking the plugin) persists**. The plugin *does* now read and create pieces of a *persistable*
workflow — a conscious softening of the old "DB-free" line, acceptable under the first-party-trusted
model of §11: the plugin manipulates the entities; the host saves them.

**Config binding (who runs what)** — separate from the run record, the *configuration* hangs off the
user:

```
AppUser → { SpectrometerProfile ,  Plugin }
            (the calibrated unit)   (NEW entity: the signed plugin code + version — §11)
```

`AppUser` (the auth entities, `SPEC_user_auth_login.md`) has roles today but no link to a profile or
plugin — both references are new wiring. Serial = username (§7); login downloads the profile + the plugin.

**Cost (on record).** The `model/spectral/` classes (`SpectralWorkflow`, `…Phase`, `…Step`,
`SpectraContainer`, `Spectrum`) are plain Python today, not SQLAlchemy. Making the workflow the record
means **promoting them to persistable entities** (mapping them like the rest of `databaseEntity/`).

## 9.6 The pumpkin run, end to end

```
[ACQUISITION] hook declares 2 steps → REFERENCE + SAMPLE video tabs; host captures ~50 frames each
   │            (tab plots all 50 with their mean overlaid above; only the mean is persisted)
   │ Next
[PROCESSING]  hook creates steps: median (→ mean spectra) → absorption (−log10 s/r, persist=True)
   │           → optional baseline; the absorption step carries a spectrum-plot view (a tab)
[EVALUATION]  hook creates 1 step: hue(absorption) vs perfect-green → evaluationResult; swatch view
   │ Next
[METADATA]    hook declares 1 form step: remark + roasting-temp (both mandatory); host fills it
   │ Next
[PUBLISHING]  hook creates 2 steps: PDF (view = PDF tab, persist) + email-to-lab (send record)
   │ Finish → HOST persists the workflow (durable subset) against User + timestamp
```

**Resolved (2026-06-28):**
1. **Save scope** — **only the reduced mean spectra are persisted**; the raw ~50-frame bursts are
   transient (shown live with the mean overlaid, never saved). Later derived spectra (e.g. absorption)
   persist on the plugin's `persist` mark.
2. **Promotion approach** — **map the `model/spectral/` classes to SQLAlchemy directly** (one class is
   both the runtime object and the DB row); the workflow *is* the record.
3. **Phase skipping** — METADATA is kept, after EVALUATION. Any phase is **skippable** via an empty/no-op
   hook (zero steps) → the host shows no tab and does not stop there.

# 10. Plugin architecture — the SDK and the host/plugin split

A Plugin Module is **one class** (`SpectralPlugin`) with **five per-phase hooks** (§9.4). It imports from
**exactly one** curated namespace — the **Plugin SDK** — which is both the convenient toolbox and the
boundary of what a plugin can reach:

```
spectracs.plugin_sdk
  ├─ data   : SpectralWorkflow, SpectraContainer, Spectrum, SpectrumSampleType, EvaluationResult types
  ├─ ops    : MedianOp, AbsorptionOp, BaselineOp, NormalizeOp, SmoothOp   (container → container)
  ├─ util   : ColorUtil (spectrum → hue), …            ← the step-1 color utility lands here
  └─ views  : ColorSwatchView, ClassLabelView, SpectrumPlotView   (declarative ViewSpecs)
```

```python
from spectracs.plugin_sdk import SpectralPlugin, MeasurementStep, AcquisitionSpec, \
    MedianOp, AbsorptionOp, BaselineOp, ColorUtil, ColorResult, ColorSwatchView, REFERENCE, SAMPLE
```

Decision criteria and reference assets (pumpkin's perfect-green hue + thresholds, olive's LDA model) live
**in / bundled with the plugin code itself** — the plugin is the source of truth; there is no
`referenceData()` hook.

Layering:

```
HOST  (trusted — full access to Qt, DB, filesystem, device)
  ├─ Workflow engine     runs the phase hooks, owns the SpectralWorkflow, PERSISTS it
  ├─ Plugin SDK          the ONLY surface a plugin imports
  ├─ View renderer       step.view (ViewSpec) → real QWidget → step.widget
  └─ Plugin loader       loads + SIGNATURE-VERIFIES downloaded plugins
PLUGIN  (trusted; Qt-free; never persists — the host does, §9.5)
  imports only spectracs.plugin_sdk · pure logic + declarative specs
  reads/creates the (persistable) workflow entities; the host saves them
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

> The fuller, current next-steps backlog (login UI, profile↔user binding, user CRUD, virtual-spectrometer
> picture sets, the pumpkin workflow milestone) lives in [`ROADMAP.md`](ROADMAP.md). The three steps below
> are the original spine of that plan.

1. **Extract the color utility** — lift the proven `spectrasTest.py` hue pipeline into a utility/logic
   class (`ColorUtil.spectrumToHue(...)`; eventual home `spectracs.plugin_sdk.util.ColorUtil`, §10) so
   the essential, hard-won color math is **safe and reusable**. Stands alone (no dependency on roles or
   workflow wiring); becomes the body of `PumpkinPlugin.evaluate`.
   *Step-1 heads-up:* trim the dependency set — `spectrasTest.py` uses `colour-science`, `luxpy`,
   `spectres`, `BaselineRemoval`, `rgbxy`, `pyspectra`, `pandas`; check what's installed and whether
   `colour` + `colorsys` can cover most of it.
2. **Login + user roles** — the master ↔ end-user role gate (show/hide).
3. **Actually run the "workflow"** — wire `SpectralWorkflow` with its **static 5-phase spine** (§9) + the
   five plugin phase hooks, end-to-end, with **pumpkin color** as the first concrete plugin. Includes
   host-owned **Next** navigation, the **METADATA** form, and host **persistence of the workflow itself**
   (§9.5) — which means **promoting the `model/spectral/` classes to persistable entities**, **deleting**
   `DbSpectrum` + `MeasurementProfile`, and adding the `AppUser → {SpectrometerProfile, Plugin}` binding
   plus the new `Plugin` entity.

---

# Appendix A — Illustrative plugin sketches (NON-NORMATIVE)

> **Illustrative only.** The SDK names below are for *feel*, not a finalized API — they show how the §9
> model reads as plugin code and confirm the abstraction holds for both scenarios. GUI integration of
> `EvaluationResult` (rendering the view-models into tabs) is **deferred**.

**Same 5-phase spine, different organs** — the only things that genuinely move are *the steps a phase
creates* and *the criterion in `evaluation`*:

| Phase | Pumpkin (colour QM) | Olive (classifier) | What differs |
|---|---|---|---|
| ACQUISITION | REFERENCE + SAMPLE (2 steps) | SAMPLE only (1 step) | # of steps |
| PROCESSING | median → **absorption** → baseline | median → clean (no absorption — no reference to divide by) | which ops |
| EVALUATION | spectrum → hue → green/brown verdict | LDA → class label | the criterion |
| METADATA | remark + roasting temp | *(empty hook → skipped)* | present vs skipped |
| PUBLISHING | PDF (email later) | PDF (email later) | same |

```python
from spectracs.plugin_sdk import (
    SpectralPlugin, ACQUISITION, PROCESSING, EVALUATION, METADATA, PUBLISHING,
    MeasurementStep, Step, MetadataForm, Field, TEXT, NUMBER, REFERENCE, SAMPLE,
    MedianOp, AbsorptionOp, BaselineOp, NormalizeOp,            # shared container->container ops
    ColorUtil,                                                  # the spectrasTest.py hue math
    EvaluationResult, ColorSwatch, ColorSample, Label, Verdict, # renderable view-models
    SpectrumPlotView, PdfView, ReportUtil,                      # ReportUtil = host: workflow->PDF on disk
)


class PumpkinOilPlugin(SpectralPlugin):
    name          = "Pumpkin-seed-oil colour QM"
    serialPattern = "PMP-*"

    # -- the criterion: the plugin code IS the source of truth --
    PERFECT_GREEN_HUE = 0.28
    GREEN_BAND        = (0.22, 0.34)          # inside -> GREEN/PERFECT; below -> BROWN; above -> UNDER

    def acquisition(self, workflow):          # interactive -- host fills from the camera
        p = workflow.phase(ACQUISITION)
        p.addStep(MeasurementStep(REFERENCE, label="Isopropanol (reference)", frames=50, mandatory=True))
        p.addStep(MeasurementStep(SAMPLE,    label="+ pumpkin oil (sample)",  frames=50, mandatory=True))
        # host plots all 50 captures/tab with the mean overlaid; only the mean persists

    def processing(self, workflow):           # headless -- runs on Next
        ref = workflow.container(ACQUISITION, REFERENCE)        # {reference:[50]}
        smp = workflow.container(ACQUISITION, SAMPLE)            # {sample:[50]}
        p   = workflow.phase(PROCESSING)
        mean = p.compute("mean",       [MedianOp()],     inputs=[ref, smp])         # {reference:[1], sample:[1]}
        absn = p.compute("absorption", [AbsorptionOp()], inputs=[mean],             # A = -log10(sample/reference)
                         persist=True, view=SpectrumPlotView("Absorption A(lambda)"))
        p.compute("baseline", [BaselineOp(), NormalizeOp()], inputs=[absn])         # cleaned curve for evaluation

    def evaluation(self, workflow):
        spectrum = workflow.container(PROCESSING, "baseline").spectrum("absorption")
        hls      = ColorUtil.spectrumToHue(spectrum)            # hue / lightness / saturation / rgb
        result   = EvaluationResult([
            ColorSwatch(rgb=hls.rgb,                                    label="Measured colour"),
            ColorSample(rgb=ColorUtil.hueToRgb(self.PERFECT_GREEN_HUE), label="Perfect green"),
            Label(f"hue {hls.hue:.3f} - L {hls.lightness:.2f} - S {hls.saturation:.2f}"),
            Verdict(self._judge(hls.hue)),                       # GREEN / PERFECT / BROWN / UNDER
        ])
        workflow.phase(EVALUATION).addStep(Step("colour", result=result))

    def _judge(self, hue):
        lo, hi = self.GREEN_BAND
        if hue < lo:  return "BROWN"
        if hue > hi:  return "UNDER"
        return "PERFECT" if abs(hue - self.PERFECT_GREEN_HUE) < 0.03 else "GREEN"

    def metadata(self, workflow):
        workflow.phase(METADATA).addForm(MetadataForm([
            Field("remark",              TEXT,   label="Remark",             mandatory=True),
            Field("roastingTemperature", NUMBER, label="Roasting temp (C)",  mandatory=True),
        ]))

    def publishing(self, workflow):
        pdf = ReportUtil.build(workflow)                         # host util -> PDF on disk, returns a Pdf ref
        workflow.phase(PUBLISHING).addStep(Step("report", document=pdf, persist=True, view=PdfView(pdf)))
        # email step -> POSTPONED


class OliveOilPlugin(SpectralPlugin):
    name          = "Olive-oil classifier"
    serialPattern = "OLV-*"
    _LDA_MODEL    = "olive_lda.pkl"           # bundled WITH the plugin (source of truth)

    def acquisition(self, workflow):
        workflow.phase(ACQUISITION).addStep(                    # one step -- no reference
            MeasurementStep(SAMPLE, label="Olive oil (sample)", frames=50, mandatory=True))

    def processing(self, workflow):
        smp = workflow.container(ACQUISITION, SAMPLE)
        p   = workflow.phase(PROCESSING)
        mean = p.compute("mean",  [MedianOp()],                inputs=[smp])
        p.compute("clean", [BaselineOp(), NormalizeOp()],      inputs=[mean],        # NO absorption (no reference)
                  persist=True, view=SpectrumPlotView("Sample spectrum"))

    def evaluation(self, workflow):
        spectrum          = workflow.container(PROCESSING, "clean").spectrum("sample")
        label, confidence = self._classify(spectrum)           # LDA lives in the plugin
        result = EvaluationResult([
            Label(f"Class: {label}"),
            Label(f"Confidence: {confidence:.0%}"),
            Verdict(label),
        ])
        workflow.phase(EVALUATION).addStep(Step("class", result=result))

    def metadata(self, workflow):
        pass                                                    # empty -> METADATA phase auto-skipped

    def publishing(self, workflow):
        pdf = ReportUtil.build(workflow)
        workflow.phase(PUBLISHING).addStep(Step("report", document=pdf, persist=True, view=PdfView(pdf)))

    def _classify(self, spectrum):
        ...                                                     # load bundled LDA, project, return (class, confidence)
```

**What the sketch confirms:** the spine never appears in plugin code (the host walks it and calls each
hook); `compute(name, ops, inputs, …)` is the whole inner engine (it auto-stamps `inputs`/`producedBy`);
olive's empty `metadata()` exercises phase-skipping and its missing absorption is exactly why
steps-per-phase had to be dynamic; and the criterion is just plugin methods + constants/bundled assets
(`_judge` / `_classify`) — "the plugin is the source of truth," concretely.
