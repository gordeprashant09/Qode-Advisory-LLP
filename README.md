# Data Analyst Assignment – F&O Market Analysis

## 1. Overview
This project demonstrates an **end-to-end data analysis pipeline** using **Python, Pandas, and SQLite** on F&O (Futures & Options) market data.

The objective is to:
- Ingest raw exchange data
- Normalize it into an analytical database schema
- Optimize query performance
- Generate meaningful financial insights using SQL and Python

The solution is designed to be **production-realistic**, addressing data quality issues, database limitations, and performance considerations commonly encountered in real-world financial analytics.

---

## 2. Dataset
- **Source file:** `3mfanddo.csv`
- **Domain:** Futures & Options (F&O) market data
- **Granularity:** Daily instrument-level trading data

---

## 3. Data Ingestion
- Raw CSV data is loaded using **Pandas**
- An `exchange` column is added for future multi-exchange extensibility
- Data is stored in SQLite as a **staging table**: `raw_fo_data`

### Why this approach?
- Separates raw and analytical data layers
- Allows easy reprocessing without reloading the CSV
- Mimics real-world ETL pipelines

---

## 4. Database Design (Normalization)
The database follows **Third Normal Form (3NF)** to minimize redundancy and improve maintainability.

### Dimension Tables
- **exchanges** – Exchange master data (NSE, BSE, MCX)
- **instruments** – Trading symbol and instrument type (FUT / OPT)
- **expiries** – Expiry date, strike price, option type (CE / PE)

### Fact Table
- **trades**
  - OHLC prices
  - Volume
  - Open interest
  - Change in open interest

This schema supports scalable analytical queries and clean joins.

---

## 5. Performance Optimization
Indexes were created on high-cardinality and frequently queried columns:
- `trade_date` – Time-series analysis
- `instrument_id` – Join optimization
- `expiry_id` – Option chain queries
- `open_int` – Open interest–based analysis

**Result:** Significant query performance improvement on large datasets (5M+ rows).

---

## 6. Analytical Queries Implemented

### 6.1 Top Symbols by Open Interest Change
Identifies instruments with the highest change in open interest.

**Insight:** Helps detect increasing trader participation and market activity.

---

### 6.2 7-Day Rolling Volatility
- Daily average close prices computed using SQL
- Rolling standard deviation calculated in Pandas

**Why Pandas?**  
SQLite lacks advanced analytical and statistical functions in many builds. In production systems, complex analytics are often performed outside the database layer.

---

### 6.3 Put–Call Ratio (PCR)
Measures market sentiment using options data:
- **PCR > 1:** Bearish sentiment
- **PCR < 1:** Bullish sentiment

---

### 6.4 Option Chain Summary
Aggregates volume and open interest by:
- Expiry date
- Strike price
- Option type (CE / PE)

This forms the basis of derivatives market analysis.

---

### 6.5 Most Active Symbols (Last 30 Days)
Identifies highly traded instruments using time-window queries on indexed dates.

---

## 7. Data Quality Handling
- Detected malformed dates (e.g., 3-digit years)
- Normalized all date formats before analysis
- Ensured rolling calculations used correctly ordered business days

---

## 8. Project Structure
├── data/
│ └── 3mfanddo.csv
├── notebook/
│ └── analysis.ipynb
├── db/
│ └── fo_market.db
├── README.md

---

## 9. How to Run
1. Install dependencies:
   ```bash
   pip install pandas

## 8. Project Structure
