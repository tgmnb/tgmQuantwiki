---
title: Trading Regime
created: 2026-04-23
updated: 2026-04-23
type: concept
tags: [quant, strategy, regime]
confidence: high
---

# Trading Regime

## 核心思想

> **"先判断 Regime，再选策略"** — 同一指标在不同市场状态下效果截然不同。

Regime（市场状态）是量化策略的**第一层过滤器**。在趋势行情用趋势跟踪策略，在震荡行情用均值回归策略，逻辑必须匹配当前市场结构。

## 四象限模型

| 状态 | 趋势 | 震荡 |
|------|------|------|
| **高波动** | 强趋势：高动量策略 | 高波动震荡：布林带均值回归 |
| **低波动** | 弱趋势：均线交叉 | 低波动震荡：RSI/随机指标 |

## Regime 检测方法

### 1. 趋势强度指标
- **ADX** (Average Directional Index): >25 表示趋势，<20 表示震荡
- **AHNR**: 均线角度/弯曲率指标
- **SMA 交叉**: 快线/慢线距离作为趋势确认

### 2. 波动率指标
- **ATR 百分位**: 当前 ATR 处于历史区间的位置
- **Bollinger Band 带宽**: 带宽收窄 = 震荡，扩张 = 趋势或突破
- **标准差比率**: 当前波动率 vs 20日均值

### 3. 量化 Regime 模型（Smart Software）

```python
import numpy as np
import pandas as pd

def detect_regime(prices: pd.Series, lookback: 20) -> pd.DataFrame:
    """检测市场状态"""
    # 趋势强度：ADX
    adx = calculate_adx(prices, lookback)
    trend = adx > 25

    # 波动率：ATR 百分位
    atr = calculate_atr(prices, lookback)
    atr_percentile = (atr - atr.rolling(100).min()) / (atr.rolling(100).max() - atr.rolling(100).min())
    high_vol = atr_percentile > 0.7

    # Regime 分类
    regime = pd.Series(index=prices.index)
    regime[trend & high_vol] = "strong_trend"
    regime[trend & ~high_vol] = "weak_trend"
    regime[~trend & high_vol] = "high_vol_range"
    regime[~trend & ~high_vol] = "low_vol_range"

    return regime

def select_strategy(regime: str) -> list[str]:
    """根据 Regime 匹配策略"""
    mapping = {
        "strong_trend": ["趋势跟踪", "动量策略", "突破策略"],
        "weak_trend": ["均线交叉", "趋势确认策略"],
        "high_vol_range": ["布林带均值回归", "RSI 逆势"],
        "low_vol_range": ["RSI", "随机指标", "窄幅突破"]
    }
    return mapping.get(regime, [])
```

## 多时间框架确认 (MTF)

- **日线**: 定方向（多头/空头/中性）
- **4H**: 找入场点（等待回调或突破）
- **1H**: 精确入场时机

**原则**: 大周期确认方向，小周期找时机。

## 常见错误

- **在震荡行情用趋势策略** → 反复止损
- **在趋势行情用均值回归** → 逆势单被扫
- **只看单一指标** → ADX 假突破

## 相关概念

- [[technical-analysis]] — 技术分析工具
- [[strategy-frameworks]] — 策略开发框架
- [[risk-management]] — 风险管理
