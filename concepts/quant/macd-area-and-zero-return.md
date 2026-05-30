---
title: MACD Area and Zero Return
created: 2026-04-24
updated: 2026-04-24
type: concept
tags: [quant, indicator, macd, histogram, mean-reversion, momentum]
confidence: medium
---

# MACD Area and Zero Return（面积守恒与零轴回归）

## 概念澄清

"红柱绿柱守恒"和"面积"这两个概念在 MACD 官方文献中并无正式定义，但在中国 A 股/期货社区广为流传，需要区分：

| 术语 | 来源 | 含义 |
|------|------|------|
| 红柱/绿柱 | 中文看盘软件 | 直方图在零轴上方为红，下方为绿（或相反） |
| 面积守恒 | 民间概念 | 红绿柱面积趋于平衡的理论 |
| Zero Return | 英文技术分析 | MACD 偏离信号线后必然回归 |

**核心本质**：三者描述的是同一现象——MACD 作为 EMA 差值系统，具有**均值回归**特性。

---

## 1. 面积守恒（民间说法）

### 什么是"面积"

```
面积 = 柱体高度 × 时间宽度（柱的根数）

正面积 = 所有红柱（MACD > Signal）的 (MACD - Signal) 之和
负面积 = 所有绿柱（MACD < Signal）的 |MACD - Signal| 之和
```

### "守恒"的含义

民间说法认为：在一波完整的上涨+下跌周期内，**正面积 ≈ 负面积**。

但这**不是严格守恒**：
- MACD 不是价格，而是 EMA 差值
- EMA 差值的均值回归不保证面积相等
- 更准确的说法是：**趋势延续时面积会持续扩张，趋势衰竭时面积收缩**

### 实际应用

| 形态 | 含义 |
|------|------|
| 连续出现同一颜色大柱 | 趋势强劲，面积持续扩张 |
| 红柱越来越小 | 动量衰减，可能反转 |
| 红柱消失（收缩到零） | 交叉即将发生 |

---

## 2. 零轴回归（Return to Zero）

### 数学本质

MACD 的回归特性来自 EMA 的数学性质：

```
MACD(t) = EMA_fast(t) - EMA_slow(t)

当 EMA_fast 偏离 EMA_slow 后，
新数据点的 EMA 更新会让两者差距缩小
→ MACD 向零回归
```

这与价格围绕均线的均值回归类似，只是这里的"均线"是另一条 EMA。

### 回归速度

| 参数 | 回归速度 |
|------|----------|
| 快参数（5,13,5） | 快，容易回归后再次偏离 |
| 慢参数（19,39,9） | 慢，偏离后需要更长时间回归 |

### 回归实战含义

- **偏离越大，回归张力越大**：MACD 离零轴越远，未来回归的可能性和幅度越大
- **趋势中继 vs 反转**：强趋势中，MACD 可以在零轴一侧停留很久，不必然快速回归
- **回归不保证价格反转**：MACD 回归只说明短期 EMA 和中期 EMA 的差距在缩小，不代表价格反转

---

## 3. 柱体收缩：领先预警

这是"面积守恒"最实用的部分——**直方图收缩是交叉的提前 1-3 根 K 线预警**：

```
上涨行情顶部：
红柱开始缩小（但仍为红）→ MACD 线在减速 → 即将死叉（绿柱出现）
```

```
下跌行情底部：
绿柱开始缩小（但仍为绿）→ MACD 线在减速 → 即将金叉（红柱出现）
```

**识别要点**：
- 柱体高度变化率下降 → 动量衰竭
- 柱体高度触及零轴 → 交叉已完成
- 柱体收缩速度加快 → 交叉临近

---

## 4. MACD 的三种回归轴

MACD 有三个潜在的"回归目标"：

| 回归轴 | 含义 | 信号强度 |
|--------|------|----------|
| **信号线** | MACD → Signal（最小回归） | 日常信号 |
| **零轴** | MACD → 0（中期 EMA = 长期 EMA） | 趋势确认 |
| **长期均线** | 价格 → 长期均线（最大回归） | 超长周期 |

多数"守恒"讨论实际上混淆了这三个层次。

---

## 5. 与[[macd-histogram]]中斜率分析的关系

[[macd-histogram#斜率分析领先于交叉]] 已经详细描述了柱体收缩的领先预警逻辑。"面积守恒"可以看作是斜率分析的另一种表述：

- **斜率 = 面积变化率**：斜率由正转负 → 柱体收缩 → 面积在调整
- **"守恒" = 回归必然性**：EMA 系统保证 MACD 最终会回归信号线

两者本质相同，只是民间用"面积"概念来直观理解。

---

## 6. Python 模拟：验证回归特性

```python
import pandas as pd
import numpy as np

def simulate_macd_reversion(n=100, seed=42):
    """模拟 MACD 的均值回归特性"""
    np.random.seed(seed)
    # 随机游走（价格）
    returns = np.random.randn(n) * 0.02
    price = 100 * np.exp(np.cumsum(returns))

    close = pd.Series(price)
    ema12 = close.ewm(span=12, adjust=False).mean()
    ema26 = close.ewm(span=26, adjust=False).mean()
    macd = ema12 - ema26
    signal = macd.ewm(span=9, adjust=False).mean()
    histogram = macd - signal

    # 计算面积
    pos_area = histogram[histogram > 0].sum()
    neg_area = histogram[histogram < 0].abs().sum()

    print(f"正面积: {pos_area:.4f}")
    print(f"负面积: {neg_area:.4f}")
    print(f"面积比: {pos_area/neg_area:.2f} (理想=1.0)")

    return histogram

# 注意：随机数据下面积比通常不=1，因为趋势会持续
# 在震荡市场或长期来看，比值更接近1.0
```

---

## 相关概念

- [[macd]] — MACD 基础公式
- [[macd-histogram]] — 斜率分析和领先预警（核心应用）
- [[macd-crossovers]] — 交叉作为最终确认
- [[macd-divergences]] — 背离作为回归失效的信号
- [[technical-analysis]] — 均值回归是技术分析核心理念
