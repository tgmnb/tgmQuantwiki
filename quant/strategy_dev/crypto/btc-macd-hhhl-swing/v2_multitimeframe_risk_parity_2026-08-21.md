---
title: v2 多周期风险平价 710 币 inverse-vol 最终审计
date: 2026-08-21
updated: 2026-08-22
status: inverse-vol-audited-not-adopted
confidence: high
priority: research
tags: [crypto, shared-account, risk-parity, inverse-vol, mtm-ledger, no-lookahead, audit]
---

# v2 多周期风险平价：710 币 inverse-vol 最终审计

## 裁决

710 币 `inverse_vol_capped` runner 已正常退出并完成真实产物落盘；独立台账审计通过。但当前**否决采纳该配比**：funding 尚未接入、slippage=0 尚未做敏感性测试，而且同口径 710 币 manual-D 对照仍由 PID `3971940` 在另一目录运行，尚无完整可比产物。结论是“产物可验收、配置不可采纳”，不是 runner 失败。

## 710 inverse-vol 审计证据

- 日志：`/home/tgm/quant/btc-macd-hhhl-swing/v2_multitimeframe_risk_parity_20260821/final710_inverse_vol_capped_frozen.log`
- 账本：`final710_inverse_vol_capped_frozen/dev_710/inverse_vol_capped/period_ledger.csv`，234,528 行、40,056,606 bytes。
- 时间：`2019-09-08 00:00:00` 至 `2026-05-16 23:00:00`，58,632 小时，每小时严格 4 周期。
- 独立验证器 exit code 0；schema、columns_exact、资金恒等式、1W=0、无前视均 PASS。
- 最大资金恒等式误差 `1.546140993013978e-11`；权重源前视违规 0；funding=`not_integrated`。
- 接受候选 1,038，拒绝候选 237,132；累计手续费约 1,960.19 USD；最大腿数 27，最大名义 41,427.19 USD。

## 真实 DEV/OOS/FULL 指标

`2024-01-01 00:00:00` 为切段边界；三段来自同一次连续状态机账本。

| 区间 | 期末 MTM | 区间收益 | MaxDD | 接受/拒绝 | 手续费口径 |
|---|---:|---:|---:|---:|---:|
| DEV 2019-09-08 至 2023-12-31 | 36,824.36 | 268.24% | -18.44% | 1,037 / 75,817 | 1,960.02 |
| OOS 2024-01-01 至 2026-05-16 | 36,694.02 | -0.35% | -0.41% | 1 / 161,315 | 连续账本累计值 |
| FULL 2019-09-08 至 2026-05-16 | 36,694.02 | 266.94% | -18.44% | 1,038 / 237,132 | 1,960.19 |

OOS 只有 1 个 accepted candidate，不能据此宣称策略稳定性已被验证。DEV 的复利收益也不能单独作为采纳证据。

## manual-D 可比性边界

当前可读的 `manualD30_oos_reference` 是 30 币参考，不是 710 币对照。其 DEV 期末 17,862.17、收益 78.62%、MaxDD -7.31%；OOS 期末 23,183.12、分段收益约 29.01%、MaxDD -11.27%。这些数值只作背景，不能与 710 inverse-vol 直接排名。

710 manual-D full 任务 PID `3971940` 在本次收尾期间持续运行；本次未杀死、未重启、未覆盖其日志或产物。由于尚无完整 710 manual-D ledger/summary/validation，710 manual-D 的 DEV/OOS/FULL 指标均应记为“未完成/不可用”。

## 成本限制与下一道门

手续费已计入并通过恒等式审计；funding 未接入；slippage 为 0 且未做情景扫描。没有盘口深度、参与率或逐笔成交数据，不能把最大名义当成容量可行性证明。

下一步：等待 710 manual-D 自然结束并独立审计；在同一切段、同一成本口径下完成 manual-D/inverse-vol 对照；补 funding 与 slippage 情景。上述门槛通过前，保持 `NO_ALLOCATION_ADOPTED`。

## 本地交付

完整中文报告：`/home/tgm/quant/btc-macd-hhhl-swing/v2_multitimeframe_risk_parity_20260821/REPORT_CN.md`；自包含 HTML：`report.html`；独立复核：`final710_inverse_vol_capped_frozen/independent_audit_rerun.json`；真实账本和周期汇总位于同一 `final710_inverse_vol_capped_frozen` 目录。
