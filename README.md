# RHIE — Rail Health Intelligence Engine

**Far Away 2026 · Team Moshi Moshi Japan**

Physics-informed predictive maintenance dashboard for Indian Railways track. Built for P-Way engineers to get a clear, numbers-backed picture of any track section in under a minute.

---

## The Problem

India has 68,000+ km of track and thousands of trains running every day. The engineers responsible for maintaining that track mostly work off scheduled inspections and experience. There's no quick way to see how a specific section is actually holding up — how much of its life is consumed, whether a detected crack is urgent or manageable, whether the summer heat is pushing a welded rail toward buckling.

RHIE is an attempt to fix that.

---

## What It Does

You put in what you know about a section — where it is, what rail type, how old, how much traffic, any defects detected in the last inspection. The tool runs it through the actual physics of what's happening to that track and gives you back a complete picture.

Four things are computed under the hood:

- **Hertz contact mechanics** — the pressure a wheel exerts on the rail head, whether it's in the plastic regime (ratcheting wear) or elastic
- **Winkler bending model** — bending stress at the rail foot under moving loads, accounting for track quality and speed
- **Paris Law crack propagation** — if a USFD-detected crack is entered, how fast it's growing, how many days until fracture, what speed restriction is warranted
- **CWR thermal stress** — how far the rail temperature is from its stress-free temperature, whether buckling or fracture risk is present

From those, the tool produces a Health Index (0–100), a stage classification (0–4), a ranked defect and repair list tied to IRPWM paragraphs, and a full P-Way assessment with immediate actions, maintenance programme, and TSR recommendation.

A Gradient Boosting ML model also runs silently in the background to classify the dominant defect type and predict remaining useful life in MGT.

---

## Repo Contents

```
index.html          — The dashboard. One file, open in any browser.
Dossier.pdf      — Full technical documentation of every formula,
                        constant, dataset, and model used.
RHIE_ML_metrics.csv   — ML model performance metrics from scikit-learn
                        validation run (1500 samples, 200 trees).
/screenshots          — Simulation screenshots
/simulations          — Simulation videos.
```

Important Context:
1. Z direction= passage of train.
2. Y direction= the loading direction.
3. X direction=lateral.

The simulation videos are NOT exact demonstrations of what is actually happening, the whole system involves hertzian mechanics and contact patch-based physics, which isn't free to use in the simulation software. The simulation however, closely mimics the effects of a train passing by modelling the pressure variation mathematically. The rail segment under observation is simulated for 5 seconds, corresponding to 3-4 axles passing over it. The thermal analysis isn't very prominent but useful in the overall analysis through thermal stresses.


## Running It

Download `RHIE_v3.html`. Open it in a browser. That's it.

No server, no install, no internet required after the file loads. React, ReactDOM and Recharts are bundled directly into the file. Everything — physics, ML training, assessment generation — runs locally in the browser.

Tested on Chrome, Firefox, Edge.

---

## Dashboard Tabs

| Tab | What's in it |
|---|---|
| Track Input | IR Zone → Division → Station selection. 15 track parameters including actual trains/day per type. |
| GMT Tracker | Cumulative GMT chart, wear depth projection, train load breakdown, renewal timeline. |
| Thermal Analysis | Monthly CWR stress chart, seasonal risk table, SFT deviation, risk classification. |
| Health Analysis | Health gauge, ML inference, actionable insights, stress utilisation bars, Paris Law crack trajectory. |
| Defect & Repair | Stage card, governing conditions ranked by impact, top-6 IRPWM-referenced interventions. |
| AI Assessment | Rule-based P-Way report with condition summary, risk ranking, immediate actions, TSR, inspection schedule. |

---

## Coverage

- **20 IMD climate zones** — monthly ambient temperatures and solar gain from 1991–2020 normals, zone-specific SFT for CWR
- **68 IR divisions** — mapped to climate zones, with 4–6 verified stations per division for section validation
- **7 route classes** — HM through SUB, with actual freight axle counts per consist (trains/day is user-editable per section)
- **48 repair methods** — geometry, rail, CWR, sleeper, structural, all with IRPWM references
- **GR1080 IRS T12-2009** — all material constants verified against Nippon Steel K003en test data

---

## ML Model

Gradient Boosting, two tasks:

- **Regression** — remaining useful life in MGT. R² = 0.993, RMSE = 17.1 MGT, 5-fold CV R² = 0.993 ± 0.001
- **Classification** — defect type across 5 classes (No Defect, Corrugation, RCF/Head Wear, Fatigue Crack, CWR Thermal Risk). Accuracy = 93.7%, Weighted F1 = 0.937

Training data is synthetic, generated from the same physics equations used in the dashboard. Full metrics, confusion matrix, feature importance, and training curve are in `RHIE_ML_metrics.csv`. The dossier explains what each number means.

---

## Technical Dossier

`RHIE_dossier.md` covers everything in detail — every physics formula with derivation and worked examples, all 20 climate zones with monthly data tables, the complete health score formula with rationale for each component, ML architecture and all metrics explained in plain language, all 48 repair methods, and the assessment engine logic. If you want to understand why a specific number comes out of the dashboard, it's in there.

---

## Stack

- React 18.2.0 + Recharts 2.8.0 (bundled inline via esbuild)
- scikit-learn 1.8.0 for Python-side ML validation
- No backend, no database, no API

---

*Far Away 2026 · Organised by Zuup · Team Moshi Moshi Japan*
