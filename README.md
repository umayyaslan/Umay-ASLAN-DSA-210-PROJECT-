# DSA210 Project: Do Disasters Increase Internal Out-Migration in Turkey? (2008–2023)

## Overview
This project studies whether major natural disasters lead to measurable “moving-away” behavior from affected provinces in Turkey. The main focus is **internal migration**, especially **out-migration** and **net migration**, rather than total population growth (which is strongly influenced by birth-rates and long-term demographics related economic growth).

## Main Goal
To test if **internal out-migration** changes after major disaster events and to model/predict migration changes using machine learning.

## Research Questions
1. Do disaster events cause a noticeable change in **out-migration** from the affected province?
2. Do disaster events cause a noticeable change in **net migration** (inflow − outflow)?
3. Are the effects short-term shocks or persistent shifts?
4. For earthquakes, do **major-disruptive** events differ from **minor/non-disruptive** events?
5. Can we **predict** post-event migration change using event and province-level features?

## Hypotheses
**H0:** Disasters do not cause a statistically meaningful change in post-event **out-migration growth** and/or **net migration** in the selected provinces.

**H1:** At least one disaster type and/or province shows a meaningful post-event change in **out-migration growth** and/or **net migration**.

## Provinces and Disaster Groups
### Earthquakes
- Istanbul
- Izmir
- Elazığ
- Van
- Kahramanmaraş
- Hatay

### Floods
- Kastamonu
- Sinop

### Wildfires
- Aydın
- Muğla
- Antalya
- Çanakkale

## Event List (t0 dates)
Each event has a defined event date `t0` used for pre/post comparisons.

| Province | Disaster type | Event name | t0 | severity_class |
|---|---|---|---|---|
| Istanbul | Earthquake | 2019 Silivri Offshore / Marmara Sea Earthquake (Mw 5.8) | 2019-09-26 | minor |
| Izmir | Earthquake | 2020 Aegean Sea “Samos (Sisam) / Seferihisar” Earthquake | 2020-10-30 | major |
| Elazığ | Earthquake | 2020 Elazığ–Sivrice Earthquake (Mw 6.8) | 2020-01-24 | major |
| Van | Earthquake | 2011 Van–Erciş Earthquake (Mw 7.2) | 2011-10-23 | major |
| Kahramanmaraş | Earthquake | 6 Feb 2023 Kahramanmaraş Earthquakes (Mw 7.7 / 7.6) | 2023-02-06 | major |
| Hatay | Earthquake | 6 Feb 2023 Kahramanmaraş Earthquakes (Hatay impact) | 2023-02-06 | major |
| Kastamonu | Flood | 11 Aug 2021 Western Black Sea Flood (Bozkurt) | 2021-08-11 | — |
| Sinop | Flood | 11 Aug 2021 Western Black Sea Flood (Ayancık) | 2021-08-11 | — |
| Antalya | Wildfire | 2021 Manavgat Wildfire | 2021-07-28 | — |
| Muğla | Wildfire | 2021 Marmaris Wildfire | 2021-07-29 | — |
| Aydın | Wildfire | 2021 Aydın Wildfires (Karacasu/Çine) | 2021-07-29 | — |
| Çanakkale | Wildfire | 22 Aug 2023 Çanakkale Central Wildfire (Damyeri/Kayadere) | 2023-08-22 | — |

## Earthquake Severity Labeling Rule
Earthquake events are labeled based on real-world impact (not magnitude alone).

- **major-disruptive:** major fatalities and/or large-scale disruption to city functions (housing, infrastructure, public services)
- **minor/non-disruptive:** limited casualties/damage and no evidence of broad disruption

## Data Sources
### Internal Migration (TÜİK)
Core variables:
- Province-level internal migration inflow
- Province-level internal migration outflow
Derived metrics:
- Net migration = inflow − outflow
- Out-migration growth rate (YoY if annual data)

### Disaster Event Data
- Earthquakes: AFAD event catalogue (date/time, location, magnitude, depth)
- Floods/Wildfires: official datasets/reports identifying event timing and affected provinces

## Key Variables
Primary outcomes:
- Out-migration
- Net migration
- Out-migration growth rate (preferred)

Treatment variables:
- Event date `t0`
- Pre/post windows around `t0` (e.g., 1–2 years before vs 1–2 years after)

## Methodology
### 1) Data Preparation
- Collect and clean migration and event datasets
- Convert dates and create time identifiers (year; month if needed)
- Build an event table with `province`, `disaster_type`, `t0`, and `severity_class`

### 2) Exploratory Data Analysis (EDA)
- Plot time series of out-migration and net migration for each province
- Mark `t0` on plots to visualize changes around events
- Plot growth rates (YoY) to track changes in migration momentum

### 3) Event-Based Before/After Analysis
- Compare migration outcomes in fixed windows:
  - Pre-event window
  - Post-event window
- Summarize direction and magnitude of changes by province and disaster type

### 4) Machine Learning (Prediction + Interpretation)
Goal: predict **post-event migration change** using event/province features.

Target (examples, choose based on data frequency):
- Regression: `delta_out_migration_growth = post_window_growth - pre_window_growth`
- Classification: label `increase_out_migration = 1` if post change exceeds a threshold

Possible features:
- Disaster type (earthquake/flood/wildfire)
- Earthquake severity class (major/minor)
- Event magnitude/depth (for earthquakes, if available)
- Pre-event migration trend (baseline growth)
- Province fixed identifiers (one-hot) or region grouping (optional)

Models to try:
- Linear/Logistic Regression (baseline, interpretable)
- Random Forest / Gradient Boosting (nonlinear patterns)

Evaluation:
- Train/test split with time-aware logic if possible
- Metrics: MAE/RMSE (regression) or F1/ROC-AUC (classification)
- Feature importance (tree models) or coefficients (linear models)

## Repository Structure
- `data/raw/` : raw datasets (TÜİK migration, AFAD, flood/wildfire sources)
- `data/processed/` : cleaned and merged datasets
- `notebooks/` : notebooks (EDA, pre/post, ML)
- `src/` : reusable functions/scripts
- `figures/` : exported plots
- `README.md` : this file

## Expected Outputs
- Cleaned province-level migration dataset
- Event list file used in analysis
- Time-series + pre/post comparison plots
- ML model results (metrics + feature importance)
- Short written interpretation and limitations

## Next Steps (workflow)
1. Add the migration dataset (TÜİK) to `data/raw/`
2. Save the event list table as `data/processed/event_list.csv`
3. Clean and merge data into a single analysis table
4. Create EDA plots and pre/post comparison results
5. Build ML dataset (features + target) and train baseline models
