---
title: MA Band 信号生成
created: 2026-05-05
updated: 2026-05-05
type: concept
tags: [strategy, factor, ma-band, signal]
sources: [/mnt/c/quant/ma_band_backtest/src/strategy.py, /mnt/c/quant/ma_band_backtest/src/band_engine.py]
confidence: high
---

# MA Band 信号生成

## 概述

MA Band策略的信号系统基于**均线带触碰/穿透事件**，通过多层过滤机制生成高质量的交易信号。信号分为两大类：**入场信号**和**离场信号**。

## 信号类型

### 入场信号

```
pullback_entry: 价格回调触碰左侧均线，满足所有过滤条件后产生
```

入场信号在K线**收盘后**生成，实际执行在**下一根K线**。

### 离场信号

```
stop:          价格触及止损线
time_exit:     持仓超时（≥5根K线）且价格跌破入场价
band_inversion: 入场均线带排列反转（做多时left_ma <= right_ma，做空时反之）
eod:           到达数据末尾，强制平仓
```

## 入场过滤链

### 1. 回调检测（check_pullback_entry）

**做多方向：**
```python
prev_close > left_ma[t-1]       # 上一根收盘在左侧MA上方
AND low[t] <= left_ma[t]        # 本根最低价触碰左侧MA
AND right_ma[t] > right_ma[t-1]  # 右侧MA正在上行
AND left_ma[t] - right_ma[t] > 0  # 均线带处于多头排列
```

**做空方向：**
```python
prev_close < left_ma[t-1]       # 上一根收盘在左侧MA下方
AND high[t] >= left_ma[t]       # 本根最高价触碰左侧MA
AND close[t] < right_ma[t]      # 收盘仍在右侧MA下方（防守未失效）
```

### 2. 回调深度限制（可选）

```python
pullback_depth = (left_ma - low) / close
if pullback_depth > max_pullback_depth_pct:
    拒绝信号
```

防止价格过度穿透左侧MA后仍然入场。

### 3. 趋势过滤（check_trend_filter）

取参数池中最长的3根MA：MA_A < MA_B < MA_C（周期从长到短）

**做多要求：** `MA_C > MA_B > MA_A`（最长MA在下方，多头排列）
**做空要求：** `MA_C < MA_B < MA_A`（最长MA在上方，空头排列）

### 4. 斜率过滤（slope_filter_bars）

```python
# 右侧MA必须连续N根K线保持上行（做多）或下行（做空）
for i in range(slope_filter_bars):
    if ma[t-i] <= ma[t-i-1]:  # 做多: 须严格递增
        拒绝信号
```

### 5. 拥堵过滤（congestion_filter）

```python
# 在congestion_window_bars内，最多允许congestion_max_signals个信号
recent_signals = [b for b in congestion_signal_bars if t - b < congestion_window_bars]
if len(recent_signals) >= congestion_max_signals:
    拒绝信号
```

防止在窄幅震荡区间反复进出。

### 6. ATR门槛（ATR Gate）

```python
# atr_percentile是ATR/Close比值的滚动分位数（窗口2160根K线）
atr_pct = atr_percentile[t]
if atr_pct >= atr_gate_percentile:  # 默认0.70（70分位数）
    拒绝信号
```

在波动率过高时（超过历史70%的时间）阻止入场。

### 7. 共识模式（Consensus Mode）

```python
position_sizing_mode = 'consensus'
execution_top_n = 5

# 检查前5个均线带是否同时触发入场信号
triggered_signals = []
for band in entry_bands[:5]:
    sig = check_pullback_entry(...)
    if sig: triggered_signals.append(sig)

# 根据同时触发的信号数量调整仓位
if len(triggered_signals) == 1: risk_mult = 0.75
elif len(triggered_signals) == 2: risk_mult = 1.00
else: risk_mult = 1.25
```

多个均线带同时确认时，增加仓位。

### 8. 冷却机制（Cooldown）

```python
if use_cooldown and t < cooldown_until_bar:
    跳过信号检查
# 开仓后设置: cooldown_until_bar = t + 1 + cooldown_bars
```

防止短时间内连续开仓。

## 入场执行

信号在K线收盘后产生，实际执行使用下一根K线的价格：

```python
# 优先使用5分钟均价，无效则使用开盘价
entry_price = next_row['avg_price_5m'] if valid else next_row['open']
```

## 离场逻辑详解

### 止损（check_stop）

```python
# 做多
stop_triggered = low[t] <= stop_price
# 做空
stop_triggered = high[t] >= stop_price
```

止损触发后，在**下一根K线**平仓。使用K线最低/最高价判断止损是否触发，比仅用收盘价更保守。

### 移动止损（get_trailing_stop）

多种止损移动策略：

| 类型 | 逻辑 | 特点 |
|------|------|------|
| `ma_left` | new_stop = 入场带左侧MA当前值 | 跟随短期趋势 |
| `ma_right` | new_stop = 入场带右侧MA当前值 | 跟随长期趋势 |
| `trail_pct` | new_stop = 最高收盘 × (1 - stop_param) | 固定百分比跟踪 |
| `trail_atr` | new_stop = 最高收盘 - stop_param × ATR | 基于波动率的跟踪 |
| `trail_fixed` | 不移动止损 | 保持入场时止损不变 |

**止损移动规则：**
- 做多：止损只能**上移**（new_stop > current_stop）
- 做空：止损只能**下移**（new_stop < current_stop）
- 移动止损不产生交易成本

### 执行均线带级别的移动止损

当 `execution_top_n > 1` 时，额外检查前N个均线带的左侧MA：

```python
for band in long_pool_bands[:execution_top_n]:
    ma_val = ma_row[band.left_period]
    if ma_val > position.stop_price and ma_val > position.left_ma_value:
        new_stop = ma_val  # 使用最优均线带的左侧MA作为新止损
```

### 时间离场

```python
if holding_bars >= 5:
    if position.direction == 'long' and close[t] <= entry_price:
        exit_price = next_row['open']
        exit_reason = 'time_exit'
```

持仓5根K线后，如果价格跌回入场价以下且无利润，则离场。

### 均线带反转离场

```python
# 做多: 入场均线带排列反转（短期MA跌到长期MA下方）
if ma_left <= ma_right:
    band_inversion_hit = True

# 做空: 入场均线带排列反转（短期MA涨到长期MA上方）
if ma_left >= ma_right:
    band_inversion_hit = True
```

## 信号记录

所有信号记录包含以下信息：

| 字段 | 说明 |
|------|------|
| datetime | 信号时间 |
| signal_type | entry / exit |
| direction | long / short |
| band_id | 触发的均线带ID |
| left_ma_period / right_ma_period | 均线周期 |
| left_ma_value / right_ma_value | 均线值 |
| close / high / low | 触发时的价格 |
| trend_filter_pass | 趋势过滤是否通过 |
| entry_allowed | 最终是否入场 |
| reason | 信号原因 |

## 相关页面

- [[ma-band-architecture]] — 完整策略架构
- [[ma-band-selection]] — 均线带选择演进
- [[ma-band-version-history]] — 版本时间线
- [[risk-management]] — 风险管理概念
