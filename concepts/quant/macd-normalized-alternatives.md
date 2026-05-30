---
title: MACD 标准化与替代指标（含纵向对比专篇）
created: 2026-04-24
updated: 2026-04-24
type: concept
tags: [quant, indicator, macd, 标准化, 归一化, alternative-indicators]
confidence: high
sources: [wikipedia, tradingview, 量化知识]
---

# MACD 标准化与替代指标

## 核心问题

MACD 值受价格绝对水平影响：

| 股票 | 价格 | DIF 值 | 含义 |
|------|------|--------|------|
| 贵州茅台 | 1800 元 | DIF = 50 | 可能只是小幅波动 |
| 工商银行 | 5 元 | DIF = 0.5 | 可能已经是非常大的波动 |

**同一档 DIF 数值在不同股票、不同时间点上含义完全不同**，导致：
1. **无法横向对比**：两只股票间谁的动量更强？
2. **无法纵向对比**：当前动量相对自身历史是强是弱？

---

## 一、标准化方法

### 1. Z-Score 标准化

**思想**：将 DIF 转换为其在历史分布中的位置，以标准差为单位。

```python
import pandas as pd
import numpy as np

def macd_zscore(close: pd.Series, fast=12, slow=26, signal=9, lookback=60):
    ema_fast = close.ewm(span=fast, adjust=False).mean()
    ema_slow = close.ewm(span=slow, adjust=False).mean()
    dif = ema_fast - ema_slow

    rolling_mean = dif.rolling(lookback).mean()
    rolling_std = dif.rolling(lookback).std()
    dif_zscore = (dif - rolling_mean) / rolling_std

    return dif_zscore
```

**解读**：Z-Score = 2 表示当前 DIF 比过去 60 天均值高 2 个标准差。

**优点**：数学严谨，可跨股票/跨时间对比。
**缺点**：需要足够历史数据；假设正态分布（实际金融数据往往不是）。

---

### 2. 百分位排名（最实用）

**思想**：当前 DIF 在过去 N 天中排百分之几。

```python
def macd_percentile(close: pd.Series, fast=12, slow=26, signal=9, lookback=120):
    ema_fast = close.ewm(span=fast, adjust=False).mean()
    ema_slow = close.ewm(span=slow, adjust=False).mean()
    dif = ema_fast - ema_slow

    def pct_rank(series):
        return series.rank(pct=True).iloc[-1] * 100

    dif_pct = dif.rolling(lookback).apply(pct_rank, raw=False)
    return dif_pct
```

**解读**：任何时刻都能说"当前 DIF 处于历史 85% 分位"。

**优点**：直观（0-100%），不需要假设分布，极端值不会跑出 0-100 范围。
**缺点**：滞后——排名计算依赖窗口期。

---

### 3. 除以价格或 ATR

**思想**：以相对波动的比例来衡量 DIF。

```python
def macd_normalized_by_price(close: pd.Series, fast=12, slow=26, signal=9):
    ema_fast = close.ewm(span=fast, adjust=False).mean()
    ema_slow = close.ewm(span=slow, adjust=False).mean()
    dif = ema_fast - ema_slow
    return (dif / close) * 100  # 百分比形式

def macd_normalized_by_atr(close: pd.Series, atr_period=14, fast=12, slow=26, signal=9):
    ema_fast = close.ewm(span=fast, adjust=False).mean()
    ema_slow = close.ewm(span=slow, adjust=False).mean()
    dif = ema_fast - ema_slow

    high = close.rolling(14).max()
    low = close.rolling(14).min()
    tr = pd.concat([
        high - low,
        abs(high - close.shift(1)),
        abs(low - close.shift(1))
    ], axis=1).max(axis=1)
    atr = tr.rolling(atr_period).mean()

    return dif / atr  # DIF 是 ATR 的几倍
```

**解读**：
- 除以价格：DIF 是价格的百分之几
- 除以 ATR：DIF 是平均波动率的倍数（最常用）

**优点**：简单，物理含义清晰。
**缺点**：价格接近零时失真；ATR 在剧烈波动时会突变。

---

## 二、替代指标（成型可用）

### 1. KST (Know Sure Thing)

**创建者**：Martin Pring
**本质**：多个时间周期 ROC（变动率，Rate of Change）的加权平滑组合。

```
KST = (RCRA * 1) + (RCRB * 2) + (RCRC * 3) + (RCRD * 4) / 10
```
其中 RCRA = 10日 ROC 的 10日 SMA，其他类似，只是周期越来越长。

**为什么解决了标准化问题**：
- ROC 本身已经是百分比形式（相对变动），天然具有可比性
- 多周期组合进一步平滑，让输出范围相对有限

**信号**：KST 上穿/下穿其信号线（类似 MACD 金叉/死叉）。

---

### 2. OsMA (Oscillator of MACD)

**本质**：MACD 线 - 信号线，即 MACD 直方图。

**为什么比 MACD 更标准化**：
- 直方图 = 两个 MACD 值的差，是"差距的差距"
- 在零轴上下振荡，范围相对有限
- 不同标的之间 OsMA 的量级更可比

**注意**：OsMA 严格来说不是归一化，只是把 MACD 的输出做了变换。

---

### 3. MACD-V

**来源**：TradingView 社区指标，有专门视频介绍（B站"不聪明钱交易"，682 播放）

**核心改变**：参数调整为 (5, 35, 5)，替代标准 (12, 26, 9)。

**效果**：更短的快线周期让 MACD 对价格变动更敏感，输出数值范围相对缩小。

**注意**：不是严格归一化，只是参数变了之后跨标的可比性稍好。

---

### 4. AO (Awesome Oscillator)

**创建者**：Bill Williams
**公式**：AO = (5日简单均线 - 34日简单均线) 的中点

**本质**：用 SMA 替代 EMA 的 MACD 版本。

**与 MACD 的区别**：
- SMA 对价格变化比 EMA 更敏感
- AO 输出范围相对有限
- Bill Williams 的 **AC (Accelerator Oscillator)** 是 AO 的标准化版本——AC = AO - AO 的 5 日均线，进一步消去了 AO 的"基础动量"。

---

### 5. 其他相关指标

| 指标 | 创建者 | 解决问题 | 与 MACD 关系 |
|------|--------|----------|-------------|
| **ROC (变动率)** | 不明确 | 直接衡量百分比变化 | MACD 本质是 EMA 的 ROC |
| **RSI** | J. Welles Wilder | 标准化到 0-100 | 互补，不是同类替代 |
| **CCI** | Donald Lambert | 标准化到 ±100 范围 | 互补，超买超卖 |
| **Momentum** | 不明确 | 简单价格差值 | MACD 的前身简化版 |

---

## 三、方法对比

| 方法 | 标准化程度 | 可比性 | 实现难度 | 实战可用性 |
|------|-----------|--------|----------|-----------|
| Z-Score | ★★★★★ | 强 | 中等 | 需要足够历史 |
| 百分位排名 | ★★★★☆ | 强 | 中等 | **最推荐** |
| 除以价格 | ★★★☆☆ | 中 | 简单 | 价格过低时失效 |
| 除以 ATR | ★★★★☆ | 强 | 简单 | **推荐** |
| KST | ★★★★☆ | 强 | 需学习 | 成熟应用体系 |
| OsMA | ★★★☆☆ | 中 | 无 | 直接使用 |
| MACD-V | ★★☆☆☆ | 弱 | 无 | 参数改而已 |

---

## 四、纵向对比专篇（单标的时序比较）

> 本章节专注解决：**同一标的在不同时间段上的动量强度比较**，不涉及跨标的横向对比。

### 问题本质

纵向对比的障碍来自价格本身的长期漂移。例如：
- 2015 年牛市顶部：上证指数 5000 点，DIF 可能高达 200
- 2024 年反弹高点：3500 点，DIF 可能只有 80

**同样是"很强"的技术信号，绝对数值差了 2.5 倍**，如果只用 DIF 绝对值判断，会误判历史比较基准。

---

### 方案一：除以长期 MA（TGM 的思路）

```python
def macd_divide_ma(close: pd.Series, fast=12, slow=26, signal=9, ma_period=200):
    ema_fast = close.ewm(span=fast, adjust=False).mean()
    ema_slow = close.ewm(span=slow, adjust=False).mean()
    dif = ema_fast - ema_slow
    ma = close.rolling(ma_period).mean()
    return dif / ma
```

**优点**：实现简单，直观。`DIF / MA200` 是" DIF 是 200 日均线的百分之几"的量纲。
**缺点**：长期 MA 本身在趋势行情中会持续漂移（牛市里均线跟着价格向上走），导致比值会在一个偏高区域稳定很久，判断"现在到底比历史贵还是便宜"的精度下降。

---

### 方案二：百分比 DIF（最简单，推荐改进）

```python
def macd_pct(close: pd.Series, fast=12, slow=26, signal=9):
    """
    DIF / 当前价格 × 100
    含义：DIF 是当前价格的百分之几
    """
    ema_fast = close.ewm(span=fast, adjust=False).mean()
    ema_slow = close.ewm(span=slow, adjust=False).mean()
    dif = ema_fast - ema_slow
    return (dif / close) * 100
```

**为什么优于方案一**：
- 分母是**当前价格**，随价格即时调整，不存在均线那种"漂移滞后"
- 在趋势中比方案一更稳定，不会因为分母持续上涨而被动压低比值

---

### 方案三：对数价格空间 MACD（最推荐）

**原理**：在数学上最干净——对数变换把价格的比例变化变成线性变化。

```
log(P_t) - log(P_{t-n}) ≈ ln(P_t / P_{t-n})
```

对数空间的 EMA 差值，本质上已经是"对数收益率之差"，天然无量纲。

```python
def macd_lognormal(close: pd.Series, fast=12, slow=26, signal=9):
    """
    在对数价格空间计算 MACD
    等价于对数收益率的 EMA 差值，天然可比
    """
    log_price = np.log(close)

    ema_fast = log_price.ewm(span=fast, adjust=False).mean()
    ema_slow = log_price.ewm(span=slow, adjust=False).mean()
    dif = ema_fast - ema_slow

    signal_line = dif.ewm(span=signal, adjust=False).mean()
    histogram = dif - signal_line

    return dif * 100, signal_line * 100, histogram * 100
    # 乘 100 变成以"百分比"为单位，更直觉
```

**信号解读（与标准 MACD 完全一致）**：
- DIF > 0：短期对数均线在长期均线之上 → 上涨趋势
- DIF 上穿信号线：动量加速
- DIF 穿越零轴：趋势方向确认
- 背离：价格创新高/低但 DIF 未跟随

**为什么纵向可比**：
- 对数空间的 EMA 差值不受价格绝对水平影响
- 2015 年 DIF = 0.10 和 2024 年 DIF = 0.08 在同一尺度上，代表相近的"对数收益率差"

---

### 方案四：滚动 Z-Score（最精确）

```python
def macd_zscore_longitudinal(close: pd.Series, fast=12, slow=26, signal=9, lookback=120):
    """
    滚动 Z-Score：衡量当前 DIF 偏离自身历史均值的程度
    lookback 决定"历史"的窗口，越长越稳健但越滞后
    """
    log_price = np.log(close)
    ema_fast = log_price.ewm(span=fast, adjust=False).mean()
    ema_slow = log_price.ewm(span=slow, adjust=False).mean()
    dif = ema_fast - ema_slow

    rolling_mean = dif.rolling(lookback).mean()
    rolling_std = dif.rolling(lookback).std()
    dif_zscore = (dif - rolling_mean) / rolling_std

    signal_line = dif.ewm(span=signal, adjust=False).mean()
    signal_zscore = (signal_line - rolling_mean) / rolling_std

    return dif_zscore, signal_zscore
```

**解读**：
- Z-Score = +2：当前 DIF 比过去 120 天均值高出 2 个标准差 → 极端动量
- Z-Score = -1.5：低于均值 1.5 个标准差 → 弱势

---

### 四种方案横向对比

| 方案 | 纵向可比性 | 趋势漂移 | 计算成本 | 信号解释 |
|------|-----------|----------|----------|----------|
| 除以长期 MA | 中 | 有 | ★ | 与标准 MACD 相同 |
| 百分比 DIF | 好 | 轻微 | ★★ | 与标准 MACD 相同 |
| 对数 MACD | **最好** | 无 | ★★ | 与标准 MACD 相同 |
| 滚动 Z-Score | **最好** | 无 | ★★★ | Z-Score ±2 为极端区 |

---

### 推荐实践

**如果只做纵向比较**，最推荐的路径是：

```python
# 第一步：取对数价格
log_close = np.log(close)

# 第二步：在对数空间计算 MACD
dif, signal, hist = macd_lognormal(close)

# 第三步：判断信号（与标准 MACD 完全一致）
# DIF > 0 → 上涨趋势
# DIF 上穿 signal → 金叉（动量加速）
# DIF 下穿 signal → 死叉（动量减弱）
# DIF 穿越 0 → 趋势确认
# 价格 vs DIF 背离 → 反转信号
```

这样整个 MACD 体系不需要改变任何使用习惯，只是输入从价格换成了 log(价格)，但解决了纵向可比的问题。

---

## 五、方法对比

| 方法 | 标准化程度 | 可比性 | 实现难度 | 实战可用性 |
|------|-----------|--------|----------|-----------|
| Z-Score | ★★★★★ | 强 | 中等 | 需要足够历史 |
| 百分位排名 | ★★★★☆ | 强 | 中等 | **最推荐** |
| 除以价格 | ★★★☆☆ | 中 | 简单 | 价格过低时失效 |
| 除以 ATR | ★★★★☆ | 强 | 简单 | **推荐** |
| 对数 MACD | ★★★★★ | 强 | 简单 | **最推荐做纵向比较** |
| KST | ★★★★☆ | 强 | 需学习 | 成熟应用体系 |
| OsMA | ★★★☆☆ | 中 | 无 | 直接使用 |
| MACD-V | ★★☆☆☆ | 弱 | 无 | 参数改而已 |

---

## 六、实战建议

1. **只做纵向比较**：直接用对数 MACD，原有 MACD 信号体系不变
2. **需要判断极端程度**：叠加滚动 Z-Score，Z > 2 / < -2 为极端区
3. **最实用组合**：对数 MACD 找方向，滚动 Z-Score 判断极端程度

---

---

## 七、"保留形、改变度"——纵向标准化设计框架

> 本节内容来自 DeepSeek 分析（2026-04），是纵向对比设计的核心理念。

### 问题

MACD 做纵向对比时面临"形 vs 度"的两层需求：

| 需求 | 内容 | 是否需要标准化 |
|------|------|--------------|
| **形**（方向/结构） | 零轴之上/下、金叉/死叉、柱体方向 | **保留原始**——方向信号不能失真 |
| **度**（强度/位置） | 柱体长短、距零轴多远、历史分位 | **需要标准化**——横向纵向才可比 |

### 框架：只标准化直方图，保留黄白线

```
标准化对象：只有 MACD 直方图（histogram = DIF - Signal）
标准化方法：MACD-V = (EMA12 - EMA26) / ATR(26) × 100

黄白线（DIF / Signal）：保留原始形式
  → 零轴穿越、金叉死叉的判断逻辑完全不变
  → 柱体高度：标准化后的 ATR 倍数量纲，横向纵向均可比
```

**为什么这样设计**：
- DIF/DEA 的方向（>0/<0、交叉方向）反映的是**趋势方向**，标准化会扭曲这个基本逻辑
- 直方图的**长度/面积**是**动量强度**，最需要跨时间/跨标的可比性
- 这一框架与创作者 Alex Spiroglou（MACD-V）的原始设计思路一致

### 实现：MACD-V 直方图

```python
def macd_histogram_normalized(close: pd.Series, fast=12, slow=26, signal=9, atr_period=26):
    ema_fast = close.ewm(span=fast, adjust=False).mean()
    ema_slow = close.ewm(span=slow, adjust=False).mean()
    dif = ema_fast - ema_slow
    signal_line = dif.ewm(span=signal, adjust=False).mean()
    histogram = dif - signal_line

    # ATR 计算
    high = close.rolling(14).max()
    low = close.rolling(14).min()
    tr = pd.concat([
        high - low,
        abs(high - close.shift(1)),
        abs(low - close.shift(1))
    ], axis=1).max(axis=1)
    atr = tr.rolling(atr_period).mean()

    # 标准化直方图
    histogram_normalized = (histogram / atr) * 100
    return dif, signal_line, histogram_normalized
```

---

## 八、波动率不完美与价格等比缩放

> 本节说明为什么标准化后跨周期对比仍有细微差异。

### 问题

即便使用了 ATR 标准化，跨大周期对比时"同样是20"也可能代表不同含义：

```
1元股：DIF = 1，ATR = 0.05 → MACD-V = 20
30元股：DIF = 30，ATR = 1.5 → MACD-V = 20

表面相同，但：
- 1元股从1涨到1.05（+5%）对应 ATR 倍数
- 30元股从30涨到31.5（+5%）对应 ATR 倍数

两个5%在 ATR 标准化后可能都显示为"20"，
但这两个5%背后的波动率结构未必完全相同。
```

### 原因

波动率（ATR）与价格不是完美等比缩放的。金融资产的波动率往往：
- **高价股**：波动率绝对值大，但**百分比波动率**往往偏低（机构持股多，波动被抑制）
- **低价股**：波动率绝对值小，但**百分比波动率**往往偏高（筹码分散，情绪主导）

因此：
- ATR 标准化**解决了量纲问题**（都是ATR倍数）
- 但不等同于"百分比收益率标准化"——波动率本身的结构差异仍然存在

### 实践建议

| 对比场景 | 推荐方法 |
|----------|---------|
| 同一标的跨时间纵向对比 | 对数 MACD 或滚动 Z-Score |
| 同一时间段跨标的横向对比 | MACD-V 直方图（ATR 标准化） |
| 需要判断极端程度 | 叠加滚动分位数排名 |
| 跨大周期比较（不同年代市场） | 对数 MACD + 分位数双重过滤 |

---

## 九、传统 MACD 用法保留程度

| 用法 | 原始信号 | 标准化后保留程度 | 说明 |
|------|---------|----------------|------|
| **零轴穿越** | DIF 穿越 0 | ✅ **完全保留** | 零轴含义不依赖标准化 |
| **金叉/死叉** | DIF 上穿/下穿 Signal | ✅ **完全保留** | 交叉方向不受影响 |
| **柱体方向** | 直方图红/绿 | ✅ **保留** | 方向判断保留，强度被标准化 |
| **柱体长度/面积** | 目测判断大小 | ⚠️ **需要标准化** | 标准化后才能跨期/跨标对比 |
| **顶背离/底背离** | 价格创新高/低但 DIF 未跟随 | ✅ **形状保留** | 背离是形态判断，标准化后依然有效 |
| **顶背离/底背离强度** | DIF 峰值对比 | ⚠️ **需要分位数/滚动Z** | 绝对值对比需标准化 |
| **多周期共振** | 不同周期信号叠加 | ✅ **完全保留** | 同指标同周期比较，标准化不改变结构 |
| **交叉角度/速度** | 快速上涨/缓慢上涨 | ❌ **标准化后丢失** | ATR 标准化改变了柱体绝对高度的物理含义 |

> 核心原则：**结构类信号（方向、交叉、背离形态）保留；强度类信号（柱体大小、历史分位）需要标准化。**

---

## 相关页面

- [[macd]] — MACD 基础
- [[macd-parameters]] — 参数优化
- [[macd-histogram]] — 直方图分析
- [[macd-fangyan]] — 民间方言战法
- [[technical-analysis]] — 技术分析框架
