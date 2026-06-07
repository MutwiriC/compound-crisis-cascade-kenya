# Compound Crisis Cascade: Kenya

## How Climate Shocks Trigger Food Price Spikes and Conflict in Kenya's Vulnerable Counties

---

## What This Project Investigates

This project tests whether severe rainfall deficits in Kenyan counties systematically precede sharp increases in staple food prices, which in turn precede a rise in conflict events, and quantifies the lag between each stage of that cascade.

The analysis window covers 19 years (2006-2025), 47 Kenyan counties, and six datasets from the HDX Humanitarian API. All prices are deflated to real 2010 KES to isolate genuine food stress from general inflation.

The goal is to identify the operational window between a climate shock and a protection crisis, when anticipatory humanitarian action is still possible.

---

## Why It Matters

Kenya loses an estimated 2-3% of GDP annually to drought-related crises. The data exists to predict these cascades. What is missing is a tool that reads climate, price, and conflict signals together and in sequence. This project builds the analytical logic for that tool.

---

## What This Project Found

The cascade exists, is conditional on structural vulnerability, and does not appear in well-integrated urban counties.

- Rainfall deficits precede real maize price spikes at a 2-3 month lag across high-vulnerability counties
- Price spikes precede conflict escalation at a 3-4 month lag. Granger temporal precedence confirmed in Marsabit (Rainfall -> Price: F=3.29, p=0.040, lag 2; Price -> Conflict: F=5.42, p=0.021, lag 1). Marginal results in Turkana (p=0.077 and p=0.073). Absent in Mandera (cross-border price dynamics suspected). Reverse direction non-significant in all counties
- The two-link cascade is detectable only in 7 counties with MPI >= 0.30. It is absent in medium and low-vulnerability counties
- Each drought cycle raises the real maize price floor permanently. Cumulative multi-cycle stress, rather than individual events, drives chronic vulnerability
- A below-normal long-rains season in March should trigger price monitoring by May-June and conflict early warning by July-August in ASAL counties

**Negative control:** Kiambu and Nairobi were tested as negative controls. No significant Granger results in either direction. WFP VAM does not monitor these counties because they are not food-insecure markets. The cascade signal in high-vulnerability counties is mechanism-specific, not a Kenya-wide statistical artefact.

**VRI validation:** The Vulnerability Risk Index was validated against IPC Phase 3+ prevalence (2019-2025). 
VRI predicts IPC Phase 3+ outcomes: 
Pearson r = 0.379 (p = 0.047), Spearman rho = 0.388 (p = 0.042), n = 28 counties (2019-2025).

---

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

---

## Data Sources

All data sourced from the HDX Humanitarian API (HDX HAPI); publicly available at https://data.humdata.org/dataset/hdx-hapi-ken

| File                              | Source       | Purpose                      | Coverage                        |
| --------------------------------- | ------------ | ---------------------------- | ------------------------------- |
| `ken-rainfall-subnat-full.csv`    | WFP / CHIRPS | Climate shock indicator      | 47 counties, dekadal, 1981-2025 |
| `wfp_food_prices_ken.csv`         | WFP VAM      | Price spike response         | 27 counties, monthly, 2006-2025 |
| `hdx_hapi_conflict_event_ken.csv` | ACLED        | Violence consequence         | 47 counties, sub-weekly         |
| `hdx_hapi_population_ken.csv`     | UNFPA        | Per capita normalisation     | 47 counties, 2019 census        |
| `hdx_hapi_poverty_rate_ken.csv`   | Oxford OPHI  | Vulnerability stratification | 47 counties, 2022 survey        |
| `hdx_hapi_food_security_ken.csv`  | IPC          | Validation layer             | 47 counties, snapshot-based     |

To reproduce: download the files above and place them in `data/raw/`

---

## Methodology

1. Compute monthly rainfall anomalies against county historical baselines (CHIRPS dekadal RFQ)
2. Deflate staple food prices to real 2010 KES using World Bank CPI
3. Separate ACLED conflict events by type; retain political violence only (battles, violence against civilians, explosions)
4. Exclude 2007-2008 post-election violence period (political outlier unrelated to the climate cascade mechanism)
5. Run Augmented Dickey-Fuller stationarity tests; difference non-stationary series before lag analysis
6. Run cross-correlation analysis with lag windows (t+1 through t+8)
7. Test Granger temporal precedence in both directions — forward cascade and reverse validity check
8. Run negative control analysis on Kiambu and Nairobi (low MPI, high market integration) to confirm cascade specificity
9. Build a county-level Vulnerability Risk Index with bootstrap rank uncertainty (1,000 iterations)
10. Validate VRI against IPC Phase 3+ prevalence (2019-2025)

**Statistical note:** Approximately 90 Granger tests were run at alpha=0.05, producing 4-5 expected false positives by chance. Results are not Bonferroni-corrected (which would be overly conservative for correlated tests on related counties) but are interpreted conservatively: consistency across multiple counties is required before drawing directional inference. No result survives Bonferroni correction; the cascade interpretation rests on directional consistency and CCF pattern, not on individual p-values alone.

---

## Notebook Guide

| Notebook                            | What It Does                                                                                   | Key Output                                                                                                      |
| ----------------------------------- | ---------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------- |
| `01_data_coverage_audit.ipynb`      | Coverage mapping, missingness audit, vulnerability tier classification                         | 23-county analysis scope, temporal overlap chart                                                                |
| `02_data_cleaning.ipynb`            | CPI deflation, conflict disaggregation, panel construction                                     | `02_panel_analysis.csv`                                                                                         |
| `03_eda.ipynb`                      | Drought event deep-dives, tier comparisons, correlation matrix                                 | 12 figures including cascade panels for 2011, 2017, 2022                                                        |
| `04_lag_correlation.ipynb`          | ADF tests, cross-correlation functions, Granger temporal precedence, negative control analysis | Lag structure confirmed at 2-3 and 3-4 months; negative control results in `04_negative_control_comparison.csv` |
| `05_vulnerability_risk_index.ipynb` | Composite VRI, bootstrap uncertainty, IPC validation                                           | County priority targeting table, all 47 counties                                                                |

---

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
│   └── tables/       # Summary CSVs including Granger results and negative control comparison
│
└── README.md
```

---

## Limitations

**Predictive precedence, not structural causation.** Granger tests confirm that lagged rainfall deficits improve prediction of price changes, and lagged price changes improve prediction of conflict events. This is temporal precedence, not structural causation. Unmeasured confounders — governance quality, humanitarian aid flows, road access, cross-border trade — may independently drive both prices and conflict. The household-level behavioural mechanism (how rising prices change conflict participation decisions) is not resolved by this aggregate analysis.

**Food price data gap from 2021.** WFP VAM price coverage ends 2020-2021 for most ASAL counties. The 2022 La Nina drought window shows clear rainfall deficits and conflict escalation but price transmission cannot be confirmed for that event. The primary Granger analysis window is therefore 2006-2020.

**MPI post-dates the analysis window.** The Oxford OPHI MPI data is from the 2022 survey, which post-dates the 2009-2020 analysis window. County poverty rankings are assumed relatively stable over this period. This is a reasonable assumption for structural deprivation measures but is acknowledged as an approximation.

**Food price imputed for 38 counties.** WFP VAM directly monitors 9 counties. The remaining 38 counties use former-province median imputation, with a Kenya-wide median fallback. Imputed values are flagged throughout the analysis and excluded from the primary Granger tests, which use only directly monitored counties.

**Granger analysis restricted to three counties.** Mandera, Turkana, and Marsabit are the only counties with sufficient contiguous maize price data (minimum 36 months) for time-series analysis. Results do not generalise to all seven high-vulnerability counties.

**Negative control is data-limited.** Kiambu and Nairobi lack WFP VAM price monitoring, which prevented formal Granger testing on control counties. The absence of price monitoring is itself evidence of lower food security risk in those counties, but a formal statistical rejection of the cascade in control counties was not possible with available data.
