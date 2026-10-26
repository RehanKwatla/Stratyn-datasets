---
language:
- en
license: mit
tags:
- finance
- stock-market
- india
- nse
- bse
- time-series
- ohlcv
task_categories:
- time-series-forecasting
pretty_name: Indian Market Data (NSE/BSE)
size_categories:
- 1M<n<10M
configs:
- config_name: indices
  data_files:
  - split: train
    path: indices/*.parquet
- config_name: etfs
  data_files:
  - split: train
    path: etfs/*.parquet
- config_name: commodities
  data_files:
  - split: train
    path: commodities/*.parquet
- config_name: forex
  data_files:
  - split: train
    path: forex/*.parquet
- config_name: stocks
  data_files:
  - split: train
    path: stocks/*.parquet
---

# Indian Market Data (NSE/BSE)

Production-grade historical OHLCV dataset for Indian financial markets.
Auto-updated daily via GitHub Actions.

## Dataset Summary

| Metric | Value |
|--------|-------|
| Total Files | 2704 |
| Total Size | 289.9 MB |
| Last Updated | 2026-10-26 06:26 UTC |
| Update Frequency | Daily (weekdays) |
| Source | Yahoo Finance via yfinance |

### Asset Coverage

| Asset Type | Symbols | Size |
|-----------|---------|------|
| Stocks | 2654 | 281.3 MB |
| Indices | 17 | 3.1 MB |
| Etfs | 17 | 2.0 MB |
| Commodities | 8 | 1.8 MB |
| Forex | 8 | 1.8 MB |

## Schema

Each Parquet file contains daily OHLCV data with the following columns:

| Column | Type | Description |
|--------|------|-------------|
| `date` | `date32` | Trading date |
| `open` | `float64` | Opening price |
| `high` | `float64` | Day high |
| `low` | `float64` | Day low |
| `close` | `float64` | Closing price |
| `adj_close` | `float64` | Adjusted close (splits & dividends) |
| `volume` | `int64` | Trading volume |
| `dividends` | `float64` | Dividend amount |
| `stock_splits` | `float64` | Stock split ratio |
| `symbol` | `string` | Ticker symbol |

## Usage

```python
import pandas as pd

# Load a single stock
df = pd.read_parquet("hf://datasets/vishnun0027/indian-market-historical-ohlcv/stocks/RELIANCE.parquet")

# Load all stocks
from datasets import load_dataset
ds = load_dataset("vishnun0027/indian-market-historical-ohlcv", data_dir="stocks")
```

## File Structure

```
├── stocks/          # NSE/BSE equities (*.parquet)
├── indices/         # Market indices (*.parquet)
├── etfs/            # Exchange-traded funds (*.parquet)
├── commodities/     # Commodity futures (*.parquet)
├── forex/           # Currency pairs (*.parquet)
├── metadata/        # Asset universe & catalog
└── sync_status.json # Last sync timestamp & stats
```

## Data Quality

Each sync run validates data for:
- Duplicate dates
- OHLC consistency (High ≥ Low, bounds checks)
- Missing values
- Volume anomalies
- Trading day gaps

## License

MIT
