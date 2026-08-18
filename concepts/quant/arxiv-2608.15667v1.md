---
title: Scalable Pontryagin-Guided Adjoint-to-Control Recovery for Constrained Dynamic Portfolio Choice
created: 2026-08-18
updated: 2026-08-18
type: concept
tags: [执行-做市, 强化学习-rl, factor, quant, strategy]
sources: [Scalable Pontryagin-Guided Adjoint-to-Control Recovery for Constrained Dynamic Portfolio Choice]
confidence: medium
---

# Scalable Pontryagin-Guided Adjoint-to-Control Recovery for Constrained Dynamic Portfolio Choice

> 来源：[arXiv:2608.15667v1](https://arxiv.org/abs/2608.15667v1) | 作者：

## 基本信息

| 字段 | 值 |
|------|----|
| 发表 | 2026-08-16 |
| 方法 | 强化学习/RL |
| 策略类型 | 执行/做市 |
| 资产类别 | 未特定 |

## 摘要

We develop a scalable adjoint-to-control framework for continuous-time portfolio choice under smooth pointwise constraints. A feasible direct-policy-optimization (DPO) policy supplies rollouts; after training, fixed-latent open-loop BPTT (OL-BPTT) yields first- and second-order pathwise sensitivities, whose conditional projections produce adapted adjoint inputs. A nested antithetic common-random-number regression estimates the shifted wealth-row martingale input, and deployment solves the local 

## 核心方法论

**方法：** 强化学习/RL
**策略方向：** 执行/做市

## 关键发现

- We develop a scalable adjoint-to-control framework for continuous-time portfolio choice under smooth pointwise constraints.
- A feasible direct-policy-optimization (DPO) policy supplies rollouts; after training, fixed-latent open-loop BPTT (OL-BPTT) yields first- and second-order pathwise sensitivities, whose conditional projections produce adapted adjoint inputs.
- A nested antithetic common-random-number regression estimates the shifted wealth-row martingale input, and deployment solves the local.

## 实践要点

- 细节需阅读原文确认

## 相关概念

- [[strategy-prototypes]] — 策略原型
- [[risk-management]] — 风险管理
- [[factor-research]] — 因子研究框架
