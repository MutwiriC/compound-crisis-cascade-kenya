# Compound Crisis Cascade: How Climate Shocks Trigger Food Price Spikes and Conflict in Kenya's Vulnerable Counties

## What This Project Investigates

This project tests whether severe rainfall deficits in Kenyan counties systematically precede sharp increases in staple food prices, which in turn precede a rise in conflict events, and quantifies the lag between each stage of that cascade.

The goal is to identify the 6–8 week window between a climate shock and a protection crisis, when anticipatory humanitarian action is still possible.

## Why It Matters

Kenya loses an estimated 2–3% of GDP annually to drought-related crises.
The data exists to predict these cascades. What is missing is a tool that reads climate, price, and conflict signals together and in sequence.
This project builds the analytical logic for that tool.

## Data Sources

All data sourced from the HDX Humanitarian API (HDX HAPI);
publicly available at https://data.humdata.org/dataset/hdx-hapi-ken

| File                            | Source      | Purpose                      |
| ------------------------------- | ----------- | ---------------------------- |
| hdx_hapi_rainfall_ken.csv       | WFP         | Climate shock indicator      |
| hdx_hapi_food_price_ken.csv     | WFP         | Price spike response         |
| hdx_hapi_conflict_event_ken.csv | ACLED       | Violence consequence         |
| hdx_hapi_population_ken.csv     | UNFPA       | Per capita normalisation     |
| hdx_hapi_poverty_rate_ken.csv   | Oxford OPHI | Vulnerability stratification |
| hdx_hapi_food_security_ken.csv  | IPC         | Validation layer             |

To reproduce: download the files above and place them in data/raw/

## Methodology

1. Compute monthly rainfall anomalies against county historical baselines
2. Isolate staple food price trends (maize, beans, sorghum)
3. Separate ACLED conflict events by type; retain political violence only
4. Run cross-correlation analysis with lag windows (t+1 through t+8)
5. Test Granger causality between the three variable sequences
6. Build a county-level vulnerability index for policy targeting

## Notebook Guide

| Notebook                    | What It Does                          |
| --------------------------- | ------------------------------------- |
| 01_data_exploration         | Coverage mapping, missingness audit   |
| 02_data_cleaning            | Anomaly computation, filtering, joins |
| 03_lag_correlation_analysis | Cross-correlation with lag windows    |
| 04_granger_causality        | Predictive sequencing tests           |
| 05_vulnerability_index      | County risk scoring and mapping       |

## Key Findings

_(To be updated as analysis progresses)_

## Author

Christine Mutwiri — Data Scientist | Nairobi, Kenya  
Building at the intersection of humanitarian data and anticipatory action.  
[LinkedIn](https://www.linkedin.com/in/christinekmutwiri/)
