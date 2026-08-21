---
title: BTC MACD HHHL Swing 统一公平比较最终裁决
created: 2026-08-21
updated: 2026-08-21
type: final-decision
status: complete
confidence: high
---

# BTC MACD HHHL Swing 统一公平比较最终裁决

> **最终结论：暂不采纳 v3。**
>
> 2026-08-21 统一公平比较的两批候选均已完成真实运行并通过独立产物核验：第一批 `6/6 PASS`，第二批 `7/7 PASS`。核验 PASS 只证明产物、时序、参数和内部会计契约一致，不等于通过实盘或模拟盘采纳门槛。

## 1. 研究口径

统一目录：`/home/tgm/quant/btc-macd-hhhl-swing/unified_fair_selection_rerun_20260821`

- DEV：`entry_time <= 2023-12-31 23:59:59`
- OOS：`entry_time >= 2024-01-01 00:00:00`
- 选参：仅在各预注册 family 内使用 DEV；OOS 只作冻结验收
- 币池：30 symbols，BTC 强制纳入，排除 stablecoin variants
- 初始权益：`10000`
- 手续费：每侧 `0.0005`
- 资金模型：1h continuous shared-pool state machine，unMMR sizing
- 第二批 staged：预注册文件确认没有兼容的 staged 参数，因此排除，不做静默替代

预注册与选择文件：

- `/home/tgm/quant/btc-macd-hhhl-swing/unified_fair_selection_rerun_20260821/PRE_REGISTRATION.md`
- `/home/tgm/quant/btc-macd-hhhl-swing/unified_fair_selection_rerun_20260821/SECOND_BATCH_PRE_REGISTRATION.md`
- `/home/tgm/quant/btc-macd-hhhl-swing/unified_fair_selection_rerun_20260821/DEV_SELECTION.json`

## 2. 独立核验状态

| 批次 | 候选数 | 结果 | 核验文件 |
|---|---:|---|---|
| First batch | 6 | 6/6 PASS | `/home/tgm/quant/btc-macd-hhhl-swing/unified_fair_selection_rerun_20260821/batches/first_batch_20260821_001823/verification_summary.json` |
| Second batch | 7 | 7/7 PASS | `/home/tgm/quant/btc-macd-hhhl-swing/unified_fair_selection_rerun_20260821/batches/second_batch_20260821_003930/verification_summary.json` |

## 3. First Batch DEV 锁定候选 OOS

以下是 `/home/tgm/quant/btc-macd-hhhl-swing/unified_fair_selection_rerun_20260821/DEV_SELECTION.json` 锁定的 family winner；OOS 未参与选型。

| Candidate | DEV PF | OOS trades | OOS PF | OOS MaxDD MTM | OOS end MTM equity |
|---|---:|---:|---:|---:|---:|
| `v2_baseline` | 1.4020740637165128 | 4,331 | 1.3589991516299005 | -42.662342408141804% | 229024368186062280 |
| `v3_2_amp_abs` | 1.3724522957547363 | 2,500 | 1.4753514580422864 | -41.0023445767203% | 16114314306629.758 |
| `v3_5_dynamic_1p25R` | 1.3213830936744506 | 2,735 | 1.4109479012932178 | -39.83320563448083% | 17277656898.084568 |

First-batch结构化 verification summary 未提供 OOS `win_rate_pct`，该指标在此处明确记为缺失，不补算。对应 selected artifact roots 位于：

`/home/tgm/quant/btc-macd-hhhl-swing/unified_fair_selection_rerun_20260821/batches/first_batch_20260821_001823/candidates/`

## 4. Second Batch 冻结 OOS

### v3-7 reentry

| Candidate | DEV PF | OOS trades | OOS PF | OOS net PnL | OOS MaxDD MTM | OOS end MTM equity |
|---|---:|---:|---:|---:|---:|---:|
| `v3_7_reentry_cooldown_1` | 1.1425995446640484 | 2,640 | 1.3096098972211545 | 1728423268.28 | -65.26513677857582% | 1755603635.6705818 |
| `v3_7_reentry_cooldown_3` | 1.126998247782165 | 2,624 | 1.3009455820459093 | 940282639.58 | -65.70680470677304% | 959219066.2202767 |
| `v3_7_reentry_cooldown_6` | 1.1479639643587338 | 2,548 | 1.3137549072970425 | 1578265089.49 | -65.47105997539676% | 1608806516.017169 |

### v3-10 split entry

| Candidate | DEV PF | OOS trades | OOS PF | OOS net PnL | OOS MaxDD MTM | OOS end MTM equity |
|---|---:|---:|---:|---:|---:|---:|
| `v3_10_split_frac_0p5_depth_0p5` | 0.21055373050999687 | 119 | 0.2844364937388193 | -36.0 | -91.67034630495725% | 2160.0848581146097 |
| `v3_10_split_frac_0p5_depth_0p75` | 0.2505872741830301 | 159 | 1.0874417596706036 | 8.069999999999993 | -91.85962784449131% | 2643.873289915554 |
| `v3_10_split_frac_0p7_depth_0p5` | 0.14636925184613885 | 1 | 0.0 | -0.2 | -87.95906345202597% | 1489.4475735257365 |
| `v3_10_split_frac_0p7_depth_0p75` | 0.17826950440190859 | 69 | 0.0 | -21.44 | -88.61443415224805% | 1808.0722116600311 |

来源：

- `/home/tgm/quant/btc-macd-hhhl-swing/unified_fair_selection_rerun_20260821/batches/second_batch_20260821_003930/metrics_summary.json`
- `/home/tgm/quant/btc-macd-hhhl-swing/unified_fair_selection_rerun_20260821/batches/second_batch_20260821_003930/*/metrics.json`

## 5. 裁决理由

1. v3-7 的 OOS PF 约 `1.30`，但 OOS MTM MaxDD 约 `-65%`，尚未考虑真实滑点、funding、盘口冲击、参与率与交易所约束。
2. v3-10 四个 split-entry 候选的 DEV PF 全部低于 `1`；其中 OOS PF 高于 `1` 的 `v3_10_split_frac_0p5_depth_0p75` 只有 159 笔 OOS 交易，且 OOS MaxDD 为 `-91.85962784449131%`，不构成稳健边缘。
3. 第一批锁定 v3 候选虽有正 OOS PF，但 OOS MaxDD 仍为约 `-39.83%` 至 `-41.00%`；这不足以通过执行现实性门槛。
4. 当前证据没有统一滑点、funding、order-book depth、impact cost、participation limit、exchange precision 或 PIT liquidity-universe 重建。
5. 共享资金池 unMMR 复利产生的巨大权益和手续费数字是模拟容量/会计口径的放大结果，**不得当作实盘收益**。

## 6. 保留结论

- `v2_baseline`：仅保留为参考基线，不批准部署。
- `v3_2_amp_abs`：保留为研究组件，不批准部署。
- `v3_5_dynamic_1p25R`：保留为研究组件，不批准部署。
- `v3_7_reentry_*`：保留 family 对照结果，不采纳任何 cooldown 值。
- `v3_10_split_*`：保留执行/时序审计结果，不采纳任何 split 配置。
- staged execution：因第二批契约不兼容，排除，不形成候选。

## 7. 结论

**两批均 PASS，但没有唯一候选通过最终采纳门槛；统一公平比较最终裁决为：暂不采纳 v3。**

这不是“没有跑完”，而是已完成两批真实产物与独立核验后的否决。后续若重新研究，必须先补齐执行成本、容量和更严格的时序验证，而不是把本页的复利天文数字解释成实盘收益。

## 8. 归档产物

- Final decision：`/home/tgm/quant/btc-macd-hhhl-swing/unified_fair_selection_rerun_20260821/FINAL_DECISION.md`
- State：`/home/tgm/quant/btc-macd-hhhl-swing/unified_fair_selection_rerun_20260821/STATE`
- First batch：`/home/tgm/quant/btc-macd-hhhl-swing/unified_fair_selection_rerun_20260821/batches/first_batch_20260821_001823`
- Second batch：`/home/tgm/quant/btc-macd-hhhl-swing/unified_fair_selection_rerun_20260821/batches/second_batch_20260821_003930`
