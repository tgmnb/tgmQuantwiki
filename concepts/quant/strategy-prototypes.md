---
title: Strategy Prototypes
created: 2026-04-23
updated: 2026-04-23
type: concept
tags: [quant, strategy, prototypes]
confidence: high
---

# Strategy Prototypes

## 策略分类总览

| 类别 | 子类 | 核心逻辑 | 适用 Regime |
|------|------|----------|-------------|
| 趋势跟踪 | 均线交叉、动量突破、趋势线 | 追涨杀跌，顺势而为 | 趋势 |
| 均值回归 | RSI、BB、VWAP 回归 | 价格围绕价值波动 | 震荡 |
| 突破策略 | 区间突破、K线形态突破 | 蓄势后的方向选择 | 转折点 |
| 套利策略 | 跨品种、跨期、跨市场 | 价格差收敛 | 中性 |

## 1. 均线交叉策略 (MA Crossover)

```python
import pandas as pd

def ma_crossover_strategy(prices: pd.Series, fast: int = 10, slow: int = 30):
    # fast > slow -> 多头, fast < slow -> 空头
    fast_ma = prices.rolling(window=fast).mean()
    slow_ma = prices.rolling(window=slow).mean()

    signal = pd.Series(0, index=prices.index)
    signal[fast_ma > slow_ma] = 1    # 多头
    signal[fast_ma < slow_ma] = -1   # 空头

    entry = signal.diff()
    return signal, entry
```

Note: 震荡行情会产生大量假信号，需要 [[trading-regime]] 过滤。

## 2. RSI 均值回归策略

```python
def rsi_strategy(prices: pd.Series, period: int = 14, oversold: float = 30, overbought: float = 70):
    # RSI 超卖买入，超买卖出
    delta = prices.diff()
    gain = delta.where(delta > 0, 0).rolling(window=period).mean()
    loss = (-delta.where(delta < 0, 0)).rolling(window=period).mean()
    rs = gain / loss
    rsi = 100 - (100 / (1 + rs))

    signal = pd.Series(0, index=prices.index)
    signal[rsi < oversold] = 1
    signal[rsi > overbought] = -1
    return rsi, signal
```

## 3. 布林带策略 (Bollinger Bands)

```python
def bollinger_strategy(prices: pd.Series, period: int = 20, std_dev: float = 2):
    sma = prices.rolling(window=period).mean()
    std = prices.rolling(window=period).std()
    upper = sma + std * std_dev
    lower = sma - std * std_dev

    signal = pd.Series(0, index=prices.index)
    signal[prices < lower] = 1   # 触及下轨买入
    signal[prices > upper] = -1  # 触及上轨卖出
    return upper, sma, lower, signal
```

## 4. 突破策略 (Breakout)

```python
def breakout_strategy(prices: pd.Series, high: pd.Series, low: pd.Series,
                      lookback: int = 20, regime_filter: pd.Series = None):
    rolling_high = high.rolling(window=lookback).max()
    rolling_low = low.rolling(window=lookback).min()

    signal = pd.Series(0, index=prices.index)
    long_condition = prices > rolling_high.shift(1)
    short_condition = prices < rolling_low.shift(1)

    signal[long_condition] = 1
    signal[short_condition] = -1

    # Regime 过滤
    if regime_filter is not None:
        signal[(signal == 1) & (regime_filter != "strong_trend")] = 0

    return rolling_high, rolling_low, signal
```

## 5. 动量策略 (Momentum)

```python
def momentum_strategy(prices: pd.Series, period: int = 20):
    momentum = prices.pct_change(periods=period)
    signal = pd.Series(0, index=prices.index)
    signal[momentum > 0] = 1
    signal[momentum < 0] = -1
    return momentum, signal
```

## 策略选择决策树

```
当前 Regime 是什么?
  strong_trend     -> 动量策略、趋势线突破
  weak_trend       -> 均线交叉、趋势确认策略
  high_vol_range   -> 布林带均值回归
  low_vol_range     -> RSI 随机指标、窄幅突破
```

## 相关概念

- [[trading-regime]] - 策略必须匹配市场状态
- [[technical-analysis]] - 指标计算基础
- [[risk-management]] - 策略必须带止损
