---
title: MACD 仿人工 V2 — unMMR 仓位 + 多空分离 + 止损过滤（全池 710 币）
created: 2026-08-13
updated: 2026-08-13
type: backtest-result
tags: [quant, crypto, macd, swing, unMMR, portfolio-margin, long-short-split, stop-filter, shared-pool]
confidence: high
priority: core
source: ~/quant/btc-macd-hhhl-swing/shared_pool_all/（shared_pool_all_backtest.py + run_split_tf.py + scan_split_combos.py）
数据截止: 2026-05-16
---

# MACD 仿人工 V2 — unMMR 仓位 + 多空分离 + 止损过滤

## 基本信息

| 字段 | 内容 |
|------|------|
| 版本 | V2（相对 V1 的演进） |
| 日期 | 2026-08-13 |
| 研究类型 | 仓位模型重构（unMMR 全仓保证金语义）+ 多空周期/参数分离 + 止损过滤 |
| 市场 | Binance swap 永续，全池 710 币（714 个 *-USDT.csv 剔除 4 稳定币变体） |
| 数据 | 1h K线 2019-09 → 2026-05（最长 BTC 2442 天，最短 1.38 天） |
| 费用 | 双边 0.05% taker，初始本金 10,000 USDT，复利 |
| 引擎 | `shared_pool_all_backtest.py`（V2 主引擎，含 `--sizing unmmr`）+ `run_split_tf.py`（多空分离）+ `scan_single_side.py`/`scan_split_combos.py`（选参） |

---

## 一、V2 相对 V1 的三大改动

### 1. 仓位模型：shrink（名义≤现金）→ unMMR（全仓保证金语义）

- **shrink（V1）**：`qty×entry ≤ cash` 硬缩仓，名义杠杆 ≈1x，负现金 bug（`qty=cash/fill` 后 `cash-=cost×(1+FEE)` 必然转负）
- **unMMR（V2）**：`qty = equity×risk_budget(n_open_legs) / (|entry−stop| + entry×0.1%)`，允许负现金（全仓保证金语义），强平看权益
- 参数：`MAX_POSITIONS=50`、`MAX_LEVERAGE=20`、`MMR=0.5%`（维持保证金率，强平线 `权益 ≤ 总名义市值×0.5%`，即 unMMR≥100%）
- risk_budget 分段：前 10 腿 1% → 每 10 腿一档 0.8%/0.6%/0.4%/0.2% → ≥50 腿不开

### 2. 多空分离：同周期 dual → 多空独立参数/周期

- dual（V1/m2/m3）：多空腿跑同一段序列，空腿 MACD 是死参数（复用 p_long）
- split（V2）：同一账户挂两个独立 SymbolState，多头/空头各用自己的周期、参数、MACD
- 选参结论（全池 710 币验证）：**最优是多空都是 1h**，不是"4h 多 + 1h 空"；多头用 g03_rr5（低胜率高盈亏比），空头用 g08_macd_fast 或 g09_macd_slow

### 3. 止损过滤：`--max-stop-dist`

- 研究结论：止损越紧胜率越高（<0.3% 止损 38.9% 胜率 vs >2% 29.3%，单调递减）
- 实现：`|entry−stop|/entry × 100 > X%` 的开仓直接跳过（不剔除币）
- 最优阈值：**sd>0.5%**（sd>1% 太松无效，sd>0.5% 同时改善 MaxDD 和 PF）

---

## 二、最终选型（V2 推荐配置）

| 维度 | 配置 |
|------|------|
| 多头 | 1h `g03_rr5`（MACD 12,26,9，RR=5，止损缓冲 0.1%，半仓 50%，变体 A） |
| 空头 | 1h `g08_macd_fast`（MACD 8,21,5，RR=3，止损缓冲 0.1%，半仓 50%，变体 A） |
| 仓位 | unMMR：risk_budget 分段 1%→0.2%，MAX_POSITIONS=50，MAX_LEVERAGE=20，MMR=0.5% |
| 止损过滤 | `--max-stop-dist 0.5`（止损距离 >0.5% 不开仓） |
| 运行 | `python3 scan_split_combos.py --n-symbols 710 --sizing unmmr --max-stop-dist 0.5 --focus` |

### 全池 710 币结果（unMMR，2026-08-13）

| 组合 | 收益 | MaxDD | PF | 交易 |
|------|------|-------|-----|------|
| **多g03/空g08 + sd0.5** | 2.6e25% | **-46.61%** | **2.03** | 10,173 |
| 多g03/空g09 + sd0.5 | 4.8e27% | -55.54% | 1.93 | 11,833 |
| 多g03/空g08 无过滤 | 4.9e32% | -64.97% | 1.55 | 61,738 |
| 多g03/空g09 无过滤 | 3.8e34% | -67.75% | 1.53 | 65,667 |
| V1 m3 同周期 p2（对比） | 3.2e45% | -79.34% | 1.48 | 76,130 |

**注**：收益是天文数字是 20x 杠杆 + 复利 2442 天的数学必然（TGM 已定性，不是 bug，不加名义上限）。

---

## 三、关键结论

1. **止损过滤是普适有效的**：同周期和分离版都验证 sd0.5 改善 MaxDD 和 PF（V1 m3：-79.3%→-61.7%；V2 最优组合：-65%→-46.6%）
2. **多空分离的核心收益 = 各自独立参数**，不是周期错开：全池 16 组合 top 4 全是 1h多+1h空；"4h多+1h空"全池只有 -69.95% MaxDD、PF 1.36，远差于 1h+1h 的 -46.6%/2.03
3. **30 币小池初筛的收益/MaxDD 完全不可信**（排名与全池反转，幸存者偏差），只能看方向（空头 1h 高频 > 低频、g08/g09 领先）
4. **多头 g03（RR=5）+ 空头 g08（MACD fast）** 是搜索空间最优：多头低胜率高盈亏比扛回撤，空头高频贡献收益
5. **0.5% MMR 强平线在 20x 上限内永不触发**（对应 200x 杠杆），是数学事实；unMMR 的实际约束是 20x 杠杆上限 + 50 头寸上限

---

## 四、已知问题/待办

- 空头 1h 信号过多：无过滤时 max_positions 跳过 30066 次（50 上限卡死），sd0.5 过滤后缓解
- 收益仍是天文数字（杠杆放大），实盘需按真实账户杠杆/资金重新解释
- 未做样本外验证（数据到 2026-05，无后段分割）
- 多空分离引擎 `run_split_tf.py` 的 `_MACD_CACHE` 跨周期必须清空（已处理，坑见 run_alltfs）

---

## 五、产物路径

- 引擎：`~/quant/btc-macd-hhhl-swing/shared_pool_all/shared_pool_all_backtest.py`
- 多空分离：`~/quant/btc-macd-hhhl-swing/shared_pool_all/run_split_tf.py`
- 选参脚本：`scan_single_side.py`（单侧遍历）、`scan_split_matrix.py`（周期矩阵）、`scan_split_combos.py`（组合扫描）
- 结果：`~/quant/btc-macd-hhhl-swing/shared_pool_all/split_tf/`（summary CSV + combos 目录 + 图）
- 全池汇总：`shared_pool_all/pool_summary_all*.csv`（shrink/unmmr 多档）
