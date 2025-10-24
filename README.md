# Trading-Dashboard

> Note: `Trading Dashboard.pbix` is the Power BI file containing the data model, Power Query transformations, visuals, and measures.

---

## Purpose

Analyze how trader behavior (profitability, risk, volume, leverage) aligns or diverges from market sentiment (Fear vs Greed). The dashboard allows slicing by sentiment, side (buy/sell), symbol and date to reveal trading patterns.

---

## Data sources

- `trader_data.csv` — Historical trades from Hyperliquid (columns include: `account`, `symbol`, `execution price`, `size`, `side`, `time`, `start position`, `event`, `closedPnL`, `leverage`, ...).
- `sentiment.csv` — Bitcoin Fear & Greed index (columns: `Date`, `Classification` with values `Fear` or `Greed`).

> The `csv_files/` folder contains cleaned and merged CSVs used for export/archival.

---

## How to reproduce (Power BI Desktop)

1. **Open Power BI Desktop** (recommended version: Oct 2025 or latest stable).
2. **Get Data → Text/CSV** → load `trader_data.csv` and `sentiment.csv`.
3. In **Transform Data** (Power Query Editor) apply the following transforms (see `Power Query` snippet below):
   - Standardize column names
   - Convert `time` to Date/Time
   - Add `Trade Date` (Date only)
   - Remove nulls/invalid rows
   - Merge `trader_data` with `sentiment` on `Trade Date` = `Date`
   - Expand `Classification` into the trader table
4. **Close & Apply** to load data into the model.
5. Add the DAX measures (see `DAX snippets` below).
6. Recreate visuals according to the Dashboard Layout:
   - KPI cards: Total PnL, Average Leverage, Total Trades, Profitability %
   - Profit by Sentiment: Clustered column
   - Volume trend: Line chart
   - Leverage vs PnL: Scatter chart (X=leverage, Y=closedPnL, Legend=Classification)
   - Sentiment timeline: Area chart of counts over time
7. Use slicers: `Classification`, `side`, `symbol`, `Trade Date`.

---

## Files included

- `Trading Dashboard.pbix` — full Power BI report (primary deliverable).
- `csv_files/trader_data_cleaned.csv` — cleaned trade table exported from Power Query (for reproducibility).
- `csv_files/merged_trader_sentiment.csv` — merged dataset exported.
- `outputs/` — PNG exports of key visuals.
- `ds_report.pdf` — this report exported to PDF.

---

## Short description of major transforms

- `time` → converted to DateTime; `Trade Date` extracted (Date).
- Left-join of `trader_data.Trade Date` to `sentiment.Date`.
- Created categorical/flag columns (e.g., `ProfitFlag`, `TradeDirection`).
- Ensured numeric columns are typed as Decimal (size, closedPnL, leverage).

---

## How to refresh

- In Power BI Desktop, **Home → Refresh** will re-run Power Query steps and reload data if the CSVs are updated.
- If using a data source in a folder or web link, confirm the path is accessible to the environment.
- If deployed to Power BI Service, configure dataset scheduled refresh and data gateway if CSVs remain on-prem or in a private drive.

---

## Reproducibility notes

- Keep the same column names; the Power Query steps expect `time` and `leverage` columns.
- If `leverage` has a different name (e.g., `use_leverage`), update the Query step renames accordingly.
- All visuals derive from the merged table `trader_data_merged`.

---

## Contact / Questions

If anything needs clarification (column mappings, extra measures, or alternative visuals), open an issue in this repo or contact <mkaishu2@gmail.com>.

