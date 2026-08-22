---
title: v2 多周期风险平价 inverse-vol 710币最终回测与独立审计
date: 2026-08-22
updated: 2026-08-22
status: rejected-cash-constraint-fail
confidence: high
priority: research
tags: [crypto, shared-account, risk-parity, inverse-vol, mtm-ledger, audit, no-lookahead]
---

# v2 多周期风险平价 inverse-vol 710币最终回测与独立审计

## 裁决

**否决直接采纳。** 冻结 inverse-vol capped 的710币回测已自然完成，账本结构、资金恒等式、t-1无前视、1W=0和50%周期cap均通过；但独立字段审计发现20行现金为负，最低 `-3727.18 USD`。融资、保证金和强平未建模，funding未接入，不能把该结果视作可执行资金分配结果。

## 运行与结果

- PID `884424` 自然退出；710/710币全部加载。
- 范围：`2019-09-08 00:00:00` 至 `2026-05-16 23:00:00`。
- 58,632个时间戳、234,528行ledger，每时点严格四周期。
- 初始账户MTM `10,000.00`，最终 `36,694.02 USD`，账面收益 `266.94%`，最大回撤 `-18.44%`。
- 710只使用冻结权重：1h/4h/1D/1W = `33.9919%/30.2067%/35.8014%/0%`；未用OOS结果重新调权。

| 周期 | 平均风险预算 | 最终权重 | cap | 接受/拒绝 | 最大腿数 | 最大名义USD | 手续费USD |
|---|---:|---:|---:|---:|---:|---:|---:|
| 1h | 38.54% | 33.99% | 50% | 355 / 173,433 | 13 | 27,524.55 | 1,116.41 |
| 4h | 31.71% | 30.21% | 50% | 354 / 54,947 | 12 | 18,860.50 | 607.66 |
| 1D | 29.75% | 35.80% | 50% | 329 / 8,752 | 8 | 19,188.41 | 236.12 |
| 1W | 0.00% | 0.00% | 0% | 0 / 0 | 0 | 0.00 | 0.00 |

## 独立审计

- schema、逐时四周期：PASS。
- 资金恒等式 `account_mtm = cash + sum(period_mtm)`：PASS，最大误差 `1.55e-11`。
- 权重来源 t-1 无前视：PASS，0违规。
- 1W权重最大值：0；周期cap违规：0。
- entry/setup：在实际 `open_position/open_short` 调用边界记录，接受1,038、拒绝237,132。
- 手续费：已接入；funding：明确 `not_integrated`，合计0；slippage：0。
- cash非负：FAIL，20行，最低 `-3727.18 USD`。
- 强平/保证金：未建模。

## DEV 30币事前比较

| 方案 | 终值USD | 收益 | MaxDD | 接受/拒绝 | 手续费USD |
|---|---:|---:|---:|---:|---:|
| 手工D | 17,862.17 | 78.62% | -7.31% | 358 / 1,427 | 661.10 |
| inverse-vol capped | 21,463.02 | 114.63% | -6.35% | 431 / 1,261 | 864.76 |
| shrinkage-ERC capped | 21,211.62 | 112.12% | -7.36% | 412 / 1,275 | 796.34 |

30币结果只用于冻结方案的事前选择，没有用OOS重新调权。

## 下一道门

加入cash/保证金非负约束、融资、强平规则并补做funding/slippage敏感性；随后重新运行710和独立审计。研究目录证据包括 `final710_inverse_vol_capped_frozen/independent_audit_fields.json`、逐周期ledger、`REPORT_CN.md` 和 `report_710_inverse_vol_capped_frozen.html`。
