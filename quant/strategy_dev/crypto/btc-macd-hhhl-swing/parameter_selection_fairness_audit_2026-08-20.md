---
title: BTC MACD HHHL Swing 参数选型公平性审计与统一遍历
created: 2026-08-20
updated: 2026-08-20
type: audit
status: archived
tags: [quant, crypto, macd, audit, fairness, dev-oos]
confidence: high
---

# BTC MACD HHHL Swing 参数选型公平性审计与统一遍历

## 1. 审计范围

本页只回答三件事：

1. v2 与 v3-2 至 v3-12 各分支是否做过真实参数遍历。
2. 这些遍历是否满足同一币池、同一时间切分、同一成本口径、同一 OOS 冻结线的公平比较。
3. 如果只保留“有机制价值且选型公平”的分支，最终哪个更适合实盘。

统一比较标准如下：

- 币池必须一致。
- 时间必须一致。
- 成本必须一致：1h、taker 费 0.05%、同一资金模型、同一强平/精度处理。
- 选参必须只看 DEV，不允许用 OOS 反选参。
- DEV 冻结线：`entry_time <= 2023-12-31`。
- OOS 验收线：`entry_time >= 2024-01-01`。
- 不允许 per-coin 调参。

## 2. 参数选型审计矩阵

| 版本 | 真实遍历 | 遍历参数 | 搜索范围 / 样本 | 选型指标 | DEV/OOS | 公平性判断 |
|---|---|---|---|---|---|---|
| v2 | 是 | 单侧周期×参数组；多空组合；止损过滤 `sd=0.5` | 30 币初筛 80 组合 + 30 币组合 16 组合 + 710 币最终验证 | PF / MaxDD / 交易数 | 无正式 DEV/OOS 冻结 | **部分公平**：有预注册扫描与全池复核，但不是严格 OOS 选参 |
| v3-2 ampfilter | 是 | `amp_delta` 阈值、`cap25` 容量开关；基准 `g08/g09` 固定 | DEV：2019-09-14 ~ 2023-12-31 75% 分位；OOS：2024-01-01 后全区间 | PF / MaxDD / 成本 / 并发 | **有** | **相对公平**：这是本批里唯一明确把阈值锁在 DEV、再用 OOS 验收的分支 |
| v3-2 effective swing | 否（机制验证） | 有效摆动定义 A/B 解释 | BTC 真实样例 + smoke/full runner | 机制一致性 / 交易数 / PF | 无 | **不属于选参分支** |
| v3-3 swing direction | 否（规则替换） | 摆动方向判定规则 | BTC smoke + 710 全池跑 | PF / MaxDD / 费用 | 无 | **不公平作选参依据** |
| v3-4 stoploss opt | 是 | baseline / time_stop / atr_stop / staged_stop | 30 币固定池，4 场景 | PF / MaxDD / 胜率 / 费用 | 无 | **不严格公平**：是同池同跑比较，但没有 DEV/OOS 冻结 |
| v3-5 takeprofit opt | 是 | baseline_half / variantB_full / dynamic_1.5R_half | BTC smoke + 30 币固定池 | PF / MaxDD / 收益 / 费用 | 无 | **不严格公平**：只做同场景比较，不是 OOS 选参 |
| v3-6 no partial TP | 是 | 是否取消半仓止盈 | BTC smoke + 30 币固定池 | PF / MaxDD / 胜率 | 无 | **证伪分支**，不应继续扩大搜索 |
| v3-7 reentry | 是 | A/B/C 同趋势重入规则 | BTC smoke + 30 币固定池 | PF / MaxDD / 交易数 | 无 | **不严格公平**：是机制对照，不是统一 OOS 选参 |
| v3-8 full TP | 是 | half/full target + dynamic 1.5R | BTC smoke + 30 币固定池 | PF / MaxDD / 费用 | 无 | **不严格公平**：仍是同池变体比较 |
| v3-9 signal filters | 是 | trend / vol / trend+vol | BTC smoke + 30 币，DEV/OOS/FULL 直接切分 | PF / return / MaxDD / capital util | **有** | **较公平**：有 DEV/OOS，但它是诊断型过滤器研究，不是最终实盘候选 |
| v3-10 entry opt | 是 | A/B/C entry mode | BTC smoke + 30 币固定池 | 交易数 / PF / 费用 / 进入时序 | 无 | **不严格公平** |
| v3-11 capital priority | 否（C 未运行） | A/B sizing；C 仅设计 | BTC smoke + 30 币固定池 | PF / MaxDD / 并发 / 占用 | 无 | **不完整**：C 方案未真实运行，无法公平比较 |
| v3-12 capacity priority | 是 | A 到达顺序 / B 趋势早期 / C 强度优先 | BTC smoke + 30 币固定池 | 收益 / 费用 / 并发 / 候选数 | 无 | **框架正确，但非 OOS 选参** |

## 3. 统一审计结论

### 3.1 哪些分支满足“可比较”

- `v3-2 ampfilter`：唯一明确使用 DEV 锁阈值、OOS 验收的分支。这个分支在方法上最接近公平选型。
- `v3-9 signal_filters`：也有 DEV/OOS 切分，但它更像过滤器诊断，不是最终实盘策略选型。

### 3.2 哪些分支不满足公平选型

- `v3-4`、`v3-5`、`v3-6`、`v3-7`、`v3-8`、`v3-10`、`v3-12`：都在同一场景里比较机制，但没有统一 DEV/OOS 冻结来约束选参。
- `v3-11`：C 容量优先级未运行，无法和 A/B 做公平比较。
- `v3-3`、`v3-2 effective swing`：更偏机制替换或证伪，不该当作选参成绩单。

### 3.3 v2 的位置

v2 是最早的基准分支，真实做过单侧遍历、组合扫描和 710 币最终验证，且最终参数固定为：

- long: `g03_rr5`
- short: `g08_macd_fast`
- stop-distance: `sd=0.5`
- sizing: `unmmr`

但 v2 没有严格的 DEV/OOS 冻结式选参，因此只能当“基准上下文”，不能当作严格公平的 OOS 赢家。

## 4. 实盘适用性判断

### 4.1 成本与容量门槛

实盘比较必须同时满足：

- 同币池。
- 同时间切分。
- 同手续费 / 滑点 / funding 假设。
- 同并发与资金占用约束。
- 同一 OOS 门槛下的唯一参数锁定。

现有材料里，以下缺口仍然存在：

- 没有统一滑点模型。
- 没有 funding rate。
- 没有盘口深度 / 冲击成本 / 参与率。
- 30 币和 710 币结果都明显受到复利与名义杠杆放大。

### 4.2 最终裁决

**结论：当前没有任何 v3 分支可以直接判定为可实盘。**

如果只按“谁最接近可比基准”排序：

1. `v3-2 ampfilter` 最接近严格公平选型。
2. `v3-9 signal_filters` 次之，但属于诊断型过滤器研究。
3. v3-4 / v3-5 / v3-7 / v3-8 / v3-10 / v3-12 都是机制比较，不是统一 OOS 选参。

如果只按“当前可作为保守基线”排序：

- **v2 baseline 更适合作为参考基线，不适合作为实盘直接部署。**
- 它比后续很多 v3 变体更稳定，但仍缺少严格 OOS 成本验证，因此不能硬判为可实盘。

## 5. 保留与剔除

### 保留为有价值机制

- v3-2 amp filter
- v3-4 staged stop
- v3-5 dynamic 1.5R half target
- v3-9 trend / vol filters
- v3-10 split entry
- v3-12 eventized capacity priority

### 明确剔除

- v3-6 no partial TP
- v3-8 full TP 作为统一优选结论
- v3-11 fixed 1% risk
- 任何把 30 币局部结果直接当作 710 币实盘结论的做法

## 6. 依据文件

- `/home/tgm/quant/btc-macd-hhhl-swing/fresh/package_v2_unmmr_split/result_v2_final/metrics.csv`
- `/home/tgm/quant/btc-macd-hhhl-swing/v3-2_optimization_20260817/报告_v3-2_交易特征优化_四状态机_20260817.md`
- `/home/tgm/quant/btc-macd-hhhl-swing/v3-4_stoploss_opt/REPORT.md`
- `/home/tgm/quant/btc-macd-hhhl-swing/v3-5_takeprofit_opt/REPORT.md`
- `/home/tgm/quant/btc-macd-hhhl-swing/v3-6_no_partial_tp/REPORT.md`
- `/home/tgm/quant/btc-macd-hhhl-swing/v3-7_trend_follow_reentry/SPEC.md`
- `/home/tgm/quant/btc-macd-hhhl-swing/v3-8_full_tp/REPORT.md`
- `/home/tgm/quant/btc-macd-hhhl-swing/v3-9_signal_filters/REPORT.md`
- `/home/tgm/quant/btc-macd-hhhl-swing/v3-10_entry_opt/REPORT.md`
- `/home/tgm/quant/btc-macd-hhhl-swing/v3-11_capital_priority/REPORT.md`
- `/home/tgm/quant/btc-macd-hhhl-swing/v3-12_capacity_priority/REPORT.md`
- `/home/tgm/wiki/quant/strategy_dev/crypto/btc-macd-hhhl-swing/v3_research_synthesis_2026-08-20.md`

## 7. Bottom line

**没有哪个 v3 分支已经通过严格、公平、同口径的实盘门槛。**

v2 是当前最稳的基准参考，但也只能算“参考基线”，不是实盘批准版。
