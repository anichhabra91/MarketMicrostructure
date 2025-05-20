# Beyond TWAP: Smarter Rule-Based Execution Algorithms

## Overview

This project explores two rule-based execution strategies designed to outperform the conventional Time-Weighted Average Price (TWAP) method by leveraging real-time market signals. The goal is to minimize spread costs and improve execution quality using systematic, data-driven trade decision logic.

## Objective

- Improve upon TWAP using microstructure-aware features.
- Reduce average execution spread across varying market regimes.
- Evaluate strategy robustness under both bullish and bearish scenarios.

## Dataset

- High-frequency limit order book data for 5 US stocks: AAPL, AMZN, GOOG, MSFT, INTC
- Features include timestamp, prices, volumes, order IDs, direction, mid-price, and spread

## Feature Engineering

Key derived features:
- **Spread Ticks**: `(Ask - Bid) / Tick Size`
- **Order Flow Imbalance**: Rolling imbalance of bid/ask volume
- **Momentum**: Log return of mid-price over 50-point window
- **Microprice**: Volume-weighted mid-point between bid and ask
- **Price Velocity**: 5-period difference in mid-price

Synthetic bullish data was created to complement the primarily bearish dataset.

## Strategies

### 1. Event-Driven Dual Execution Strategy (Used for GOOG, INTC, MSFT)

- Adaptive, three-stage execution: Early, Mid, and Forced
- Conditions based on spread, volume imbalance, and price momentum
- Parameters tuned via grid search to balance aggressiveness and execution quality

### 2. Combined Microprice and Spread Tiers (Used for AAPL, AMZN)

- Executes based on microprice position and tiered spread thresholds
- Time-dependent thresholds (5s, 15s, 25s, 45s, 59s)
- Aims for early fills with fallback execution at minute end

## Optimization

- Grid search used for threshold tuning
- Objective: Minimize average execution spread
- Calibrated separately for each stock

## Performance Metrics

| Symbol | Strategy | TWAP Spread | Avg Spread | Improvement |
|--------|----------|-------------|------------|-------------|
| AAPL   | Microprice-Tiers | 0.1421 | 0.0313 | 77.97% |
| AMZN   | Microprice-Tiers | 0.1031 | 0.0318 | 69.15% |
| GOOG   | Event-Driven     | 0.1992 | 0.0447 | 77.56% |
| INTC   | Event-Driven     | 0.0146 | 0.0033 | 76.71% |
| MSFT   | Event-Driven     | 0.0139 | 0.0023 | 83.45% |

## Key Takeaways

- **Spread** is the most influential feature across all strategies.
- **Microprice-Tier** strategy excels on liquid, stable stocks like AAPL and AMZN.
- **Event-Driven** strategy works better for volatile or event-sensitive stocks like GOOG.
- Rule-based methods offer practical, interpretable alternatives to complex learning models.
- Both strategies outperform TWAP significantly in diverse conditions.

## Conclusion

Simple rule-based execution strategies, when calibrated to the microstructure of individual assets, can provide substantial improvements over naïve benchmarks like TWAP. These strategies are not only effective but also interpretable and adaptable across asset classes and market conditions. Future enhancements could include integrating learning-based models or using reinforcement learning for real-time adaptive execution logic.
"""
