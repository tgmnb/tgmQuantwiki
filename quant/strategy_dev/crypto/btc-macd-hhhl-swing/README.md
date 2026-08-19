# BTC MACD HHHL Swing Strategy

- Updated: 2026-08-19
- Project: `/home/tgm/quant/btc-macd-hhhl-swing`
- Latest research archive: `/home/tgm/wiki/quant/strategy_dev/crypto/btc-macd-hhhl-swing/v3_research_synthesis_2026-08-19.md`
- Latest combo run: `/home/tgm/quant/btc-macd-hhhl-swing/combo_round1_20260819`

## Current Verdict

**暂不采纳 v3。** v3-2 至 v3-12 的有效机制已整理并做一次真实组合回测。Round 1 的唯一看似改善组合 `dynamic_1p5R + staged_stop + split` 在 30 币 FULL 接近保本，但 OOS PF 仅 0.1257、OOS 简单 PnL -3.55%、OOS MaxDD -92.02%，未达到可采纳门槛。

## Canonical Documents

| Date | Document | Status |
|---|---|---|
| 2026-08-19 | `v3_research_synthesis_2026-08-19.md` | v3-2 至 v3-12 总整理 + Round0/Round1 裁决 |

## Artifact Roots

| Type | Absolute path |
|---|---|
| Project root | `/home/tgm/quant/btc-macd-hhhl-swing` |
| v2 baseline artifact | `/home/tgm/quant/btc-macd-hhhl-swing/fresh/package_v2_unmmr_split/result_v2_final/metrics.csv` |
| Round 1 combo artifacts | `/home/tgm/quant/btc-macd-hhhl-swing/combo_round1_20260819` |
| Round 1 audit | `/home/tgm/quant/btc-macd-hhhl-swing/combo_round1_20260819/independent_audit.json` |

## Constraints

No result here includes slippage, funding, order book depth, participation limits, Binance tiered MMR, or PIT liquidity universe reconstruction. Large compounded returns and fees are capacity artifacts unless proven otherwise by a separate execution-cost study.
