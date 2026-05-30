---
title: Meta-Attention: Bayesian Per-Token Routing for Efficient Transformer Inference
created: 2026-05-30
updated: 2026-05-30
type: concept
tags: [strategy, quant, factor, transformer-llm]
sources: [Meta-Attention: Bayesian Per-Token Routing for Efficient Transformer Inference]
confidence: medium
---

# Meta-Attention: Bayesian Per-Token Routing for Efficient Transformer Inference

> 来源：[arXiv:2605.28384v1](https://arxiv.org/abs/2605.28384v1) | 作者：

## 基本信息

| 字段 | 值 |
|------|----|
| 发表 | 2026-05-27 |
| 方法 | Transformer/LLM |
| 策略类型 | 未识别 |
| 资产类别 | 未特定 |

## 摘要

Standard transformer architectures apply a single attention mechanism uniformly across all tokens and sequence positions, irrespective of local context or computational budget. We propose Meta-Attention, a framework that dynamically routes each token to the most appropriate attention strategy -- full softmax attention, linear (kernel) attention, or sliding-window local attention -- via a Bayesian Meta-Controller. Unlike prior routing approaches that use deterministic or prior-free learned routin

## 核心方法论

**方法：** Transformer/LLM
**策略方向：** 未识别

## 关键发现

- Standard transformer architectures apply a single attention mechanism uniformly across all tokens and sequence positions, irrespective of local context or computational budget.
- We propose Meta-Attention, a framework that dynamically routes each token to the most appropriate attention strategy -- full softmax attention, linear (kernel) attention, or sliding-window local attention -- via a Bayesian Meta-Controller.
- Unlike prior routing approaches that use deterministic or prior-free learned routin.

## 实践要点

- 细节需阅读原文确认

## 相关概念

- [[strategy-prototypes]] — 策略原型
- [[risk-management]] — 风险管理
- [[factor-research]] — 因子研究框架
