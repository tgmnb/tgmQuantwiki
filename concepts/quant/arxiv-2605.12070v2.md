---
title: Missing Old Logits in Asynchronous Agentic RL: Semantic Mismatch and Repair Methods for Off-Policy Correction
created: 2026-05-20
updated: 2026-05-20
type: concept
tags: [strategy, 强化学习-rl, quant, factor]
sources: [Missing Old Logits in Asynchronous Agentic RL: Semantic Mismatch and Repair Methods for Off-Policy Correction]
confidence: medium
---

# Missing Old Logits in Asynchronous Agentic RL: Semantic Mismatch and Repair Methods for Off-Policy Correction

> 来源：[arXiv:2605.12070v2](https://arxiv.org/abs/2605.12070v2) | 作者：

## 基本信息

| 字段 | 值 |
|------|----|
| 发表 | 2026-05-12 |
| 方法 | 强化学习/RL |
| 策略类型 | 未识别 |
| 资产类别 | 未特定 |

## 摘要

Asynchronous reinforcement learning improves rollout throughput for large language model agents by decoupling sample generation from policy optimization, but it also introduces a critical failure mode for PPO-style off-policy correction. In heterogeneous training systems, the total importance ratio should ideally be decomposed into two semantically distinct factors: a \emph{training--inference discrepancy term} that aligns inference-side and training-side distributions at the same behavior-polic

## 核心方法论

**方法：** 强化学习/RL
**策略方向：** 未识别

## 关键发现

- Asynchronous reinforcement learning improves rollout throughput for large language model agents by decoupling sample generation from policy optimization, but it also introduces a critical failure mode for PPO-style off-policy correction.
- In heterogeneous training systems, the total importance ratio should ideally be decomposed into two semantically distinct factors: a \emph{training--inference discrepancy term} that aligns inference-side and training-side distributions at the same behavior-polic.

## 实践要点

- 细节需阅读原文确认

## 相关概念

- [[strategy-prototypes]] — 策略原型
- [[risk-management]] — 风险管理
- [[factor-research]] — 因子研究框架
