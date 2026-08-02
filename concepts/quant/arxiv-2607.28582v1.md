---
title: $β$-OPSD: Deriving with Policy Optimization, Training with Self-Distillation
created: 2026-08-01
updated: 2026-08-01
type: concept
tags: [factor, strategy, quant, 强化学习-rl]
sources: [$β$-OPSD: Deriving with Policy Optimization, Training with Self-Distillation]
confidence: medium
---

# $β$-OPSD: Deriving with Policy Optimization, Training with Self-Distillation

> 来源：[arXiv:2607.28582v1](https://arxiv.org/abs/2607.28582v1) | 作者：

## 基本信息

| 字段 | 值 |
|------|----|
| 发表 | 2026-07-30 |
| 方法 | 强化学习/RL |
| 策略类型 | 未识别 |
| 资产类别 | 未特定 |

## 摘要

On-policy self-distillation (OPSD) is a promising approach to improve reasoning language models, but it remains brittle in practice: making it work reliably often requires substantial engineering effort. We identify a structural source of this difficulty: vanilla OPSD is precisely the $β=1$ member of a broader policy-optimization family, where $β$ weights the KL penalty anchoring the student to a reference policy. This equivalence turns $β$ from an implicit value fixed at one into a controllable

## 核心方法论

**方法：** 强化学习/RL
**策略方向：** 未识别

## 关键发现

- On-policy self-distillation (OPSD) is a promising approach to improve reasoning language models, but it remains brittle in practice: making it work reliably often requires substantial engineering effort.
- We identify a structural source of this difficulty: vanilla OPSD is precisely the $β=1$ member of a broader policy-optimization family, where $β$ weights the KL penalty anchoring the student to a reference policy.
- This equivalence turns $β$ from an implicit value fixed at one into a controllable.

## 实践要点

- 细节需阅读原文确认

## 相关概念

- [[strategy-prototypes]] — 策略原型
- [[risk-management]] — 风险管理
- [[factor-research]] — 因子研究框架
