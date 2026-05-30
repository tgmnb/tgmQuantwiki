---
title: MA Band 均线带选择演进
created: 2026-05-05
updated: 2026-05-05
type: concept
tags: [strategy, factor, ma-band, selection]
sources: [/mnt/c/quant/ma_band_backtest/src/selector.py, /mnt/c/quant/ma_band_backtest/src/mfe_selector.py, /mnt/c/quant/ma_band_backtest/src/band_sim.py]
confidence: high
---

# MA Band 均线带选择演进

## 概述

均线带选择是策略的核心——决定哪些MA配对在当下市场中最有效。选择系统经历了三代演进，从简单的统计排序到复杂的模拟驱动评分。

## 选择系统架构

```
全量触碰/穿透事件
        ↓
  全量统计（每月/每周）
  累计touch_count, penetrate_count
  计算penetration_ratio, ratio_drop
        ↓
  长期池 Top 20（每周更新）
  从全量统计中按评分选出
        ↓
  短期池 Top 5（每天更新）
  在长期池内，限定近期窗口重新评分选出
        ↓
  执行池 Top N（每根K线可用）
  策略层直接使用的前N个均线带
```

## V3: ratio_drop 选择器（原始方案）

### 核心逻辑

```python
# band_engine.py
penetration_ratio[i] = penetrate_count[i] / touch_count[i]
ratio_drop[i] = penetration_ratio[i-1] - penetration_ratio[i]

# selector.py
eligible = stats_total.sort_values("ratio_drop", ascending=False)
selected = eligible.head(top_n)
```

### 原理

`ratio_drop`衡量的是：从前一个带到当前带，穿透概率**下降了多少**。

- 下降越大 → 当前带的防守能力显著优于上一个带 → 这是一个"断点"
- 断点意味着市场"尊重"这个区间——价格很少突破它

### 局限性

1. **间接指标**：ratio_drop衡量的是"防守质量"，而非"盈利潜力"
2. **噪声大**：短周期MA的触碰/穿透次数少，ratio_drop不够稳定
3. **无交易模拟**：不测试每个带在实际交易中的表现
4. **过度依赖穿透**：只关心价格是否跌破，不关心有利方向的移动幅度

---

## V4: 模拟驱动选择器

### 核心创新

引入 `band_sim.py` 的 `simulate_band_trades`，**对每个候选均线带进行交易模拟**：

```python
# band_sim.py
def simulate_band_trades(df, ma_matrix, band, hold_bars=3, cost_rate=0.0008):
    for i in range(warmup_bars, len(df) - 1):
        if low_t <= left_ma_t:          # 触碰左侧MA
            entry_price = open[i+1]      # 下一根开盘入场
            exit_price = close[i+hold_bars]  # 固定持仓后离场
            net_return = gross_return - 2 * cost_rate
```

### 评分体系

```python
# selector.py (v4 mode)
# 初筛：过滤交易频率和毛收益
filtered = sim_df[
    (trades_per_year >= min_trades_py) &
    (trades_per_year <= max_trades_py) &
    (avg_gross_return >= gross_threshold) &
    (avg_net_return > 0)
]

# 评分：净收益 × sqrt(交易次数)
score = avg_net_return * sqrt(trade_count)
```

### 优势

1. **直接衡量盈利潜力**：模拟真实交易，不是间接统计
2. **过滤低质量带**：交易频率过滤（年化8-80笔）、毛收益门槛（>1.5×往返成本）
3. **考虑成本**：模拟中已扣除往返费率

### 局限性

1. **固定持仓时间**：hold_bars=3的固定持仓不反映最优离场
2. **无止损模拟**：不模拟止损触发，高估表现
3. **计算成本高**：每个候选带都需要完整模拟

---

## V5: MFE选择器（当前方案）

### 核心创新

用**最大有利偏移（Maximum Favorable Excursion, MFE）**替代简单的固定持仓模拟。

```python
# mfe_selector.py
def calculate_band_mfe_stats(long_events, df, band, end_idx, horizon_bars=20):
    for bar_idx in valid_events:
        entry_price = df['open'][entry_idx]
        future_high = df['high'][entry_idx:entry_idx+horizon_bars].max()
        mfe = (future_high - entry_price) / entry_price   # 最大有利偏移%
        mfe_net = mfe - 2 * cost_rate                      # 扣除成本
```

### MFE vs 固定持仓

| 维度 | V4 固定持仓 | V5 MFE |
|------|-----------|--------|
| 离场方式 | 固定hold_bars后平仓 | horizon内最高价 |
| 利润衡量 | 实际平仓盈亏 | 最大潜在利润 |
| 成本扣除 | 固定2×cost_rate | 固定2×cost_rate |
| 止损模拟 | 无 | 无 |
| 代表含义 | "持N根能赚多少" | "最多能赚多少" |

### 评分列

```python
# mfe_selector.py
stats['score_tail']  = top20_mfe * sqrt(event_count)        # top20% MFE × 开根号事件数
stats['score_avg']   = avg_mfe_net * sqrt(event_count)      # 平均净MFE × 开根号事件数
stats['score_log']   = top20_mfe * log(event_count + 1)     # top20% MFE × 对数事件数
stats['score_cap50'] = top20_mfe * sqrt(min(count, 50))     # top20% MFE × 事件数封顶50
stats['score_cap80'] = top20_mfe * sqrt(min(count, 80))     # top20% MFE × 事件数封顶80
```

### 过滤器

```python
def filter_bands_v5(band_stats):
    min_events_per_year=5.0    # 年化至少5个事件
    max_events_per_year=100.0   # 年化不超过100个（过滤高频噪声）
    min_top20_mfe=0.03          # top20% MFE至少3%
    min_avg_mfe_net=0.0         # 平均净MFE必须为正
```

### 评分列选择指南

| 评分列 | 适用场景 | 特点 |
|--------|---------|------|
| `score_cap80` | **生产推荐** | 平衡事件数和MFE，避免过拟合高频带 |
| `score_cap50` | 保守策略 | 更严格的事件数封顶 |
| `score_tail` | 追求高回报 | 关注极端有利情况 |
| `score_avg` | 稳健策略 | 关注平均表现 |
| `score_log` | 探索阶段 | 对数压缩事件数影响 |

### V11-V13实际使用

在V11到V13中，生产配置使用：
- 选择器版本：`v5_mfe`
- 评分列：`score_cap80`
- MFE展望窗口：20根K线
- 事件数封顶：80
- 成本：0.06%

## 三层选择时间表

```python
# backtester.py
if is_update_needed(current_time, last_update_full, "monthly"):
    full_stats = calculate_full_band_stats_from_events(...)
    # 重新计算所有带的累计统计

if is_update_needed(current_time, last_update_long, "weekly"):
    long_pool_bands = select_long_pool(full_stats, top_n=20)
    # 从全量统计中选出长期池

if is_update_needed(current_time, last_update_short, "daily"):
    short_stats = calculate_full_band_stats_from_events(...窗口限定)
    current_bands = select_short_pool(short_stats, long_pool_bands, top_n=5)
    # 在长期池内，用近期数据选出短期池
```

## 选择器对比总结

| 维度 | V3 ratio_drop | V4 模拟 | V5 MFE |
|------|:---:|:---:|:---:|
| 评估方式 | 穿透概率下降 | 固定持仓模拟 | 最大有利偏移 |
| 是否模拟交易 | ❌ | ✅ | ✅ |
| 是否衡量盈利 | ❌ | ✅ | ✅ |
| 计算成本 | 低 | 中 | 中 |
| 过拟合风险 | 中 | 中 | 低（事件数封顶） |
| 生产使用 | V1-V3 | V4实验 | V5-V13 |

## 相关页面

- [[ma-band-architecture]] — 完整策略架构
- [[ma-band-signals]] — 信号生成细节
- [[ma-band-version-history]] — 版本时间线
- [[v14-design]] — V14设计方案
