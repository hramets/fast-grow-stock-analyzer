# Fast-Grow Stock Analyzer

A CLI tool that identifies stocks with strong relative strength (RS) momentum within a given index and time period. It finds stocks that are outperforming the broader market and whose RS has recently crossed above its moving average.

## How It Works

1. Fetches all tickers for a chosen index (S&P 500 or NASDAQ 100) from Wikipedia
2. Downloads historical price data from Yahoo Finance
3. Calculates **Relative Strength (RS)** — the ratio of a stock's price to the index price
4. Calculates a **21-day RS moving average (RS MA)**
5. Filters stocks by two criteria:
   - RS grew over the selected period
   - RS has stayed above RS MA for at least N consecutive days (you choose N)
6. Prints the qualifying ticker symbols

## Project Structure

```
fast-grow-stock-analyzer/
├── stock_analyzer/
│   ├── analyzer.py      # Core classes: Yfinance, MetricsCalculator, DataFilter
│   ├── scraper.py       # Web scraping helper (fetches Wikipedia tables)
│   ├── config.py        # Index definitions and MA window constant
│   ├── cli.py           # Interactive CLI entry point
│   └── __main__.py      # Enables python -m stock_analyzer
├── tests/
│   ├── test_analyzer.py
│   └── test_scraper.py
├── requirements.txt
└── README.md
```

## Setup

**Requirements**: Python 3.10+

```bash
# Create and activate a virtual environment
python -m venv venv
source venv/bin/activate   # Windows: venv\Scripts\activate

# Install dependencies
pip install -r requirements.txt
```

## Usage

```bash
python -m stock_analyzer
```

**Interactive prompts:**

```
Start period (yyyy-mm-dd): 2024-01-01
End period (yyyy-mm-dd): 2024-06-01
0 - S&P 500
1 - NASDAQ 100
Put one of the nr: 0
Moving average window: 10
```

**Output:** a comma-separated list of qualifying ticker symbols, e.g. `NVDA, META, AAPL`

## Configuration

Edit `stock_analyzer/config.py` to add new indexes or change the MA window:

| Constant | Default | Description |
|----------|---------|-------------|
| `MA_WINDOW` | `21` | Rolling window (days) for RS moving average |
| `INDEXES_INFO` | S&P 500, NASDAQ 100 | Index definitions (Wikipedia URL, table number, ticker) |

## Testing

```bash
python -m unittest discover tests
```

> Note: `test_analyzer.py` and `test_scraper.py` include integration tests that make live network requests to Yahoo Finance and Wikipedia.
