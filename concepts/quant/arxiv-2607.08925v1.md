---
title: SafeExplorer: An Unbiased Policy Gradient for Reinforcement Learning with Recovery Interventions
created: 2026-07-13
updated: 2026-07-13
type: concept
tags: [强化学习-rl, strategy, quant, factor]
sources: [SafeExplorer: An Unbiased Policy Gradient for Reinforcement Learning with Recovery Interventions]
confidence: medium
---

# SafeExplorer: An Unbiased Policy Gradient for Reinforcement Learning with Recovery Interventions

> 来源：[arXiv:2607.08925v1](https://arxiv.org/abs/2607.08925v1) | 作者：

## 基本信息

| 字段 | 值 |
|------|----|
| 发表 | 2026-07-09 |
| 方法 | 强化学习/RL |
| 策略类型 | 未识别 |
| 资产类别 | 未特定 |

## 摘要

Training reinforcement-learning agents directly on physical robots makes every fall costly, since a fall can damage the platform and cannot be undone like a simulator reset; the goal is therefore to minimize falls during training rather than trade them off against return, as constrained Markov decision process (MDP) formulations do. A standard mitigation hands control to a separate recovery policy whenever the agent leaves a designer-specified safe region (a subset of state space it should stay 

## 核心方法论

**方法：** 强化学习/RL
**策略方向：** 未识别

## 关键发现

- Training reinforcement-learning agents directly on physical robots makes every fall costly, since a fall can damage the platform and cannot be undone like a simulator reset; the goal is therefore to minimize falls during training rather than trade them off against return, as constrained Markov decision process (MDP) formulations do.
- A standard mitigation hands control to a separate recovery policy whenever the agent leaves a designer-specified safe region (a subset of state space it should stay.

## 实践要点

- 细节需阅读原文确认

## 相关概念

- [[strategy-prototypes]] — 策略原型
- [[risk-management]] — 风险管理
- [[factor-research]] — 因子研究框架
