---
title: Weak-to-Strong On-Policy Distillation
created: 2026-08-01
updated: 2026-08-01
type: concept
tags: [factor, strategy, quant, 强化学习-rl]
sources: [Weak-to-Strong On-Policy Distillation]
confidence: medium
---

# Weak-to-Strong On-Policy Distillation

> 来源：[arXiv:2607.26246v1](https://arxiv.org/abs/2607.26246v1) | 作者：

## 基本信息

| 字段 | 值 |
|------|----|
| 发表 | 2026-07-28 |
| 方法 | 强化学习/RL |
| 策略类型 | 未识别 |
| 资产类别 | 未特定 |

## 摘要

On-policy distillation (OPD), which aligns a student with the teacher's token-level distribution on the student's own rollouts, is an effective paradigm for transferring capabilities across LLMs. Prevailing approaches assume a teacher at least as capable as the student: they either distill a larger model into a smaller one, which fails at the frontier where no larger teacher exists, or consolidate multiple domain experts trained from a shared base, which requires costly training at the student's

## 核心方法论

**方法：** 强化学习/RL
**策略方向：** 未识别

## 关键发现

- On-policy distillation (OPD), which aligns a student with the teacher's token-level distribution on the student's own rollouts, is an effective paradigm for transferring capabilities across LLMs.
- Prevailing approaches assume a teacher at least as capable as the student: they either distill a larger model into a smaller one, which fails at the frontier where no larger teacher exists, or consolidate multiple domain experts trained from a shared base, which requires costly training at the student's.

## 实践要点

- 细节需阅读原文确认

## 相关概念

- [[strategy-prototypes]] — 策略原型
- [[risk-management]] — 风险管理
- [[factor-research]] — 因子研究框架
