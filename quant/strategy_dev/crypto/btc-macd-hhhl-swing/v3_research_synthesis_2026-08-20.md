# BTC MACD HHHL Swing v3 Research Synthesis

Date: 2026-08-20  
Project: `/home/tgm/quant/btc-macd-hhhl-swing`  
Wiki index: `/home/tgm/wiki/quant/strategy_dev/crypto/btc-macd-hhhl-swing/README.md`

## 1. Final Verdict

**暂不采纳 v3。**

我没有只信历史摘要，而是逐项核对了 v3-2 至 v3-12 的真实报告、CSV、审计结果，并把可组合的机制做了独立 Round 0 / Round 1 迭代。Round 1 的三组候选都通过了内部审计，但没有一组同时满足：
- OOS 不劣于基准
- 成本后质量改善
- 回撤 / 并发可执行
- 真实状态机而非台账筛选

Round 1 最好候选 `B_dynamic_staged_split` 只是在 FULL 口径上把 30 币期末权益从 v3-10 C 的 2,158.44 提高到 9,546.07，但 OOS 仍为负，OOS PF 只有 0.1257，OOS simple pnl 为 -3.5505%，OOS MaxDD 约 -92.02%。因此停止 Round 2 / 3，不继续调参。

## 2. Evidence Grading

A = 真实状态机回测 + 独立审计 + DEV/OOS 或等价分段验证。  
B = 真实回测但存在容量伪影、OOS 不稳定或口径只能视作诊断。  
C = smoke、失败、未完成或机制已被证伪。

| Version | Mechanism | Key real numbers | Grade | Decision |
|---|---|---:|---|---|
| v2 baseline | old unMMR split shared pool | 710 symbols, 10,173 trades, PF 2.0259, MaxDD -46.6096, total_return 2.553883e25% | B | Baseline context only |
| v3-2 ampfilter | remove stop-distance filter + amp_cur > amp_prev | 710 symbols, 54,386 trades, PF 1.5443, MaxDD -63.2506, total_return 8.958245e39% | B | Useful base filter, not deployable evidence |
| v3-2 effective swing | ignore non-breakout middle swings | BTC smoke -10.4638%; preliminary full PF 0.6471, trade 41 | C | Not adopted |
| v3-3 swing direction | latest up/down length direction | BTC smoke +42.7432%, PF 1.0423, fee 19,886.74; full trades 99,655, PF 1.2240, MaxDD -77.6295 | C | Excluded: cost/capacity risk |
| v3-4 stoploss | baseline/time/ATR/staged | staged_stop 30币 trades 7,221, PF 1.7047, MaxDD -26.8032, audit PASS; time/ATR FAIL | B | staged_stop useful candidate |
| v3-5 takeprofit | dynamic 1.5R half-tail | 30币 dynamic trades 7,174, PF 1.4178, MaxDD -41.3153; baseline PF 1.3743, MaxDD -58.2890 | B | useful candidate, but capacity artifact |
| v3-6 no partial TP | remove half TP | 30币 baseline_no_partial final +61.2557%, MaxDD -93.7141; variantB PF 1.2908, MaxDD -80.0234 | C | Excluded |
| v3-7 reentry B | same-trend re-entry | 30币 A trades 422 final +61.2557%, MaxDD -93.7141; B trades 6,971, PF 1.3042, MaxDD -79.3905 | B/C | Not selected after Round 1 |
| v3-8 full TP | full target exit | 30币 full dynamic return higher but PF 1.3435 < half dynamic 1.4178; fees 31.10B > 19.10B | C | Excluded |
| v3-9 filters | trend / vol / trend+vol | 30币 trend+vol FULL PF 1.2133, MaxDD -47.1984; OOS no stable edge | B/C | risk-control only, not alpha |
| v3-10 entry C | split entry | 30币 C trades 1,039, final_eq 2,158.44, fee 1,368.30, timing audit pass | B | execution-valid baseline |
| v3-11 capital priority | fixed 1% risk | 30币 B fixed-risk final_eq 2,096.61, PF 0.2043, MaxDD -98.43 | C | Excluded |
| v3-12 capacity priority | eventized priority A/B/C | 30币 A final_eq 2,158.436; B 2,096.597; C 2,096.599; audit PASS; no priority edge | B | useful framework, priority not adopted |

## 3. Useful / Not Useful / Possibly Useful

Useful:

| Mechanism | Reason |
|---|---|
| v3-2 amp filter | Strict pre-entry amplitude condition, used by later engines |
| v3-4 staged_stop | Strong 30币 PF / MaxDD improvement with audit PASS |
| v3-5 dynamic_1.5R half TP | Better than original target in its own audited run |
| v3-10 split entry | Timing/accounting valid and auditable |
| v3-12 eventized candidate collection | Correct architecture for same-bar priority experiments |

Not useful:

| Mechanism | Reason |
|---|---|
| no partial TP | Removes dynamic target effect and worsens drawdown / quality |
| full TP | Raises nominal returns but lowers PF and increases fees |
| ATR/time stops | Verification FAIL or no quality improvement |
| vol single filter | OOS unstable / negative evidence |
| fixed 1% risk | PF ~0.204 and near-account wipeout |
| simple v3-3 direction | Very high trade count and cost; weak PF |

Possibly useful but not accepted:

| Mechanism | Why not accepted now |
|---|---|
| same-trend reentry B | Added real trades, but Round 1 C worsened B and raised costs |
| trend+vol risk filter | Reduced drawdown in v3-9, but not stable enough to prove alpha |
| capacity priority sorting | Framework audited, but B/C did not beat A |

## 4. Round 0 Baseline and Artifact Reconciliation

Round 0 did not run a new optimization. It reconciled existing artifacts and fixed the comparison basis.

| Item | Source path | Evidence |
|---|---|---|
| v2 baseline | `/home/tgm/quant/btc-macd-hhhl-swing/fresh/package_v2_unmmr_split/result_v2_final/metrics.csv` | 710 symbols, 10,173 trades, PF 2.0259, MaxDD -46.6096 |
| v3-10 C baseline for capacity stage | `/home/tgm/quant/btc-macd-hhhl-swing/v3-10_entry_opt/run30_20260819/C/metrics.csv` | 1,039 trades, final_eq 2,158.44, fee 1,368.30, timing audit pass |
| v3-12 eventized priority | `/home/tgm/quant/btc-macd-hhhl-swing/v3-12_capacity_priority/run30_20260819/summary.csv` | A/B/C all PASS, no priority edge |
| Round 1 engine copy | `/home/tgm/quant/btc-macd-hhhl-swing/combo_round1_20260819/backtest_v311.py` | Isolated copy; old v2/v3 dirs not modified |

No Git repository exists at the project root, so version isolation evidence is file-system based: independent directories, logs, file sizes, and audit outputs.

## 5. Round 1 Pre-Registered Combos

Round 1 variants were fixed before reading the results:

| Variant | Mechanism |
|---|---|
| A_dynamic_baseline_split | dynamic_1.5R + baseline stop + split entry + unmmr |
| B_dynamic_staged_split | dynamic_1.5R + staged_stop + split entry + unmmr |
| C_dynamic_staged_reentry_priority | dynamic_1.5R + staged_stop + split entry + same-trend reentry B + strength priority + unmmr |

## 6. Round 1 Real Results

Runner: `/home/tgm/quant/btc-macd-hhhl-swing/combo_round1_20260819/run_combo.py`  
Log: `/home/tgm/quant/btc-macd-hhhl-swing/combo_round1_20260819/round1.log`  
Completion: `ROUND1_COMPLETE`, `EXIT=0`  
Audit: `/home/tgm/quant/btc-macd-hhhl-swing/combo_round1_20260819/independent_audit.json`, `VERDICT PASS`

### BTC smoke

| Variant | Trades | Final eq | Return% | PF | MaxDD% | Fee | Verdict |
|---|---:|---:|---:|---:|---:|---:|---|
| A_dynamic_baseline_split | 313 | 202,358.3815 | 1,923.5838 | 25.4064 | -90.0862 | 1,686.31 | PASS |
| B_dynamic_staged_split | 279 | 30,167.0323 | 201.6703 | 6.3901 | -59.3200 | 2,278.07 | PASS |
| C_dynamic_staged_reentry_priority | 292 | 29,927.5506 | 199.2755 | 6.1837 | -59.3801 | 2,391.13 | PASS |

BTC OOS failed for all three: OOS simple pnl was -13.9053%, -12.7885%, -13.3848 respectively; OOS PF was 0 under this segmentation.

### 30币 FULL

| Variant | Trades | Final eq | Return% | PF | MaxDD% | Fee | Max concurrent | Avg concurrent | Audit |
|---|---:|---:|---:|---:|---:|---:|---:|---:|---|
| A_dynamic_baseline_split | 1,039 | 2,158.4361 | -78.4156 | 0.2109 | -98.2365 | 1,368.30 | 23 | 6.5474 | PASS |
| B_dynamic_staged_split | 3,030 | 9,546.0734 | -4.5393 | 0.9532 | -98.2198 | 4,574.85 | 38 | 10.5471 | PASS |
| C_dynamic_staged_reentry_priority | 3,123 | 9,213.4334 | -7.8657 | 0.9189 | -98.3710 | 4,745.60 | 29 | 10.7552 | PASS |

### 30币 DEV / OOS

| Variant | Segment | Trades | PF | Simple pnl% | Segment MaxDD% | Max concurrent | Decision |
|---|---|---:|---:|---:|---:|---:|---|
| A_dynamic_baseline_split | DEV | 920 | 0.2106 | -78.0535 | -96.1636 | 23 | Reject |
| A_dynamic_baseline_split | OOS | 119 | 0.2844 | -0.3600 | -91.6703 | 10 | Reject |
| B_dynamic_staged_split | DEV | 2,437 | 0.9894 | -0.9859 | -95.7527 | 38 | Not enough quality |
| B_dynamic_staged_split | OOS | 593 | 0.1257 | -3.5505 | -92.0187 | 17 | Reject |
| C_dynamic_staged_reentry_priority | DEV | 2,511 | 0.9530 | -4.3698 | -96.0622 | 29 | Worse than B |
| C_dynamic_staged_reentry_priority | OOS | 612 | 0.1203 | -3.4960 | -92.1567 | 17 | Reject |

## 7. Final Decision

No v3 candidate is accepted.

Rejection basis:

| Gate | Result |
|---|---|
| OOS not worse than baseline | FAIL: Round 1 B/C OOS PF 0.1257 / 0.1203 and OOS pnl negative |
| PF / cost quality improvement | FAIL: FULL PF remains < 1 in Round 1 B/C, fees rise materially |
| MaxDD / concurrency executable | FAIL: 30币 MaxDD about -98% FULL, OOS segment MaxDD about -92%; max concurrent up to 38 |
| Audit evidence | PASS for Round 1 artifacts, but audit PASS only proves internal consistency |
| Execution realism | FAIL / not tested: no slippage, funding, order book, precision, participation, PIT universe |

Therefore: **暂不采纳 v3**. Keep v3-4 staged_stop, v3-5 dynamic target, v3-10 split entry, and v3-12 eventized collection as useful research components, but not as a deployable v3 strategy.

## 8. Limitations

- No slippage model.
- No funding-rate model.
- No order book depth, impact cost, participation limit, minimum notional, or exchange precision matching.
- MMR is a uniform 0.5% approximation, not Binance tiered maintenance margin.
- Fixed 30币 pool is not a point-in-time tradable/liquidity universe.
- Shared-pool compounding creates extreme nominal returns and fees; this is a capacity artifact until independently modeled.
- DEV/OOS split here is from the same continuous state-machine run by trade entry time; it is valid for diagnosis, but not a full walk-forward framework.
- No 710币 Round 3 was run because no Round 1 candidate passed acceptance gates.

## 9. Paths

- `/home/tgm/quant/btc-macd-hhhl-swing/combo_round1_20260819/round1.log`
- `/home/tgm/quant/btc-macd-hhhl-swing/combo_round1_20260819/independent_audit.json`
- `/home/tgm/quant/btc-macd-hhhl-swing/combo_round1_20260819/audit_summary.csv`
- `/home/tgm/quant/btc-macd-hhhl-swing/combo_round1_20260819/segment_summary.csv`
- `/home/tgm/quant/btc-macd-hhhl-swing/v3_final_iterations_20260820/STATE.json`
