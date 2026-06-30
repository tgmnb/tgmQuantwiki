---
title: Persona-Trained Monte Carlo: Estimating Market-Outcome Distributions via Swarms of Persona-Conditioned Neural Policy Bots in a Limit Order Book
created: 2026-06-30
updated: 2026-06-30
type: concept
tags: [factor, strategy, 强化学习-rl, quant, 执行-做市]
sources: [Persona-Trained Monte Carlo: Estimating Market-Outcome Distributions via Swarms of Persona-Conditioned Neural Policy Bots in a Limit Order Book]
confidence: medium
---

# Persona-Trained Monte Carlo: Estimating Market-Outcome Distributions via Swarms of Persona-Conditioned Neural Policy Bots in a Limit Order Book

> 来源：[arXiv:2606.29556v1](https://arxiv.org/abs/2606.29556v1) | 作者：

## 基本信息

| 字段 | 值 |
|------|----|
| 发表 | 2026-06-28 |
| 方法 | 强化学习/RL |
| 策略类型 | 执行/做市 |
| 资产类别 | 未特定 |

## 摘要

We propose Persona-Trained Monte Carlo (PTMC), a method for estimating distributions of market-outcome statistics by repeatedly simulating limit-order-book interaction among swarms of persona-conditioned neural-policy trading bots. Each run instantiates many bots sharing one trained policy network but conditioned on heterogeneous, individually sampled persona parameters drawn from a learned trader-heterogeneity distribution; the bots interact in a continuous double auction, and the resulting pri

## 核心方法论

**方法：** 强化学习/RL
**策略方向：** 执行/做市

## 关键发现

- We propose Persona-Trained Monte Carlo (PTMC), a method for estimating distributions of market-outcome statistics by repeatedly simulating limit-order-book interaction among swarms of persona-conditioned neural-policy trading bots.
- Each run instantiates many bots sharing one trained policy network but conditioned on heterogeneous, individually sampled persona parameters drawn from a learned trader-heterogeneity distribution; the bots interact in a continuous double auction, and the resulting pri.

## 实践要点

- 细节需阅读原文确认

## 相关概念

- [[strategy-prototypes]] — 策略原型
- [[risk-management]] — 风险管理
- [[factor-research]] — 因子研究框架
