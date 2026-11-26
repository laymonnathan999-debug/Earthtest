# THE ANTICIPATION TEST — OBSERVATIONAL PROTOCOL

**Hypothesis (signed):** a climate-system state variable at time *t* leads computed insolation at *t* + Δ, with Δ > 10 kyr, after controlling for insolation history.

**If supported:** something in the Earth system holds a model of a future forcing. **If not:** it is an integrator, i.e., a metabolism.

---

## 1. THE FATAL TRAP, STATED FIRST

**Most deep-time climate records are orbitally tuned.** LR04, the standard benthic δ¹⁸O stack, was constructed by adjusting core chronologies until the record matched a target orbital curve. Any lead or lag measured against that curve is **an artefact of the tuning, by construction.**

This single issue invalidates most of the literature. **Rule 1: only records with chronologies independent of the insolation curve being tested.**

| Chronology type | Independent? | Use? |
|---|---|---|
| Orbital tuning to obliquity/precession | **No** | **Exclude** |
| Layer counting (ice, varves) | Yes | Include |
| U–Th dated speleothems | **Yes — absolute** | **Primary** |
| ⁴⁰Ar/³⁹Ar volcanic horizons | Yes | Include |
| ¹⁰Be / geomagnetic tie points | Partly | Include with caveat |
| Bacon/modelled age–depth with ¹⁴C | Yes, with uncertainty | Include |

---

## 2. DATASETS

| Role | Dataset | Why |
|---|---|---|
| **Primary system variable** | Hulu / Sanbao speleothem δ¹⁸O (Cheng et al.) | **U–Th absolutely dated**, 1–10 yr sampling, monsoon intensity tracks precession |
| Secondary | EPICA Dome C δD, CO₂ (Bereiter et al. 2015) | Independent tie points; 2 Myr span |
| Secondary | Lake Suigetsu varves; Cariaco Basin | Varve-counted, independent |
| Tertiary | Antarctic ice cores with layer counting (WDC, EDML) | Upper sections only, where counting is secure |
| **Forcing** | **La2011 / La2004 orbital solution (Laskar)** | Computed from celestial mechanics, independent of any climate proxy |
| Cross-check forcing | Berger & Loutre (1991) | Two independent orbital solutions — disagreement bounds the forcing uncertainty |
| Excluded | LR04, Lisiecki–Raymo stack; any tuned record | Circular by construction |

---

## 3. PREPROCESSING

1. **Interpolate to a 1 kyr grid** (precession Nyquist is 11.5 kyr — 1 kyr is ample).
2. **Detrend** with a high-pass at 400 kyr, to remove secular cooling and the 400-kyr eccentricity beat.
3. **Bandpass** separately into precession (19–24 kyr), obliquity (40–42 kyr), eccentricity (95–125 kyr). Test each band independently — a lead in one band and a lag in another is a different result from a global lead.
4. **Zero-phase filtering only.** Any causal filter introduces a phase shift that can mimic a lead. Verify the filter's own phase response on synthetic data before use.
5. **Propagate age uncertainty explicitly.** Most records carry ±2–5 kyr; the effect being tested is 10 kyr. Age uncertainty must enter the null model, not be ignored.
6. **Never linearly interpolate between sparse dates.** Use a proper age–depth model (Bacon) carrying its posterior.

---

## 4. PRE-REGISTRATION DOCUMENT

Must contain, committed and hashed before analysis:

1. **The signed hypothesis**: peak at Δ > 0, specified as greater than +10 kyr.
2. **The Δ grid**: 0 to +25 kyr in 1 kyr steps, plus the correction for testing 26 values.
3. **Every dataset**: version, DOI, download date, and **the chronology used for each**.
4. **The exclusion list** and the reason for each exclusion — written before looking.
5. **The complete pipeline**, as executable code, with a hash of the repository.
6. **The statistic**: band-specific cross-correlation with complex demodulation for a time-varying phase; Granger causality as a secondary test.
7. **The null models** (all three required):
   - **Phase-randomised surrogates** preserving the power spectrum — destroys phase, preserves everything else.
   - **Age-model perturbation** within the published uncertainty — if the lead vanishes under age resampling, it was never there.
   - **Synthetic climate** forced only by insolation through a known physical response function — establishes what a *lag* looks like in this pipeline.
8. **The decision rule** (§6 below), stated numerically.
9. **The stopping rule**: one analysis, one look. No re-running with new filters.

---

## 5. SELECTION EFFECTS THAT MANUFACTURE A LEAD — AND THE FIXES

| # | Effect | How it fakes a lead | Fix |
|---|---|---|---|
| 1 | **Orbital tuning of the chronology** | Forces zero lag or whatever lag the tuner assumed | Independent chronologies only |
| 2 | **Tuning to a different orbital parameter** | Tuning to obliquity and testing against precession creates a spurious lead | Test only against the parameter *not* used in tuning |
| 3 | **Record selection** | With 50 records, some show a lead by chance | Pre-register the list, or require the effect in a stack of all qualifying records |
| 4 | **Interval selection** | Terminations look different from inceptions | Full record, or pre-registered windows |
| 5 | **Filter phase error** | Causal filtering shifts phase | Zero-phase filters, validated on synthetic data |
| 6 | **Interpolation artefacts** | Linear interpolation between sparse dates shifts phase | Proper age–depth modelling |
| 7 | **Look-elsewhere over Δ** | Best of 26 lags is inflated | Bonferroni over the Δ grid |
| 8 | **Aliasing** | Coarse sampling wraps precession onto a longer period | Require ≤1 kyr sampling |
| 9 | **Shared trend** | Both series trend together at all lags | Detrend, then test against phase-randomised surrogates |
| 10 | **Analyst confirmation bias** | The analyst expects a lag and finds one — or wants a lead | **Blind the analysis** (§5a) |

### 5a. THE PIPELINE CALIBRATION — the decisive safeguard

**Before touching real data, run the entire pipeline on synthetic records with known phase relationships** — some with a 10 kyr lead, some with a 10 kyr lag, some with zero, randomised and **unlabelled**.

**Acceptance criterion: the pipeline must recover the true sign and magnitude in ≥95% of injections, with no systematic bias.**

> **If the pipeline cannot distinguish a 10 kyr lead from a 10 kyr lag on data where the answer is known, it cannot be trusted on data where it isn't.**

This is the step that separates a measurement from a pattern. It should be done first, published as a methods paper, and the code released.

---

## 6. DECISION RULE

| Outcome | Criterion |
|---|---|
| **SUPPORT** | Peak at **Δ ≥ +10 kyr**, p < 0.001 after Bonferroni over the Δ grid **and** over records; recovered in **≥2 independent records**; survives age-uncertainty resampling; **and** the pipeline passed calibration |
| **REFUTATION** | Peak at **Δ ≤ 0** in all bands, or no peak above the surrogate 99.9th percentile |
| **INCONCLUSIVE** | Peak between 0 and +10 kyr, or age uncertainty ≥ effect size, or calibration failed |

---

## 7. COST AND FEASIBILITY

| Item | Value |
|---|---|
| New observations required | **None** |
| Data | All public |
| Compute | A laptop |
| Dominant cost | **The chronology work**, not the statistics |
| Realistic duration | 4–8 weeks with proper calibration |

**The reason this has not been done:** nobody had a reason to look at a *lag* as a sign rather than a nuisance. The data have existed for decades.
