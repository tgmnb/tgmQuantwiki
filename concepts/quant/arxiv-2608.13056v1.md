---
title: Simulating Stress Laws under Extremal Dependence: Characterizing What Generative Models Must Preserve
created: 2026-08-14
updated: 2026-08-14
type: concept
tags: [quant, 风险, 极值, 压力测试]
sources: [Simulating Stress Laws under Extremal Dependence: Characterizing What Generative Models Must Preserve]
confidence: medium
---

# Simulating Stress Laws under Extremal Dependence: Characterizing What Generative Models Must Preserve

> 来源：[arXiv:2608.13056v1](https://arxiv.org/abs/2608.13056) | 作者：Mantu Gupta, Anand Deo

## 基本信息

| 字段 | 值 |
|------|----|
| 发表 | 2026-08-13 |
| 方法 | 极值依赖/生成模型 |
| 策略类型 | 压力场景生成 |
| 资产类别 | 未特定 |

## 摘要

We study stress-scenario generation for systems driven by multivariate heavy-tailed risk factors. Within regions where several financial losses are simultaneously extreme, stress analysis concerns both the conditional law of the risk factors and the most plausible configurations producing those losses. We show that both are governed by the same limiting tail law. Preserving its measure recovers rare-event probabilities and scaled conditional stress laws, while misspecifying extremal dependence distorts some regular joint-stress probability. Its density governs reverse-stress optimization, whose maximizers identify the most plausible stress configurations. To exploit this common structure in finite samples, we develop SSGEN (Self-Similar Generative Estimation), which learns extremal dependence from intermediate exceedances and extrapolates to rarer levels using a Pareto radial component. Even when the target event is absent from the sample, we establish convergence rates for the generated conditional law, and data-driven reverse-stress solutions.

## 核心方法论

**方法：** 极值依赖/生成模型
**策略方向：** 压力场景生成

## 关键发现

- We study stress-scenario generation for systems driven by multivariate heavy-tailed risk factors.
- Within regions where several financial losses are simultaneously extreme, stress analysis concerns both the conditional law of the risk factors and the most plausible configurations producing those losses.
- We show that both are governed by the same limiting tail law.

## 实践要点

- 细节需阅读原文确认

## 相关概念

- [[strategy-prototypes]] — 策略原型
- [[risk-management]] — 风险管理
- [[factor-research]] — 因子研究框架
