---
title: Certified High-Dimensional Wasserstein Robust Portfolio Optimization
created: 2026-08-10
updated: 2026-08-10
type: concept
tags: [quant, 组合优化, 鲁棒优化, 风险, Wasserstein]
sources: [Certified High-Dimensional Wasserstein Robust Portfolio Optimization]
confidence: medium
---

# Certified High-Dimensional Wasserstein Robust Portfolio Optimization

> 来源：[arXiv:2608.07032v1](https://arxiv.org/abs/2608.07032) | 作者：Chung-Han Hsieh, Rong Gan

## 基本信息

| 字段 | 值 |
|------|----|
| 发表 | 2026-08-07 |
| 方法 | 鲁棒优化/Wasserstein分布鲁棒 |
| 策略类型 | 组合优化 |
| 资产类别 | 未特定 |

## 摘要

We develop a certified, scalable approximation for high-dimensional Wasserstein distributionally robust portfolio optimization. For expected-utility maximization under order-one Wasserstein ambiguity, standard duality yields a semi-infinite convex program. For long-only portfolios with box support under the one-norm ground metric, an exact sample-specific vertex reformulation provides an exponential-size computational benchmark. We then majorize the utility by supporting hyperplanes and dualize the support subproblems, obtaining a finite hyperplane--dual formulation over compact polyhedral supports. Under the one-norm ground metric and polyhedral portfolio constraints, this formulation is a polynomial-size linear program. The uniform utility-approximation error bounds both the robust-value error and the near-optimality gap for the original robust problem. Experiments validate the certified approximation and demonstrate monthly 476-asset rebalancing and computational scalability to 1,000 assets.

## 核心方法论

**方法：** 鲁棒优化/Wasserstein分布鲁棒
**策略方向：** 组合优化

## 关键发现

- We develop a certified, scalable approximation for high-dimensional Wasserstein distributionally robust portfolio optimization.
- For expected-utility maximization under order-one Wasserstein ambiguity, standard duality yields a semi-infinite convex program.
- For long-only portfolios with box support under the one-norm ground metric, an exact sample-specific vertex reformulation provides an exponential-size computational benchmark.

## 实践要点

- 细节需阅读原文确认

## 相关概念

- [[strategy-prototypes]] — 策略原型
- [[risk-management]] — 风险管理
- [[factor-research]] — 因子研究框架
