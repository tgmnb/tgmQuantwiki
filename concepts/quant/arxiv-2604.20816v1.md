---
title: raw/articles/2604.20816v1.md
created: 2026-05-01
updated: 2026-05-01
type: concept
tags: [强化学习-rl, factor, quant, strategy]
sources: [raw/articles/2604.20816v1.md]
confidence: medium
---

# ParetoSlider: Diffusion Models Post-Training for Continuous Reward Control

> 来源：[arXiv:2604.20816v1](https://arxiv.org/abs/2604.20816v1) | 作者：

## 基本信息

| 字段 | 值 |
|------|----|
| 发表 | 2026-04-22 |
| 方法 | 强化学习/RL |
| 策略类型 | 未识别 |
| 资产类别 | 未特定 |

## 摘要

Reinforcement Learning (RL) post-training has become the standard for aligning generative models with human preferences, yet most methods rely on a single scalar reward. When multiple criteria matter, the prevailing practice of ``early scalarization'' collapses rewards into a fixed weighted sum. This commits the model to a single trade-off point at training time, providing no inference-time control over inherently conflicting goals -- such as prompt adherence versus source fidelity in image edit

## 核心方法论

**方法：** 强化学习/RL
**策略方向：** 未识别

## 关键发现

- Reinforcement Learning (RL) post-training has become the standard for aligning generative models with human preferences, yet most methods rely on a single scalar reward.
- When multiple criteria matter, the prevailing practice of ``early scalarization'' collapses rewards into a fixed weighted sum.
- This commits the model to a single trade-off point at training time, providing no inference-time control over inherently conflicting goals -- such as prompt adherence versus source fidelity in image edit.

## 实践要点

- 细节需阅读原文确认

## 相关概念

- [[strategy-prototypes]] — 策略原型
- [[risk-management]] — 风险管理
- [[factor-research]] — 因子研究框架
