# DVC Round 4：小时线触发顺序可执行口径回测

数据日期：2026-07-06  
运行日期：2026-07-06  
回测区间：2014-01-01 至 2026-07-03  
原始结果目录：`/home/tgm/quant/research/dvc-iteration-round4-execution-20260706`

## 结论

Round 4 的目的，是把 Round 3 的“日终全知排序”改成实盘可执行口径。结论：D/E 两种小时线触发顺序口径都保留了策略收益，不是研究版自嗨。

主推执行版：`AB0_NAV12_HOLD2_NONE_D_TRIGGER_ORDER_TOP4`。它按小时线触发顺序每天买前 4 个，每只 12% NAV，持有 2 个交易日，不加指数退出风控。

## 执行模式定义

| 模式 | 含义 | 是否实盘可执行 |
|---|---|---|
| C | Round 3 研究版：日终知道当天全部触发股后，按 MA60 乖离排序取前4 | 否 |
| D | 小时线触发顺序：当天谁先突破谁先进队列，只买前4个 | 是 |
| E | 小时线触发顺序：一直买到现金不够 | 是 |

## 数据质量

| 项目 | 数值 |
|---|---:|
| 小时线文件数 | 5767 |
| N-shape候选数 | 16130 |
| 成功匹配小时线 | 15888 (98.5%) |
| 缺小时线文件 | 8 (0.05%) |
| 有小时线但无盘中触发 | 234 (1.45%) |

## D/E 平均表现

| 模式 | 平均年化 | 平均MDD | 平均恢复天数 | 平均交易数 | 平均资金利用率 |
|---|---:|---:|---:|---:|---:|
| D_TRIGGER_ORDER_TOP4 | 174.66% | 11.54% | 90 | 4554 | 62.46% |
| E_TRIGGER_ORDER_UNTIL_FULL | 188.38% | 12.14% | 101 | 4797 | 65.31% |

## 最优 D/E 策略

| 策略 | 年化 | MDD | 恢复天数 | 交易数 | Sharpe | 胜率 | 资金利用率 | 最差完整年 | 盈利年份数 | score |
|---|---:|---:|---:|---:|---:|---:|---:|---:|---:|---:|
| `AB0_NAV12_HOLD2_NONE_D_TRIGGER_ORDER_TOP4` | 220.99% | 11.65% | 50 | 5904 | 4.710 | 61.08% | 79.24% | 18.43% | 12 | 18.97 |
| `AB0_NAV12_HOLD2_NONE_E_TRIGGER_ORDER_UNTIL_FULL` | 237.58% | 10.55% | 175 | 6126 | 4.654 | 60.89% | 81.39% | 16.65% | 12 | 22.52 |

## D组 Top 5

| 策略 | 年化 | MDD | 恢复天数 | 交易数 | Sharpe | 胜率 | 资金利用率 | 最差完整年 | 盈利年份数 | score |
|---|---:|---:|---:|---:|---:|---:|---:|---:|---:|---:|
| `AB0_NAV12_HOLD2_NONE_D_TRIGGER_ORDER_TOP4` | 220.99% | 11.65% | 50 | 5904 | 4.710 | 61.08% | 79.24% | 18.43% | 12 | 18.97 |
| `AB0_NAV15_HOLD2_NONE_D_TRIGGER_ORDER_TOP4` | 229.64% | 12.25% | 180 | 4846 | 4.510 | 61.04% | 79.17% | 24.15% | 12 | 18.75 |
| `AB0_NAV18_HOLD2_NONE_D_TRIGGER_ORDER_TOP4` | 248.83% | 13.74% | 187 | 4223 | 4.368 | 60.88% | 81.00% | 28.68% | 12 | 18.11 |
| `AB0_NAV10_HOLD2_NONE_D_TRIGGER_ORDER_TOP4` | 168.53% | 9.77% | 50 | 5988 | 4.663 | 61.02% | 65.33% | 15.21% | 12 | 17.25 |
| `AB0_NAV18_HOLD2_INDEX_MA25_EXIT_D_TRIGGER_ORDER_TOP4` | 162.01% | 9.69% | 104 | 3147 | 3.995 | 61.36% | 57.10% | 28.68% | 12 | 16.72 |

## E组 Top 5

| 策略 | 年化 | MDD | 恢复天数 | 交易数 | Sharpe | 胜率 | 资金利用率 | 最差完整年 | 盈利年份数 | score |
|---|---:|---:|---:|---:|---:|---:|---:|---:|---:|---:|
| `AB0_NAV12_HOLD2_NONE_E_TRIGGER_ORDER_UNTIL_FULL` | 237.58% | 10.55% | 175 | 6126 | 4.654 | 60.89% | 81.39% | 16.65% | 12 | 22.52 |
| `AB0_NAV10_HOLD2_NONE_E_TRIGGER_ORDER_UNTIL_FULL` | 204.53% | 10.03% | 39 | 6721 | 4.635 | 61.12% | 75.00% | 13.41% | 12 | 20.39 |
| `AB0_NAV15_HOLD2_NONE_E_TRIGGER_ORDER_UNTIL_FULL` | 236.72% | 12.25% | 177 | 4906 | 4.505 | 61.19% | 79.56% | 23.78% | 12 | 19.32 |
| `AB0_NAV18_HOLD2_NONE_E_TRIGGER_ORDER_UNTIL_FULL` | 247.07% | 13.74% | 180 | 4238 | 4.330 | 60.85% | 80.98% | 29.28% | 12 | 17.98 |
| `AB0_NAV15_HOLD2_INDEX_MA25_EXIT_E_TRIGGER_ORDER_UNTIL_FULL` | 156.37% | 9.65% | 104 | 3662 | 4.088 | 61.63% | 55.92% | 23.78% | 12 | 16.20 |

## 执行建议

- 首选 D：每天按小时线触发顺序买前 4 个，避免 E 的满仓挤压。
- E 收益更高，但恢复期明显更长，适合作为激进版本，不适合第一版无人值守。
- `INDEX_MA25_EXIT` 风控版降低收益，适合心理压力较大时使用，不作为主版本。

## 产出文件

- D/E 明细 CSV：`/home/tgm/quant/research/dvc-iteration-round4-execution-20260706/round4_DE_details.csv`
- D/E 明细 HTML：`/home/tgm/quant/research/dvc-iteration-round4-execution-20260706/round4_DE_details.html`
- 原始 48 策略结果：`/home/tgm/quant/research/dvc-iteration-round4-execution-20260706/round4_results.csv`
- 逐笔交易日志：`/home/tgm/quant/research/dvc-iteration-round4-execution-20260706/round4_trade_log.csv`
- 原始报告：`/home/tgm/quant/research/dvc-iteration-round4-execution-20260706/round4_report.html`
