# Spectracs — DB entity knowledge base

> Persistence model of the Spectracs app. Source: **`spectracsPy-model`**
> (`sciens/spectracs/model/databaseEntity/…`), mapped with **SQLAlchemy** to a local
> **SQLite** file (`<appdata>/spectracsPy.db`). Last updated 2026-06-27.

**Diagram:** [`db_entities.svg`](db_entities.svg) — generated from [`db_entities.puml`](db_entities.puml).
Regenerate with:

```bash
java -jar plantuml.jar -tsvg db_entities.puml
```

## Conventions

- **Every entity** inherits `id : String` (primary key, a uuid) and its table name from
  `DbBaseEntityMixin` (CamelCase → snake_case, e.g. `SpectrometerProfile` → `spectrometer_profile`).
- **Owned** (composition, `*--` in the diagram): the child belongs to one parent; deleting the
  parent should delete the child.
- **Shared / reference** (FK arrow `-->`): the row is shared lookup/catalog data; **do NOT delete it**
  when deleting a referencing profile.
- Roles: **core** (device profile + calibration), **catalog**/**reference** (shared lookup),
  **join** (association), **stub** (defined but unused).

---

## 1. Core — device profile + calibration

### `SpectrometerProfile`  (`spectrometer_profile`)
A user-created device instance: a catalog spectrometer + a serial + its own calibration.

| Field | Type | Notes |
|---|---|---|
| `id` | String | PK |
| `serial` | String | user-invented free text (see UI/serial notes) |
| `spectrometerId` | String | FK → `spectrometer` (**shared catalog**) |
| `spectrometerCalibrationProfileId` | String | FK → `spectrometer_calibration_profile` (**owned**) |

Relationships: `spectrometer` → `Spectrometer` (shared); `spectrometerCalibrationProfile` →
`SpectrometerCalibrationProfile` (owned, **1:1, no cascade declared** — must be deleted explicitly).

### `SpectrometerCalibrationProfile`  (`spectrometer_calibration_profile`)
The pixel→wavelength calibration for one profile: ROI rectangle + cubic polynomial + detected lines.

| Field | Type | Notes |
|---|---|---|
| `id` | String | PK |
| `regionOfInterestX1/Y1/X2/Y2` | Integer | ROI rectangle |
| `interpolationCoefficientA/B/C/D` | Float | pixel→nm cubic polynomial |

Relationship: `spectralLines` → `[SpectralLine]` (**owned**, `cascade="all, delete-orphan"` —
deleting the calibration deletes its lines automatically).

### `SpectralLine`  (`spectral_line`)
One calibrated line: links a detected pixel position to a known reference wavelength.

| Field | Type | Notes |
|---|---|---|
| `id` | String | PK |
| `pixelIndex` | Integer | detected peak x-position |
| `spectrometerCalibrationProfile_id` | String | FK → owning calibration (`back_populates`) |
| `spectralLineMasterDataId` | String | FK → `spectral_line_master_data` (**shared reference**) |

---

## 2. Catalog — spectrometer & parts (all **shared**, do not delete with a profile)

### `Spectrometer`  (`spectrometer`)
A spectrometer *model* (predefined catalog entry). Multiple profiles may reference the same one.

| Field | Type | Notes |
|---|---|---|
| `id` | String | PK |
| `modelName` | String | |
| `spectrometerSensorId` | String | FK → `spectrometer_sensor` |
| `spectrometerVendorId` | String | FK → `spectrometer_vendor` |
| `spectrometerStyleId` | String | FK → `spectrometer_style` |

### `SpectrometerSensor`  (`spectrometer_sensor`)
| Field | Type | Notes |
|---|---|---|
| `id` | String | PK |
| `vendorId, vendorName, sellerName, modelId, description, codeName` | String | |
| `isVirtual` | Boolean | **the virtual-device flag** — marks a virtual spectrometer sensor |
| `spectrometerSensorChipId` | String | FK → `spectrometer_sensor_chip` |

### `SpectrometerSensorChip`  (`spectrometer_sensor_chip`)
| Field | Type | Notes |
|---|---|---|
| `id` | String | PK |
| `vendorName, productName` | String | |

### `SpectrometerStyle`  (`spectrometer_style`)
| Field | Type | Notes |
|---|---|---|
| `id` | String | PK |
| `styleId, styleName, description` | String | |

### `SpectrometerVendor`  (`spectrometer_vendor`)
| Field | Type | Notes |
|---|---|---|
| `id` | String | PK |
| `vendorName, vendorId` | String | |

---

## 3. Reference — spectral-line master data (**shared**)

### `SpectralLineMasterData`  (`spectral_line_master_data`)
The known emission-line catalog (e.g. mercury lines of a CFL) used as calibration targets.

| Field | Type | Notes |
|---|---|---|
| `id` | String | PK |
| `name` | String | |
| `colorName, mainColorName` | String | colour buckets used by the calibration colour check |
| `nanometer` | Float | the reference wavelength |

---

## 4. Application config — device registration

### `ApplicationConfig`  (`application_config`)
The singleton app configuration row.

Relationship: `spectrometerProfilesMapping` → `[ApplicationConfigToSpectrometerProfile]`.

### `ApplicationConfigToSpectrometerProfile`  (`application_config_to_spectrometer_profile`)
**Join/registration row.** This is what the *Connect spectrometer* dropdown reads, and what the
*Download/Register* action creates. Composite primary key.

| Field | Type | Notes |
|---|---|---|
| `application_config_id` | String | **PK**, FK → `application_config` |
| `spectrometer_profile_id` | String | **PK**, FK → `spectrometer_profile` |
| `isDefault` | Boolean | which registered profile is the active/configured device |

Relationship: `spectrometerProfile` → `SpectrometerProfile`.
⚠️ **Delete hazard:** this row *points at* a profile. Deleting a profile must delete its mapping
row(s) first, and reset the configured device if `isDefault` / currently selected.

---

## 5. Measurement — stub (not implemented)

### `MeasurementProfile`  (`measurement_profile`)  — **unused stub**
| Field | Type | Notes |
|---|---|---|
| `id` | String | PK |
| `spectrometerProfileId` | String | FK → `spectrometer_profile` |

Defined but **never instantiated, persisted, or shown**; the Settings *"Measurement profiles"*
button is not wired, and no view exists. Presumably intended as a *saved measurement* tied to a
spectrometer profile, but never built.

### `DbSpectrum` — **empty stub**, not a mapped entity
`class DbSpectrum(): pass`. Spectra are **not persisted**. At runtime a measurement is the in-memory
`SpectralJob` (a `QObject`) holding `spectraBySampleTypes: { SpectrumSampleType → [Spectrum] }`;
each `Spectrum` carries `valuesByNanometers` (what the graph plots), the burst frames, and a
`sampleType` (REFERENCE / SAMPLE / DARK / BLANK / UNSPECIFIED). All of it is discarded on
navigate-away.

---

## Delete cheat-sheet (deleting a `SpectrometerProfile`)

1. Delete its `ApplicationConfigToSpectrometerProfile` mapping row(s); reset the configured device if it was default/selected.
2. Delete the owned `SpectrometerCalibrationProfile` → its `SpectralLine`s cascade away automatically.
3. Delete the `SpectrometerProfile`.
4. **Leave** all `<<catalog>>` (`Spectrometer`, `…Sensor`, `…SensorChip`, `…Style`, `…Vendor`) and
   `<<reference>>` (`SpectralLineMasterData`) rows — they are shared.

---

# Appendix — Runtime (non-persisted) models

> These are **in-memory** model objects (not DB entities). They are **never written to SQLite** and
> are discarded on navigate-away. Included here for completeness because they are easy to confuse with
> the persisted entities above (e.g. `SpectralJob` vs the `MeasurementProfile` stub).

**Diagram:** [`runtime_models.svg`](runtime_models.svg) — from [`runtime_models.puml`](runtime_models.puml).

## Live — the measurement at runtime

- **`SpectralJob`** (`QObject`, transient) — `title`, and `spectraBySampleTypes : { SpectrumSampleType → [Spectrum] }`.
  This is what the spectrum graphs plot. **Not persisted** — there is no DB write for a measurement.
- **`Spectrum`** — `valuesByNanometers : {nm→intensity}` (the plotted curve), `capturedValuesByNanometers`
  (the burst frames), `colorsByPixelIndices`, and `sampleType`.
- **`SpectrumSampleType`** (enum) — `REFERENCE`, `SAMPLE`, `DARK`, `BLANK`, `UNSPECIFIED`.

## Live — signal payloads (Qt, cross-thread)

| Signal | Carries |
|---|---|
| `VideoSignal` (base) | `image : QImage`, `currentFrameIndex`, `framesCount` |
| `SpectralVideoThreadSignal` *(extends VideoSignal)* | `spectralJob` — emitted per frame by `SpectrumVideoThread` |
| `SpectrometerCalibrationProfileWavelengthCalibrationVideoSignal` *(extends SpectralVideoThreadSignal)* | (calibration video) |
| `SpectrometerCalibrationProfileHoughLinesVideoSignal` *(extends VideoSignal)* | Hough/bounding `QLine`s for ROI detection |
| `DbEntityChangedSignal[E]` → `SpectrometerProfileSignal` | `entity`, `operation : DbEntityCrudOperation` (drives the profile list) |
| `NavigationSignal` | `target : str` (page navigation) |
| `ApplicationStatusSignal` | `stepsCount`, `currentStepIndex`, `text`, `isStatusReset` (status bar) |

## Live — settings

- **`VirtualSpectrometerSettings`** (Singleton) — `virtualCameraImage : QImage` (the "Set picture" frame
  for the virtual device), `doSavePhysicallyCapturedImages : bool`.

## Unwired — `SpectralWorkflow` family (dead scaffolding)

`SpectralWorkflow → SpectralWorkflowPhase → SpectralWorkflowStep` (+ concrete `Acquirement…` /
`AcquireView…` subclasses, the `SpectralWorkflowPhaseType` enum, and the nestable `SpectraContainer`
tree) are **defined but never instantiated by the app** — the classes only reference each other. An
aspirational multi-phase acquisition model never connected to the capture path (parallels the unused
`MeasurementProfile` DB stub). `SpectraContainer` is also visibly unfinished (duplicate `__init__`).
