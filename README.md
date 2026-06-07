# S&P 500 Market Analysis and Portfolio Tracking Dashboard

## Project Purpose
This project delivers an end-to-end data engineering and business intelligence solution that automates the ingestion, transformation, storage, and visualization of equity market data. By compiling over 425,000 rows of historical time-series data for all 503 constituent companies of the S&P 500 index, the pipeline calculates key risk-reward metrics and benchmarks a multi-asset investment portfolio against the broader market index.

---

## System Architecture and Data Pipeline

### 1. Data Ingestion and ETL (Python & Jupyter Notebook)
The data pipeline executes sequentially within a modular Jupyter Notebook environment:
* **Ingestion:** Scrapes real-time constituent listings and Global Industry Classification Standard (GICS) sector metadata from Wikipedia, then programmatically extracts daily historical pricing arrays spanning 2023 through 2026 using the `yfinance` API.
* **Transformation:** Utilizes `pandas` to concatenate multi-year data, melt data frames from wide to long format, filter anomalies, and map sector metadata.
* **Feature Engineering:** Computes time-series financial metrics including dynamic Daily Returns, 30-day Simple Moving Averages (SMA), 30-day Rolling Volatility, and Cumulative Returns per equity.
* **Portfolio Vectorization:** Ingests a matrix of unique buy-order transactions, accounts for variable purchase dates, aggregates holding balances, and vectorizes daily blended portfolio performance against matching S&P 500 temporal baselines.

### 2. Database Layer (SQLite)
Processed operational data and index baselines are pushed to a relational `SQLite` database. Structured SQL queries utilize table `JOIN` operations on data keys to generate clean, tabular views for the visualization engine.

---

## Power BI Data Model and Analytics Layer

### Relational Schema
Five core data tables are joined within Power BI via relationship keys:
* `stock_prices`: Historical daily granularity tracking prices, moving averages, and volatility arrays.
* `sp500_index`: Benchmark index closing values and matching cumulative returns.
* `master_summary`: Static tabular dimensions tracking rank matrices, classification categories, and index-relative performance.
* `portfolio_analysis`: Transactional table mapping individual asset weights, cost bases, market values, and alpha generation metrics.
* `portfolio_daily`: Time-series log tracking blended daily portfolio valuation fluctuations against index trends.

### Core DAX Measures Engineered
* **Beats Matrix:** Aggregates total equities outperforming the S&P 500 based on minimum row-context summarizations.
* **Alpha Identification:** Isolates maximum and minimum percentage yields across portfolio holdings via `CALCULATE` and `TOPN` functions.
* **Dynamic Benchmarking:** Computes weighted asset returns and cross-report filtration capabilities using native `MAX()` expressions to secure row-context compatibility in tabular visuals.

---

## Visual Deliverables

### Page 1: Market Overview
* **Purpose:** Evaluates macroeconomic health and sector momentum across 503 equities.
* **Components:** Chronological index trend line chart, sector performance distributions, and top/bottom 10 equity leaderboard matrices.

### Page 2: Portfolio Analyzer
* **Purpose:** Provides comprehensive performance accountability for a $23.4K capital investment matrix.
* **Components:** Chronological blended performance comparison chart mapping portfolio value against the S&P 500 on a shared axis, transactional ledger tables with asset weighting, and automated gain/loss indicators.

### Page 3: Stock Explorer 
* **Purpose:** Facilitates user-driven single-stock exploratory data analysis.
* **Components:** Slicers for ticker matching and date scopes, a cumulative return comparison line chart, and a risk-return scatter plot mapping volatility against yield across all 503 constituents.
