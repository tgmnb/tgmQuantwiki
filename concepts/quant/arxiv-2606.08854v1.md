---
title: sGPO: Trading Inference FLOPs for Training Efficiency in RLVR
created: 2026-06-09
updated: 2026-06-09
type: concept
tags: [强化学习-rl, quant, factor, strategy]
sources: [sGPO: Trading Inference FLOPs for Training Efficiency in RLVR]
confidence: medium
---

# sGPO: Trading Inference FLOPs for Training Efficiency in RLVR

> 来源：[arXiv:2606.08854v1](https://arxiv.org/abs/2606.08854v1) | 作者：

## 基本信息

| 字段 | 值 |
|------|----|
| 发表 | 2026-06-07 |
| 方法 | 强化学习/RL |
| 策略类型 | 未识别 |
| 资产类别 | 未特定 |

## 摘要

Standard Reinforcement Learning with Verifiable Rewards (RLVR) training allocates a fixed rollout budget to every query, without regard for what each query's difficulty means for the current policy. This leads to two symmetric failure modes: easy queries produce near-zero advantage because the policy already solves them, while unsolvable queries produce no signal because the policy never solves them. Both regimes waste training FLOPs without contributing to a learning gradient. We introduce sort

## 核心方法论

**方法：** 强化学习/RL
**策略方向：** 未识别

## 关键发现

- Standard Reinforcement Learning with Verifiable Rewards (RLVR) training allocates a fixed rollout budget to every query, without regard for what each query's difficulty means for the current policy.
- This leads to two symmetric failure modes: easy queries produce near-zero advantage because the policy already solves them, while unsolvable queries produce no signal because the policy never solves them.
- Both regimes waste training FLOPs without contributing to a learning gradient.

## 实践要点

- 细节需阅读原文确认

## 相关概念

- [[strategy-prototypes]] — 策略原型
- [[risk-management]] — 风险管理
- [[factor-research]] — 因子研究框架
