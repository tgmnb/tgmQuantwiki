---
title: An Extreme Value Perspective on Learning Stress Laws
created: 2026-08-14
updated: 2026-08-14
type: concept
tags: [quant, 风险, 极值, 生成模型]
sources: [An Extreme Value Perspective on Learning Stress Laws]
confidence: medium
---

# An Extreme Value Perspective on Learning Stress Laws

> 来源：[arXiv:2607.10700v1](https://arxiv.org/abs/2607.10700) | 作者：Mantu Gupta, Anand Deo

## 基本信息

| 字段 | 值 |
|------|----|
| 发表 | 2026-07-12 |
| 方法 | SS-GEN自相似生成 |
| 策略类型 | 尾部事件模拟 |
| 资产类别 | 未特定 |

## 摘要

We introduce Self-Similar Generative Estimation (SS-GEN), a method for simulating multivariate tail events and estimating rare-event probabilities in both heavy and light-tailed settings. SS-GEN exploits asymptotic tail structure to decompose the tail distribution into an explicit radial component and a nonparametric angular component, reducing tail learning to a compact-domain problem that can be handled by off-the-shelf deep generative models. The resulting sampler generates representative extreme scenarios and supports probability estimation far beyond the observed data. Under mild nonparametric tail assumptions, we show that the SS-GEN density is asymptotically exact in the tail, with vanishing uniform relative error for regularly varying distributions and vanishing uniform log-relative error for Weibull-type distributions. Unlike existing approaches that rely on specialized architectures or parametric tail specifications, SS-GEN leverages asymptotic tail structure to enable standard generative models to generate representative extreme samples and estimate rare-event probabilities beyond the observed data.

## 核心方法论

**方法：** SS-GEN自相似生成
**策略方向：** 尾部事件模拟

## 关键发现

- We introduce Self-Similar Generative Estimation (SS-GEN), a method for simulating multivariate tail events and estimating rare-event probabilities in both heavy and light-tailed settings.
- SS-GEN exploits asymptotic tail structure to decompose the tail distribution into an explicit radial component and a nonparametric angular component, reducing tail learning to a compact-domain problem that can be handled by off-the-shelf deep generative models.
- The resulting sampler generates representative extreme scenarios and supports probability estimation far beyond the observed data.

## 实践要点

- 细节需阅读原文确认

## 相关概念

- [[strategy-prototypes]] — 策略原型
- [[risk-management]] — 风险管理
- [[factor-research]] — 因子研究框架
