---
source_url: https://en.wikipedia.org/wiki/MACD
ingested: 2026-04-24
---

# MACD (Wikipedia)

## Formula

**MACD Line** = EMA_12 − EMA_26
**Signal Line** = EMA_9(MACD Line)
**Histogram** = MACD Line − Signal Line

Notated as MACD(a,b,c): MACD series = EMA(a) − EMA(b), average series = EMA(c) of MACD.

## Historical Origin

Created by Gerald Appel in late 1970s. Parameters 12/26/9 derived from 6-day trading week:
- 12 ≈ 2 weeks
- 26 ≈ 1 month
- 9 ≈ 1.5 weeks

Now with 5-day trading weeks, alternatives are legitimate.

## Trading Interpretation

### Signal-line Crossover
MACD crosses signal line → buy (bullish) or sell (bearish). Indicates trend acceleration direction change.

### Zero Crossover
MACD crosses horizontal zero axis. Change from negative to positive = bullish, positive to negative = bearish. More confirmation than signal line, but lagging.

### Divergence
- **Bullish divergence**: Price makes new low, MACD does NOT make new low → reversal signal
- **Bearish divergence**: Price makes new high, MACD does NOT make new high → reversal signal
- Divergence can occur on MACD line or histogram

### Timing
Use weekly MACD before daily to avoid fighting intermediate trend. Parameters can be adjusted for short-term vs long-term tracking.

### False Signals
MACD generates false signals. Filters include: requiring MACD to stay above/below signal for N days. Reduces false signals but also reduces captured profit.

## Key Mathematical Property
MACD is a lagging indicator because it is based on moving averages. Less useful for non-trending (ranging) stocks.
