---
title: Multi-Asset Liquidation in Dark Pools with Adverse Selection
created: 2026-08-11
updated: 2026-08-11
type: concept
tags: [quant, 执行, 最优清算, 暗池, 多资产, 随机控制]
sources: [Multi-Asset Liquidation in Dark Pools with Adverse Selection]
confidence: medium
---

# Multi-Asset Liquidation in Dark Pools with Adverse Selection

> 来源：[arXiv:2607.27019v1](https://arxiv.org/abs/2607.27019) | 作者：Guanxing Fu, Johannes Ruf, Xiaomin Shi等

## 基本信息

| 字段 | 值 |
|------|----|
| 发表 | 2026-07-29 |
| 方法 | 矩阵值BSDE/带跳随机控制 |
| 策略类型 | 最优执行/清算 |
| 资产类别 | 未特定 |

## 摘要

We study the optimal liquidation of a multi-asset portfolio using both a traditional exchange and dark pools in the presence of quadratic adverse-selection costs. The problem leads to a matrix-valued backward stochastic differential equation with jumps and a singular terminal condition. We establish existence and uniqueness of its solution and use it to characterize the value function and the optimal liquidation strategy. The uniqueness result is the main mathematical contribution and strengthens the existing theory even in simpler special cases; the existence result is also new.   For a two-asset model, we distinguish the roles of asset correlation, own-asset adverse selection, and cross-asset spillover in adverse-selection costs. Under diagonal temporary impact and in the absence of cross-asset spillover, an initially well-diversified portfolio remains well diversified during optimal liquidation and, for a fixed sign of the correlation, its liquidation cost is strictly decreasing in the magnitude of the correlation. By contrast, under the same diagonal-impact specification, under explicit conditions and sufficiently close to the liquidation horizon, cross-asset spillover makes a well-diversified portfolio more costly to liquidate than its poorly diversified sign-reversed counterpart and causes sufficiently unbalanced well-diversified portfolios to become poorly diversified with positive probability. Separately, without requiring diagonal temporary impact, we show that, in the absence of cross-asset spillover, own-asset adverse selection introduces an explicit shrinkage factor in the optimal dark-pool order relative to the order minimizing the post-execution continuation value. Finally, we derive an explicit condition under which a dark-pool execution transforms a poorly diversified portfolio into a well-diversified one.

## 核心方法论

**方法：** 矩阵值BSDE/带跳随机控制
**策略方向：** 最优执行/清算

## 关键发现

- We study the optimal liquidation of a multi-asset portfolio using both a traditional exchange and dark pools in the presence of quadratic adverse-selection costs.
- The problem leads to a matrix-valued backward stochastic differential equation with jumps and a singular terminal condition.
- We establish existence and uniqueness of its solution and use it to characterize the value function and the optimal liquidation strategy.

## 实践要点

- 细节需阅读原文确认

## 相关概念

- [[strategy-prototypes]] — 策略原型
- [[risk-management]] — 风险管理
- [[factor-research]] — 因子研究框架
