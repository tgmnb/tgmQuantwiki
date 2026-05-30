---
title: MACD Variants Comparison
created: 2026-04-24
updated: 2026-04-24
type: comparison
tags: [quant, indicator, macd, comparison, variants]
confidence: medium
---

# MACD Variants Comparison（MACD 方言变体比较）

## 标准 MACD

| 属性 | 值 |
|------|-----|
| 全称 | Moving Average Convergence Divergence |
| 创建者 | Gerald Appel (1970s) |
| 公式 | MACD = EMA(12) − EMA(26); Signal = EMA(9) of MACD |
| 标注形式 | MACD(12,26,9) |
| 信号类型 | 趋势方向、动量强度、反转预警 |

---

## MACD-S（Smoothed MACD / 平滑 MACD）

| 属性 | 值 |
|------|-----|
| 公式变更 | Signal Line = EMA(9) of **Price**（非 MACD 线） |
| MACD Line | 仍为 EMA(12) − EMA(26) |
| 主要改进 | 减少信号滞后 |
| 适用场景 | 需要更快反应的趋势跟踪 |

**核心理念**：用价格自身的 EMA 做平滑，而非 MACD 线的二次平滑，减少了一层滞后。

---

## MACD-R（Reversed MACD / 反向 MACD）

| 属性 | 值 |
|------|-----|
| 创建者 | John Foreman (Linear Capital) |
| 公式变更 | MACD-R = Price_EMA − MACD_Signal |
| 主要改进 | 零轴交叉对应价格本身 |
| 适用场景 | 习惯从价格视角解读信号的交易者 |

**核心理念**：把信号线变成主角，MACD 线变成配角，改变零轴交叉的含义。

---

## Zero-Lag MACD（零滞后 MACD）

| 属性 | 值 |
|------|-----|
| 改进方式 | 在 EMA 中引入价格与 EMA 差值的加权项 |
| 目标 | 减少传统 EMA 的固有滞后 |
| 实现方式 | 不同平台实现方式略有差异 |
| 适用场景 | 日内交易，需要快速反应 |

---

## PPO (Percentage Price Oscillator)

|| 属性 | 值 |
|------|-----|
| 全称 | Percentage Price Oscillator |
| 公式 | PPO = (EMA12 - EMA26) / EMA26 × 100 |
| 与 MACD 本质区别 | MACD 是绝对值差，PPO 是相对值（百分比） |
| 横向对比能力 | **强** — 百分比量纲，天然可比 |
| 纵向对比能力 | **弱** — 不解决波动率差异问题，跨周期仍有漂移 |
| 特点 | Signal 线 = PPO 的 EMA(9) |

**PPO vs MACD 的关键差异**：

```
MACD：DIF = EMA12 - EMA26（绝对值）
PPO：DIF = (EMA12 - EMA26) / EMA26 × 100（百分比）

跨标的对比：PPO 更可比（都是百分比）
但跨时间纵向：两者都有漂移，PPO 不比 MACD 更好
```

---

## MACD-V (Volume-Weighted MACD)

|| 属性 | 值 |
|------|-----|
| 全称 | MACD with Volume (有时写作 MACD-V) |
| 创建者 | **Alex Spiroglou** |
| 奖项 | 2022 年 NAAIM Founders Award |
| 公式变更 | 参数 (5, 35, 5) + 除以 ATR(26) |
| 标准化方式 | (EMA5 - EMA35) / ATR(26) × 100 |
| 横纵向能力 | **横向强，纵向也强**（相对 ATR 量纲） |

**为什么 MACD-V 是迄今最成熟的标准化方案**：
1. **参数调整**：5/35 比标准 12/26 对短期波动更敏感，输出数值范围更紧凑
2. **ATR 归一化**：除以 ATR 将 DIF 变成"ATR 倍数"，解决了价格绝对值问题
3. **行业认可**：NAAIM Founders Award 是美国投资管理行业协会的年度奖项，有公信力

> ⚠️ 注意：MACD-V 不是民间指标，是有量化背景的正式改进方案。

---

## TTM MACD（Trending Trend Mode MACD）

| 属性 | 值 |
|------|-----|
| 来源 | TTM (Trends in Motion) 指标组 |
| 特点 | 在直方图上叠加颜色编码，区分趋势模式和区间模式 |
| 视觉改进 | 红绿区域显示当前趋势方向 |
| 适用场景 | 快速识别趋势 vs 震荡 |

---

## 参数方言对比

|| 参数组合 | 风格 | 适用场景 | 信号频率 | 备注 |
|----------|------|----------|----------|----------|------|
|| 5, 35, 5 | 超短线 | 日内/剥头皮 | 极高 | MACD-V 默认参数 |
|| 5, 13, 5 | 短线 | 日内波段 | 高 ||
|| 8, 17, 9 | 短中线 | 外汇日内 | 中高 ||
|| 12, 26, 9 | 标准 | 通用（默认） | 中 | Gerald Appel 原始参数 |
|| 19, 39, 9 | 中长线 | 股票波段 | 低 ||
|| 25, 75, 50 | 长线 | 趋势跟踪 | 极低 ||
| PPO | (12,26,9) % | 百分比版 | 跨标的对比 | 中 | 横比强，纵比弱 |
| MACD-V | (5,35,5) / ATR | ATR 标准化 | 横向+纵向 | 高 | 2022 NAAIM 奖 |

---

## 横向时间周期叠加

不同时间周期的 MACD 信号相互确认：

| 组合 | 含义 |
|------|------|
| 周线 MACD 金叉 + 日线 MACD 金叉 | 强强联合，趋势确认 |
| 周线 MACD 金叉 + 日线 MACD 死叉 | 长短分歧，高级别趋势优先 |
| 日线 MACD 背离 + 4H MACD 背离 | 多周期共振，反转概率极高 |

---

## 与其他动量指标横向对比

| 指标 | 类型 | 超买超卖 | 滞后性 | 趋势/区间 |
|------|------|----------|--------|-----------|
| MACD | 趋势+动量 | 无直接读数 | 中等 | 均可 |
| [[RSI]] | 纯动量 | 有（0-100） | 中等 | 区间更强 |
| Stochastic | 动量 | 有（0-100） | 较强 | 区间更强 |
| [[CCI]] | 趋势+动量 | 有（±100） | 中等 | 趋势更强 |

---

## 方言选用建议

1. **标准 MACD(12,26,9)**：默认值，最广泛使用，与多数交易者信号同步
2. **8,17,9**：外汇市场流行参数，适应 24 小时连续交易
3. **MACD-S**：如果觉得标准 MACD 滞后太多，可尝试
4. **5,35,5**：高波动资产（加密货币）或极短线交易
5. **Zero-Lag**：日内交易者追求快速反应

**不要过度优化参数** — 标准参数的自我实现特性是其重要优势。

---

## 相关概念

- [[macd]] — 标准 MACD 基础框架
- [[macd-divergences]] — 各类变体中共用的背离逻辑
- [[macd-crossovers]] — 交叉信号在不同变体中含义不同
- [[macd-histogram]] — 直方图是多数变体的共同元素
- [[technical-analysis]] — 指标选择是技术分析决策的一部分
