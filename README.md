# DSA210 Project: Do Disasters Increase Internal Out-Migration in Turkey? (2008–2024)

Personal Motivation and Background

The motivation behind this project is not only academic, but also personal. I am an active member of a university search and rescue club, and I currently serve as the president of this organization. Through both training and field experience, I have closely observed how people react psychologically and socially to disaster risks, especially earthquakes.

In many cases, when a major earthquake occurs, people experience intense fear and anxiety in the first days or weeks. During this period, disaster awareness increases, and conversations about preparedness, relocation, or structural safety become more frequent. However, as time passes, this fear often fades. Daily routines return, and many individuals do not take concrete action, either because they underestimate the long-term risk or because they lack the economic and social capacity to change their living conditions.

In large cities, this effect is even stronger. Urban advantages such as employment opportunities, education, healthcare, and social networks often outweigh perceived disaster risks. For many people, especially those with limited economic resources, relocating to another city is simply not a realistic option, even if they are aware of the danger. These observations raised a central question for me: do disasters actually lead to lasting population movements, or do social and economic constraints limit migration responses?

This project is an attempt to explore this question empirically using internal migration data and statistical methods.

Goal: The primary objective of this project is to analyze internal migration flows in Turkey to determine if major disaster events (Earthquakes, Floods, Wildfires) cause statistically significant spikes in out-migration and net-migration. Furthermore, the project aims to use Machine Learning to predict migration shifts based on event severity.

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
| Istanbul | Earthquake | 11 Jan 2020 Marmara Sea / Silivri Offshore Earthquake (Mw 4.7) | 2020-01-11 | minor |
| Izmir | Earthquake | 2020 Aegean Sea “Samos (Sisam) / Seferihisar” Earthquake | 2020-10-30 | major |
| Izmir | Earthquake | 1 Feb 2021 Karaburun Offshore Earthquake (Mw 5.1) | 2021-02-01 | minor |
| Izmir | Earthquake | 4 Nov 2022 Buca (İzmir) Earthquake (Mw 4.9) | 2022-11-04 | minor |
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

