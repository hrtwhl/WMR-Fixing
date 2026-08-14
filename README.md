# TRB vs. WMR FX Fixing Analysis

## Project Overview

This project downloads and compares intraday foreign exchange rates from LSEG/Refinitiv for six EUR currency pairs:

- EUR/USD
- EUR/CHF
- EUR/JPY
- EUR/GBP
- EUR/CZK
- EUR/HUF

For each business day, the analysis compares:

1. The traded TRB mid-price at 12:00 and 13:00 local time
2. The corresponding WMR benchmark fixing at 12:00 and 13:00 local time

The objective is to measure how closely traded market prices align with the official WMR fixing rates and to identify systematic differences, outliers, or time-of-day effects.

## Data Collection

Data is retrieved through the LSEG/Refinitiv Data Library for Python using `rd.get_history()`.

Two instrument groups and fields are used:

- **TRB traded rates:** `MID_PRICE`
- **WMR fixing rates:** `QUOTE_VAL`

The WMR timestamps returned by the API are interpreted as UTC and converted to `Europe/Vienna` local time. During the analyzed summer period, this converts 10:00 UTC and 11:00 UTC into 12:00 and 13:00 CEST.

TRB timestamps are retained as provided after confirming that they already represent the required local observation times.

The analyzed period is:

- **Start date:** 2026-01-01
- **End date:** 2026-08-13

## Data Processing

The processing workflow includes:

- Converting timestamps to pandas datetime values
- Converting WMR timestamps from UTC to Vienna local time
- Retaining only the 12:00 and 13:00 local observations
- Removing weekends and duplicate timestamps
- Standardizing instrument names across both datasets
- Joining TRB and WMR observations by timestamp and currency pair
- Checking for missing observations and timestamp mismatches

## Analysis

For every currency pair and fixing hour, the project calculates:

- Signed difference: `TRB - WMR`
- Absolute difference
- Percentage difference
- Difference in basis points
- Mean bias
- Mean absolute error (MAE)
- Root mean squared error (RMSE)
- Median absolute error
- Maximum absolute deviation
- Share of observations within selected basis-point thresholds

The analysis also compares results between the 12:00 and 13:00 fixing times.

## Visualizations

The project includes charts for:

- Distribution of TRB-WMR differences by currency pair and hour
- Mean absolute deviation by pair and fixing hour
- Development of deviations over time
- TRB rates plotted against WMR fixing rates
- Daily deviation heatmaps
- Identification of the largest outliers

## Main Files

- `fx_mid_prices_12_13.csv`: TRB mid-prices at 12:00 and 13:00
- `wmr_fixings_12_13.csv`: WMR fixing values at 12:00 and 13:00
- `analysis.ipynb`: Analysis notebook containing the data preparation, metrics, and visualizations

## Requirements

```bash
pip install pandas numpy matplotlib seaborn refinitiv-data
