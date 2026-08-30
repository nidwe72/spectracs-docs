# Spectracs — Roadmap (working backlog)

## ▶▶▶▶▶▶▶▶▶ WHAT TO DO NEXT  *(the standing TODO list — RESET 2026-08-25. Ask "what to do next?" and start here.)*

> ⭐⭐⭐ **`Rv` IS THE VERDICT METRIC. Edwin's decision, 2026-08-25.** Implementing it is the single
> highest-priority item on this page. `SPEC_red_ratio_metric.md` is the contract.
>
> ```
> Rv = 100 · (A[622–627] − A_valley) / (A[565–580] − A_valley)      T = 52,  higher = greener
> ```
>
> ⛔ **`Q%` is what the code ships TODAY and keeps the pill until `Rv` lands and clears M9.** Nothing below
> changes that. What changed is which metric the programme is building toward.
>
> ⭐⭐⭐ **AND SUNFLOWER OIL IS THE SOLVENT.** Same decision, same day. `DOC_sample_physics.md` **§4A** is
> the case; `SPEC_capture_quality.md` §16.12.7g is the technical record.
>
> ⭐ **The two decisions fit each other, and that is not a coincidence.** `Rv`'s threshold `T = 52`
> transfers across solvents **unchanged and with zero errors** — and its margin is **widest in sunflower**
> (+67.9 against isopropanol's +52.0). `Q%` could not have migrated: the same Lugitsch oil reads 13.5–15.5
> in isopropanol, 16.2–16.7 in sunflower and 20.6–20.8 in white spirit. **`Rv` is what makes the solvent
> change cheap; sunflower is where `Rv` works best.**
>
> ⛔ **CHOSEN, NOT MIGRATED.** Isopropanol is still the recipe. §4A.5 lists the three things that must
> happen first — record solvent + bottle in every report, run the preparation arm, and re-run the
> pre-registered test **in sunflower**.

### ⭐⭐⭐ 0 — THE σ_fill RUN  *(NEXT. Set 2026-08-27. One evening. Everything below waits on it.)*

> ⛔⛔ **THE 2026-08-26 SESSION FOUND THE PREPARATION, NOT THE OIL, IS THE DOMINANT TERM.** Six fills
> across three oils, all under the new recipe, first two distinct reads each:
>
> ```
> WITHIN a fill, Rv repeats to   sd 1.36        <- the measurement is fine
> BETWEEN fills of ONE oil        8.4 Rv (Lugitsch, 3 fills)
>                                12.7 Rv (Esterer, 2 fills)     <- ten times the measurement
> pooled sigma_fill               6.30 Rv       <- from 5 fills / 3 df. THAT IS THE PROBLEM.
> ```
>
> **Nothing else on this page can be judged until this number exists.** Not the 08-24 → 08-26 step
> (12.8 Rv — indistinguishable from one evening's fill spread), not the Lugitsch-vs-Esterer separation
> (18.5 Rv, and Esterer's own two fills differ by 12.7), not M9's error count, not the tracker's
> tolerance. Every one of them is currently being compared against a yardstick that does not exist.

**The run.** **Six fills of ONE oil (Lugitsch), one evening, one bottle, one operator**, two distinct
reads each ⇒ 12 runs at ~2.6 min. Aliquots **DARK from the moment they are drawn** (`P6′`).
⛔ **VARY NOTHING ELSE.** Not the oil, not the solvent bottle, not the turnover count, not the order of
capillary clearing. This run measures one thing and it cannot measure it if anything else moves.

⚠ **THE RECIPE HAS CHANGED SINCE THIS ITEM WAS WRITTEN.** From 2026-08-27 (Edwin): **1 ml sunflower +
2 capillaries, which empty themselves — no arm-swing — then ~45 s of FAST rotation at the bottom while
still concentrated, up to 8 ml, ~60 s more, and 4 ml poured into the jar.** The 40 slow inversions are
gone. Two capillaries in 8 ml is the same working strength as one in 4 ml, so `A_Q` and the archive stay
comparable. ⛔ `prepProtocol` in the header is a hardcoded constant and still says
`invert-40-after-capillaries-clear` — **it must be updated before this run** (§0b).

⭐⭐ **AND IT CHANGED AGAIN 2026-08-28 — the rotation is now a VORTEX MIXER.** The whole recipe, with its
volume rule and the two cautions on the ultrasonic bath, is written down at last in
`SPEC_capture_quality.md` **§16.23.2b**; §16.23.2's inversions are marked superseded. ⛔ The **sonication**
step is NOT adopted — it is on trial and gated by two fills vortex-only against two vortex+sonic, run
*before* this evening. ⭐ For the σ_fill run itself nothing else moves: Lugitsch at the 4 mL default lands
the DN guard at **40.7 DN**, mid-window (§16.23.10j), so there is no dosing question to settle first.

⭐⭐ **THE 8 ml PREP MAKES THE BATCH BIGGER THAN THE READ — USE THE OTHER HALF.** Pour the remaining 4 ml
into a second jar and measure it too. Same preparation, two jars ⇒ it **partitions σ_fill into its two
possible homes**: preparation-and-dilution against jar-and-beam. If the split-batch pair agrees while
independent fills scatter, the variance is in the dilution and no mixing device is needed; if both
scatter equally, the preparation is exonerated. It costs one extra read per fill and nothing else.
⚠ Keep the waiting half **DARK** (§39: +1.34 `Rv` from light on a waiting aliquot) and rotate again
immediately before the second pour — §6.6 found the prep stratifies.

⭐ **BRACKET THE EVENING.** Measure fill 1 again at the END. The 2026-08-26 session drifted 77 → 107
across nine hours with oil perfectly confounded against time-of-evening, and afterwards there was no way
to separate drift from oil. One extra fill turns that into a measurement.

### ⭐⭐⭐ 0-METRIC — `RvLin` IS THE WORKING CHOICE FOR THE CONTINUOUS NUMBER  *(Edwin, 2026-08-29)*

> ⭐⭐ **`Rv` keeps the VERDICT. `RvLin` becomes the candidate for the NUMBER** — the history tracker, and
> anything reading `Rv` as a quantity rather than as a side of a threshold. **The boundary is the
> decision.** Nothing about the shipped classification changes: `Rv` got all seven fills of 2026-08-28/29
> right with nothing within 25 units of the line.
>
> ```
> RvLin = 100 · (A622-627 − B(624.5)) / (A_Q − B(572.5))
>         B = the straight line through (530, A_valley) and (613.5, A612-615)
>         T = 58.0   pre-registered 2026-08-29, same-jar recipe   (SPEC_metric_research §16.10.2)
> ```
>
> ⭐ **The one-sentence case:** with `Δ = A[612–615] − A_valley` = 0 it is `Rv` **exactly**. It is not a
> rival metric — it is `Rv` with the baseline's slope *measured* instead of assumed, and it is the only
> candidate that is both affine-invariant and short-lever (11 nm, against `RvCont`'s 20.5).
>
> ⛔ **CHOSEN, NOT BUILT** — as §15 was for `Rv`. Threshold fitted on four fills of one night, the
> pre-registration unrun, and the whole ranking comes from same-jar fills whose blank is still in
> question (§0-JAR). `SPEC_metric_research.md` §16.11 carries the argument and the three things that
> would overturn it.

### ⭐⭐⭐ 0-NEXT — THE NEXT LAB EVENING, and it is pre-registered  *(set 2026-08-29, after the six-fill session)*

**Four fills, two reference runs, one evening.** Thresholds are **already fixed** in
`SPEC_metric_research.md` §16.10.2 and may not move after the numbers are seen.

| # | what | why it is there |
|---|---|---|
| **0** | ⭐ **Two-reference run** — `R₁ₐ`, `R₁ᵦ` (same jar re-seated), `R₂` (second jar); 4 mL pure sunflower each, **no oil**, exposure untouched | §0-JAR below. Ten minutes. The jar term, and the session's start state |
| 1 | **Lugitsch · same-jar** | a green oil `RvLin` has never seen, against the pre-registered `T` = 58.0 |
| 2 | ⭐⭐ **Lugitsch · TWO-JAR** | **same oil, same evening, only the reference method differs** — the one thing the archive cannot supply, and what decides which blank is right |
| 3 | **Billa Clever · same-jar** | holds the brown end so the corridor is measured, not assumed |
| 4 | **Lugitsch · same-jar again** | a second fill of the same oil — supplies the replicate pair the metric test needs. ⛔ **NOT a drift measurement** |

> ⭐⭐ **ALTERNATE THE OILS — L · B · L · B, not L · L · B · B.** This is the highest-value change to the
> plan and it costs nothing but reordering. On 2026-08-29 the session ran Esterer · Esterer · Esterer ·
> Billa · Billa, so **"later" and "brown" were the same axis** and the drift could not be told from the
> oil. Alternating makes the ramp hit both classes equally and cancel out of the comparison — and it works
> **whatever the drift turns out to be**, temperature or not.
>
> ⛔ **THIS IS SESSION PROTOCOL, NOT RECIPE.** The miller never does it, and it must not leak into
> `SPEC_capture_quality.md` §16.23.2b. The *recipe* — capillary, volume, vortex, box, 6 min — is what
> ships; the *session protocol* — alternating, the bracket, the reference runs, the running order — exists
> only to stop OUR comparisons being confounded. The two had been muddled.
| **4a** | ⭐ **RE-READ fill 1**, kept dark and capped all evening | two minutes, no new fill — and it is what makes the drift separable at all |
| **5** | **Two-reference run again** | does the blank drift across an evening? §16.8a says something does |

⚠ **Two reads per fill, times logged, order kept and written down.** With a drift of this size the order
is data.

> ⭐ **AND SETTLE IN A POLYSTYRENE BOX, not under a bedsheet.** No heater, no thermostat, nothing to plug
> in — the box does not *hold* a temperature, it slows the room's change from hours to most of a night.
> ⭐⭐ **Keep the solvent bottle and the clean jars in it**, because six minutes in still air will not
> equilibrate a jar (still-air time constant ≈ 8 min for 4 mL) — conditioning the inputs removes the lag
> instead of fighting it. `SPEC_capture_quality.md` §16.23.2b step 7.
>
> ⭐⭐⭐ **AND LOG THE SOLVENT / ROOM TEMPERATURE WITH EVERY FILL. One number, a thermometer and a pen.**
> `SPEC_capture_quality.md` **§16.38**: across 2026-08-29's six fills the scatter floor per unit pigment
> **more than doubled in 2½ hours, linear against the clock (r = +0.981)** — straight through an oil
> change, at constant dose, with the **reference leg flat**. Edwin ruled the jar, the bottle and the recipe
> out from the bench, and a fresh jar every fill kills the residue mechanism outright.
>
> ⭐ What is left that varies linearly with the clock on a 03:30 → 06:00 session is **the room**, and this
> system's temperature response is the best-measured effect in the whole spec: §16.36 — **heat CLEARS the
> fill, cooling re-clouds it, cloud point 35–40 °C.** The sign is right, and it is the only candidate that
> predicts a **falling sample leg with a flat blank**.
>
> ⛔⛔ **It is recorded NOWHERE** — not the header, not `monitorRecord`, not the archive. Every multi-hour
> session since §16.36 has been running an uncontrolled temperature experiment underneath every fill.
> ⇒ **`F`, `A_valley` and every σ_fill from a long session carry an unknown temperature term until it is
> logged.**
>
> ⭐⭐ **The one-evening test, if you want it settled rather than watched:** two fills at the start and two
> at the end, with the late pair's solvent **warmed back to the early pair's temperature** before dosing.
> Ramp disappears ⇒ temperature. Ramp survives ⇒ the clock and something else.
>
> ⚠ **AND IT IS A LAB PROBLEM FOR THE VERDICT, A PRODUCT PROBLEM FOR THE TRACKER** (§16.38.5). Scaled to
> the miller — one measurement, ~20 min — the drift is worth **~1.5 units against a 45-unit corridor**,
> i.e. invisible.
>
> ⭐⭐ **AND THE OPERATING RANGE IS SPEC'D: 22–28 °C, bureau or office, NOT the mill floor** (Edwin,
> 2026-08-29 — §16.38.6; it appeared nowhere in the docs before). ⛔ An earlier draft of this item argued
> from "January in a cold mill against July", a 15 °C swing. **Wrong product.** The range is **6 °C**, and
> at an estimated 1.5–3 `RvLin` units per °C that is **9–18 units**: ✅ fine against the verdict's 45-unit
> corridor, ⛔ still a problem for a tracker needing 4–6.
>
> ⭐ A specified range is a **control**, not only a constraint: the instrument can **refuse outside
> 22–28 °C** (a guard in the DN-guard family), the tracker can **refuse to compare across more than n °C**,
> and any correction only has to span 6 °C. ⇒ still add `temperatureC` to §0b's header list.
>
> ⭐⭐ **And 2026-08-29 is close to a WORST case** — 03:30–06:00 in an unheated room, against a miller
> measuring in working hours in a heated office. The drift measured is **larger than the product will
> normally see.**

⭐ **If the evening runs short, protect step 2.** Every other fill can be repeated another night; a
matched same-jar / two-jar pair on ONE oil in ONE evening is the only thing that resolves the blank
question, and without it every number from the new recipe stays unplaceable against the archive.

> ⛔ **STEP 4 IS NOT A DRIFT MEASUREMENT — corrected 2026-08-29 before the session ran.** Steps 1 and 4 are
> two DIFFERENT fills, so their difference is `drift + fill scatter`, and that same difference is already
> the σ_fill estimate — using it for both would be circular. ⭐ **Step 4a is what makes the set solvable:**
>
> | | what is compared | what is allowed to differ |
> |---|---|---|
> | **X** | step 0 vs 5 — the reference runs | the clock only, **no oil anywhere** |
> | **Y** | **step 1 vs 4a — fill 1 read twice** | the clock **+ the sample sitting there** |
> | **Z** | step 1 vs 4 — two fills of one oil | + **the fill being made again** |
>
> ⛔ **IN QUADRATURE, not linearly:** `σ_fill = √(Z² − Y²)`, not `Z − Y`. With `Z` = 4.44 and a
> hypothetical `Y` = 3.0 those give **1.44** and **3.27** — a factor of two apart, on the number the whole
> history-tracker case rests on. ⚠ Which form applies is itself a finding: a monotone trend subtracts
> nearly linearly, random scatter in quadrature, and the shape of the two reference runs says which.
>
> ⭐⭐ **`Y` HAS NEVER BEEN MEASURED** — no run in the archive reads one jar at the start of an evening and
> again at the end. If `Y` ≈ 0 then 4.44 is real fill scatter as assumed; if `Y` ≈ 4.4 then **σ_fill ≈ 0**,
> the fills are essentially perfect, and everything called σ_fill for weeks is the session drifting.
> ⛔ Not a silly hypothesis: `20260828EstererF/002` repeated to four decimals, and `A_valley` climbed
> monotonically across ten runs and two oils the same night.
> ⚠ Keep fill 1 **dark and capped** — §39 measured +1.34 `Rv` from light on a waiting aliquot — and note
> its jar must survive the whole evening untouched, which competes with step 2 if jars are short.

**What it returns:** the jar term and its drift · a direct method comparison · **a drift measurement that
is actually separable (0/5, 1-vs-4a, 1-vs-4)** · `RvLin`'s first out-of-sample oil · a second brown fill.

> ⭐⭐ **THE RECIPE THAT PRODUCED THIS.** Same jar for reference AND sample — 4 mL solvent into the
> measurement jar, capture the reference, *then* dose with the capillary — vortex, **no ultrasonic bath**,
> **6 minutes standing in the dark**. `SPEC_capture_quality.md` §16.23.2b is the written recipe;
> `SPEC_metric_research.md` §16.8 is the evidence. It gave the steadiest fill ever recorded
> (`20260828EstererF/002`: 0.003 %/min, `F` and `A_valley` repeating to four decimals).
> ⛔ **NOT YET ADOPTED** — the session drift is present in the same-jar fills and absent from the two-jar
> pair, and method is confounded with clock. Step 2 breaks that confound.

### ⭐⭐ 0-JAR — THREE CAPTURES, NO OIL, TEN MINUTES. DO IT BEFORE THE σ_fill RUN.  *(2026-08-29)*

**The cheapest experiment on this page, and it may be the most important.** Every absorbance in the
archive is `−log10(S/R)` where `S` and `R` come from **two different jars**. `SPEC_capture_quality.md`
§16.9.3h prices *re-seating one jar* at 2.81 % and two jar operations at 8.4 % — but it never measured
the **jar-to-jar** term, jar #1's own glass and ring pattern against jar #2's, because every disturbance
probe used a single jar.

⭐ **And the sample cancels**, so no oil is needed:

```
A1 − A2 = −log10(S/R1) + log10(S/R2) = log10(R1/R2)
```

1. Jar #1, 4 mL pure sunflower, seat, capture → **R₁ₐ**
2. Lift jar #1, replace it, capture → **R₁ᵦ**   ⇒ `log10(R₁ₐ/R₁ᵦ)` = **re-seating alone**
3. Jar #2, 4 mL pure sunflower, seat, capture → **R₂**   ⇒ `log10(R₁ₐ/R₂)` = **jar-to-jar + re-seating**

⭐⭐ **Run it at the START of a session and again at the END** — that also measures whether the blank
drifts across an evening, which §16.8a shows something does.

⚠ **Controls:** exposure untouched across all three, and the same fill level in both jars (§16.9.3h
prices jar *level* separately at 1.68 %, additive and non-cancelling).

**Why it blocks:** Edwin's same-jar method (`SPEC_metric_research.md` §16.8) removes the jar-to-jar term
by construction, and its six fills of 2026-08-28/29 read **systematically differently** from the archive —
the two blank shapes differ ~11 % at the Q band, worth 0.060 in `pq` against a within-method scatter of
0.007. ⛔ **One of the two blanks is wrong, possibly both**, and until that is known no fill made either
way can be placed against the other. That includes the σ_fill run's own numbers.

⇒ If a structured (λ-dependent) jar term is found, it cancels in **no** metric in the `Rv` family and
would be the largest single finding of the programme. If it is flat or absent, the archive stands and the
same-jar method becomes a free improvement.

### ⛔⛔ 0a — ONE REFERENCE PER FILL. NON-NEGOTIABLE.  *(2026-08-27 — the finding that changes the design)*

**The 2026-08-26 session used TWO references for sixteen runs across nine hours**, and the fills split by
reference epoch exactly where the numbers change:

| epoch | reference taken | fills | `Rv` |
|---|---|---|---|
| **1** | before 17:21 | Esterer A · Stekko A · Lugitsch A · Lugitsch B | 73–102 |
| **2** | ~00:26 | Lugitsch C · Esterer B · Esterer C? · **Esterer D** | 85–110 |

⭐ **Every `Rv` above 105 is in epoch 2. Every Esterer below 80 is in epoch 1.** Lugitsch's jump
(99.9 → 107.2) and Esterer's (77.5 → 107.4) both straddle that boundary.

⭐⭐ **AND THE LAMP TILT IS NOW MEASURED, NOT INFERRED.** The report stores the whole `REFERENCE` curve
(1634 points), so the two epochs can be divided one by the other. The lamp lost ~8 % overall — **and not
evenly**, normalised at the Soret:

```
   440 nm  -8.9 %        <- sags
   480      +2.4 %
   540      +0.4 %        the body of the spectrum holds
   600      +0.5 %
   572      -1.9 %       <- Rv's DENOMINATOR
   624      -2.8 %       <- Rv's NUMERATOR
   632      -5.1 %       <- and it accelerates into the clamp
```

⛔ **The red end fades faster than the middle, and 624 nm is already the dimmest part of the lamp** —
41.9 counts against 130.7 at the Soret. A −2.8 % relative loss lands on the weakest channel `Rv` has,
which is also the one with **no reference channel in the monitor record**.

⚠ **A re-captured reference CANCELS a lamp change** — `A = log10(ref/sample)`, so a tilt divides out when
both are taken under the same lamp. It survives only when the lamp moves **between** the reference and
the sample. ⇒ the fault is not that the lamp drifts; it is that **one reference served four fills over
two hours**, so `20260826EstererD` (02:17) was read against a reference taken at 00:26.

⇒ **THE σ_fill RUN MUST TAKE A REFERENCE BEFORE EVERY FILL.** Without it the session measures fill
variance and lamp drift as a single number, which is exactly what 2026-08-26 did — and why that evening
cannot say whether a 30 `Rv` Esterer spread is preparation or bench.

⭐ **Two more blocking terms fall out of the same argument:**

* ⛔ **Time and fill are confounded unless the order is broken.** Tonight `Rv` rose monotonically with the
  CLOCK in both oils — **+1.34 `Rv`/h for Lugitsch, +2.78 for Esterer**, `r = +0.90` and `+0.89`.
  Concentration correlates too, but with **opposite signs** in the two oils, so it cannot be the common
  cause; the clock can. ⇒ **re-measure fill 1 at the END of the evening.** If it has moved, the drift is
  the session's, not the fills'. ⚠ n = 3 fills per oil — the leading hypothesis, not a result.
* ⭐ **Add the 622–627 reference channel first** (§53.5). It is a few lines, and it turns "the red end may
  have drifted" into a number on every run instead of an archaeology exercise on two stored curves.

| readout | decision it settles |
|---|---|
| **σ_fill for `Rv`** (5 df, not 3) | the yardstick everything above is missing |
| σ_fill for `Q%` on the same fills | free — both metrics come off one trajectory |
| **is `±20` earned?** | σ_fill ≤ 4 ⇒ ±20 is ≥ 5σ, the coarse tracker ships. σ_fill ≥ 10 ⇒ the tolerance must widen and the tracker's CLAIM must narrow with it |
| the first `clearing-4.0` σ_fill | the sunflower read has never been measured against replicates |

⭐⭐ **WHY ±20 AND WHY "COARSE" IS THE HONEST WORD.** Against σ_fill = 6.30, a ±20 alarm sits at 3.2σ —
it catches Lugitsch→Stekko (28.7 Rv, 4.5σ) and Lugitsch→Spar (69.5 and 77.2 Rv, 11–12σ, and the 08-24
photographs show that difference plainly by eye). It **cannot** see Lugitsch→Esterer (18.5 Rv, 2.9σ), two
oils that ARE distinguishable by eye. ⛔ A UI that lets "no alarm" read as "unchanged" is wrong by an
Esterer-sized margin. The coarse job survives even the recipe change: 12 Rv against a ~70 Rv signal
leaves ≈ 5σ of headroom with nothing corrected.

⚠ **The number 20 is not yet earned.** With 3 df the confidence interval on σ_fill runs roughly 4–13 Rv;
at the top of that range ±20 is 1.5σ and false-alarms. This run is what earns it or replaces it.

⛔ **This SUPERSEDES item 4's five-fill evening and sharpens it**: one oil, six fills, nothing else
varied. Item 4's preparation arm (item 5) is the NEXT evening, not this one — mixing two questions into
one session is how the 08-26 session ended with three explanations and no way to separate them.

### ⛔⛔ 0b — RECORD THE INSTRUMENT STATE IN THE REPORT HEADER. BEFORE THE σ_fill RUN.  *(2026-08-27)*

> ⚠ **PARTLY DONE 2026-08-30 — AND THE PART THAT MATTERS MOST IS STILL OPEN.** Two header defects were
> fixed (`spectracsPy` `60c5129`): `timestampIso` is now stamped at the **measurement** rather than at Save
> — it had been `null` in *every report in the archive*, so runs could not be put in time order at all —
> and `prepProtocol` is resolved **per run** by `PrepProtocolResolver` from `prepProtocol.txt` in the app
> data directory instead of being compiled in. `SPEC_metric_research.md` §16.15.4 records both.
> ⛔⛔ **THE EXPOSURE IS STILL NOT IN THE HEADER**, which is what this section actually asks for and the
> one instrument setting known to move the verdict by −13.5 %. **§0b is NOT cleared, and it still gates
> the σ_fill run of `SPEC_metric_research.md` §16.16.**

**The exposure is not written down anywhere.** Verified today against `20260826EstererB/001` and
`20260826EstererE/001`: the header carries `solvent`, `prepProtocol` and `captureDecode` — and nothing
about the camera. Yet `SPEC_capture_quality.md` **§16.24.1** is the finding that a single exposure change
moved the verdict **−13.5 %**. ⇒ **the one instrument setting known to move the number is invisible in
the travelling record**, exactly like the missing red reference channel of §0a.

⭐⭐ **AND THE ARCHIVE ALREADY CONTAINS SEVERAL INSTRUMENT STATES THAT NOBODY CAN NAME.** The only
retrospective handle is the stored `REFERENCE` curve's own peak:

```
⛔ THESE ARE LINEAR VALUES, NOT DN — corrected 2026-08-28, see item 3 below.
session                 reference peak     at nm     A624      far red 632-636   ENCODED peak DN
20260822Lugitsch              221.3         473.3     46.8        30.8                239.1
20260824 (all three)          226.1         473.3     44.7        28.1                241.4
20260826 A/B/Stekko           217.5         473.3     41.2        25.7                237.2
20260826 C / EstererB-D       203.7         473.6     36.8        22.1                230.2
20260826EstererE              199.4         473.3     34.4        20.3                228.0
```

⛔ **The far red fell 28 % in three days while the peak fell 12 %** — the red end degrades at twice the
rate of the lamp overall, and that is where `Rv`'s numerator lives. ⚠ **But it is not near a floor**:
§16.23.10j measures the *sample* minimum inside 622–627 across the sunflower era at a median **86.3 DN**,
worst non-opaque run 71.4. The red end is degrading from a comfortable height, which makes this a
**trend to watch, not a measurement at risk**. Whether any of this is exposure,
lamp ageing or re-seating **cannot be recovered**, because only one of the three was ever logged.

**What to add**, beside `captureDecode` in the report header — the `captureDecode` precedent applies, so
this is a travelling record and needs **no Alembic migration**:

| field | why |
|---|---|
| **exposure** (and gain / white balance if separately settable) | §16.24.1's 13.5 %. Without it no run can be compared to another across a settings change |
| **`referencePeakDn` + its wavelength** | the headroom to clipping, in one number, on every run. A clipped reference is a ruined run and nothing currently says so. ⛔ **ENCODED DN, not the linear trace's peak** — reading one for the other is what produced the withdrawn item 3 below |
| **`referenceRed` = mean 622–627 of the reference** | §0a / §53.5. `Rv`'s numerator is the only band with no reference channel in `monitorRecord` |
| **`prepProtocol` must MOVE WITH THE RECIPE** | it is a hardcoded constant. `20260826EstererE` was made two-stage and the header still claims `invert-40-after-capillaries-clear`. A stale recipe string is worse than none |
| ⭐⭐ **`temperatureC`** — the solvent/room temperature at the fill | ⛔ **added 2026-08-29.** §16.38 makes it the largest systematic in the data and it is recorded NOWHERE. §16.38.5 makes it a **product** requirement, not only a bench habit: the tracker compares January against July, a 15 °C difference, and needs 4–6 unit precision to do it |

⭐⭐ **WHY THIS BLOCKS THE σ_fill RUN AND NOT MERELY IMPROVES IT.** That run is also the **baseline for the
history tracker** (§0a's ±20 question). A baseline is a claim about an oil *measured on a particular
instrument*; anchored to a camera state nobody can reconstruct, it cannot be compared to anything six
months later, and the evening has to be repeated. Recording four fields is a few lines against an
evening of bench time.

⭐ **The exposure recommendation, once it is logged** — in this order, and not before:

1. ⛔ **Do not change exposure during the σ_fill run.** That run measures preparation variance; an
   instrument change inside it makes it uninterpretable. One variable.
2. ⭐⭐ **THE A/B HAS ALREADY RUN — corrected 2026-08-27.** §16.24.1's *"exposure does NOT cancel"* was
   bounded long ago by **§16.27.1** (`M448` moved **+0.04 %** across a 104/90 step on one fill, three
   re-seats), and §16.27.1a now computes the same control **for `Rv`**: `20260807B/001` at exposure 104
   reads **32.17** against B/002 at exposure 90's **32.08** — **0.09 `Rv`** — while the two runs held at
   the *same* exposure differ by **4.38**. **The exposure effect is an order of magnitude below the
   re-seat noise**, and structurally so: §16.27.3 localises the whole exposure sensitivity to 440–447 nm,
   and `Rv` never touches the Soret. ⚠ The tested step is ×1.156; a larger one is not covered.
3. ⛔⛔ **WITHDRAWN 2026-08-28 — THERE IS NO FREE HEADROOM, AND THE STEP WOULD CLIP.** This item read
   *"`20260826EstererE` peaks at 199.4 of 255, so ~28 % of the range is currently unused"* and recommended
   exposure **×1.15**. ⛔ **199.4 is the LINEAR peak, not DN.** The report stores the linear 0–255 spectrum
   and the guard encodes once — `DN = 255·(linear/255)^(1/2.2)` — so that reference actually peaks at
   **228.0 DN**, and the sunflower era spans **228.0–243.7 DN, median 237.2**, against an AE target of 245
   and a clip at 255. **A ×1.15 step puts 37 of 37 sunflower references at or past 255.**
   ⇒ **The exposure is already where this item wanted it. Change nothing before the σ_fill run.**
   `SPEC_capture_quality.md` §16.23.10j is the audit; `diagnostics/dn_guard_audit.py` recomputes it.
   ⭐ Items 1, 2 and 4 are untouched — the A/B bound and the 473 nm rule stand; what is gone is the *room*.
   ⭐⭐ **And it sharpens this section's own headline**: the retrospective handle §0b proposed — the stored
   reference peak — was read in the wrong units by the section proposing it. ⇒ record
   **`referencePeakDn`, already ENCODED**, and name the field so it cannot be confused with the linear trace.
4. ⛔ **Leave the 473 nm spike alone.** It is the single binding constraint — letting it clip buys a
   further ×1.44 — but it sits **12 nm** from the Soret band `Q%` and the domain guard depend on, and
   §16.24.8 already documents leakage from that line *unsaturated*. Take that gain **optically** (amber
   filter, or a lower blue drive) as part of `SPEC_lamp_rebuild.md`, where a full `Rv` re-validation is
   already budgeted under §6.7.

### ⭐⭐⭐ 1 — IMPLEMENT `Rv`  *(highest priority; desk work, no rig time)*

> ⛔⛔ **THE BASELINE IS AN OPEN TERM — added 2026-08-29.** Scored over 124 labelled runs in three solvents,
> **`Rv`'s green and brown OVERLAP by 11.5 units** at its best single threshold (1 error). Two coherent
> alternatives — `R` (§12) and `RvCont` (a fitted continuum) — both reach **0 errors with a positive
> corridor** at one shared cut. `SPEC_metric_research.md` **§16** is the write-up,
> `diagnostics/red_anchor_ab.py` reproduces every number, and `SPEC_red_ratio_metric.md` **§13** carries the
> summary in `Rv`'s own spec.
>
> ⚠ **This does NOT change the build order.** Every cut in §16 is fitted on that corpus and any swap costs a
> full M9 re-registration. **P2 still ships `Rv` beside `Q%` and moves no verdict** — that is exactly why it
> was scoped that way. What changes is that **P6, the gauge and `RV_THRESHOLD`, must not be reached without
> §16 having been answered**, because a threshold is a claim about a baseline.

| phase | what | gate |
|---|---|---|
| **P0** | promote the session scripts to `diagnostics/red_ratio_archive.py`; ✅ the `sorted(os.walk(...))` bug is already fixed | — |
| **P1** | pull the raw frames of `20270729B/002` — the one archive run `Rv` misclassifies | — |
| ⛔ **P1a** | ⭐⭐ **NEW 2026-08-29 — read `SPEC_metric_research.md` §16 BEFORE P1.** That run is not a raw-frame problem: it is a **baseline** problem. Under a fitted continuum it reads 65.5 against a cut of 58.6 and is classified correctly, and the same change takes `Rv`'s isopropanol corridor from **−11.5 (overlapping) to +18.0** with 0 errors on all 88 runs. ⇒ P1 may be looking in the wrong place | — |
| **P2** | ⭐ **`Rv` + two metric rows + band marker (6) at 622–627 on `Absorption (bands)`. NO gauge.** Moves no verdict, so it can ship immediately | P0 |
| **P3** | record the finding in `SPEC_metric_research.md` §12 as `R`'s valley-referenced sibling | P2 |
| **P6** | the gauge and `RV_THRESHOLD`, **only after M9 (item 3) passes** | P4, P5 |

> ⭐⭐⭐ **THE THRESHOLD AND THE NUMBER ARE TWO DIFFERENT PRODUCTS — 2026-08-29.** The first brown
> replicate under the settled recipe (`20260828BillaCleverA` vs `B`, same oil, 32 min apart) moves `Rv`
> by **36.3 units**, four times its worst green-pair scatter. ⭐ And it costs the verdict **nothing**: all
> seven fills of that night classify correctly against `T` = 52, greens 83–94 and browns −10 and 26, with
> nothing near the line.
>
> ⇒ **`Rv` keeps the verdict.** ⛔ But it cannot carry `SPEC_history_tracker.md`, which needs `Rv` as a
> *continuous* quantity. `SPEC_metric_research.md` §16.9 makes **`RvLin`** the leading candidate for that
> job — same brown pair, 6.3 units — on the figure of merit that matters, gap ÷ worst-case replicate
> scatter: **7.17 against `RvTest` 6.33, `RvCont` 3.53 and `Rv` 1.56**.
>
> ⚠ **Not a decision.** `RvLin` loses the archive-wide hold-out (6/36 against `Rv`'s 0/36), its threshold
> is fitted, and §0-JAR's blank question is open — the fills that produced this ranking use the same-jar
> reference and cannot yet be placed against the archive. ⇒ **the history tracker stays blocked**, but it
> is now blocked on a *named candidate and one experiment* rather than on not having a metric at all.

⭐ **P2 is the only phase that touches shipping code before a lab session, and it moves no number.**

### ⛔ 2 — THE THREE DESK ITEMS THAT STILL BLOCK RIG TIME

| # | item | why it is still here | where |
|---|---|---|---|
| **2a** | ⛔⛔ **The clear-case read** | **the only item that fixes something currently WRONG.** `readAs = FIRST_SETTLED_WINDOW` returns the LAST look, not the first — up to **0.482 units**, one-directional, and it hits the GOOD fills. ⭐ It gates σ_fill for `Rv` exactly as it gated it for `Q%` | `SPEC_settled_measurement.md` §29 · ⚠ §29.6's trap: 2σ = 0.13 holds only at W = 60 — derive it from the window |
| **2b** | **M3 — the `A_valley` ceiling** | metric-independent, and **`Rv` needs it MORE than `Q%` did**: `A_valley` is `Rv`'s datum, subtracted from both terms | this file, M3 · `SPEC_settled_measurement.md` W3 |
| **2c** | ⏳ **W8 — record `A(563–573)` and `A(623–626)` per frame** | ⛔ **NOT RETROACTIVE.** ⭐ **Add `A(622–627)` to it** — that is `Rv`'s own numerator, and without it no `Rv` trajectory can ever be reconstructed | `SPEC_settled_measurement.md` §52.7 |

### ⭐ 3 — M9, THE PRE-REGISTRATION — now for `Rv`

Freeze `SPEC_red_ratio_metric.md` §3's constants **by committing the spec**, then measure fills that did
not exist when it was written. ⛔ The four fitted degrees of freedom are declared in §7.2: the red window,
`T = 52`, the pedestal subtraction, and the domain band. **≥ 12 fills, ≥ 3 oils × ≥ 2 fills, eye-labelled
BEFORE the number is read.** Fails if > 1 error or the threshold has to move.

### ⭐⭐ 4 — ONE LAB EVENING, and it now buys more than it used to

**Five fills, aliquots DARK from the moment they are drawn** (`P6′`), *plus* the four-fill preparation
arm below. One session returns σ_fill for **`Rv` and `Q%` together**, the pre-registered test, the first
recorded trajectory, and item 2a exercised on real fills.

### ⭐ 5 — THE PREPARATION ARM  *(new 2026-08-25 — now a SOLVENT-MIGRATION gate, not just a nicety)*

⛔ **This is now item 2 of §4A.5's migration checklist.** Sunflower is viscous, and the preparation
**stratifies**: the second half of a prepared sample carries **15–45 % more absorber** than the first.
`Rv` absorbs that (a 40–45 % dose swing moves it < 0.8 %) but nothing level-reading does.


`Rv` **is** turbidity-sensitive, and the slopes have **opposite signs**: +105 Rv per unit turbidity on a
brown oil, −108 to −148 on a green one. Turbidity does not create the separation — it **erodes** it from
both sides. That is the whole of SparSBudget's 11.3-unit wobble on 2026-08-24.

> ⭐ **2026-08-26 corroborates the OPPOSITE-SIGNS claim, and shows why pooling hides it.** Across all 16
> sunflower runs of that evening `Rv` vs `A_valley` is **r = −0.07** — apparently no dependence at all,
> because oils with opposite slopes cancel. Split by oil it reappears: Lugitsch's three fills give **+111
> Rv per unit**, Esterer's two give a **negative** slope. ⛔ A session-wide correlation is therefore
> **meaningless** for this question and must never be quoted as one — an hour was lost to exactly that.
> ⚠ Lugitsch's sign is POSITIVE where this section predicts negative for a green oil; n = 3 fills, so it
> is a discrepancy to resolve in the preparation arm, not a refutation of either.

⇒ One SparSBudget preparation, four ways in one session: shaken/immediate · shaken/fixed 10 min ·
filtered · sonicated 5 min. ⭐ **The hardware for the last arm arrived 2026-08-28** (vortex mixer +
ultrasonic bath) and `SPEC_capture_quality.md` §16.23.2b specifies how both are used — heater off, lid
closed, de-gassing rather than dissolution, since §14.2's scatter floor survived 300 s of ultrasound. **The readout:** if turbidity falls and the pigment bands hold, filtration or
a centrifuge is safe — adopt it. If the bands fall with it, you are removing pigment, and sonication is
the right route instead. ⛔ Buy nothing until this has run.

### ⛔ DELIBERATELY NOT NEXT

| | why not |
|---|---|
| **more metric candidates** | §13.1 — one axis, and the decision is made. `dQ100` scores marginally better on the archive and is **not** being adopted: `Rv` wins on being drawable on the existing plot, guard-compatible, and physically interpretable |
| **the lamp rebuild** | ⛔⛔ **now a `Rv` BLOCKER, not just a queue item.** §6.7: on the archive's own diffuser A/B, 2 of 5 blurred runs of a GREEN oil read brown with **both guards passing**, and no cheap guard can catch it. Any optical change forces a full `Rv` re-validation |
| **the red extension** | four arguments point at it, and §6.6 adds a fifth — but it must FOLLOW M9 |

> ⭐ **If only one DESK thing gets done: P2.** `Rv` printed beside `Q%`, no gauge. It moves no verdict,
> needs no lab time, and every run from then on accumulates the number the programme is being built
> around.
>
> ⭐⭐⭐ **If only one thing gets done AT ALL: item 0, the σ_fill run.** P2 accumulates a number nobody
> can yet interpret; item 0 is what makes it interpretable. *(2026-08-27)*

---

### ✅ DONE 2026-08-24 — the colour pass, and a solvent thread that came out of it

⚠ **None of it changes the four desk items above.** Colour never fed a verdict, and the metric work is
untouched. What it adds to the backlog is two rig items, both cheap, neither needing its own evening.

#### Shipped

The colour chips rebuilt end to end (P0–P8): **HSL out of the readout**, **one CIE path for every chip**
— the 2026-08-23 red-tail fix had reached **one chip of eleven** for a year — **eleven chips down to
four** with the `×3 path` chip first, an out-of-gamut marker in both renderers, and **all 203 archived
reports re-rendered onto them**. 583 tests green, `SDK_VERSION` unchanged, five repos pushed.

`SPEC_color_retrieval.md` §7.15 is the contract · **`Spectracs_ColourGeometry.pdf`** *("From Spectrum to
Colour", 30 pp)* is the documentation · the pre-2026-08-24 reports are an exact 205-file mirror at
`spectracs-references/tmp/oldPdfs/`, ⛔ which sits INSIDE the archive root and is therefore excluded in
five walkers — anything else added under `tmp/` needs the same treatment or every archive statistic
doubles.

#### ⭐⭐ The one result

**θ_W = 244.06 ± 1.25° across all 88 archive runs** — green and brown alike, *d* = −0.02. The absorbed
direction is a **constant of the pigment family**. That is why the chips cannot discriminate, and it is
chemistry rather than software: what differs between oils is the *radius*, which is exactly what a
dilution-invariant chip discards. **Colour is a visual aid — measured now, not merely suspected.**

#### ⛔ What died on the archive, which is the useful part

| claim | how it died |
|---|---|
| purity discriminates | *d* = 0.56, ranges overlapping, 88 runs |
| dominant wavelength discriminates *(proposed as purity's replacement)* | undefined on **31 %** of runs; **r = 0.923 with the capture's blue edge** — it reports where the measurement starts |
| "LCh everywhere" improves the hue | *h_ab* 5.60° against the retired HSL's 4.15° — **worse than what it replaced** |
| turbidity explains the solvent difference | cutting the turbid runs makes the separation **worse** |
| a scattering floor explains the red peak | wrong asymmetry — it bites the Soret, not the red |
| resolution loss explains it *(proposed as that one's replacement)* | **convolution conserves area**, and the area is 16.6× down |

⭐ Two survived: the white-point complement's hue re-measured at **2.50°** against the `+180°` flip's
**52.7°** — better than the original claim on both sides — and the "transparent above 636 nm" red tail,
which reproduced byte-identically across the rewrite.

#### ⭐ The solvent thread — from a bench observation, not from the data

Edwin: the two oils separate by eye in sunflower, and the red peak grows in **both** sunflower and white
spirit. Both true, and the archive could not have seen either — `T = S/R` divides the solvent out by
construction (`SPEC_color_retrieval.md` §7.16.1).

**Sunflower carries it alone**: `area(624)/area(Soret)` = **13.6×** isopropanol at matched dose, no
overlap, weakest sunflower fill still 3.2× the strongest isopropanol one. **The ordering survives** —
green above brown in all three solvents, gap widening 0.0009 → 0.0095. ⭐ And the sunflower fill
**settled at 106 s and did not move** (span 0.03 against a 0.38 benchmark): the whole
`SPEC_settled_measurement.md` apparatus exists because isopropanol fills drift for twenty minutes.

⭐⭐ **And it found a gap in `DOC_sample_physics.md` §4.9.** That ligand table sorts by POLARITY — alcohols
coordinate the magnesium's fifth site, hydrocarbons cannot. A **triglyceride is nonpolar AND carries
ester carbonyls**, so sunflower occupies a corner the table does not have: a true solution *and* a ligand
in vast excess. ⛔ Then the step §4.9 does not take: it dismisses aggregation on a **nominal**
concentration argument, but **inside a droplet the concentration is that of NEAT OIL** — the regime where
aggregation lives. The packaging that makes dilution-invariance *structural* also pins the pigment where
it can self-interact. ⚠ An argument, not a measurement (§16.12.7g).

#### ⏸ What it adds to the backlog

| | item | why |
|---|---|---|
| **E3** | ⭐ **the SPLIT SAMPLE — sunflower vs isopropanol.** Four fills, one evening, one rig: green and brown oil × both solvents. Report `area(624)/area(Soret)` | Two fills answer *"does the band grow when only the solvent changes?"*; four also answer *"do the oils keep their ORDER?"*, which decides whether a migration is a free 2–16× signal gain or a new confound. ⚠ The ordering currently rests on **ONE brown fill**. ⛔ E3's first version — measure a narrow lamp line's WIDTH — is unrunnable: there is no narrow OPTICAL feature in this beam, both candidates are detector artefacts. `SPEC_color_retrieval.md` §7.16.4a.1/§7.16.5 |
| **Arm B** | ⭐ **UPGRADED, not new** — §16.12.7e's dilution series (1× / 0.5× / 0.25×) now also tests a MECHANISM | If the 624 nm deficit is aggregation *inside the droplets*, the ratio must be **dilution-INDEPENDENT in isopropanol** — the droplet interior never changes — and fall only at high true concentration in a solution. Same three fills per solvent; run it in **sunflower**, not white spirit. `SPEC_capture_quality.md` §16.12.7g |
| ⏸ | rig click-through of the new chips | nothing colour-related has been seen on the bench; the evidence is 583 tests and the as-built numbers |

#### ⚠ The methodological note, recorded because it earned its place

Six substantive claims were wrong today — **including both of the ones this session recommended** — plus
three defects introduced into working code. **Every one was caught by running code over data. None by
reasoning, and none by the 583-test suite**, which was green through all of them.

The two archive sweeps killed the two recommendations in succession. The bulk re-render of 203 reports
caught a regression the unit tests passed. The area test killed a mechanism argued for two messages
earlier. ⇒ **On this project, a claim is worth what it has been run against.** Seven fills of one oil
pair is not enough — which is also the honest caveat on the sunflower result above.

---

## ▶▶▶▶▶▶▶▶ WHAT WE CAN CLAIM FOR `Q%` — defend vs advertise  *(Edwin 2026-08-21 evening)*

> ⭐⭐ **The one sentence:** `Q%` is defensible today as **an instrument that reproducibly measures a real
> chemical quantity**. It is **not yet** defensible as **a classifier that assigns a verdict to an arbitrary
> oil**. Those are different claims, and "advertise" usually means the second.

### ⭐ What carries the DEFENCE — strong, and strong in the way that matters

| | |
|---|---|
| ⭐⭐ **it reads a named chemical step** | loss of the central Mg²⁺, protochlorophyll → protopheophytin. **Mechanism, not correlation** — not *"these two bottles differ"* |
| ⭐⭐ **and the mechanism is MEASURED** | the see-saw: bands anti-coupled at **−0.832** between oils, **+0.811** within one fill, and a **conserved sum** (CV 5.4 %, class-blind at *d* = 0.28). That is a conversion signature (`SPEC_metric_research.md` §13.5) |
| **its construction is justified term by term** | the **difference** kills what adds, the **division** kills what multiplies — one nuisance each (`DOC_metric_algebra.md` §5.2) |
| ⭐ **the first objection is already answered** | *"does it separate because the ruler shrinks for brown oils?"* Measured: `A_Soret` is class-blind at *d* = **0.16**, and the discrimination sits in the **numerator** (*d* = 1.69). ⇒ the failure mode a sceptic reaches for first **is not there** |
| **it reproduces** | pooled within-oil **1.100** units; two pours of one dilution agree to **0.076** |
| **the number is not an accident of timing** | the settling algorithm decides when the fill is ready, documented on real trajectories (`DOC_metric_algebra.md` ch. 6) |
| ⭐ **it has a picture a person can check by eye** | `SNV(Q) − SNV(Qy)` **is** the metric. Almost no fitted index can be audited that way |

### ⚠ What an outsider WILL find — and we wrote it down ourselves

⛔ **The classes overlap across the archive: 7 of 88.** Alongside it: the threshold has never been checked
against **independent ground truth**; `Q%` is **not solvent-portable** (it reads a green oil as brown in
white spirit); it has not been tested across a lamp change; σ_fill is confirmed on **four fills of one
oil**; and one oil in the panel (`Spar Premium`) has **no defensible label** (§13.2a).

⇒ Anyone serious will test it and find these. **They are not to be hidden, and there is nothing to gain by
trying** — they are already in three internal PDFs.

### ⭐⭐ THE REFRAME — for the lab channel, the defensible claim IS the product claim

`spectracs-references/business/SPEC_wirtschaftliches.md` makes **lab-as-channel-partner** the key route.
⭐ A lab does not want a green/brown pill. It wants **a reproducible number with a documented error budget,
a stated mechanism, and an honest account of what breaks it** — which is exactly what now exists, in three
PDFs, with every constant traced to a measurement and every weakness named.

⇒ ⭐ **The honesty is an ASSET on that route, not a liability.** A document set that opens with *"here is
what is wrong with it"* is far more persuasive to someone who will test it than one that does not.

### What closes the gap to the MILLER-facing verdict

| | |
|---|---|
| **M9 — the pre-registration** | see the block above. Turns the next fills into a test instead of another fit |
| ⭐⭐ **reference oils with ground truth from OUTSIDE the spectrum** | taste, the mill's roast record, provenance. ⛔ §16.31.4's standing blocker: no spectral quantity can supply it, because one pigment system means every spectral statistic is a projection of the same chemistry. **Mostly a conversation with a mill, not an engineering task** |

⇒ **Advertise "a measurement you can audit", not "a verdict you can trust on any bottle".** The first is
true today; the second is one honest evening plus a phone call away.

---

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

> **⭐⭐ UPDATE 2026-08-07/08 — THE FOUR-OIL SESSION RAN, and it changed the recipe, the gauge and PRIO 2.**
> [`SPEC_capture_quality.md`](../spectracsPy/docs/SPEC_capture_quality.md) **§16.27**; tool
> `diagnostics/spar_three_oils.py`. Four oils on the capillary protocol, one night, three re-seats each:
> **Steirerkraft 9.956 · Spar ggA 8.755 · Spar Premium 7.691 · S-Budget 6.505** (`M448`), **all six pairs
> resolved**, *d* 4.09–15.57. Five things came out of it:
> 1. ⭐ **PRIO 2's green-vs-green ran** — on one protocol, one rig, one night, with no session confound. The
>    *signal* side of the gate is confirmed; ⚠ **σ_fill is not** (every set is re-seats of ONE fill), so the
>    gate is **not closed** — see the new σ_fill item below.
> 2. ⭐ **The dose is now measured: 2 capillaries / 10 mL** (§16.23.2a). 2/12 mL left two oils under the `A_Q`
>    floor; ⛔ 3/12 mL was rejected because it puts every oil's 448–460 band **at or below the 16 DN guard**.
>    **The dose and the 448 trim are one decision** — the stronger fill is only safe on the trimmed metric.
> 3. ⭐⭐ **The exposure control §16.24.1 was missing now exists** — one fill measured at AE 90 and 104 four
>    minutes apart moved `M448` by **+0.04 %**, with a leg-scale mismatch of 2.70 % (larger than the corrupted
>    run's 2.5 %). Exposure alone does **not** corrupt the metric at working strength; §16.24.1 is *bounded*,
>    not overturned. And the entire exposure sensitivity sits in **440–447 nm** — the best argument yet for the
>    NEXT TASK trim.
> 4. ⛔ **Two shape statistics retired across exposure states** — `rise/Q amp` and 590–610/Soret (state effect
>    184 % and 160 %, larger than the gaps they were asked to carry). Within-state use stands.
> 5. ⭐ **The gauge/verdict split** — see the new item below.
>
> **▶ PRIORITY ORDER SET BY EDWIN 2026-08-06:**
>
> ⭐⭐ **UPDATE 2026-08-21 (evening) — `Q%` KEEPS THE VERDICT; `dQ100 v2` SHIPS AS A SCALAR.** The morning's
> *"`dQ100` owns the pill"* decision is **reversed on the verdict half** — `dQ100` fails §16.31.3a's bar
> under the `Spar Premium` relabel, and every constant in the family is still fitted on the corpus it is
> scored on. `Q%` keeps the gauges, the history tracker and the too-brown verdict; `dQ100 v2` is printed
> as a number beside it. ⭐ The evening also produced two better packagings (`B`, `Q%_k`), the rule that
> *differences survive and ratios do not*, and four independent arguments for the **red extension past
> 660 nm**. ⬇ First section below.

---

## ▶▶▶▶▶▶▶▶ DECIDED 2026-08-21 (evening) — `Q%` KEEPS THE VERDICT; `dQ100 v2` BECOMES A SCALAR UNDER EVALUATION  *(Edwin; DESIGN, nothing built)*

> ⭐⭐ **This REVERSES the verdict half of the decision immediately below, taken the same morning.** The metric
> research behind `dQ100` is unchanged and still stands — what changed is how much of it may be *shipped*
> before it has been tested on data it was not fitted on. The evening session re-measured the whole archive
> from the reports on disk (133 traces, 88 labelled runs) and answered four of Edwin's questions in order:
> is `Q%` more reproducible · can `A_valley` be the baseline · can the two bands be compared against a local
> reference · can `Q%` and `dQ100` be combined. ⭐ **The research record is `SPEC_metric_research.md` §13.**

### The decision

| | |
|---|---|
| ⭐⭐ **`Q%` keeps the gauges, the history tracker and the "too brown" verdict** | it stays exactly as shipped — `RoastQPercentGaugeView`, `T_V = -18.6`, the tracker band, `ClearingEvaluator`'s `qPercent` column. ⛔ **Nothing is repointed** |
| ⭐ **`dQ100 v2` is evaluated as a metric SCALAR** | printed as a number beside `Q%`, both band pairs marked on the plot. ⛔ **No pill, no gauge, no verdict, no tracker alarm** until it is pre-registered and tested out of sample |
| ⛔ **`M4` is WITHDRAWN** | *"retire `Q%`'s gauge"* does not happen. The report shows ONE pill — `Q%`'s — and `dQ100` as a figure beside it |
| ⭐ **the shipping solvent stays ISOPROPANOL** | unchanged from the morning decision |

### Why the reversal — three measurements, in order of weight

**1 · ⛔⛔ `dQ100`'s `0 / 68` is conditional on ONE contested label, and it fails `SPEC_capture_quality.md`
§16.31.3a's own bar.** Scored under all three treatments of `Spar Premium`:

```
                       Premium BROWN        Premium GREEN        Premium EXCLUDED
                    corridor    mis      corridor    mis       corridor    mis
   Q%                 -2.807   7 / 88      -2.807   6 / 88       -2.807   6 / 85
   dQ100 v2           +6.846   0 / 88   ⛔ -0.280   2 / 88      +10.011   0 / 85
```

§16.31.3a's rule is *"no statistic may be quoted under a labelling derived from it"*, and the `Spar Premium`
relabel was derived from the red far slope — which is the same region `dQ100` reads. `M448` cleared that bar
by separating under **all three** treatments. **`dQ100` does not.** ⇒ it may not be quoted as evidence for the
relabel it agrees with, and if `Spar Premium` is really green the main metric owns two errors.
⚠⚠ **AND MORE FILLS OF IT MAY NOT SETTLE IT — corrected 2026-08-21 evening.** Excluded from the fit, `B`
opens a **15-unit** gap (green max 52.93 → brown min 67.93) and `Spar Premium`'s three runs land **inside
it**, at 63.08 / 63.06 / 65.78 — 70–85 % of the way across, on neither side. If the oil is genuinely
intermediate then more fills locate that intermediate value more precisely and **settle nothing about the
label**. What the tube actually blocks is any claim of the form *"metric X scores 0/88"*, because that
count depends on a label the spectrum cannot justify (§16.31.3a: one pigment system, so every spectral
statistic is a projection of the same chemistry).

⇒ **Two ways out, and neither is more fills:**

| | |
|---|---|
| ⭐ **exclude it and say so** | report **0 / 85** with the exclusion stated. This is the branch §16.31.3a explicitly allows, and both `dQ100` and `B` pass it |
| ⭐⭐ **get ground truth from OUTSIDE the spectrum** | taste, the mill's roast record, provenance — for that one oil. Settles it permanently, and nothing else does |

⚠ The extra fills are still worth taking for the *other* question they answer — whether that single tube is
representative — but that is a research item, not a gate.

**2 · ⚠ Every constant in the candidate family is fitted on the corpus it is scored on** — the `563–573`
half-width (already flagged), and now also every mixing weight and threshold found this evening. Broad
plateaus and 100 % leave-one-oil-out say the *choices* are stable; they do not make the corridor numbers
free. ⇒ **pre-registration is the remedy** (see the build list).

**3 · ⭐ For the TRACKER specifically, `Q%` is the better scalar, and this is now measured.** On repeat fills
of one oil `Q%` resolves 0.076–0.128 units between pours; `dQ100`'s within-fill scatter is several times
larger in the same terms, and on the capillary panel's Steirerkraft it reads 8.64 → 12.88 → 18.62 across
three runs (sd **5.01**) while `Q%` moves 0.45. ⇒ `[[spectracs-history-tracker-spec]]`'s drift scalar stays
`Q%`. *(The reverse is also true and is why `dQ100` stays on the page: per unit of the green→brown DECISION
`dQ100` is 2.4× better — within-fill scatter 10.7 % of the class gap against `Q%`'s 26.2 %.)*

**4 · the settling algorithm could not be repointed anyway** — `W8` is still unbuilt, `dQ100` has never been
recorded per frame, and no `dQ100` settling curve has ever been observed (`SPEC_settled_measurement.md`
§52.7a). Unchanged by this decision.

### ⚠ The two risks this decision accepts, recorded openly

| risk | measured | mitigation |
|---|---|---|
| ⛔ **`Q%` misreads a green oil in WHITE SPIRIT** | `20260821LugitschA` — a green oil, both runs read 20.79 / 20.62 ⇒ **BROWN**. `dQ100` (−4.9), `Q%₀.₇₅` (4.16) and `B` (28.55) all read green. The only genuinely out-of-sample data in the settled set, and `Q%` goes 2 / 4 while the red-band forms go 4 / 4 | white spirit is deferred to `S4`; ⚠ the verdict pill must **never** be shown on a non-IPA fill until `S4` runs |
| ⚠ **`Q%`'s brown margin is thin where it matters** | matched-recipe pair (Lugitsch 7 runs vs Billa Clever 6, both fully settled): `Q%`'s worst brown fill clears `T` by **1.8 sd = 6.9 % of the gap**, against `dQ100` 4.4 sd / 14.7 % and `B` 7.4 sd / 18.8 %. The green side is comfortable (12–16 sd) on every metric | the brown class is the binding one — this is the same finding series E was already commissioned for (§16.11.11) |
| ⛔ **`Q%` returns a badly wrong GREEN on an opaque fill** | `20280819BillaClever/003` → `Q%` 8.45 ⇒ green on a brown oil. `dQ100` (180.7) and `B` (194.3) stay BROWN — but with obviously-broken values, which is the safer failure | `M3` — the `A_valley` ceiling in `tooDark` — is what actually fixes this, and it is metric-independent. ⭐ **Raised in priority by this decision** |

### ⭐ What the evening session ADDED to the metric picture

**The two packagings that came out of it** — both `0 / 88`, both 100 % leave-one-oil-out, both DESIGN only:

```
   B      =  dQ100 + 1.617 x Q%                                   T = 58.0
   Q%_k   =  100 [ (A_Q - A_valley) - k (A_Qy - A_valley) ] / A_Soret     k = 0.75, T = 11.36
```

| | |
|---|---|
| `Q%_k` — what it is | **`Q%` with a red-band term.** `k = 0` is `Q%` exactly; anywhere in `k = 0.5 … 1.0` it goes from `7 / 88` to `0 / 88`. Best noise-to-margin measured all session: **0.28×** against `dQ100`'s 0.63×. ⭐ Keeps `Q%`'s units, its pill, and continuity with 143 archived reports — **one new constant and one extra band mean** |
| `B` — what it is | best robustness measured: diffuser **0.71×**, dose **0.23×**, and `0 / 88` under all three `Spar Premium` treatments |
| ⭐⭐ **and they are the same axis** | `r(Q%₀.₇₅, dQ100) = 0.990` · `r(B, dQ100) = 0.997`. **Once the red band is in `Q%`, `dQ` has been re-derived.** The variants differ in CONDITIONING, not in information |
| ⚠ the bar-passing is not a deep property | whether a form clears §16.31.3a is a continuous function of how much `Q%` weight it carries (`k = 0.50` and `B` pass; `k = 0.75` gives 3; `dQ100` gives 2). It costs ~30 % of the decision margin and must be chosen deliberately |

**⭐⭐ The finding that decides where the metric research goes next:** the measured green→brown direction in
SNV space is **+0.12 at 571 nm and −0.14 at 624–626 nm** — i.e. the optimal linear read available in
440–630 nm is a positive weight on the Q band and a negative one on the Qy band, **which is what `dQ100`
already computes.** ⇒ **metric algebra inside this window has hit diminishing returns.** What is left is
windows, conditioning, and the clamp.

### ⛔ What was REFUTED this evening — the durable part

| idea | result |
|---|---|
| `A_valley` as the DENOMINATOR of `dQ` | worst option measured, `11 / 88`. The level swings **19×** across the archive (0.013–0.248 A) — it is the fill's pedestal, not the oil's property |
| a chord through the valley and ~600 nm | ⭐ it *works* — corridor doubles, noise-to-margin 0.63× → 0.24× — but ⛔ **it is a third band read, not a baseline**: `A(600–606)/A_Soret` carries class signal at *d* = 1.23 while `A(623–626)/A_Soret` is the QUIETEST window in the red (*d* = 0.45). A right-anchor scan shows a broad plateau 588–603 and a **cliff at 606** — the 607 nm lamp line, which the ~1 nm de-spike does not remove |
| the 590–615 rise as a SLOPE or a STEP | ⛔ 5–43 misclassified. The 607 nm line sits in the middle and the two sides behave differently |
| the trough between the bands as a baseline | ⛔ **there is no trough.** The argmin pins against its own search edge in 43 / 93 runs; with the bump excised the deepest point is a coin flip between two sides 10 nm apart (sd 5–7 nm) |
| one horizontal line + the two band AREAS above it | ⛔ `7–15 / 88`, under all four ways of drawing the line, for the same reason as `/A_valley` |
| the two-species (protochlorophyll → protopheophytin) model, *inside our window* | ⛔ PC1 of the SNV'd red region is the **lamp**, not the oil (46 % of variance, loading a dipole on 608–610 nm, and it does **not** separate the classes) · ⛔ **no isosbestic point** (every class-mean crossing has *d'* = 0.01–0.48) · ⛔ the archive cannot supply the degradation direction (within-fill drift vs the class axis, cos ≤ 0.57; the +24 h aged pair only +0.40) |
| the Qy peak POSITION by parabola | ⛔ unconstrained on a shoulder (green 626.4 ± 129.6 nm) |
| the RED CENTROID | ⭐ the one position metric that works — green **617.31 ± 0.63 nm** vs brown **611.87 ± 1.39 nm**, `0 / 88`, and the **best diffuser robustness of anything measured (0.26×)**. ⚠ But it integrates to 626 nm — the clamp — so it is a truncated statistic. Parked as a candidate |

⭐⭐ **THE RULE THAT UNIFIES ALL OF IT, and it is worth more than any single candidate:**

> **Differences of equal-width windows survive; ratios of baseline-subtracted quantities do not.**
> Every candidate baseline in this spectrum — valley, trough, flat bottom, the 607 nm bump — varies **more
> between fills than between oils**. A difference cancels it exactly. A ratio multiplies by it.

That is `SPEC_metric_research.md` §12.8c's `R`-versus-`(3)/(2)` finding, re-derived from three independent
directions on the same evening. ⇒ **no future candidate may divide by a locally-estimated baseline.**
⚠ One trap inside the rule: the windows must be **equal width**, or the baseline does not fully cancel
(20 nm vs 9 nm leaves a residual `b × Δwidth` term — measured, r = 0.926 rather than 1.000).

### ⭐⭐ THE CLAMP IS NOW THE BINDING CONSTRAINT — not the arithmetic

Four independent lines hit the same 630 nm wall in one evening:

| line | how it hits the clamp |
|---|---|
| the red centroid | integrates to 626 nm ⇒ a truncated centroid is biased by wherever you truncate |
| the two-species model | the chl→pheo isosbestic lives near 660–680 nm, outside |
| valley-to-valley baselines | there is only ONE real valley inside the window (500–560); the second sits ~640–660 nm |
| Qy itself | read as a **shoulder**, never as a band — which is why the parabola fit is unconstrained |

⇒ `ROADMAP` item 5's **red extension past ~660 nm** now has four arguments it did not have this morning,
on top of `KB_spectroscopy_physics.md` §4.1's own note that the widening *"should be re-costed"* on the
≈625 nm Qy finding. ⚠ Nothing here is a metric problem.

### ⭐ DOC ITEMS — for the next internal PDF  *(Edwin's call, 2026-08-21 evening)*

| | to be written up, with the figure |
|---|---|
| ⭐⭐ **the band relation, and that it IS the metric** | `SNV(Q) − SNV(Qy)` equals `dQ100 / 100` **exactly** — verified to **1.1e-15** over 133 runs, because the SNV mean cancels in the difference and only the SNV scale survives. Brown sits ABOVE green at the Q band (*d* = **+2.03**) and BELOW it at the Qy band (*d* = **−2.33**) — the relation **reverses**. ⭐ Turbidity does not fake it: the within-fill slopes (green −0.84, brown +1.18) cannot even agree with each other, and the classes stay apart at every pedestal level. ⚠ The identity holds **only** when the SNV window is `dQ100`'s own 448–626 (a 440–626 window shifts it by −8.90 ± 16.30 units). ⇒ **the metric has a picture a person can verify by eye**, which almost no fitted index has |
| ⭐ **two metrics, distinct bands, one direction** | `r(Q%, dQ100) = +0.842` — they use different bands and different references, yet point the same way; ~29 % of the variation is independent, which is exactly why blending them gains anything at all. ⭐ And once the red band enters `Q%`, `r` rises to **0.990** ⇒ one axis, three packagings |
| figure | rendered: `~/spectracs_band_relation.png` (4 panels — raw / SNV / per-oil / turbidity control) and `~/spectracs_recipe_pair.png` (the 13 matched-recipe settled runs, margins to threshold). ⚠ Both are scratch renders — the doc versions must be regenerated from a committed script |

### Build-list changes

```
 ==============================================================================================
  CHANGED BY THIS DECISION
 ----------------------------------------------------------------------------------------------
  M1  UNCHANGED and now the whole of the dQ100 delivery: a dQ100 v2 metric ROW beside Q%,
      both band pairs marked on the "Absorption (bands)" plot. A NUMBER, not a verdict.
  M3  RAISED IN PRIORITY - the A_valley ceiling in `tooDark`. It is metric-independent and
      it is what actually protects against the 003-style opaque fill that made Q% print 8.45.
  M4  |X| WITHDRAWN - Q%'s gauge is NOT retired. One pill, and it is Q%'s.
  M6  add to the test list: a regression guard pinning Q% as the verdict source, so a later
      refactor cannot silently repoint the pill.
  M9  NEW - PRE-REGISTRATION BLOCK. Before the next rig session, freeze IN WRITING:
      the windows, k (or w), and T for whichever red-band form is carried forward, plus the
      prediction each makes for the fills about to be taken. Then let the new fills test it
      on data it has never seen. |X| This is the ONLY thing that converts today's best
      candidate into a validated one; everything else is another fit on the same 88 runs.
  M10 |X| DONE 2026-08-21 - the write-ups landed, with committed generators:
      . SPEC_metric_research.md §13 - the research record: one axis, the RULE, the refuted
        list, the coupling + conserved sum, the Soret correction, the clamp, the solvent
      . DOC_sample_physics.md §3.4a  -> Spectracs_LightPigmentSolvent.pdf  (Figure 5)
      . DOC_metric_algebra.md RESTRUCTURED -> Q% is chapter 5, the settling algorithm is 6,
        dQ100 is 7; the Pigment Index moved to Appendix E, its far-anchor history to Ea;
        §5.8 carries the see-saw and §5.2 the numerator/denominator decomposition
      . docs/tools/build_tilt_figures.py      -> figures/tilt_seesaw.svg
      . docs/tools/build_settling_figures.py  -> settling_sequence / _cases / _drawdown .svg
      . diagnostics/metric_algebra_plots.py   -> Figure 2 now shades the SHIPPED windows and
        draws NO baseline (it implied Q% subtracts something, and it subtracts nothing)
      |X| STILL OWED: the three settling figures read their trajectory cache from a /tmp
          scratch file. Fold that extraction into the generator before relying on them.
 ==============================================================================================
```

---

## ▶▶▶▶▶▶▶ ⚠ SUPERSEDED ON THE VERDICT 2026-08-21 (evening) — `dQ100` BECOMES THE MAIN METRIC, AND ISOPROPANOL STAYS  *(Edwin; the METRIC RESEARCH stands, the VERDICT half is reversed by the section above)*

> ⭐⭐ Two decisions, taken after a night of analysis on `20260821LugitschA` / `20260821BillaCleverA` and a
> re-reading of the whole archive. The write-up is [`SPEC_metric_research.md`](../spectracsPy/docs/SPEC_metric_research.md)
> §12; the session that produced it is `SPEC_capture_quality.md` §16.12.7f and `SPEC_settled_measurement.md` §52.

### The two decisions

| | |
|---|---|
| ⛔ ~~**`dQ100 v2` is the main metric — it owns the verdict pill**~~ | **REVERSED the same evening** — `Q%` keeps the pill, `dQ100 v2` ships as a scalar under evaluation. See the section above |
| ⛔ ~~**`Q%` stays, as a NUMBER only**~~ — **REVERSED**, `Q%` keeps its gauge, the tracker and the verdict | ~~its gauge is retired — the report must never show two contradictory pills (they disagree on 5 of 6 runs of the Spar session)~~. ⭐ The two-pill argument still holds and is why `dQ100` ships as a NUMBER: **one pill, and it is `Q%`'s** |
| ⭐⭐ **the shipping solvent stays ISOPROPANOL** | white spirit is **deferred to an experiment on an already-shipped metric**, not a prerequisite for shipping one |
| ⚠ **white spirit's contribution stands** | it is what made the 624 nm band visible enough to find. That debt is real and is recorded in §16.12.7f |

### The metric

```
                mean A over [563, 573]  -  mean A over [623, 626]
  dQ100 = 100 x ---------------------------------------------------
                       sd of A over [448, 626]
```

- on the **de-spiked RAW absorbance** — ⛔ no baseline, no pedestal correction, no SNV applied first
- ⭐⭐ **NATIVE SAMPLING**, the same convention `V` uses (§10.1a) — ⛔ **not** a resampled grid. Measured:
  a 0.25 nm resample shifts every run by up to 0.889 units and moves `T` from 30.03 to 29.64. The two
  give different numbers and only one of them is the one that was validated.
- **higher = browner.** Negative means the 624 nm band stands *taller* than the 563–573 one — the
  intact-pigment state. ⭐ Zero is a real landmark, not an arbitrary origin, which is why there is **no shift**.
- ⭐ dilution- and flat-pedestal-invariant **by construction**: the numerator is a difference (an additive
  `b` cancels) and `sd` is offset-blind and scales with `c`.

### The gauge — it draws itself, and no number in it is invented

```
   within-session sd, median over 15 sessions  =  7.65 % of the 40.5 gap  =  3.10 units
   T +/- 1 sigma                               =  26.9 .. 33.1
   the archive's own EMPTY CORRIDOR            =  26.6 .. 33.5     <- the same interval
```

| | |
|---|---|
| `dQ100 < 26.6` | good — green |
| `26.6 .. 33.5` | **borderline — re-measure** ⭐ = the empty corridor = the measured within-fill sd |
| `dQ100 > 33.5` | probably too brown |
| band `[-35, +65]` | headroom past the corpus (−26.9 … +59.2) |
| domain guard | same withhold-don't-clamp contract as `V_VERDICT_BAND`: outside the band the numbers and the plot survive, only the **pill** is withheld |

⭐ `T = 30.0`. Three classes, not two, for the same reason `RoastQPercentGaugeView` has three: a fill whose
runs straddle the line has no verdict and the gauge must say so.

### Why — the evidence, all of it on the ISOPROPANOL archive

```
  68 runs · 8 named oils · 16 sessions · both rig eras · all isopropanol
  ----------------------------------------------------------------------
                            Q%           dQ100 v2
   corridor               -1.660          +6.85
   misclassified          5 / 68          0 / 68
  ----------------------------------------------------------------------
  the 5 runs Q% gets wrong are ALL the Spar session — the two adjacent oils
  it has never separated, and 4.3 already flags as an extrapolation:
     Spar ggA    /001 /002 /003   Q% 18.06-18.80 -> BROWN   dQ100 +17..+23 -> green
     Spar Premium/001      /003   Q% 17.14,17.87 -> GREEN   dQ100 +35,+37  -> brown
  ⭐ and dQ100 agrees with §16.30.1a's relabel of Spar Premium, where T_V contradicts it.
```

**Out of sample, across the rig rebuild** (2026-07-27, a *different lamp*, judged on the post-rebuild
threshold it never saw, diffuser-IN runs excluded):

```
   Q%      green 8/11 correct · brown 9/9   corridor -1.558  OVERLAP
   dQ100   green 11/11        · brown 9/9   corridor +17.33  CLEAN     <- 20 of 20
```

⭐ And it sees ageing where `Q%` cannot: the 24 h set moves `dQ100` to **1.93×** its own alarm threshold
against `Q%`'s **1.04×**.

### ✅⭐⭐ R1 IS DONE — 2026-08-21, and the turbidity confound is NOT THERE  *(`SPEC_metric_research.md` §12.11)*

The one question that gated the pill: does `dQ100` separate green from brown **because brown oils scatter
more**? Answered on 68 archived runs, analysis only, no rig time. Turbidity index
`tau = A(510–540)/A_Soret`, concentration-free.

```
  ⭐⭐ THE DECIDING TEST — ANCOVA, the turbidity coefficient pooled from WITHIN-session
     deviations only, so every between-oil difference is removed:

              within-session slope      between-class slope     ratio
   dQ100          -21.4  (r -0.13)          +1349.1           -63x   OPPOSITE SIGN
   Q%             +10.4  (r +0.36)           +138.5           +13x   same sign

  If turbidity drove the separation the two slopes would AGREE. For dQ100 they point in
  OPPOSITE directions. => adjusting for turbidity makes the corridor WIDER: +6.846 -> +6.950

  ⭐ AND AT MATCHED TURBIDITY THE CLASSES STILL SEPARATE, IN EVERY BIN:
     tau 0.060-0.090  gap 27.50 CLEAN | 0.090-0.110  gap  6.85 CLEAN
     tau 0.110-0.130  gap 21.81 CLEAN | 0.130-0.160  gap 14.53 CLEAN
```

⚠ **The contamination belongs to `Q%`, not `dQ100`** — `Q%`'s within-session slope has the SAME sign as its
between-class slope, and turbidity explains 21 % of its variance against `dQ100`'s 12 %.

⛔ **Two corrections to this project's own earlier alarm** (§12.11d): the `r = −0.94` figures were
per-session correlations on n ≈ 6 whose slopes scatter from −718 to +75 — noise wearing a coefficient; and a
naive **pooled** regression of `dQ100` on `tau` is the wrong adjustment, because the classes differ in both
variables and a pooled fit removes the class signal along with the covariate.

⚠ **R1 is an ELIMINATION, not an identification.** It rules out the one alternative we could name. **S1, the
acid test, is what would make the claim positive**, and it is still unrun. The designed version of R1 is
still **R2** — one monitored fill with `W8`'s columns, read through a full clearing curve.

⇒ ⭐ **THE PILL IS NO LONGER BLOCKED.**

### ⚠ What remains open

Two debts. The `563–573` window half-width was chosen on the same corpus it is scored on
(leave-one-oil-out 94 %, but still fitted); and the green class is **bimodal** — Ja!Natürlich (−26) and
Lugitsch (−16) sit 30 units below Kiendler / Steirerkraft / Spar ggA (+11…+19), four corridor-widths
apart. Real structure or a mislabel, and worth knowing before the threshold hardens.

### The settling algorithm — ⭐ DO NOT REPOINT IT YET

`ClearingEvaluator` keeps watching `Q%`. §51 shipped it, 511 tests are green, and it is correct.
⛔ **`dQ100` has never been recorded per-frame** — the `MonitorRecord` carries `qPercent · soret · valley ·
qBand` and neither `A(563–573)` nor `A(623–626)` exists in any run ever taken. **No `dQ100` settling curve
has ever been observed**, and designing a gate for a curve nobody has seen is how §16.17 happened.

⚠ Theory and evidence disagree about what that curve looks like, which is exactly why it must be measured:

| | |
|---|---|
| theory says | a flat pedestal cancels in the numerator and leaves `sd` untouched ⇒ clearing is **invisible** to `dQ100` ⇒ flat, then rising ⇒ **no minimum**, the first look is the answer, and §40's drawdown / §41's D2 / the hunt / the vertex are simply **inapplicable** |
| the data says | `r = −0.94` against `A_valley` ⇒ it may well **have** a clearing limb after all |

⇒ **W8 then one run decides it.** If flat-then-rising: read the first look and do not apply the vertex
machinery to `dQ`. If V-shaped: the existing rules transfer unchanged and only the column changes.
⭐ Nothing is deleted either way.

### The build list

```
 ==============================================================================================
  DESK — no rig time
 ----------------------------------------------------------------------------------------------
  M1  dQ100 v2 metric row beside Q%; both bands marked on the "Absorption (bands)" plot
  M2  W8 — record A(563-573) and A(623-626) as MONITOR_COLUMNS
      ⛔ BEFORE any further lab session, and ⛔ NOT retroactive: every run taken before
         this is permanently un-analysable for dQ trajectories.
      ⭐ Specified in SPEC_settled_measurement.md §52.7
  M3  W3 — extend `tooDark` with an absolute A_valley ceiling (0.21)
      reuses D1's dropping, §32.4a's carryOn and NEVER_SETTLED's no-value contract;
      metric-independent — it refuses a fill that should not be measured at all.
      On BC-1 all nine rows become "not a look" and the run waits instead of latching at 6.7 s
  M4  |X| WITHDRAWN 2026-08-21 evening - Q%'s gauge is NOT retired. One pill, and it is Q%'s.
  M5  a core `bandSd(spectrum, lo, hi)` — the plugin tier carries no numpy (§1)
  M6  tests: the worked example, the 8-oil ladder as a fixture, the 5 Spar runs as a
      regression guard, and a native-vs-resample assertion so the convention cannot drift
  M7  ✅ DONE 2026-08-21 — four spec corrections written:
        · §16.11.16 item 3 — LIGHT DOSE is a fourth confound on set A; the mechanism
          paragraph predates §16.36 and §39 and attributed the browning to time+temperature
        · §16.11.15's working-window note — the binding requirement is DARK (P6'), not FAST;
          ⚠ the DARK ageing rate has still never been measured (-> S2)
        · KB_spectroscopy_physics §4.1a — ⛔ the Q band is ~568 nm, NOT ~574; the "574" is
          the ramp into the 581 nm Bayer crossover. The Qy at 623-626 is unaffected and is
          now confirmed a FOURTH time by the white-spirit fills (622.8-625.0 nm)
        · DOC_lamp_rebuild §6.1 — NEW: what the ~583 nm step COSTS. It sits inside the
          shipped A_Q window (565-580); `Pigment D_Q` finds the ramp rather than the pigment
          in 93 % of 110 isopropanol runs; ⚠ and trimming A_Q off it does NOT rescue Q%
          (measured: d 2.78 -> 2.33-2.45, still overlapping) so V's windows must NOT be re-tuned
  M8  ⛔ resolve the STALE SEALED dev-plugin row v1.1.0 (§51.0) BEFORE publishing —
      a core/plugin skew fails SILENTLY; §45's readPhase is the detector
 ==============================================================================================
  MEASUREMENT
 ----------------------------------------------------------------------------------------------
  R1  ✅ DONE 2026-08-21 — the turbidity arm. dQ100 survives at matched turbidity in all
      four populated bins; within-session and between-class slopes have OPPOSITE signs.
      §12.11. ⇒ the pill is unblocked. ⚠ an elimination, not an identification.
  R2  one monitored fill WITH W8 -> the first dQ100 trajectory ever recorded -> decides
      the settling question above, AND is the DESIGNED version of R1
 ==============================================================================================
  OPEN SCIENCE — unscheduled
 ----------------------------------------------------------------------------------------------
  S1  the ACID TEST, in isopropanol. Pheophytinisation is acid-catalysed and irreversible;
      if a trace of acid drives dQ100 hard in the brown direction it stops being a
      correlation on 8 oils and becomes a measurement of the protopheophytin :
      protochlorophyll ratio that `KB_spectroscopy_physics.md` §4.1 already nominates as
      THE quality axis. ⚠ Do it in IPA, not white spirit — acids barely dissociate in a
      de-aromatised hydrocarbon.
  S2  DARK ageing rate — one foil-wrapped vial, read 0 / 4 h / 1 d / 3 d / 7 d.
      Separates "time" from "light dose", which no run on disk does (§39.6/P6').
  S3  the bimodal green class — real structure, or a mislabel?
  S4  WHITE SPIRIT, as an experiment: 5 oils x >= 3 INDEPENDENT dilutions, GATE 0/1/2 first,
      P6' dark aliquots, arm B (dilution invariance — still untested and can still refuse
      the solvent), and one deliberate optics perturbation.
      ⚠ Its cost is now visible: a solvent switch ZEROES the history tracker's reference
      library (143 IPA reports -> 0) and needs 3 fills per oil before it can alarm once.
 ==============================================================================================
```

### ⛔ The known failure mode, carried forward

**A paper diffuser erases the 624 nm band.** `20260727B` is the archive's diffuser A/B test (§16.7.2f):
diffuser IN → `R` 0.121 ± 0.126 with `P2` collapsing to ~0; diffuser OUT → 0.635 ± 0.013. `dQ100 v2` is the
only candidate that survives it — but by **0.9× its corridor**, i.e. an optics change of that size eats 92 %
of the decision margin. ⚠⚠ **`SPEC_lamp_rebuild.md`'s rebuild is a far larger optical change than a
diffuser. Test `dQ100` against it BEFORE ordering, not after.**

---

## ▶▶▶▶▶ PREVIOUS TOP PRIORITY — TWO ITEMS, IN THIS ORDER  *(Edwin 2026-08-18; ⚠ still open, now BELOW the 2026-08-21 decision above)*

> ⭐⭐ The measurement below (ONE FILL, ONE WAIT) is **BUILT and PROVEN ON THE RIG** — see
> [`SPEC_settled_measurement.md`](../spectracsPy/docs/SPEC_settled_measurement.md) §27 (as-built) and §28
> (series F, the first measurements it produced). These two items are what remains between the instrument
> and a product that can be trusted on brown oil.

### ▶▶▶▶▶▶ 1 · THE CLEAR-CASE READ — `SPEC_settled_measurement.md` §29  *(small, and it blocks the other one)*

⛔⛔ **The clear branch reports the most lamp-damaged look it saw.** `readAs = FIRST_SETTLED_WINDOW`
returns the LAST look, not the first: the gate needs ~105 s to confirm a fill is stable and the lamp
bleaches the sample throughout. Measured biases on series F: 0.013 · 0.037 · 0.084 · **0.482**.
⇒ visible, one-directional (so it never averages out), fill-specific (0.291 vs 0.008 `Q%`/min — a factor
of 35, so nothing can be subtracted afterwards), and ⚠ **it hits the GOOD fills**: a clean sample takes
this branch every time, while the muddy path — where all the design effort went — is the protected one.

⭐ **THE FIX (§29.3), and it also simplifies the algorithm.** The branch decision moves off turbidity and
onto the curve being read: **how far below the FIRST look does the `Q%` minimum sit?** Series F separates
into two clusters with a **15× gap** (0.000 · 0.010 ‖ 0.149 · 0.279 · 7.233) and the threshold — 2σ of the
measured window noise — falls inside it, DERIVED rather than chosen. ⇒ `A_valley` is the GATE (*when to
stop*), `Q%` depth is the READ (*what to report*); interior minimum → vertex, otherwise → the first look.
⚠ §29.6 flags the trap inside its own fix: **2σ = 0.13 is only right at W = 60** (noise ∝ 1/√W) — derive
it from the window, do not paste the number.

⛔ **WHY IT BLOCKS ITEM 2:** a +0.48 bias is **37 % of brown's margin to `T`**, and brown is the binding
class. Running the load-bearing measurement through a known one-directional bias spends the session and
produces a number nobody can use.

### ▶▶▶▶▶ 2 · THE BROWN OIL SERIES — series E, finally runnable  *(`SPEC_capture_quality.md` §16.11.11)*

> ⚠⚠ **UPDATED 2026-08-21 — this is no longer a FIRST measurement, and the item is smaller than it reads.**
> The Billa Clever sessions did what series E asks: `SPEC_settled_measurement.md` §39.7 records a **clean
> set of four fills of one brown stock at sd 0.198–0.220**, comfortably inside the ≤0.3 pass condition.
> ⛔ **What is owed is a CONFIRMATION, not a first run**, and for one specific reason: §39 then discovered
> that light on the *waiting aliquot* is worth **+1.34 / +0.74 units — larger than the 0.20 scatter the
> clean set reports.** That number was therefore taken before its dominant confound was known, with one
> fill hand-rescued. ⇒ **the run is §35's T1 under P6′** (aliquots dark from the moment they are drawn),
> five fills, one evening.

⭐ Brown is where the risk lives: it sits **1.297** from `T` against green's 1.651, so it is the class that
decides whether ONE measurement is enough (§16.13's σ_fill ≤ 0.307 boundary).

**The run:** six fills of ONE brown stock, identical handling — the capillary recipe and the 3-minute bath,
nothing varied — each measured ONCE, all in one sitting. ⭐ Plus **one deliberately UNWARMED muddy fill**,
which costs one extra slot and buys three things: it exercises §27.25's retention fix (still unexercised —
series F's vertex winners all sat 1–3 rows back, inside the old window), it is §17/C1's field case with no
water bath, and it is the hardest test of the θ = 0.005 gate.

**What one session then answers:** σ_fill for the binding class · the §16.13 decision table's open row ·
the history tracker's band (`SPEC_history_tracker.md` §11 — its σ is a by-product) · and whether series F's
0.276 was a good night or the new normal.

### ▶▶▶▶ 3 · IS THIS A GOOD FILL? — a verdict about the FILL, not the oil  *(Edwin 2026-08-18; TO BE SPECIFIED)*

⛔ **Preparation is now the dominant error term, and the app is silent about it.** Series F measured the
instrument at **σ 0.276** when the handling was careful — and a **0.47** gap between two halves of one
dilution when it was not (`SPEC_settled_measurement.md` §28.5). ⇒ the largest remaining variable is the
person holding the pipette, and a miller will not be as careful as the person who built the rig.

⭐⭐ **AND THE APP ALREADY HOLDS THREE INDEPENDENT SIGNALS ABOUT THE FILL — it just never combines them:**

| signal | where it comes from | what it says about the FILL |
|---|---|---|
| **DN guard** | `SPEC_capture_quality.md` §16.23.10 — min(S) in 500–560 against the target pair | too thick / too thin — a dilution error |
| **clearing time** | `MonitorRecord.clearingSeconds` (§2.4) | how muddy it arrived, and whether the bath did its job |
| ⭐ **browning rate** | `SPEC_settled_measurement.md` §29.5 (new) | how fast this fill degrades under the lamp — 0.291 vs 0.008 `Q%`/min across series F, a factor of 35 |

⇒ **specify a FILL verdict, separate from and shown beside the OIL verdict**: *"this measurement is sound"*
versus *"this measurement is about a good oil"*. ⭐ Cheap, because all three numbers are already computed
and already travel in the record — nothing new is measured, only combined and said out loud.
⚠ ⛔ **It must never be folded into the oil verdict** — the §2 rule that keeps the zero-dose extrapolation
separate applies here for the same reason: a quality number that quietly contains a handling number cannot
be argued with.

⭐ **Why it is the highest-value usability work after 1 and 2**: it is what lets someone who is not Edwin
trust — or correctly distrust — their own measurement, which is the precondition for the wizard ever
leaving the bench.

---

---

## ✅▶▶▶▶▶ BUILT — ONE FILL, ONE WAIT, ONE BEST MEASUREMENT  *(Edwin 2026-08-15; implemented and rig-proven 2026-08-17/18 — §27 as-built, §28 series F. ⭐ The proof: a muddy fill that a fixed-time read would have called 15.005 was carried to 13.990 — **1.015 units**. Superseded as "next" by the two items above.)*

> ⭐⭐ **Spec: [`SPEC_settled_measurement.md`](../spectracsPy/docs/SPEC_settled_measurement.md).**
> Evidence: [`SPEC_capture_quality.md` §16.36](../spectracsPy/docs/SPEC_capture_quality.md).
> Working prototype: `diagnostics/clearing_time_course.py`.

**⛔ WHY THIS COMES FIRST, BEFORE THE σ_fill RUN AND BEFORE THE LAMP REBUILD.** On 2026-08-14/15 the
Lugitsch session established that the lamp **changes the sample while measuring it** — its heat CLEARS a
muddy fill (reversible) and its light BROWNS it (irreversible: 13.7 % of the pigment destroyed in 72 min,
`Q%` drifting +1.0 to +1.6 per hour, a 90-minute session moving 1.5 units against a 0.21 refill floor).

⭐⭐ **The consequence is a retired premise, and Edwin's words are the right ones:** *"we are no longer
misled by measurements on the 'same oil' as the oil IS IN FACT NOT THE SAME OIL — otherwise we would have
hunted things and searched for explanations for weeks."* Every multi-run session in the archive treated
the fill as a fixed object being measured repeatedly. It was two processes with OPPOSITE signs on `Q%`,
overlapping in time — which is precisely why the scatter resisted explanation (§16.36.8).

**WHAT TO BUILD.** The device measures a **time course** during ACQUISITION, gates on `|ΔA_valley| < 0.005`
(the CAUSE, not `Q%`'s turn — which costs 10 further minutes of light), reads the `Q%` minimum as a
**parabola vertex**, reports the zero-dose extrapolation separately, and stops. One fill, one wait, one
value. ⚠ The 15-minute wait is ACCEPTED deliberately — ordinary lab equilibration, explainable to a
miller, and it avoids the heated-holder complication.

**EVERYTHING ELSE WAITS, AND HERE IS WHY:**

| queued work | why it must wait |
|---|---|
| ⭐ **σ_fill / the 3-refill run** | it measures the reproducibility OF A MEASUREMENT. Until the measurement is defined, it measures the protocol's own drift. ⇒ and it is now **one measurement per fill**, so spend the captures on MORE FILLS: 9 × 1 beats 3 × 3 by 4× on σ_fill (§16.34.3d) |
| **the DIY lamp rebuild** | a lamp swap moves the scale 4.84 units. Re-deriving thresholds on a drifting measurement bakes the drift into the new scale |
| **the internal PDFs** | they would document numbers the next protocol supersedes |
| **PRIO 3a ground truth** | labels compared against a moving measurement are labels compared against nothing |

⇒ **Everything downstream assumes a measurement that can be trusted. This is that measurement.**

---

## ▶▶▶▶ WAS HIGHEST PRIORITY — IMPLEMENT THE `V` METRIC IN THE DEV PLUGIN  *(Edwin 2026-08-14; ✅ IMPLEMENTED 2026-08-14, V2–V7 + V9/V10)*

> ⭐⭐ **`V` is the best metric this project has produced, and it is the next thing to build.**
> Definition, threshold, band, weaknesses and pre-registration:
> [`SPEC_metric_research.md` §10](../spectracsPy/docs/SPEC_metric_research.md). Every number
> reproduces with `diagnostics/box_metrics.py`.

```
V = (A_valley − A_Q) / A_Soret          on the DE-SPIKED RAW absorbance — NO baseline
    A_valley = mean over 500–560 nm     A_Q = mean over 565–580 nm     A_Soret = mean over 448–460 nm
    reported as V × 100 · less negative = greener
    T_V = −18.57   (corridor midpoint, §16.20.4's own 18-run corpus)
    history-tracker band  ±1.0
```

**Why it wins.** Class gap **5.05 σ** against M448's 3.80 · **17 of 18** archive fills ordered
correctly · separates under **both** contested labellings where M448 manages only one · denominator
stays **10 σ** from zero where M448's `B_Q` comes within **6** (which is why `Ja! Natürlich` inflates
to 22.24 and `20260811A` returns **−9.72**) · **no fitted baseline**, so the fill-dependent chord tilt
of `SPEC_capture_quality.md` §16.35 cannot touch it · dose-robust to ±40 %.

**Build order in the DEV plugin:**

| # | task |
|---|---|
| **V1** | `V` computed on the de-spiked raw absorbance, alongside the existing metrics — display only, no verdict |
| **V2** | declare the three windows in `declaredEvalBands()` so they are visible in the report, as §16.20 did for the far anchor |
| **V3** | `W = (A_Q − A_valley)/(A_Soret − A_valley)` as the explain-it number — the Q : Soret band ratio the physics talks about |
| **V4** | `T_V` verdict + the **±1.0** deviation band against a stored reference (this is the history tracker; `SPEC_history_tracker.md` §7 for the protocol) |
| **V5** | ⛔ guards: a fill whose runs **straddle `T_V`** gets NO verdict; `S1` outside −0.372 ± 0.04 flags an invalid run |

⛔ **Two weaknesses that must be surfaced in the UI, not buried:** a **lamp swap moves `V` by 4.89** —
more than the whole class gap — so a stored reference is void after a lamp change; and **half
concentration moves it 2.18**, so the capillary recipe is a precondition, not a nicety.

⚠ **Do NOT re-tune the windows.** They are frozen for the PRIO 2c test — `V` was found by scanning ~9
candidates on 13 fills, and re-tuning would destroy the only thing that can retire that selection risk.

⚠ **`T_V` may not be quoted as classifying the two Spar g.g.A. oils** — the derivation corpus excludes
them, exactly as it does for M448 (§16.30.1a, §16.31.3a).

### ▶▶▶ NEXT — UPDATE THE INTERNAL PDFs FOR `V` AND THE FAR-ANCHOR FINDING  *(Edwin 2026-08-14)*

The 2026-08-14 session changes what several shipped documents assert. Each has a markdown master and a
generator under `spectracsPy/docs/tools/`, so the work is edit-then-regenerate:

| document | master · generator | what changed |
|---|---|---|
| ⭐⭐ **Spectracs_MetricAlgebra.pdf** | `DOC_metric_algebra.md` · `build_metric_algebra_pdf.py` | **the biggest one.** Its whole subject is `M448`'s algebra and dilution-invariance proof. Needs `V`/`W`, the Gouterman reading, and §16.35's finding that the chord's far foot sits on the Qy band |
| **Spectracs_PedestalCorrection.pdf** | `DOC_pedestal_correction.md` · `build_pedestal_correction_pdf.py` | `r_Q` corrects a chord whose far anchor is on a band — that framing needs §16.35 |
| **Spectracs_CapabilityProof status** | `build_capability_status_pdf.py` | quotes `M448` numbers and thresholds |
| **Spectracs_Oil_Panel_2026-08-07.pdf** | `business/internal/commmunication/build_oil_panel_pdf.py` | ⚠ its numbers are **transcribed, not recomputed** (§16.27.10); it quotes `M448` only. Not in git |
| `DOC_sample_physics.md`, `DOC_capture_fidelity.md` | their generators | check for `M448`/anchor claims |

⚠ **Sequence it after V1–V3**, so the documents describe what the plugin actually computes rather than
a proposal. ⛔ And do not regenerate the Oil Panel PDF without re-deriving its numbers — §16.27.10
already flags the transcription as a dating hazard.

---

## ▶▶▶ σ_fill, AND IT NOW GATES THE PRODUCT  *(Edwin 2026-08-13; ⭐ **FIRST DATA 2026-08-18 — series F, `SPEC_settled_measurement.md` §28: σ 0.276 across five separate preparations, 4.5× tighter than the archive's WITHIN-FILL scatter of 1.255.** The binding number is still brown's — see item 2 at the top.)*

> ⭐ **UPDATED 2026-08-14:** this run is now also the **pre-registered test of `V`**
> (`SPEC_metric_research.md` §10). Compute `V` alongside `M448` from the same captures — it costs
> nothing extra and it is the only thing that can retire `V`'s selection risk. **Pass condition:
> across-fill sd of `V × 100` ≤ ~0.3 units on the capillary recipe.** The archive's prior is
> **0.21**, but from the *drop-based* recipe — ⛔ **no refill pair exists on the capillary recipe with
> a non-Billa oil**, which is precisely the gap this run fills.

> ⭐⭐ **This is the only number between "this looks like a product" and "this is a product."**
> `SPEC_capture_quality.md` **§16.34.3** (the gate) + **§16.21.1** (the original design) + §16.34 (why it
> matters now). Promoted from PRIO 2b, which stays below as the historical framing.

**What changed on 2026-08-13.** A day of metric work (§16.30–§16.32) established that **no statistic separates
the oils in a way that survives the class labels** — two candidates separate under *opposite* labellings
(§16.31.3), so the labels were carrying the results. ⇒ The absolute "is this oil good?" claim is blocked on
**ground truth**, which the spectra cannot supply.

⭐ **But a HISTORY / QM product needs none of that** (§16.34): no labels, no ground truth, no absolute
threshold, and no cross-instrument calibration — a mill compares against its own history on its own
instrument. It needs **one** thing: reproducibility across independent fills. **That is σ_fill.**

### The run

```
   7 oils  x  3 INDEPENDENT fills  x  3 runs      = 63 captures, ~2 evenings
   (3 oils x 3 fills answers the sigma_fill question alone — the rest adds per-oil variation)
```

⛔ **HOLD THE DOSE FIXED.** Three fills at ONE nominal dose (2 capillaries / 8 mL), **not** another dilution
series. Every existing multi-fill pair in the archive also varies concentration on purpose, which is exactly
why none of them prices preparation alone.

**Three controls, or the answer will be wrong (§16.34.3a):**

1. ⛔ **Standardise the settling time** — one protocol, **15 min**, held for every fill. Different settle
   times put drift into σ_fill and return a pessimistic number that is not preparation variance.
2. ⛔ **Keep the muddy oils out of the pooled figure** (§16.33) — `BillaClever` and `Ja! Natürlich` drift
   −27 %/run against −3…−8 % for everything else. Measure them, label them, exclude them from the pool.
3. ⛔ **Record `B_Q` per fill** — it decides which oils are trackable at all (break at ≈ 0.065), and it is free.

### ⭐⭐ The criterion — FIXED BEFORE THE RUN

⛔ *"The metrics look the same"* is **not** a criterion. With `n` runs averaged,
`detection = 3 x CV / sqrt(n)`; at n = 3 that is `1.73 x CV`:

| across-fill CV measured | detection claim for the datasheet | verdict |
|---|---|---|
| ≤ 3 % | **"detects a 5 % change"** | strong |
| **~4 %** | **"detects a 7 % change"** | ⬅ **the prior from today's data** |
| ~6 % | "detects a 10 % change" | usable |
| ⛔ ≳ 12 % | "detects a 20 % change" | **NO PRODUCT** — the eye already does that |

⇒ **The go/no-go sits only at the bottom.** Above ~10 % across-fill CV there is no history product; below it
there is one, and the claim scales with whatever number comes out. **The experiment calibrates the datasheet,
it does not pass or fail it.**

### It returns three more answers free

* ⭐ the **muddy check on the three untested oils** — turns §16.33 from n = 2 into a real class, or kills the
  brand association
* ⭐ **whether `B_Q` ≥ 0.065 is reachable at the standard recipe, per oil** — the trackable / not-trackable list
* ⭐ a **σ_fill per oil** rather than pooled; if it tracks `B_Q` that is a predictive rule, not a lookup table

⚠⚠ **What the "go" does NOT mean.** It establishes the instrument is **reproducible**. It does **not**
establish that a change it detects is a change a customer cares about — that is still the ground-truth gap
(§16.31.4), and it is why the product framing must be **deviation alarm feeding a lab relationship**, never
*quality meter*. Reproducibility is necessary and it is not sufficient.

---


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

### ⭐ PRIO 2c — THREE MORE `Spar Premium` FILLS *(added 2026-08-14; one evening, and it unblocks three statistics at once)*

⛔ **`Spar Premium g.g.A.` has exactly ONE fill in the whole archive** — `20260807C`, one evening, three runs.
That single tube is currently the load-bearing member under **`Q_snv`**, **`S2`** and **`M448`'s entire
+0.51 σ margin**, because it is the one fill that sits on the class boundary and it decides in every case
whether a statistic separates or overlaps
([`SPEC_capture_quality.md`](../spectracsPy/docs/SPEC_capture_quality.md) §16.30.1a, §16.31.3a).

**The run:** 2–3 independent fills, standard 2 cap / 10 mL recipe, 3 runs each, one session, one exposure
state, 15-minute settle. **~1 evening.**

⚠ **What it CANNOT do:** settle the class. `Spar Premium` has now been labelled brown → green → brown in
three days, and §16.30.1a establishes that **no spectral quantity can be independent evidence about that
label** — one pigment system means every statistic is a projection of the same axis (ρ = 0.84–0.94 between
all pairs measured, colour included). The class needs taste, roast records or provenance — PRIO 3a.

⭐ **What it CAN do, and why it is worth an evening:** settle whether that one tube is *representative*.
Three outcomes, all useful:

| outcome | what it means |
|---|---|
| stays low on the red slope | the one fill was real; the boundary problem is about the LABEL, not the data |
| moves up among the greens | the 08-13 green label is supported, `S2` is dead, and `M448`'s real margin is far better than +0.51 σ |
| scatters across both | σ_fill for this product is enormous — which explains the whole boundary mess and is a finding in itself |

⇒ Cheapest item on this page that changes what three separate results are worth. ⚠ Not a substitute for
**σ_fill** above (which needs several oils) — but it is the same protocol, so it can ride along with it.

### ⭐⭐ PRIO 2b — σ_fill: MULTIPLE FILLS PER OIL *(Edwin 2026-08-07, "yes, the multiple fills per oil is one test to be done")*

> ⭐⭐ **PROMOTED 2026-08-13 to HIGHEST PRIORITY** — see the block near the top of this file. It now
> gates the **product**, not just the capability claim: `SPEC_capture_quality.md` §16.34.3 fixes the pass
> criterion and the datasheet mapping. What follows is the original framing and the archive bounds.


**The one term the whole archive has never measured.** Every set on record — series D, Kiendler A/B/C,
Steirerkraft B/C, all four fills of §16.27 — is **re-seats of ONE fill**, so every *d* and every CV in this
document prices **seating**, not **preparation**. `SPEC_capture_quality.md` §16.21.1 F1 has carried this as the
gate number since 2026-08-03 and it is now the only thing between PRIO 2 and a closed capability claim.

**The run:** ⭐ **3 independent tubes per oil × 3 re-seats each**, on the new 2/10 mL dose, ≥ 2 oils (one green,
one brown). Nine runs per oil, one evening per oil. What it returns: σ_fill against the 0.4–2 % seating term
already measured (§16.26) — i.e. *does the capillary deliver on §16.23.7's SNR 1.8 → 18 projection?*

⛔ **HOLD THE DOSE FIXED** — three tubes at ONE nominal dose, **not** another dilution series. That is the one
design point §16.27.9a adds, and it is what every existing multi-fill pair fails to be: the archive already
holds three Steirerkraft fills, three Kiendler fills and two S-Budget fills, but **every pair also varies the
concentration on purpose**, so none of them prices preparation alone. What they do give is a crude bound —
Steirerkraft B/C agree to **0.22 %** (*t* = 0.09) across a deliberate 17 % concentration difference, while the
best `A_Q`-matched pair on record (Kiendler B/C) differs by **4.15 %** (*t* = −1.82, n = 2 runs each). ⇒ σ_fill
is **bounded under ~4 % and may be far smaller**, and the two most informative pairs disagree.

⚠ **It also gates the jury study** (`SPEC_roast_ampel.md` §9.4): a study comparing *oils* must carry the term
that says whether two bottles of the same oil land closer together than two different oils do.

### ⭐⭐ THE GAUGE / VERDICT SPLIT + the user-defined target *(Edwin 2026-08-07/08; `SPEC_roast_ampel.md` §9 — DESIGN, build on explicit request)*

> Edwin: *"the green gauge per se is okay when one interprets it purely as greenness. But the verdict is
> misleading."*

The shipped Ampel asserts *"green = fine, nothing to act on"* (§0/§2), which encodes a **monotone quality
model** that has never been tested. Four items, in build order:

| # | change | blocked on |
|---|---|---|
| **1** | ⭐ gauge renders **symmetric**, **verdict text removed from the green end** — it stops making an unsupported claim | nothing; do it first |
| **2** | verdict metric **`M base+ped` → `M448`**, over-roast line **7.17**, labelled PROVISIONAL. `M448` gives the brown line **2.7 σ** of margin against the shipped metric's **1.3 σ** — the pedestal correction *costs* class separation because it compresses the greener oils more | the NEXT TASK 448 trim |
| **3** | ⭐ **user-defined target greenness** + signed deviation readout, **default unset**. The miller stores a batch he knows is good and the gauge shows deviation from *his own* profile | 1, 2 |
| **3a** | ⭐ **UN-PARKED 2026-08-14 — a deviation THRESHOLD on that target**, alarming when a batch leaves a tolerance band around the user's own reference (`SPEC_roast_ampel.md` §9.3a). ⭐ The only alarm in the spec that needs **no universal optimum and no jury**. ⭐ **DESIGNED as a SHAPE alarm** — [`SPEC_history_tracker.md`](../spectracsPy/docs/SPEC_history_tracker.md): SNV-quotient shape distance `D = √(1−r²)` over 550–600 nm, which needs no metric choice, no denominator and no labels. Its control limit is **σ_fill**, so the run below prices it **free**. The *scalar* form of the same alarm is still undesigned | 3, and **σ_fill** (a band tighter than σ_fill alarms on the tube, not the oil) |
| **4** | a shipped **default** target | ⛔ the jury study, §9.4 |

⭐ **Cheapest open item on the whole gauge: a SECOND BROWN OIL (Hofer).** The over-roast line rests on one
product (S-Budget) measured twice; it reproduced to 1.7 % across a week, a new bottle and a changed recipe, so
it is well *reproduced* and not broadly *calibrated*. One bottle and one evening begin to fix that, and step 2
above is gated on it.

⭐ **A colleague-facing one-pager exists** — `spectracs-references/business/internal/commmunication/
Spectracs_Oil_Panel_2026-08-07.pdf` (+ its generator; not in git, like everything under `business/`). First
result presentable outside the project: the four oils on `M448` alone, the trust numbers, the drift alarm, the
brown alarm, the jury study, and every open caveat on the page. ⚠ **Numbers are transcribed, not recomputed** —
regenerate it when the 448 trim lands, when σ_fill is measured, or when the second brown oil arrives
(`SPEC_capture_quality.md` §16.27.10).

⛔ **A universal ideal of 8.0 was tested against the corpus and refuted**: it would rank the S-Budget closer to
ideal than any Kiendler fill. And a mid-scale target must not sit on uncorrected `M448` — that metric carries a
**9.4 % fill-turbidity artifact**, larger than the deviations it would be reading.

### ⭐⭐ PRIO 3a — THE SMALL VALIDATION *(Edwin 2026-08-06; the milestone that freezes the thresholds)*

**A scaled-down validation that removes the part of PRIO 3 that was months.** Eight shop-available oils is a
weekend's shopping, not a mill relationship; two judges is a diary entry, not a panel.

**Panel — 9 oils, three tiers:**

| tier | n |
|---|---|
| brown (over-roasted) | 2 |
| green, **non-premium** | ⭐ **3** |
| green, **premium** | 4 |

⭐ **The three tiers make this a WITHIN-GREEN test in the field**, which is exactly the half of the capability
claim PRIO 2 measures in the lab. ⇒ P3a is the field confirmation of P2's number, not a separate exercise.

#### ⛔ WHY 3 AND NOT 2 — the panel was originally 2, and 2 cannot reach significance

A rank test on n non-premium against m premium has a **floor on its p-value** set by the number of possible
orderings — reached only when the separation is *perfect*:

| design | best possible p | |
|---|---|---|
| **2 vs 4** *(the original panel)* | **0.067** | ⛔ **cannot reach p < 0.05, however well the instrument performs** |
| 3 vs 3 | 0.050 | ⛔ exactly on the boundary |
| 2 vs 5 | 0.048 | reachable |
| ⭐ **3 vs 4** *(adopted)* | ⭐ **0.029** | reachable |
| 4 vs 4 | 0.014 | reachable |

⇒ **One extra bottle takes the best case from 0.067 to 0.029.** With 2 vs 4 there are only 15 orderings, so
even both non-premium greens landing cleanly below all four premium ones leaves `p = 1/15`. ⚠ The result Edwin
most wants from P3a — *a non-premium green showing up as non-premium* — would have been **suggestive but not
defensible**, and a chemist colleague is exactly the person who would say so.

⚠ **The asymmetry is in your favour and worth knowing before the run:** a **positive** result is clean, a
**null** result is uninformative — if the tiers do not separate you cannot tell whether the metric failed or
whether "premium" simply does not track roast. That is a further reason to spend the bottle that makes the
positive case reachable.

#### ⚠ Green-vs-green is a PROBABILISTIC call, and must be presented as one

Cohen's *d* translated into what it means for judging a **single** sample (error per side, optimal cut):

| case | *d* | error |
|---|---|---|
| **green vs brown**, measured | 7.50 | **0.01 %** — effectively deterministic |
| green vs green, projected best | 5.00 | 0.62 % |
| green vs green, projected low | 3.50 | 4.01 % |
| green vs green, at the ≳ 3 bar | 3.00 | 6.68 % |
| green vs green, **today** | 1.34 | 25.14 % |

⇒ ⭐ **The brown verdict can stay a BADGE; a within-green result must be a RANKING WITH A CONFIDENCE.**
Presenting them identically would overclaim the weaker one by three orders of magnitude.

⇒ And what makes the hoped-for observation *evidence* rather than a coincidence: the judges said so too,
blind, before seeing any output — and the gap exceeds the within-oil scatter. ▶ **Report it as "gap of X
against a within-oil σ of Y", never as a bare ordering.**

**Judges:** two — Edwin (developer) and the colleague (chemist).

#### ⭐⭐ ASK FOR SUB-SCORES, NEVER A TOTAL *(Edwin 2026-08-07 — "put this somewhere so it is not forgotten")*

⛔ **A single overall score cannot answer this study's question.** A tasting total conflates roast with
**rancidity, oxidation and seed quality**. If a green oil scores badly for rancidity, that is **not** evidence
against greenness — and with only a total there is no way to tell the two apart. One bad bottle scored on the
wrong axis would look exactly like the metric failing.

⇒ **The score sheet must separate at least: roast/aroma · rancidity/oxidation · bitterness · overall.**

- the **dependent variable** is the **roast/aroma sub-score** — that is the axis the metric claims to measure;
- the others are **controls**: an oil marked down for rancidity is **excluded from the roast regression**, not
  counted against the metric;
- ⚠ **applies to bought-in scores too** (`SPEC_roast_ampel.md` §9.4's "measure oils that have already been
  juried"): **a source that publishes only totals cannot answer this question.** Say so and use a different
  source rather than regressing on a total.

⭐ Same rule, one level up: **record the market tier and the price at purchase** (§16.27.6b) — a second
variable, never ground truth.

#### ⭐ PARALLEL TASK — the oil shopping *(start it in week 1)*

⚠ **The only item on the whole path that depends on anyone but Edwin.** Everything else is bench or desk work
under his control; this one needs shops to be open and stocked.

- **9 oils: 2 brown · ⭐ 3 non-premium green · 4 premium green.** ⚠ The third non-premium is not padding — see the p-value floor above; at 2 the study cannot reach significance.
- ⛔ **In hand before P3a starts** — on the current schedule that is **Wed 19 August**, i.e. **8 working days
  of lead time** from day 1. It is *lead time, not effort*: a few hours of actual shopping spread over the
  window.
- ⚠ **Keep the receipts and labels.** The premium / non-premium designation is a **second variable** in the
  study, recorded separately from the judges' roast scores — so where each oil sits in the market matters and
  must be written down at purchase, not reconstructed later.
- ⚠ Sourcing 8 oils is what makes P3a cheap where PRIO 3 is expensive. If any tier cannot be filled locally,
  say so early: **2 browns is already the minimum** that lets the class claim rest on more than one bottle
  (§2.2), and dropping to one would put P3a back where the archive is today.

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

### ⭐ INSTRUMENT TEST — the Q-band red-side landmarks as an INTERNAL WAVELENGTH RULER *(added 2026-08-13; easy, no rig time, no calibration capture)*

`SPEC_capture_quality.md` **§16.31.5b**. Two features on the red flank of the Q band sit at the **same place in
every oil measured so far**, green or brown, across eleven fills, two lamps and five weeks:

```
   mu1     narrow sub-band centre (two-component fit)   580.4 +/- 0.4  nm    total spread 1.3 nm
   lam_f   red-flank inflection (d1 minimum)            582.1 +/- 0.36 nm    total spread 1.2 nm
   ---- for contrast ----
   lam_r   BLUE-flank inflection                        563.6                total spread 7.2 nm
```

⇒ ⭐ **If a landmark does not move with oil state, then whatever movement it DOES show is wavelength-scale
error** — measured directly from ordinary oil spectra, with no calibration lamp and no extra capture.

**What it delivers, in order of usefulness:**

1. ⭐⭐ **A bound on scale stability, from the archive, today.** The total spread is **≤ 1.3 nm** *including*
   any genuine oil-to-oil variation. §16.30.7b assumed **±1.5 nm** when it computed the doublet's wavelength
   sensitivity and concluded the doublet was unusable — that assumption was an over-estimate, so the
   objection is partly retired. §16.30.6 lists the scale as an open question; this narrows it without the
   Eu³⁺ capture.
2. ⭐ **A per-capture QC check, in the same family as the DN guard** — flag a run whose measured `lam_f`
   departs from 582.1 by more than a set tolerance. That is a wavelength-calibration alarm the operator
   currently has no way to trigger.
3. A **cross-check on the 607 nm lamp line**, whose apparent wander (§16.30.6b) is currently unexplained —
   two independent position references disagreeing would localise the cause to the lamp rather than the scale.

**Why it is easy.** Both landmarks come out of code already written for `SPEC_capture_quality.md` §16.30.7f /
§16.31.5: the two-component Gaussian fit over 560–590 nm for `mu1`, and a Savitzky-Golay first derivative for
`lam_f`. No new maths, no new hardware, and it runs on every archived spectrum.

⚠ **What it CANNOT do, stated so it is not oversold.** Using the band alone it gives an **UPPER BOUND** on
scale error, not a measurement — it cannot separate *"the scale moved"* from *"the band moved"*. Separating
those needs a reference that is not the pigment: the 607 nm lamp line, or §16.28.4's queued Eu³⁺ capture
(687.7 / 693.7 / 707.0 nm). ⇒ This item **narrows** that question cheaply; it does not replace it.

⚠ And the landmarks' constancy is itself an **observation on eleven fills of six oils**, not a law. A future
oil whose Q band genuinely shifts would be misread as a calibration fault. The tolerance in item 2 should be
set loose enough to allow that, and any alarm should be phrased as *"wavelength check"*, never as a verdict.

### ⚪ LOW PRIORITY — automatic detection of an UNDISSOLVED fill *(added 2026-08-13; the operator's eye already does this well)*

`SPEC_capture_quality.md` **§16.33**. Two oils (`BillaClever`, `Billa Ja! Natürlich`) do **not dissolve** in
the standard capillary recipe — the fill stays visibly muddy. ⭐ Edwin's operator rule is the primary control
and it is sufficient today:

> **If the fill still looks muddy after the standard recipe, do not measure it.**

⚠ **Why it matters that this is caught at all:** a scattering pedestal inflates the small `B_Q` denominator
proportionally more than the numerator, so an undissolved fill reads **brown-shifted with normal-looking
scatter inside a single burst**. It looks confident and it is wrong.

**The automatic version, if it is ever wanted.** The condition is measurable from **two consecutive captures**
with no new hardware — turbidity drift per run:

```
   Billa oils (undissolved)   -27 % per run
   every other oil             -3 .. -8 % per run      <- 7.5x separation
```

⇒ Flag when `A(520-540)` falls by more than ~15 % between consecutive captures of one fill. Cheap, but it
costs the operator a second capture before any verdict, so it is **not** worth building while the visual check
works.

⚠ **n = 2 products, one brand.** Edwin is checking the three remaining untested oils for clarity; until then
the brand association may be coincidence, and whether a different solvent / warming / longer agitation would
dissolve them is **untested** — "cannot be measured" is a statement about THIS protocol, not the product.

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
