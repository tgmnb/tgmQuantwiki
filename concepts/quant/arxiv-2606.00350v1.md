---
title: Drift Q-Learning
created: 2026-06-02
updated: 2026-06-02
type: concept
tags: [factor, strategy, 集成方法, quant, 强化学习-rl]
sources: [Drift Q-Learning]
confidence: medium
---

# Drift Q-Learning

> 来源：[arXiv:2606.00350v1](https://arxiv.org/abs/2606.00350v1) | 作者：

## 基本信息

| 字段 | 值 |
|------|----|
| 发表 | 2026-05-29 |
| 方法 | 强化学习/RL, 集成方法 |
| 策略类型 | 未识别 |
| 资产类别 | 未特定 |

## 摘要

Offline reinforcement learning requires improving a policy from fixed data while avoiding out-of-distribution actions with unreliable value estimates. Diffusion and flow policies handle this trade-off by modeling the behavior distribution to regularize the RL objective, but they require iterative denoising, solver integrations, and in more efficient variants, distillation or other approximations at inference. We propose DriftQL, which combines a drift-based behavioral regularizer with critic-dri

## 核心方法论

**方法：** 强化学习/RL, 集成方法
**策略方向：** 未识别

## 关键发现

- Offline reinforcement learning requires improving a policy from fixed data while avoiding out-of-distribution actions with unreliable value estimates.
- Diffusion and flow policies handle this trade-off by modeling the behavior distribution to regularize the RL objective, but they require iterative denoising, solver integrations, and in more efficient variants, distillation or other approximations at inference.
- We propose DriftQL, which combines a drift-based behavioral regularizer with critic-dri.

## 实践要点

- 细节需阅读原文确认

## 相关概念

- [[strategy-prototypes]] — 策略原型
- [[risk-management]] — 风险管理
- [[factor-research]] — 因子研究框架
