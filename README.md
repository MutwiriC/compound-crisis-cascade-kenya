# Compound Crisis Cascade: How Climate Shocks Trigger Food Price Spikes and Conflict in Kenya's Vulnerable Counties

## What This Project Investigates

This project tests whether severe rainfall deficits in Kenyan counties systematically precede sharp increases in staple food prices, which in turn precede a rise in conflict events, and quantifies the lag between each stage of that cascade.

The analysis window covers 19 years (2006-2025), 47 Kenyan counties, and six datasets from the HDX Humanitarian API. All prices are deflated to real 2010 KES to isolate genuine food stress from general inflation.

The goal is to identify the operational window between a climate shock and a protection crisis, when anticipatory humanitarian action is still possible.

## Why It Matters

Kenya loses an estimated 2-3% of GDP annually to drought-related crises.
The data exists to predict these cascades. What is missing is a tool that reads climate, price, and conflict signals together and in sequence.
This project builds the analytical logic for that tool.

## What This Project Found

The cascade exists and it's conditional.

- Rainfall deficits precede real maize price spikes at a **2-3 month lag** across high-vulnerability counties
- Price spikes precede conflict escalation at a **3-4 month lag** - Granger causality confirmed in Mandera and Turkana
- The two-link cascade is detectable only in **7 counties with MPI >= 0.30** - it is absent in medium and low-vulnerability counties
- Each drought cycle raises the real maize price floor permanently. Cumulative multi-cycle stress rather than individual events, drive the chronic vulnerability

A below-normal long-rains season in March should trigger price monitoring by May-June and conflict early warning by July-August in ASAL counties.

## The 7 High-Risk Counties

The cascade is concentrated in Kenya's arid and semi-arid land (ASAL) counties:

| Rank | County     | MPI  | Tier |
| ---- | ---------- | ---- | ---- |
| 1    | Turkana    | 0.50 | High |
| 2    | Mandera    | 0.46 | High |
| 3    | Samburu    | 0.42 | High |
| 4    | Wajir      | 0.40 | High |
| 5    | Tana River | 0.38 | High |
| 6    | West Pokot | 0.35 | High |
| 7    | Marsabit   | 0.35 | High |

Geographically undifferentiated interventions will miss the cascade signal entirely.

## Data Sources

All data sourced from the HDX Humanitarian API (HDX HAPI);
publicly available at https://data.humdata.org/dataset/hdx-hapi-ken

| File                              | Source       | Purpose                      | Coverage                        |
| --------------------------------- | ------------ | ---------------------------- | ------------------------------- |
| `ken-rainfall-subnat-full.csv`    | WFP / CHIRPS | Climate shock indicator      | 47 counties, dekadal, 1981-2025 |
| `wfp_food_prices_ken.csv`         | WFP VAM      | Price spike response         | 27 counties, monthly, 2006-2025 |
| `hdx_hapi_conflict_event_ken.csv` | ACLED        | Violence consequence         | 47 counties, sub-weekly         |
| `hdx_hapi_population_ken.csv`     | UNFPA        | Per capita normalisation     | 47 counties, 2019 census        |
| `hdx_hapi_poverty_rate_ken.csv`   | Oxford OPHI  | Vulnerability stratification | 47 counties, 2022 survey        |
| `hdx_hapi_food_security_ken.csv`  | IPC          | Validation layer             | 47 counties, snapshot-based     |

To reproduce: download the files above and place them in `data/raw/`

## Methodology

1. Compute monthly rainfall anomalies against county historical baselines (CHIRPS dekadal RFQ)
2. Deflate staple food prices to real 2010 KES using World Bank CPI
3. Separate ACLED conflict events by type; retain political violence only (battles, violence against civilians, explosions)
4. Exclude 2007-2008 post-election violence period (this is a political outlier unrelated to the climate cascade)
5. Run Augmented Dickey-Fuller stationarity tests; difference non-stationary series before lag analysis
6. Run cross-correlation analysis with lag windows (t+1 through t+8)
7. Test Granger causality in both directions - forward cascade and reverse validity check
8. Build a county-level Vulnerability Risk Index with bootstrap rank uncertainty (1,000 iterations)

## Notebook Guide

| Notebook                            | What It Does                                                           | Key Output                                               |
| ----------------------------------- | ---------------------------------------------------------------------- | -------------------------------------------------------- |
| `01_data_coverage_audit.ipynb`      | Coverage mapping, missingness audit, vulnerability tier classification | 23-county analysis scope, temporal overlap chart         |
| `02_data_cleaning.ipynb`            | CPI deflation, conflict disaggregation, panel construction             | `02_panel_analysis.csv`                                  |
| `03_eda.ipynb`                      | Drought event deep-dives, tier comparisons, correlation matrix         | 12 figures including cascade panels for 2011, 2017, 2022 |
| `04_lag_correlation.ipynb`          | ADF tests, cross-correlation functions, Granger causality              | Lag structure confirmed at 2-3 and 3-4 months            |
| `05_vulnerability_risk_index.ipynb` | Composite VRI, bootstrap uncertainty, IPC validation                   | County priority targeting table, all 47 counties         |

## Project Structure

```
compound-crisis-cascade-kenya/
│
├── data/
│   ├── raw/          # Source files from HDX HAPI (not tracked)
│   └── processed/    # Clean CSVs and panel outputs
│
├── notebooks/        # Five analysis notebooks (run in order)
│
├── outputs/
│   ├── figures/      # All charts (PNG, 150 DPI)
│   └── tables/       # Summary CSVs
│
└── README.md
```

## Limitations

- Food price data has no WFP VAM coverage post-2021 for most ASAL counties. The 2022 drought window shows rainfall and conflict but not price transmission
- MPI is from the 2022 survey, which post-dates the 2009-2020 analysis window; county poverty rankings are assumed stable
- Granger causality confirms predictive precedence, not structural causation — the household-level mechanism is not resolved by this analysis
- Food price imputed for 38 counties using former-province medians; imputed values flagged throughout

## Author

Christine Mutwiri — Data Scientist | Nairobi, Kenya  
Building at the intersection of humanitarian data, anticipatory action, and behavioural data science.  
[LinkedIn](https://www.linkedin.com/in/christinekmutwiri/)
