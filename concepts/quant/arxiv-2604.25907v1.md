---
title: raw/articles/2604.25907v1.md
created: 2026-05-01
updated: 2026-05-01
type: concept
tags: [强化学习-rl, factor, quant, strategy]
sources: [raw/articles/2604.25907v1.md]
confidence: medium
---

# How Fast Should a Model Commit to Supervision? Training Reasoning Models on the Tsallis Loss Continuum

> 来源：[arXiv:2604.25907v1](https://arxiv.org/abs/2604.25907v1) | 作者：

## 基本信息

| 字段 | 值 |
|------|----|
| 发表 | 2026-04-28 |
| 方法 | 强化学习/RL |
| 策略类型 | 未识别 |
| 资产类别 | 未特定 |

## 摘要

Adapting reasoning models to new tasks during post-training with only output-level supervision stalls under reinforcement learning from verifiable rewards (RLVR) when the initial success probability $p_0$ is small. Using the Tsallis $q$-logarithm, we define a loss family $J_Q$ that interpolates between RLVR (at $q{=}0$, the exploitation pole) and the log-marginal-likelihood over latent trajectories (at $q{=}1$, the density-estimation pole). All members share the same per-example gradient directi

## 核心方法论

**方法：** 强化学习/RL
**策略方向：** 未识别

## 关键发现

- Adapting reasoning models to new tasks during post-training with only output-level supervision stalls under reinforcement learning from verifiable rewards (RLVR) when the initial success probability $p_0$ is small.
- Using the Tsallis $q$-logarithm, we define a loss family $J_Q$ that interpolates between RLVR (at $q{=}0$, the exploitation pole) and the log-marginal-likelihood over latent trajectories (at $q{=}1$, the density-estimation pole).
- All members share the same per-example gradient directi.

## 实践要点

- 细节需阅读原文确认

## 相关概念

- [[strategy-prototypes]] — 策略原型
- [[risk-management]] — 风险管理
- [[factor-research]] — 因子研究框架
