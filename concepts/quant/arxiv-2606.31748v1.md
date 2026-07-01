---
title: Addressing Over-Refusal in LLMs with Competing Rewards
created: 2026-07-01
updated: 2026-07-01
type: concept
tags: [factor, quant, strategy, 强化学习-rl]
sources: [Addressing Over-Refusal in LLMs with Competing Rewards]
confidence: medium
---

# Addressing Over-Refusal in LLMs with Competing Rewards

> 来源：[arXiv:2606.31748v1](https://arxiv.org/abs/2606.31748v1) | 作者：

## 基本信息

| 字段 | 值 |
|------|----|
| 发表 | 2026-06-30 |
| 方法 | 强化学习/RL |
| 策略类型 | 未识别 |
| 资产类别 | 未特定 |

## 摘要

Safety training on language models often induces over-refusal: improved safety on harmful prompts at the cost of increased refusal on harmless ones. Though this trade-off can be mitigated by training models with reinforcement learning (RL) to reason before answering, it does not remove the underlying problem that reasoning can often be a "rubber stamp" for a predetermined response. In this paper, we address the safety-refusal trade-off by rethinking how models are trained to reason about safety.

## 核心方法论

**方法：** 强化学习/RL
**策略方向：** 未识别

## 关键发现

- Safety training on language models often induces over-refusal: improved safety on harmful prompts at the cost of increased refusal on harmless ones.
- Though this trade-off can be mitigated by training models with reinforcement learning (RL) to reason before answering, it does not remove the underlying problem that reasoning can often be a "rubber stamp" for a predetermined response.
- In this paper, we address the safety-refusal trade-off by rethinking how models are trained to reason about safety.

## 实践要点

- 细节需阅读原文确认

## 相关概念

- [[strategy-prototypes]] — 策略原型
- [[risk-management]] — 风险管理
- [[factor-research]] — 因子研究框架
