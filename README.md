# Kalshi Implied Distribution Extractor

Extract and visualize implied probability distributions from Kalshi ladder markets using a three-stage pipeline: market data extraction, monotonic enforcement, and distribution smoothing.

## Overview

This project analyzes Kalshi's CPI Year-Over-Year (KXCPIYOY) ladder market to extract a continuous probability distribution from discrete binary contract prices. The methodology combines market microstructure analysis, weighted isotonic regression, and spline interpolation to produce a calibrated probability density function.

## Quick Start

### Installation

```bash
pip install requests numpy pandas matplotlib scipy scikit-learn
```

### Usage

Run the Jupyter notebook to extract the distribution:

```bash
jupyter notebook distribution.ipynb
```

## Repository

Full project available at: [https://github.com/lewistu24/kalshi-distribution](https://github.com/lewistu24/kalshi-distribution)

## Notes

- Market data is fetched in real-time, so results will vary with current market conditions
- The methodology is generalizable to any Kalshi ladder market (change `SERIES_TICKER`)
- Liquidity weighting ensures the model prioritizes reliable market signals
