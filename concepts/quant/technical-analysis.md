---
title: Technical Analysis
created: 2026-04-23
updated: 2026-04-24
type: concept
tags: [quant, technical-analysis, indicators]
confidence: high
---

# Technical Analysis

## 概述

技术分析是通过价格和成交量数据来判断市场走势的方法论。在量化交易中，每个技术指标都是对市场某一特征的数学抽象。

**三大假设**：
1. 价格反映一切（Market Discounts Everything）
2. 价格按趋势运动（Prices Move in Trends）
3. 历史会重演（History Repeats）

## 指标分类

### 趋势类指标
| 指标 | 说明 | 核心用途 |
|------|------|----------|
| SMA/EMA | 简单/指数移动平均 | 判断方向、支撑阻力 |
| MACD | 移动平均收敛发散 | 趋势方向、动量、交叉、背离 |
| Supertrend | ATR-based 趋势线 | 趋势跟踪、入场信号 |

### 动量类指标
| 指标 | 说明 | 核心用途 |
|------|------|----------|
| RSI | 相对强弱指数 | 超买超卖 |
| Stochastic | 随机指标 | 动量反转 |
| CCI | 商品通道指标 | 趋势强度 |

### 波动率类指标
| 指标 | 说明 | 核心用途 |
|------|------|----------|
| Bollinger Bands | 布林带 | 突破/均值回归 |
| ATR | 平均真实波幅 | 止损/仓位 |
| Keltner Channel | 肯特纳通道 | 趋势跟踪 |

### 成交量类指标
| 指标 | 说明 | 核心用途 |
|------|------|----------|
| OBV | 能量潮 | 趋势确认 |
| VWAP | 成交量加权均价 | 入场基准 |
| Volume Profile | 成交量分布 | 支撑阻力 |

## Python 计算示例

```python
import pandas as pd
import numpy as np

def calculate_sma(prices: pd.Series, period: int) -> pd.Series:
    return prices.rolling(window=period).mean()

def calculate_ema(prices: pd.Series, period: int) -> pd.Series:
    return prices.ewm(span=period, adjust=False).mean()

def calculate_rsi(prices: pd.Series, period: int = 14) -> pd.Series:
    delta = prices.diff()
    gain = delta.where(delta > 0, 0).rolling(window=period).mean()
    loss = (-delta.where(delta < 0, 0)).rolling(window=period).mean()
    rs = gain / loss
    return 100 - (100 / (1 + rs))

def calculate_atr(high: pd.Series, low: pd.Series, close: pd.Series, period: int = 14) -> pd.Series:
    tr = pd.concat([
        high - low,
        (high - close.shift()).abs(),
        (low - close.shift()).abs()
    ], axis=1).max(axis=1)
    return tr.rolling(window=period).mean()

def calculate_bollinger_bands(prices: pd.Series, period: int = 20, std_dev: float = 2):
    sma = prices.rolling(window=period).mean()
    std = prices.rolling(window=period).std()
    return sma + std * std_dev, sma, sma - std * std_dev

def calculate_macd(close: pd.Series, fast: int = 12, slow: int = 26, signal: int = 9):
    """MACD: EMA差值动量指标。返回 (MACD线, 信号线, 直方图)"""
    ema_fast = close.ewm(span=fast, adjust=False).mean()
    ema_slow = close.ewm(span=slow, adjust=False).mean()
    macd_line = ema_fast - ema_slow
    signal_line = macd_line.ewm(span=signal, adjust=False).mean()
    histogram = macd_line - signal_line
    return macd_line, signal_line, histogram
```

## 量价关系

- **价涨量增**: 趋势延续（健康）
- **价涨量缩**: 趋势可能反转（背离）
- **价跌量增**: 抛压重，可能加速
- **价跌量缩**: 底部整固，可能反弹

## K线形态（Candlestick Patterns）

### 反转形态
- **锤子线 (Hammer)**: 下跌底部，长下影线
- **射击星 (Shooting Star)**: 上涨顶部，长上影线
- **吞没形态 (Engulfing)**: 实体完全包裹前一根

### 持续形态
- **旗形 (Flag)**: 短暂整理后延续
- **三角整理**: 收敛后突破
- **矩形整理**: 区间震荡

## 多指标共振

单一指标容易产生假信号，**多指标共振**可以过滤噪音：

```
多头信号 = 趋势向上 + 动量正向 + 成交量确认 + 无超买背离
空头信号 = 趋势向下 + 动量负向 + 成交量确认 + 无超卖背离
```

## 相关概念

- [[trading-regime]] — 市场状态判断是技术分析的前提
- [[strategy-prototypes]] — 基于技术指标的策略
- [[risk-management]] — 指标用于设置止损
- [[macd]] — MACD 体系完整覆盖（基础/背离/交叉/参数/直方图）
- [[macd-divergences]] — MACD 背离四种类型详解
- [[macd-crossovers]] — 信号线交叉与零轴交叉深度对比
- [[macd-parameters]] — 参数选择与 MACD 方言变体
