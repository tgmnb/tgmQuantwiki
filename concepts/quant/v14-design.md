---
title: V14 Strategy Architecture Design
category: quant
tags: [strategy, v14, walk-forward-optimization, anti-overfitting, position-sizing]
created: 2026-05-05
version: 14.0
status: draft
parent: [[v13-design]]
wiki_dir: /home/tgm/wiki/concepts/quant
source_refs:
  - /mnt/c/quant/ma_band_backtest/src/strategy.py
  - /mnt/c/quant/ma_band_backtest/src/risk.py
  - /mnt/c/quant/ma_band_backtest/src/band_engine.py
  - /mnt/c/quant/ma_band_backtest/src/selector.py
  - /mnt/c/quant/ma_band_backtest/src/mfe_selector.py
  - /mnt/c/quant/ma_band_backtest/src/metrics.py
  - /mnt/c/quant/ma_band_backtest/src/portfolio_backtester.py
  - /mnt/c/quant/ma_band_backtest/outputs/v13_bidir/V13_final_recommendation.md
---

# V14 Strategy Architecture Design

> **Design Version**: 14.0-draft
> **Created**: 2026-05-05
> **Starting Capital**: $700
> **Primary Target**: >1000% annual return, <20% max drawdown under worst-case friction
> **Secondary Target**: >300% annual return under high slippage stress

---

## Table of Contents

1. [[#1-friction-model-specification]]
2. [[#2-anti-overfitting-protocol]]
3. [[#3-strategy-architecture]]
4. [[#4-position-sizing-and-risk-management]]
5. [[#5-expected-performance-projections]]
6. [[#6-implementation-roadmap]]

---

## 1. Friction Model Specification

### 1.1 Exchange Fee Structure (Binance Futures VIP0)

| Fee Type | Rate | Notes |
|:---------|:----:|:------|
| Maker | 0.02% | Limit orders receive rebate |
| Taker | 0.05% | Market orders pay fee |
| **Round-trip** | **0.10%** | Assumes all orders are TAKER (worst case) |

### 1.2 Slippage Model (Square-Root Model)

```
slippage = base_slippage × sqrt(order_notional / $10,000)

Base Slippage by Liquidity Tier:
├── BTC/ETH tier:  0.10%  (high liquidity)
├── Mid-cap tier:  0.30%  (moderate liquidity)
└── Low-cap tier:  0.50%  (low liquidity)
```

**Examples for $175 Position (0.25 × $700)**:

| Coin Tier | Notional | Slippage Formula | Slippage |
|:----------|:--------:|:-----------------|:---------:|
| BTC/ETH | $175 | 0.10% × √(175/10000) | 0.042% |
| Mid-cap | $175 | 0.30% × √(175/10000) | 0.126% |
| Low-cap | $175 | 0.50% × √(175/10000) | 0.210% |

### 1.3 Funding Rate Model

| Parameter | Value | Notes |
|:----------|:-----:|:------|
| Historical average | 0.01%/8h | Per funding interval |
| Holding period | 2 bars (12H) | Average trade duration |
| Funding per trade | 0.02% | Shorts pay, longs receive |
| Conservative model | **0.03%** | Short entries only (paid) |

### 1.4 Total Friction Per Trade

| Tier | Fees | Slippage | Funding | **Total** |
|:-----|:----:|:--------:|:-------:|:---------:|
| BTC/ETH | 0.10% | 0.10% | 0.02% | **0.22%** |
| Mid-cap | 0.10% | 0.30% | 0.02% | **0.42%** |
| Low-cap | 0.10% | 0.50% | 0.02% | **0.62%** |
| **Worst case** | 0.10% | 0.60% | 0.03% | **0.84%** |
| **High stress** | 0.10% | 0.90% | 0.03% | **1.26%** |

### 1.5 Friction Impact on $700 Capital

```
Per-trade cost at worst-case (0.84%):
  $175 × 0.84% = $1.47 per trade

At 100 trades/year:
  Total friction cost = $147

At 500 trades/year:
  Total friction cost = $735 (exceeds starting capital!)
```

**Critical Constraint**: With $700 capital, must maintain **<200 trades/year** at worst-case friction to keep costs below 50% of starting capital.

---

## 2. Anti-Overfitting Protocol

### 2.1 Walk-Forward Optimization (WFO) Schedule

| Phase | Period | Purpose |
|:------|:------:|:--------|
| Train | 2020-01-01 to 2023-12-31 (4 years) | Parameter optimization |
| Test | 2024-01-01 to 2026-05-01 (2.3 years) | Out-of-sample validation |
| Gap | 10% of window | Purified separation |

```
┌──────────────────────────────┬──────────────────────────────┐
│         TRAIN (4Y)            │   GAP   │      TEST (2.3Y)        │
│   2020-01 ────────── 2023-12 │  30d    │  2024-02 ──── 2026-05   │
└──────────────────────────────┴─────────┴─────────────────────────┘
         ▲ Retrain monthly          ▲ Rebalance weekly
```

### 2.2 Retraining Frequencies

| Frequency | Trigger | Action |
|:----------|:-------:|:-------|
| Monthly | New calendar month | Full parameter re-optimization |
| Weekly | New calendar week | Rebalance band weights |
| Daily | New calendar day | Update short pool selection |
| **Continuous** | ATR regime change | Volatility filter adjustment |

### 2.3 Parameter Count Constraints

**Maximum 4 free parameters** to control overfitting:

| # | Parameter | Type | V13 Equivalent |
|:--|:----------|:-----|:--------------|
| 1 | `long_strat` | Enum | MA Band event selection strategy |
| 2 | `long_p` | Float | Long-side parameter value |
| 3 | `short_strat` | Enum | Short-side strategy type |
| 4 | `short_p` | Float | Short-side parameter value |

**Note**: In V13, `long_strat + long_p` is effectively 2 degrees of freedom (strategy type + magnitude). V14 formalizes this as 4 parameters maximum.

### 2.4 Plateau Selection Criterion

```python
def is_plateau(opt_result, param, tolerance=0.20):
    """
    Parameter is on plateau if ±20% perturbation
    keeps performance within ±30% of optimum.
    """
    base_sharpe = opt_result[param].sharpe_ratio
    perturbed_sharpe_plus = opt_result[param * 1.20].sharpe_ratio
    perturbed_sharpe_minus = opt_result[param * 0.80].sharpe_ratio

    return (
        abs(perturbed_sharpe_plus - base_sharpe) / base_sharpe <= 0.30
        and abs(perturbed_sharpe_minus - base_sharpe) / base_sharpe <= 0.30
    )
```

### 2.5 Statistical Significance Requirements

| Requirement | Threshold | Rationale |
|:------------|:---------:|:---------|
| Minimum OOS trades | 100 | Central limit theorem |
| Win rate significance | p < 0.05 | Chi-square test vs 50% |
| Profit significance | p < 0.01 | t-test vs zero |
| Sharpe ratio | > 0.95 deflated | Deflated Sharpe (Bailey et al.) |

### 2.6 Monte Carlo Stability Test

```python
def monte_carlo_stability(strategy_returns, n_reshuffles=100, success_threshold=0.01):
    """
    100 reshuffles of trade sequence.
    Success if null hypothesis (random trading) rejected at p < 0.01.
    """
    observed_sharpe = calculate_sharpe(strategy_returns)
    null_distribution = []

    for _ in range(n_reshuffles):
        shuffled = resample_trades(strategy_returns)
        null_distribution.append(calculate_sharpe(shuffled))

    p_value = np.mean([s >= observed_sharpe for s in null_distribution])
    return p_value < success_threshold
```

### 2.7 Regime Decomposition

| Regime | Period | Market Condition | Required Performance |
|:-------|:------:|:-----------------|:---------------------|
| Bull | 2020-2021 | Strong uptrend | Profitable |
| Bear | 2022 | FTX collapse, crypto winter | Profitable |
| Recovery | 2023 | Post-bear bounce | Profitable |
| Mixed | 2024-2025 | Sideways, choppy | Profitable in ≥3/4 |

**Acceptance Criterion**: Strategy must be **net profitable in at least 3 of 4 regimes**.

---

## 3. Strategy Architecture

### 3.1 Core Engine (Preserved from V13)

#### MA Grid Configuration
```python
MA_GRID_CONFIG = {
    'start': 5,           # Starting period
    'ratio': 1.10,        # Fixed ratio (V14: adaptive 1.05-1.15)
    'max_period': 720,    # Maximum period
    'round_method': 'ceil'
}
```

#### Band Event Detection
```python
# From band_engine.py - touch/penetrate with no-future-leak
touch = (prev_close > left_ma_prev) & (low <= left_ma)
penetrate = (low <= right_ma) & touch
```

#### Multi-Layer Selection Pipeline
```
1. Long pool (monthly, top 20 bands by ratio_drop/MFE)
2. Short pool (daily update, intersection with long pool)
3. Execution top_n (weekly rebalance, top 5 by score)
```

#### Entry Mechanism (Pullback with Trend Filter)
```python
# From strategy.py check_pullback_entry()
if direction == "long":
    if (
        prev_close > left_ma_t_minus_1
        and low_t <= left_ma_t
        and right_ma_t > right_ma_t_minus_1
    ):
        return EntrySignal(...)
```

#### Trailing Stop (MA Band Trailing)
```python
# From strategy.py get_trailing_stop()
if position.direction == 'long':
    if (
        close_t_minus_1 <= new_left_ma_t_minus_1
        and close_t > new_left_ma_t
        and new_right_ma_t > current_stop
    ):
        return new_right_ma_t  # Trail up
```

### 3.2 Key Improvements for V14

#### 3.2.1 Adaptive MA Grid Ratio

```python
def get_adaptive_ratio(atr_percentile: float, regimes: dict) -> float:
    """
    Adapt MA grid ratio based on volatility regime.

    Low vol (ATR pct < 30):   ratio = 1.05  (tighter grid)
    Normal (30-70):           ratio = 1.10  (baseline)
    High vol (ATR pct > 70):  ratio = 1.15  (wider grid)
    Crisis (ATR pct > 90):    ratio = 1.20  (widest)
    """
    if atr_percentile < 0.30:
        return 1.05
    elif atr_percentile < 0.70:
        return 1.10
    elif atr_percentile < 0.90:
        return 1.15
    else:
        return 1.20
```

#### 3.2.2 Multi-Timeframe Confirmation

```python
TIMEFRAMES = ['1h', '4h', '12h']

def get_timeframe_alignment(df_dict: dict, direction: str) -> int:
    """
    Count how many timeframes agree on trend direction.
    Requires ≥2/3 timeframes aligned for entry.
    """
    alignment_count = 0

    for tf, (df, ma_matrix) in df_dict.items():
        if check_trend_filter(df.iloc[-1], ma_matrix.iloc[-1], direction):
            alignment_count += 1

    return alignment_count
```

#### 3.2.3 Kelly-Fraction Position Sizing

```python
def calculate_kelly_fraction(win_rate: float, avg_win: float, avg_loss: float) -> float:
    """
    Kelly criterion: f* = (bp - 1) / (b - 1)
    Where b = avg_win/avg_loss, p = win_rate
    """
    if avg_loss == 0:
        return 0.0

    b = avg_win / avg_loss
    f_star = (b * win_rate - (1 - win_rate)) / b

    # Cap at 25% of equity per position
    return min(max(f_star, 0), 0.25)
```

#### 3.2.4 Drawdown Circuit Breakers

```python
DRAWDOWN_CIRCUIT_BREAKERS = {
    0.10: ('halve_positions', 'reduce_size_to', 0.5),   # 10% DD → 50% size
    0.15: ('stop_new_entries', None),                    # 15% DD → no new entries
    0.20: ('close_all_positions', None),                  # 20% DD → flat
}
```

#### 3.2.5 Funding-Rate-Aware Short Entries

```python
def is_short_entry_allowed(funding_rate: float, threshold: float = 0.0005) -> bool:
    """
    Only short when funding rate < 0.05% (not extremely negative).
    Avoids shorting into high negative funding (bullish sentiment).
    """
    return funding_rate < threshold
```

#### 3.2.6 Volatility Regime Filter (from V11-V13)

```python
def is_volatility_gate_open(df: pd.DataFrame, atr_percentile: float, gate_pct: float = 0.70) -> bool:
    """
    ATR percentile gate - only enter when volatility is below gate.
    Tightens during high vol regimes.
    """
    return atr_percentile < gate_pct
```

#### 3.2.7 Correlation-Aware Portfolio

```python
SECTOR_CLUSTERS = {
    'L1': ['BTC-USDT', 'ETH-USDT', 'SOL-USDT', 'AVAX-USDT'],
    'DeFi': ['UNI-USDT', 'AAVE-USDT', 'CRV-USDT'],
    'Meme': ['DOGE-USDT', 'SHIB-USDT'],
}

def check_sector_limits(new_coin: str, open_positions: list, max_per_sector: int = 2) -> bool:
    """
    Max 2 positions in same sector at any time.
    """
    sector = get_coin_sector(new_coin)
    same_sector_count = sum(1 for c in open_positions if get_coin_sector(c) == sector)
    return same_sector_count < max_per_sector
```

#### 3.2.8 Volatility-Based Time Exit

```python
def calculate_exit_bars(atr_percentile: float, base_bars: int = 5) -> int:
    """
    Instead of fixed 5-bar exit, use volatility-based duration.
    High vol → fewer bars (faster exit)
    Low vol → more bars (let winners run)
    """
    if atr_percentile > 0.80:
        return int(base_bars * 0.5)   # 2-3 bars
    elif atr_percentile > 0.60:
        return int(base_bars * 0.75)  # 3-4 bars
    elif atr_percentile > 0.40:
        return base_bars               # 5 bars
    else:
        return int(base_bars * 1.5)   # 7-8 bars
```

### 3.3 Strategy Architecture Diagram

```
┌─────────────────────────────────────────────────────────────────────┐
│                         V14 STRATEGY ENGINE                          │
├─────────────────────────────────────────────────────────────────────┤
│                                                                     │
│  ┌──────────────┐    ┌──────────────┐    ┌──────────────────────┐  │
│  │ Multi-TF     │    │ Adaptive MA  │    │ Volatility Regime    │  │
│  │ Confirmation │───▶│ Grid Ratio  │───▶│ Filter (ATR gate)    │  │
│  │ (1H,4H,12H)  │    │ (1.05-1.20) │    │                      │  │
│  └──────────────┘    └──────────────┘    └──────────────────────┘  │
│         │                    │                      │                │
│         ▼                    ▼                      ▼                │
│  ┌─────────────────────────────────────────────────────────────────┐ │
│  │                    BAND EVENT ENGINE                          │ │
│  │  Touch/Penetrate Detection (no-future-leak)                  │ │
│  │  Events: long_touch, long_penetrate, short_touch, short_pen  │ │
│  └─────────────────────────────────────────────────────────────────┘ │
│                              │                                        │
│         ┌────────────────────┴────────────────────┐                  │
│         ▼                                         ▼                   │
│  ┌─────────────────┐                     ┌─────────────────┐       │
│  │   LONG POOL     │                     │   SHORT POOL    │       │
│  │  (Monthly WFO)  │                     │  (Daily Update) │       │
│  │  Top 20 bands   │                     │  Top 5 bands    │       │
│  │  ratio_drop/MFE │                     │  Funding-aware   │       │
│  └─────────────────┘                     └─────────────────┘       │
│                              │                                        │
│                              ▼                                        │
│  ┌─────────────────────────────────────────────────────────────────┐ │
│  │                   ENTRY GENERATION                             │ │
│  │  • Pullback entry with trend filter                           │ │
│  │  • Sector correlation check                                    │ │
│  │  • Funding rate check (shorts only)                            │ │
│  │  • Kelly-fraction position sizing                             │ │
│  └─────────────────────────────────────────────────────────────────┘ │
│                              │                                        │
│                              ▼                                        │
│  ┌─────────────────────────────────────────────────────────────────┐ │
│  │                    RISK MANAGEMENT                            │ │
│  │  • Trailing stop (MA band)                                     │ │
│  │  • DD circuit breakers (10%/15%/20%)                           │ │
│  │  • Max positions cap (4 concurrent)                            │ │
│  │  • Volatility-based exit timing                                │ │
│  └─────────────────────────────────────────────────────────────────┘ │
│                                                                     │
└─────────────────────────────────────────────────────────────────────┘
```

---

## 4. Position Sizing and Risk Management

### 4.1 Position Sizing at $700 Capital

| Parameter | Value | Rationale |
|:----------|:-----:|:----------|
| Initial equity | $700 | Starting capital |
| Max leverage | 3x | Limit total notional to $2,100 |
| Max concurrent positions | 4 | Diversification without overdilution |
| Per-position notional | $175 | 0.25 × $700 |
| Kelly-fraction cap | 25% | Safety cap on risk per trade |

### 4.2 Position Sizing Formulas

```python
def calculate_position_size_v14(
    equity: float,
    entry_price: float,
    stop_distance_pct: float,
    kelly_fraction: float,
    max_leverage: float = 3.0,
    max_position_pct: float = 0.25,
) -> dict:
    """
    V14 position sizing combining Kelly with risk-based stop.
    """
    # Kelly-based risk amount
    risk_amount = equity * kelly_fraction

    # Risk-based quantity
    unit_risk = entry_price * stop_distance_pct
    qty = risk_amount / unit_risk
    notional = qty * entry_price

    # Leverage check
    leverage = notional / equity
    if leverage > max_leverage:
        qty = equity * max_leverage / entry_price
        notional = qty * entry_price
        leverage = max_leverage

    # Position cap check
    max_qty_by_cap = equity * max_position_pct / entry_price
    if qty > max_qty_by_cap:
        qty = max_qty_by_cap
        notional = qty * entry_price
        leverage = notional / equity

    return {
        'qty': qty,
        'notional': notional,
        'leverage': leverage,
        'risk_amount': min(risk_amount, equity * max_position_pct),
    }
```

### 4.3 Risk Management Rules

#### 4.3.1 Drawdown Circuit Breakers

| Drawdown Level | Action | Parameters |
|:---------------|:-------|:-----------|
| 10% | Halve position sizes | reduce to 50% of normal |
| 15% | Stop new entries | No new positions |
| 20% | Close all positions | Exit everything |

#### 4.3.2 Stop Loss Strategy

```python
STOP_CONFIG = {
    'initial_stop': 'ma_left',      # Use entry band left_ma as initial stop
    'trailing_stop': 'ma_right',     # Trail using right_ma (longer period)
    'stop_type': 'trail_atr',        # ATR-based trailing
    'stop_param': 2.5,               # 2.5 × ATR
}
```

#### 4.3.3 Maximum Exposure Limits

```python
EXPOSURE_LIMITS = {
    'max_leverage': 3.0,
    'max_positions': 4,
    'max_positions_per_symbol': 1,
    'max_positions_per_sector': 2,
    'max_notional_per_trade': 250,   # USDT notional
}
```

### 4.4 Friction Cost Budget

```
Starting Capital:          $700
Annual Target (1000%):     $7,000 profit

Friction Cost Budget (max 20% of gross):
  Maximum allowed friction = $1,750/year
  At $1.47/trade (worst case) = 1,190 trades max
  At $0.77/trade (mid case)   = 2,272 trades max

Recommended:
  Target 100-150 trades/year at worst-case friction
  Target 200-300 trades/year at mid-case friction
```

---

## 5. Expected Performance Projections

### 5.1 Scenario Definitions

| Scenario | Coin Selection | Max Positions | Friction Model | Target DD |
|:---------|:--------------|:-------------:|:--------------|:---------:|
| **Conservative** | BTC/ETH only | 2 | Worst (0.84%) | <15% |
| **Base** | Top 20 coins | 4 | Mid (0.42%) | <20% |
| **Aggressive** | 50+ coins | 8 | Low (0.22%) | <25% |

### 5.2 Performance Projections

#### Conservative Scenario (BTC/ETH, 2 positions, 0.84% friction)

| Metric | Value | Notes |
|:-------|:-----:|:------|
| Annual Return | 200-400% | $1,400 - $2,800 on $700 |
| Max Drawdown | 10-15% | Strict capital protection |
| Trade Frequency | 50-80/year | Quality over quantity |
| Win Rate | ≥60% | High-conviction setups only |
| Avg Profit/Trade | 3-5% | Larger moves, longer holds |
| Net Sharpe Ratio | 1.5-2.0 | Risk-adjusted |

#### Base Scenario (Top 20, 4 positions, 0.42% friction)

| Metric | Value | Notes |
|:-------|:-----:|:------|
| Annual Return | 500-800% | $3,500 - $5,600 on $700 |
| Max Drawdown | 15-20% | Balanced risk/reward |
| Trade Frequency | 100-150/year | Moderate activity |
| Win Rate | ≥55% | Diversified signals |
| Avg Profit/Trade | 2-4% | Normal trade duration |
| Net Sharpe Ratio | 1.2-1.8 | Good risk-adjusted |

#### Aggressive Scenario (50+ coins, 8 positions, 0.22% friction)

| Metric | Value | Notes |
|:-------|:-----:|:------|
| Annual Return | 800-1500% | $5,600 - $10,500 on $700 |
| Max Drawdown | 20-25% | Higher risk tolerance |
| Trade Frequency | 200-400/year | High activity |
| Win Rate | ≥50% | More signal noise |
| Avg Profit/Trade | 1.5-3% | Shorter holds, more turnover |
| Net Sharpe Ratio | 0.8-1.5 | Variable conditions |

### 5.3 Key Performance Drivers

```python
# To achieve 1000% annual return with $700:
TARGET_ANNUAL_RETURN = 10.0  # 1000%

# Required metrics:
REQUIRED_WIN_RATE = 0.55      # 55%+
REQUIRED_AVG_PROFIT = 0.02    # 2% per trade
REQUIRED_TRADES_PER_YEAR = 100  # At 2% avg, 55% WR → ~50% net expectancy

# Gross profit calculation:
GROSS_PROFIT = 100 * 0.55 * 0.02 * 700  # = $770
GROSS_PROFIT += 100 * 0.45 * (-0.01) * 700  # = -$315
NET_PROFIT = $455 on $700 = 65%

# With leverage 3x:
# Net profit = $455 * 3 = $1,365 = 195%
# Still short of 1000% without compounding!
```

**Reality Check**: 1000% annual return requires significant compounding or higher avg trade returns. With $700 and 3x leverage:

```python
# Compounding math for 1000%:
# Need ~10x equity multiplication
# At 2% avg trade, need ~115 winning trades with full compounding
# Or ~230 winning trades at 2% avg with 50% position retention
```

### 5.4 Sensitivity Analysis

| Friction Cost | Trades/Year | Break-even Win Rate | Required Avg Profit |
|:-------------|:-----------:|:------------------:|:-------------------:|
| 0.22% | 100 | 46% | 1.8% |
| 0.22% | 200 | 46% | 1.8% |
| 0.42% | 100 | 52% | 2.1% |
| 0.42% | 200 | 52% | 2.1% |
| 0.84% | 100 | 58% | 2.5% |
| 0.84% | 150 | 63% | 3.0% |

---

## 6. Implementation Roadmap

### 6.1 Week 1: Core Framework Port

**Goals**: Port V13 signal generation to V14 framework with realistic friction model

| Day | Task | Deliverable |
|:----|:-----|:-----------|
| 1-2 | Port MA grid with adaptive ratio | `ma_grid.py` with `get_adaptive_ratio()` |
| 3 | Port band event engine | `band_engine.py` touch/penetrate |
| 4 | Implement multi-TF confirmation | `timeframe_alignment.py` |
| 5 | Integrate friction model | Cost calculation with slippage model |

**Milestone**: Basic backtester runs with friction on single coin.

### 6.2 Week 2: WFO Pipeline

**Goals**: Implement walk-forward optimization with plateau selection

| Day | Task | Deliverable |
|:----|:-----|:-----------|
| 6-7 | Implement WFO train/test split | `wfo_engine.py` |
| 8 | Implement plateau selection | `is_plateau()` criterion |
| 9 | Implement Monte Carlo stability | `monte_carlo_stability()` |
| 10 | Implement regime decomposition | `RegimeAnalyzer` |

**Milestone**: WFO pipeline produces parameter sets with OOS validation.

### 6.3 Week 3: Risk System Implementation

**Goals**: Implement all risk management components

| Day | Task | Deliverable |
|:----|:-----|:-----------|
| 11-12 | Implement Kelly sizing | `calculate_kelly_fraction()` |
| 13 | Implement DD circuit breakers | `DrawdownCircuitBreaker` |
| 14 | Implement sector correlation | `check_sector_limits()` |
| 15 | Implement funding-aware short filter | `is_short_entry_allowed()` |

**Milestone**: Risk system operational with all circuit breakers.

### 6.4 Week 4: Validation and Tuning

**Goals**: Full out-of-sample validation, Monte Carlo, final report

| Day | Task | Deliverable |
|:----|:-----|:-----------|
| 16-17 | Run full OOS validation | 3+ years of OOS results |
| 18 | Run Monte Carlo (100 reshuffles) | Stability report |
| 19 | Regime performance verification | 3/4 regimes profitable |
| 20 | Fine-tune parameters | Final parameter set |
| 21 | Produce final report | `v14-design.md` final |

**Milestone**: Complete validation package with all metrics.

---

## Appendix A: V14 Configuration Schema

```yaml
v14_strategy:
  # MA Grid
  ma:
    start: 5
    adaptive_ratio: true
    ratio_range: [1.05, 1.20]
    max_period: 720
    round_method: ceil

  # Selection
  selection:
    selector_version: v5
    long_pool_size: 20
    short_pool_size: 5
    execution_top_n: 3
    use_mfe_selector: true
    mfe_horizon_bars: 20
    mfe_score_col: score_tail

  # Risk
  risk:
    initial_equity: 700
    max_leverage: 3.0
    max_positions: 4
    position_sizing_mode: kelly
    kelly_cap: 0.25
    atr_gate_percentile: 0.70

    # Drawdown circuit breakers
    dd_circuit_breakers:
      - level: 0.10
        action: halve_positions
        reduce_to: 0.5
      - level: 0.15
        action: stop_new_entries
      - level: 0.20
        action: close_all

  # Friction (Production-grade)
  friction:
    fee_taker: 0.0005
    slippage_model: sqrt
    slippage_base_btc: 0.001
    slippage_base_mid: 0.003
    slippage_base_low: 0.005
    funding_rate_avg: 0.0001
    funding_rate_conservative: 0.0003

  # Anti-overfitting
  anti_overfitting:
    train_years: [2020, 2023]
    test_years: [2024, 2026]
    retrain_frequency: monthly
    rebalance_frequency: weekly
    max_free_params: 4
    plateau_tolerance: 0.20
    min_oos_trades: 100
    monte_carlo_reshuffles: 100
    deflated_sharpe_threshold: 0.95
    required_regimes_profitable: 3
```

---

## Appendix B: Risk Disclaimer

**Backtesting results are not indicative of future performance.**

The V14 strategy, like all quantitative strategies, is subject to:
- Market regime changes
- Liquidity constraints at scale
- Exchange API changes
- Regulatory changes
- Slippage model inaccuracies

**Paper trading recommended before live deployment.**

---

## Appendix C: Wikilinks

- [[v13-design]] - V13 strategy design document
- [[v12-design]] - V12 strategy design document
- [[strategy-prototypes]] - Historical strategy prototypes
- [[risk-management]] - General risk management principles
- [[technical-analysis]] - TA foundations for MA bands

---

*Document Version: 14.0-draft*
*Last Updated: 2026-05-05*
