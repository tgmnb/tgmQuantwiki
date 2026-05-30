---
title: MACD Histogram
created: 2026-04-24
updated: 2026-04-24
type: concept
tags: [quant, indicator, macd, histogram, momentum]
sources: [raw/articles/macd-wikipedia-2026.md]
confidence: high
---

# MACD Histogram（直方图）

## 公式

```
Histogram = MACD Line − Signal Line
```

本质是 MACD 线与信号线之间的**差值**，以柱状图形式呈现。

- **柱在零轴上方** → MACD Line > Signal Line → 多头动量
- **柱在零轴下方** → MACD Line < Signal Line → 空头动量
- **柱的高度** → 两线差距的大小 → 动量强弱

---

## 斜率分析：领先于交叉

直方图最重要的用法：**斜率变化早于 MACD 线交叉**

| 直方图形态 | 含义 |
|-----------|------|
| 柱体放大（远离零轴） | 动量加速，趋势加强 |
| 柱体收缩（靠近零轴） | 动量减弱，警惕反转 |
| 柱体由负转正并放大 | 多头动量启动（早于信号线交叉） |
| 柱体由正转负并放大 | 空头动量启动（早于信号线交叉） |

**实战意义**：可以通过观察直方图**提前 1-3 根 K 线**预判即将发生的交叉。

---

## 直方图背离

与 MACD 线背离类似，但直方图背离有时比 MACD 线背离更早出现：

| 类型 | 价格走势 | 直方图走势 | 信号 |
|------|----------|------------|------|
| 底背离 | LL（创新低） | 对应低点抬高 | 看涨 |
| 顶背离 | HH（创新高） | 对应高点降低 | 看跌 |

直方图背离与 [[macd-divergences]] 中的常规底背离/顶背离本质相同，可以结合使用。

---

## 直方图作为动量晴雨表

```
直方图极值 = 潜在趋势终点
```

当直方图达到极端高度时：
- **极高位**：多方力量耗尽，可能反转（配合顶背离 → 卖出）
- **极低位**：空方力量耗尽，可能反弹（配合底背离 → 买入）

这与 [[RSI]] 的超买超卖逻辑类似，都是动量极值判断工具。

---

## 与信号线交叉的时序关系

```
1. 直方图柱开始收缩（但仍为正/负）
2. 直方图柱消失（= MACD 线 = Signal 线 = 零轴交叉点）
3. 直方图柱反向放大
```

步骤 1 出现时，步骤 2/3 即将发生——这给了交易者**提前预判**的机会。

---

## Alexander Elder 三重屏幕中的直方图

在 Elder 的三重屏幕系统中：
- **第一屏幕（日线）**：判断主趋势方向（看 MACD 线方向）
- **第二屏幕（小时）**：只在顺着日线方向时，关注 MACD 直方图变绿/变红
- **第三屏幕（分钟）**：精确入场时机

直方图在第二屏幕中起到**趋势延续确认**的作用。

---

## Python 计算

```python
def macd_histogram(close: pd.Series, fast=12, slow=26, signal=9):
    ema_fast = close.ewm(span=fast, adjust=False).mean()
    ema_slow = close.ewm(span=slow, adjust=False).mean()
    macd_line = ema_fast - ema_slow
    signal_line = macd_line.ewm(span=signal, adjust=False).mean()
    histogram = macd_line - signal_line
    return histogram

# 斜率变化预警
def histogram_slope_change(histogram, lookback=3):
    slopes = histogram.diff(lookback)
    return slopes.diff()  # 斜率的变化方向
```

---

## 相关概念

- [[macd]] — 直方图是 MACD 三个组成部分之一
- [[macd-crossovers]] — 直方图收缩是交叉的提前预警
- [[macd-divergences]] — 直方图顶底背离含义相同
- [[technical-analysis]] — 直方图是动量类指标
