---
title: Train Often, Deploy Selectively: Forward-Gated Model Replacement in Crypto Markets
created: 2026-08-01
updated: 2026-08-01
type: concept
tags: [强化学习-rl, quant, factor, strategy]
sources: [Train Often, Deploy Selectively: Forward-Gated Model Replacement in Crypto Markets]
confidence: medium
---

# Train Often, Deploy Selectively: Forward-Gated Model Replacement in Crypto Markets

> 来源：[arXiv:2607.28577](https://arxiv.org/abs/2607.28577) | 作者：Aditya Dutta

## 基本信息

| 字段 | 值 |
|------|----|
| 发表 | 2026-07-31 |
| 方法 | 强化学习/RL |
| 策略类型 | 未识别 |
| 资产类别 | 期货/商品, 加密货币 |

## 摘要

Production forecasting systems retrain models regularly, but a retrained candidate does not necessarily outperform a continuously maintained incumbent that has continued to learn. We introduce Shadow Before Swap (SBS), a deployment policy that warm-refits a challenger off the serving path, evaluates it against the maintained incumbent on the same next week of delayed labels, and promotes it only after a fixed paired negative-log-likelihood (NLL) advantage. In historical replay over two nonoverlapping Binance episodes spanning 48 UTC weeks, three seeds, eight underlyings, and two perpetual-futures contract types, SBS reduces NLL by 0.1472% relative to calendar replacement, 0.0755% relative to schedule-matched automatic promotion, and 0.0428% relative to continuous maintenance. The corresponding episode-stratified four-week block intervals are 0.1139%-0.1754%, 0.0521%-0.0980%, and 0.0301%-0.0554%, respectively. SBS promotes 114 of 528 challengers, reducing deployed model changes by 78.4% while improving the serving trajectory. The effect remains directionally consistent across seeds, trial budgets, promotion margins, an earlier 20-asset panel, and a topology-matched supervised objective. SBS thus provides a practical deployment policy that improves probabilistic forecasts while limiting consequential model-state transitions.

## 核心方法论

**方法：** 强化学习/RL
**策略方向：** 未识别

## 关键发现

- Production forecasting systems retrain models regularly, but a retrained candidate does not necessarily outperform a continuously maintained incumbent that has continued to learn.
- We introduce Shadow Before Swap (SBS), a deployment policy that warm-refits a challenger off the serving path, evaluates it against the maintained incumbent on the same next week of delayed labels, and promotes it only after a fixed paired negative-log-likelihood (NLL) advantage.
- In historical replay over two nonoverlapping Binance episodes spanning 48 UTC weeks, three seeds, eight underlyings, and two perpetual-futures contract types, SBS reduces NLL by 0.

## 实践要点

- 策略报告了超额收益（需核实样本外表现）
- 基于历史回测，警惕过拟合风险

## 相关概念

- [[strategy-prototypes]] — 策略原型
- [[risk-management]] — 风险管理
- [[factor-research]] — 因子研究框架
