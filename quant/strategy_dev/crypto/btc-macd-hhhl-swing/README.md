# BTC MACD HHHL Swing Strategy

- Updated: 2026-08-21
- Project: `/home/tgm/quant/btc-macd-hhhl-swing`
- Unified fair-selection final decision: `/home/tgm/wiki/quant/strategy_dev/crypto/btc-macd-hhhl-swing/fair_selection_final_2026-08-21.md`
- Unified fair-selection artifacts: `/home/tgm/quant/btc-macd-hhhl-swing/unified_fair_selection_rerun_20260821/FINAL_DECISION.md`
- Latest research archive: `/home/tgm/wiki/quant/strategy_dev/crypto/btc-macd-hhhl-swing/v3_research_synthesis_2026-08-20.md`
- Latest combo run: `/home/tgm/quant/btc-macd-hhhl-swing/combo_round1_20260819`
- Final iteration state: `/home/tgm/quant/btc-macd-hhhl-swing/v3_final_iterations_20260820/STATE.json`

## Current Verdict

**暂不采纳 v3。**

理由不是“没跑”，而是已经做了真实核对和一轮组合迭代：
- v3-2 至 v3-12 的真实报告、CSV、审计结果已逐项核对。
- Round 0 已完成兼容性/基准重整。
- Round 1 已真实运行并审计通过，但三候选都未通过最终采纳门槛。
- 当前没有候选同时满足 OOS 不劣于基准、成本后质量改善、回撤/并发可执行、审计 PASS 之外的实盘门槛。

## Canonical Documents

| Date | Document | Status |
|---|---|---|
| 2026-08-20 | `v3_research_synthesis_2026-08-20.md` | v3-2 至 v3-12 总整理 + Round0/Round1 裁决 |
| 2026-08-20 | `parameter_selection_fairness_audit_2026-08-20.md` | 参数选型公平性审计与统一遍历 |
| 2026-08-19 | `v3_research_synthesis_2026-08-19.md` | 历史归档 |

## Artifact Roots

| Type | Absolute path |
|---|---|
| Project root | `/home/tgm/quant/btc-macd-hhhl-swing` |
| v2 baseline artifact | `/home/tgm/quant/btc-macd-hhhl-swing/fresh/package_v2_unmmr_split/result_v2_final/metrics.csv` |
| Round 1 combo artifacts | `/home/tgm/quant/btc-macd-hhhl-swing/combo_round1_20260819` |
| Round 1 audit | `/home/tgm/quant/btc-macd-hhhl-swing/combo_round1_20260819/independent_audit.json` |
| Final iteration state | `/home/tgm/quant/btc-macd-hhhl-swing/v3_final_iterations_20260820/STATE.json` |

## Constraints

No result here includes slippage, funding, order book depth, participation limits, Binance tiered MMR, or PIT liquidity universe reconstruction. Large compounded returns and fees are capacity artifacts unless proven otherwise by a separate execution-cost study.
