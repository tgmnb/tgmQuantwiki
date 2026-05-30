---
title: Support and Resistance
created: 2026-04-23
updated: 2026-04-23
type: concept
tags: [quant, technical-analysis, support-resistance]
confidence: high
---

# Support and Resistance

## 核心概念

- **支撑 (Support)**: 需求集中的价格区域，价格在此止跌
- **阻力 (Resistance)**: 供给集中的价格区域，价格在此止涨
- **角色互换**: 支撑被跌破后成为阻力，反之亦然

## 支撑阻力类型

### 静态（水平）
- 前高/前低
- 整数关口
- 均线（SMA 50/200）

### 动态（倾斜）
- **趋势线**: 连接依次上移的低点（上升趋势线）或依次下移的高点（下降趋势线）
- **管道线**: 趋势线的平行线
- **均线**: EMA/SMA 动态支撑

### 成交量轮廓
- **VPVR**: 高成交量节点 = 强支撑/阻力
- **VWAP**: 日内价值中枢

## Python 识别示例

```python
import pandas as pd
import numpy as np

def find_support_resistance(prices: pd.Series, window: int = 5) -> dict:
    """识别支撑和阻力位"""
    highs = prices.rolling(window=2*window+1).max()
    lows = prices.rolling(window=2*window+1).min()

    # 局部高点：比前后 window 根 K 线都高
    resistance_mask = (prices == highs) & \
                      (prices.shift(window) < prices) & \
                      (prices.shift(-window) < prices)

    # 局部低点：比前后 window 根 K 线都低
    support_mask = (prices == lows) & \
                   (prices.shift(window) > prices) & \
                   (prices.shift(-window) > prices)

    return {
        'resistance_levels': prices[resistance_mask].unique(),
        'support_levels': prices[support_mask].unique()
    }
```

## 支撑阻力的强度判断

| 因素 | 强度影响 |
|------|----------|
| 测试次数 | 多次触及更强（过度测试会削弱） |
| 成交量 | 成交放大确认强度 |
| 时间 | 长期横盘积累的价位更强 |
| 与均线的距离 | 远离均线回调的空间更大 |

## 相关概念

- [[technical-analysis]] — 指标框架
- [[trading-regime]] — 在震荡行情 S/R 更有效
