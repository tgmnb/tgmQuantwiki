---
title: MACD (Moving Average Convergence Divergence)
created: 2026-04-24
updated: 2026-04-24
type: concept
tags: [quant, indicator, momentum, trend, macd]
sources: [raw/articles/macd-wikipedia-2026.md]
confidence: high
---

# MACD (Moving Average Convergence Divergence)

## 概述

MACD 是 Gerald Appel 于 1970 年代末创建的技术指标，全称"移动平均收敛/发散"。用于揭示价格趋势的**强度、方向、动量和持续时间**的变化。

**核心思想**：比较不同周期的 EMA 之间的差值，捕捉短期动量与中期动量的相对变化。

## 标准公式

MACD(a,b,c) 表示：MACD 系列 = EMA(a) − EMA(b)，信号线 = EMA(c) of MACD

| 参数 | 标准值 | 含义 |
|------|--------|------|
| 快线周期 a | 12 | 约 2 周（6 天交易周） |
| 慢线周期 b | 26 | 约 1 个月（6 天交易周） |
| 信号线周期 c | 9 | MACD 线的 9 日 EMA |

**三个时间序列**：
1. **MACD 线** = EMA(12) − EMA(26)，反映短期与中期动量之差
2. **信号线** = MACD 线的 9 日 EMA，起平滑作用
3. **直方图** = MACD 线 − 信号线，反映两者差距的扩大或收缩

## Python 计算

```python
import pandas as pd
import numpy as np

def macd(close: pd.Series, fast: int = 12, slow: int = 26, signal: int = 9):
    ema_fast = close.ewm(span=fast, adjust=False).mean()
    ema_slow = close.ewm(span=slow, adjust=False).mean()
    macd_line = ema_fast - ema_slow
    signal_line = macd_line.ewm(span=signal, adjust=False).mean()
    histogram = macd_line - signal_line
    return macd_line, signal_line, histogram

# 使用
close = pd.Series([...])  # 收盘价序列
macd_line, signal_line, histogram = macd(close)
```

## 三大核心信号

| 信号类型 | 触发条件 | 信号含义 | 可靠性 |
|----------|----------|----------|--------|
| **信号线交叉** | MACD 线上穿/下穿信号线 | 动量方向转变 | 领先但假信号多 |
| **零轴交叉** | MACD 线穿越 0 轴 | 趋势方向确认 | 滞后但更可靠 |
| **背离** | 价格创新高/低但 MACD 未跟随 | 潜在趋势反转 | 最强但稀少 |

## 历史起源与参数选择

原始参数 12/26/9 来自 6 天交易周：
- 12 ≈ 半个月
- 26 ≈ 1 个月
- 9 ≈ 1.5 周

**现代争议**：现为 5 天交易周，参数是否仍最优存在讨论。部分交易者使用替代参数。

## 局限性

- **本质是滞后指标**：基于 EMA，信号滞后于价格
- **对震荡市无效**：价格在区间内震荡时信号频繁假突破
- **趋势明确时效果好**：顺势使用时可靠性更高

## 相关概念

- [[macd-divergences]] — 背离类型详解（最重要信号）
- [[macd-crossovers]] — 信号线与零轴交叉深度分析
- [[macd-parameters]] — 参数方言与优化
- [[macd-histogram]] — 直方图动量分析
- [[macd-area-and-zero-return]] — 面积守恒/零轴回归/柱体收缩
- [[macd-fangyan]] — 民间方言与实战战法（水上水下/缩头缩脚/空中加油/三板斧等）
- [[macd-normalized-alternatives]] — MACD 标准化：Z-Score/百分位/ATR归一化/KST/替代指标
- [[technical-analysis]] — 指标在技术分析中的位置
