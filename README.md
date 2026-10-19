# Longitudinal Pediatric Vaccine Surveillance & Relational Analytics Pipeline

A reproducible epidemiological data engineering and analytics pipeline built on official WHO/UNICEF Estimates of National Immunization Coverage (WUENIC 2023) across 164 countries and 16 pediatric antigens (2000–2023).

---

## Public Health Motivation
Routine immunization is the cornerstone of pediatric global health and child survival. Evaluating multi-year national trajectories, tracking the introduction of life-saving vaccines (e.g., Pneumococcal Conjugate Vaccine - PCV), and quantifying systemic pandemic disruptions require transforming fragmented surveillance registries into structured, queryable relational databases.

---

## Dataset Description & Source
* **Dataset Name:** WHO/UNICEF Estimates of National Immunization Coverage (WUENIC 2023 Revision)
* **Source Platform:** [UNICEF Data Warehouse](https://data.unicef.org/topic/child-health/immunization/) / [Kaggle Dataset (`fahimvj/immunization-data-unicef`)](https://www.kaggle.com/datasets/fahimvj/immunization-data-unicef)
* **File Used:** `wuenic2023rev_web-update.xlsx`
* **Coverage Scope:** 164 countries/territories, 16 core pediatric vaccines/doses (BCG, DTP1, DTP3, HEPB3, HEPBB, HIB3, IPV1, IPV2, MCV1, MCV2, MENGA, PCV3, POL3, RCV1, ROTAC, YFV) from 2000 to 2023.

---

## Pipeline Architecture & Data Engineering

1. **Automated Sheet Ingestion & Unpivoting:**
   * Ingested multi-sheet Excel records where each antigen was recorded as a wide-format matrix ($Year$ columns from `2000` to `2023`).
   * Unpivoted (melted) the sheets into a normalized longitudinal cohort: `[unicef_region, iso3, country, vaccine, year, coverage_pct]`, compiling **58,080 surveillance records**.

2. **Embedded Relational SQL Schema:**
   * Streamed cleaned records into an embedded SQLite database (`wuenic_immunization.db`).
   * Executed structured analytical queries and relational self-joins for cohort aggregation, dropout rate tracking, and disruption profiling.

3. **Interactive Surveillance Reporting:**
   * Constructed unified epidemiological time-series visual benchmarks using Plotly.

---

## Key Epidemiological Findings

### 1. Bangladesh Pediatric Immunization Trajectory (2005–2023)
![Bangladesh Coverage](assets/bangladesh_trajectory.png)
* **High Sustained Baseline:** Core routine vaccines (BCG, DTP1, DTP3, MCV1) maintained high, steady coverage (>94% to 99%) over the past two decades.
* **Rapid PCV Scale-Up:** Following the national introduction of the Pneumococcal Conjugate Vaccine (PCV3) in 2015 at 48% baseline, coverage rapidly climbed to **97% in 2016** and reached **99% by 2017**, demonstrating exceptional programmatic uptake.

### 2. South Asia Regional Benchmarking (DTP3 Completion)
![South Asia DTP3](assets/south_asia_dtp3.png)
* **Regional Resilience:** Bangladesh demonstrated superior stability in third-dose diphtheria-tetanus-pertussis (DTP3) coverage (~98%), remaining insulated from major drops during 2020–2021.
* **Pandemic Shock Contrast:** Comparative peer tracking shows sharp declines during COVID-19 across regional neighbors (India fell from 91% in 2019 to 85% in 2020–2021; Nepal declined from 93% to 84%), followed by varying recovery trajectories post-2022.

### 3. COVID-19 Immunization Disruption Analysis
A SQL self-join evaluating national DTP3 coverage between pre-pandemic (2019) and pandemic (2021) periods isolated severe global delivery shocks:
* **DPR Korea:** 97.0% $\rightarrow$ 41.0% (**-56.0% drop**)
* **Myanmar:** 90.0% $\rightarrow$ 37.0% (**-53.0% drop**)
* **Mozambique:** 85.0% $\rightarrow$ 56.0% (**-29.0% drop**)
* **Vanuatu:** 90.0% $\rightarrow$ 62.0% (**-28.0% drop**)
* **Djibouti:** 85.0% $\rightarrow$ 59.0% (**-26.0% drop**)
