---
title: TT-DAC-PS: Twin-Target Deterministic Actor-Critic with Policy Smoothing for Optimal Trade Execution
created: 2026-06-09
updated: 2026-06-09
type: concept
tags: [强化学习-rl, 集成方法, 执行-做市, factor, strategy, quant]
sources: [TT-DAC-PS: Twin-Target Deterministic Actor-Critic with Policy Smoothing for Optimal Trade Execution]
confidence: medium
---

# TT-DAC-PS: Twin-Target Deterministic Actor-Critic with Policy Smoothing for Optimal Trade Execution

> 来源：[arXiv:2606.08379v1](https://arxiv.org/abs/2606.08379v1) | 作者：

## 基本信息

| 字段 | 值 |
|------|----|
| 发表 | 2026-06-07 |
| 方法 | 强化学习/RL, 集成方法 |
| 策略类型 | 执行/做市 |
| 资产类别 | 股票 |

## 摘要

This study addresses the optimal execution of large stock sell programs by introducing TT-DAC-PS (Twin-Target Deterministic Actor-Critic with Policy Smoothing), a deterministic actor-critic architecture that combines twin exponential-moving-average critic targets with pessimistic min backup, TD3-style target policy smoothing noise, delayed actor updates, and conservative Q regularisation to curb overestimation. Exploration uses Ornstein-Uhlenbeck (OU) noise with a hybrid schedule: deterministic 

## 核心方法论

**方法：** 强化学习/RL, 集成方法
**策略方向：** 执行/做市

## 关键发现

- This study addresses the optimal execution of large stock sell programs by introducing TT-DAC-PS (Twin-Target Deterministic Actor-Critic with Policy Smoothing), a deterministic actor-critic architecture that combines twin exponential-moving-average critic targets with pessimistic min backup, TD3-style target policy smoothing noise, delayed actor updates, and conservative Q regularisation to curb overestimation.
- Exploration uses Ornstein-Uhlenbeck (OU) noise with a hybrid schedule: deterministic.

## 实践要点

- 细节需阅读原文确认

## 相关概念

- [[strategy-prototypes]] — 策略原型
- [[risk-management]] — 风险管理
- [[factor-research]] — 因子研究框架
