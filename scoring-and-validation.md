# Scoring and validation logic

This document exists so judges and future contributors can see exactly how StreamPulse turns raw readings into the numbers shown on screen — nothing is a black box in the prototype.

## Health score (0–100)

Computed per site from its four latest readings:

```
score = 100
score -= min(35, turbidity_NTU * 1.6)              # cloudier water penalises more, capped
score -= min(25, max(0, 8.5 - dissolved_O2) * 18)   # oxygen below 8.5 mg/L penalises, capped
score -= min(30, ecoli_cfu_per_100mL / 12)          # higher bacterial indicator penalises, capped
score += (macroinvertebrate_index - 60) * 0.15      # more/diverse stream life is rewarded
score = clamp(score, 4, 98)
```

Bands: **Good** (≥70) · **Watch** (45–69) · **Elevated risk** (<45).

This is a simple, transparent weighted heuristic chosen for explainability, not a calibrated water-quality index. A production version should be validated against an established index (e.g. a national/regional stream condition index) and reviewed with domain scientists before it informs any real decision.

## Explainable AI-assist validation (human-in-the-loop)

This is a **real statistical method**, computed live in the browser — not a scripted message and not a trained neural network. When a volunteer submits a reading, StreamPulse runs a **z-score anomaly check** against that site's own recent history:

```
history = site's last 10 turbidity readings
mean, sd = mean(history), standard_deviation(history)
z = (submitted_value − mean) / sd
flag = |z| > 2        # > 2 standard deviations from the site's own norm
```

A z-score is a standard, well-understood statistical technique for outlier detection (it's the same idea behind most simple anomaly-detection systems): values beyond ±2σ fall outside roughly the middle 95% of a typical distribution, so they're worth a second look without being definitely wrong.

If flagged, the person sees:
- **what** looked inconsistent — their exact z-score and the site's mean, in plain numbers,
- **why it might have happened** — a short list of plausible causes (recent rain, a test-strip timing issue, or a real, newsworthy change),
- **two explicit actions** — keep the reading as reported, or go back and re-check it.

Nothing is auto-corrected or silently dropped. This mirrors the Track 3 brief directly: statistics support the assessment by surfacing a check a person might miss, but the person — not the system — decides what's true.

**Why a z-score and not a trained ML model:** it's fully transparent (a judge or a volunteer can verify the math by hand), needs no training data or external API calls, and works entirely client-side — important since this prototype is a static published page. A production version would extend this to a multi-metric ensemble (checking turbidity, oxygen, and E. coli together) and could incorporate a trained model once enough labeled QA/QC history exists — see [`roadmap.md`](roadmap.md).

## Early-warning forecast (real predictive model)

The Insights banner and forecast cards are driven by **least-squares linear regression**, fit live on each site's last 7 turbidity readings:

```
fit a line  y = slope·x + intercept  through the last 7 readings (least squares)
projected_value = intercept + slope × (7 + 2)     # 2 days ahead
alert = projected_value ≥ 12 NTU  AND  current_value < 12 NTU   # about to cross, not already over
```

This is a genuine (if simple) time-series forecasting technique, not a hardcoded warning message — the slope, the projected value, and which sites are flagged are all recalculated from whatever data is currently in each site's array. Swap in real sensor data and the same code keeps working.

**Production extension:** move from a same-metric trend line to a model that also takes real precipitation forecasts and catchment features (impervious surface %, distance from outfalls) as inputs, as described in [`roadmap.md`](roadmap.md).
