# Kalshi Implied Distribution Extractor

Extract and visualize implied probability distributions from Kalshi ladder markets using a three-stage pipeline: market data extraction, monotonic enforcement, and distribution smoothing.

## Overview

This project analyzes Kalshi's CPI Year-Over-Year (KXCPIYOY) ladder market to extract a continuous probability distribution from discrete binary contract prices. The methodology combines market microstructure analysis, weighted isotonic regression, and spline interpolation to produce a calibrated probability density function.

## Features

- **Micro-price Weighting**: Uses order book imbalance to compute true market sentiment
- **Monotonic Enforcement**: Applies liquidity-weighted isotonic regression to ensure valid cumulative probabilities
- **Distribution Smoothing**: Cubic spline interpolation with tail control and normalization
- **Visualization**: Publication-quality plots showing uncertainty, liquidity concentration, and final PDF

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

Or run all cells programmatically to generate visualizations:

```python
# The notebook will:
# 1. Fetch live market data from Kalshi API
# 2. Compute weighted micro-prices
# 3. Apply isotonic regression
# 4. Extract and normalize PDF
# 5. Generate three visualizations (saved as PNG)
```

## Methodology

### Stage 1: Market Data Extraction

Fetches orderbook data and computes weighted micro-prices using order book imbalance:

```
Micro-Price = (Bid × Ask_Size + Ask × Bid_Size) / Total_Size
```

This captures market sentiment by weighting toward the heavier side of the book.

### Stage 2: Monotonic Enforcement

Applies weighted isotonic regression with liquidity as sample weights to ensure P(CPI ≥ X) decreases monotonically:

```
min_f Σ w_i(y_i - f(x_i))² subject to f(x₁) ≥ f(x₂) ≥ ... ≥ f(xₙ)
```

### Stage 3: Distribution Smoothing

- Adds anchor points at distribution tails (P=1.0 at 0%, P=0.0 at max_strike + 0.2%)
- Applies cubic spline interpolation (k=3, s=0.001)
- Extracts PDF via CDF derivative
- Clips negative values and normalizes to integrate to 1.0

## Output

The notebook generates three visualizations:

1. **market_data.png**: Raw market data with bid-ask spreads and open interest
2. **isotonic.png**: Monotonic regression fit
3. **final_pdf.png**: Final probability density function with market expectation

## Results

For December 2025 expiration:
- Expected CPI YoY: ~2.6-2.8%
- Total probability mass: 1.00 (calibrated)
- Concentration: Most probability within 0.5% range

## Technical Details

- **Market**: Kalshi CPI Year-Over-Year ladder (KXCPIYOY)
- **API**: Public Kalshi Trade API (no authentication required for market data)
- **Libraries**: scipy, scikit-learn, pandas, matplotlib
- **Validation**: Monotonicity check, probability mass = 1.0, expected value within liquid strikes

## Files

- `distribution.ipynb`: Main analysis notebook
- `writeup.tex`: Academic writeup with methodology and formulas
- `market_data.png`, `isotonic.png`, `final_pdf.png`: Generated visualizations

## Citation

If you use this code, please cite:

```
Kalshi Implied Distribution Extractor
https://github.com/[your-username]/kalshi-distribution
```

## License

MIT License - feel free to use and modify for your own analysis.

## Notes

- Market data is fetched in real-time, so results will vary with current market conditions
- The methodology is generalizable to any Kalshi ladder market (change `SERIES_TICKER`)
- Liquidity weighting ensures the model prioritizes reliable market signals

## Contact

For questions or improvements, please open an issue on GitHub.
