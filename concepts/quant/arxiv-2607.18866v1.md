---
title: Optimizing Regret
created: 2026-07-22
updated: 2026-07-22
type: concept
tags: [strategy, factor, 动量-趋势跟踪, 强化学习-rl, 均值回归, quant]
sources: [Optimizing Regret]
confidence: medium
---

# Optimizing Regret

> 来源：[arXiv:2607.18866v1](https://arxiv.org/abs/2607.18866v1) | 作者：

## 基本信息

| 字段 | 值 |
|------|----|
| 发表 | 2026-07-21 |
| 方法 | 强化学习/RL |
| 策略类型 | 动量/趋势跟踪, 均值回归 |
| 资产类别 | 未特定 |

## 摘要

Building on the identity that expected regret equals the covariance between costs and decisions, this paper develops the complete derivative theory of the covariance regret functional. We derive the Gâteaux derivative, showing that the universal steepest-descent direction is the contrarian policy $-(c-\bar{c})$, while ascent yields momentum. For linear policies $\hatπ(c) = Ac+b$, the gradient is the cost covariance matrix $Σ_c$, with a zero Hessian implying boundary-optimal solutions such as the

## 核心方法论

**方法：** 强化学习/RL
**策略方向：** 动量/趋势跟踪, 均值回归

## 关键发现

- Building on the identity that expected regret equals the covariance between costs and decisions, this paper develops the complete derivative theory of the covariance regret functional.
- We derive the Gâteaux derivative, showing that the universal steepest-descent direction is the contrarian policy $-(c-\bar{c})$, while ascent yields momentum.
- For linear policies $\hatπ(c) = Ac+b$, the gradient is the cost covariance matrix $Σ_c$, with a zero Hessian implying boundary-optimal solutions such as the.

## 实践要点

- 细节需阅读原文确认

## 相关概念

- [[strategy-prototypes]] — 策略原型
- [[risk-management]] — 风险管理
- [[factor-research]] — 因子研究框架
