---
title: VIMPO: Value-Implicit Policy Optimization for LLMs
created: 2026-06-19
updated: 2026-06-19
type: concept
tags: [quant, 强化学习-rl, factor, strategy]
sources: [VIMPO: Value-Implicit Policy Optimization for LLMs]
confidence: medium
---

# VIMPO: Value-Implicit Policy Optimization for LLMs

> 来源：[arXiv:2606.20008v1](https://arxiv.org/abs/2606.20008v1) | 作者：

## 基本信息

| 字段 | 值 |
|------|----|
| 发表 | 2026-06-18 |
| 方法 | 强化学习/RL |
| 策略类型 | 未识别 |
| 资产类别 | 未特定 |

## 摘要

Reinforcement learning with verifiable rewards has become a central tool for improving the reasoning ability of large language models, but current methods face a trade-off between simplicity and credit assignment. Group-relative methods such as GRPO avoid training a critic, but typically assign a trajectory-level advantage to every token. Actor-critic methods provide denser learning signals, but require a learned value function with its own training instability. We introduce VIMPO, a critic-free

## 核心方法论

**方法：** 强化学习/RL
**策略方向：** 未识别

## 关键发现

- Reinforcement learning with verifiable rewards has become a central tool for improving the reasoning ability of large language models, but current methods face a trade-off between simplicity and credit assignment.
- Group-relative methods such as GRPO avoid training a critic, but typically assign a trajectory-level advantage to every token.
- Actor-critic methods provide denser learning signals, but require a learned value function with its own training instability.

## 实践要点

- 细节需阅读原文确认

## 相关概念

- [[strategy-prototypes]] — 策略原型
- [[risk-management]] — 风险管理
- [[factor-research]] — 因子研究框架
