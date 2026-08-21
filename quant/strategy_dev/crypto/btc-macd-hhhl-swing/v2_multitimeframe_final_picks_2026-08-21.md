---
title: BTC MACD HHHL Swing 多周期最终选型共享资金池审计
created: 2026-08-21
updated: 2026-08-21
type: research-blocker
status: blocked
---

# 多周期最终选型共享资金池审计

## 结论

本轮研究被精确阻塞：没有足够的正式周期专属最终选型证据，不能真实运行 1h/4h/1D/1W 同时共用一个 Account 的风险配比回测。

1h 有可追溯的最终选择记录和 split 参考基线；4h、1D、1W 没有正式锁定的多头、空头、周期、stop-dist、target、sizing 组合。旧 all-tf 的 `m3` 只是各周期独立账户画像，不能替代最终选型。

## 证据

- 审计目录：`/home/tgm/quant/btc-macd-hhhl-swing/v2_multitimeframe_final_picks_20260821/`
- 审计文件：`FINAL_SELECTION_AUDIT.md`
- 状态文件：`STATE`
- 报告：`REPORT_CN.md`
- 1h 选择记录：`/home/tgm/quant/btc-macd-hhhl-swing/unified_fair_selection_rerun_20260821/DEV_SELECTION.json`
- 1h 最终决策：`/home/tgm/quant/btc-macd-hhhl-swing/unified_fair_selection_rerun_20260821/FINAL_DECISION.md`
- 旧多周期画像：`/home/tgm/quant/btc-macd-hhhl-swing/shared_pool_all_alltfs/pool_summary_alltfs.csv`
- 上一轮审计：`/home/tgm/quant/btc-macd-hhhl-swing/v2_multitimeframe_shared_pool_20260821/AUDIT.md`

## 禁止的替代

不得把旧 `m3` 作为 4h/1D/1W 最终选型；不得把 1h 参数静默复制到其他周期；不得把独立账户指标改写为共享账户结果。

## 未执行

因此没有执行共享 Account runner、BTC/9 币契约测试、30 币 DEV/OOS 配比、四种预注册配比扫描或 710 币 detached 运行，也没有生成伪造的回测结果。

## 恢复条件

补齐四个周期各自正式的 split 多空最终选型记录（含 stop-dist、target、sizing、DEV 锁定和 OOS 冻结证据）后，才能继续实现事件化共享账户 runner。
