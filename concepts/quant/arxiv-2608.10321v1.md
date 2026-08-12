---
title: Multi-Credit Calibration via Elastically Stopped Lévy Processes
created: 2026-08-12
updated: 2026-08-12
type: concept
tags: [quant, strategy, 信用风险, 定价, Lévy过程]
sources: [Multi-Credit Calibration via Elastically Stopped Lévy Processes]
confidence: medium
---

# Multi-Credit Calibration via Elastically Stopped Lévy Processes

> 来源：[arXiv:2608.10321v1](https://arxiv.org/abs/2608.10321) | 作者：Graeme Baker, Agostino Capponi

## 基本信息

| 字段 | 值 |
|------|----|
| 发表 | 2026-08-10 |
| 方法 | 随机过程建模/校准 |
| 策略类型 | 信用/多资产定价 |
| 资产类别 | 未特定 |

## 摘要

We calibrate credit default swaps and index tranches with elastically stopped Lévy processes: each firm defaults when the running supremum of a latent, spectrally positive distress process crosses an independent exponential barrier. This yields a Cox construction with totally inaccessible default times, while retaining the interpretability and explicit formulas of a structural approach. Adding a single common compound Poisson jump factor to every firm's latent driver gives a parsimonious multi-credit model with simultaneous defaults, which is priced by an exact Wiener--Hopf Monte Carlo scheme. Its tractability rests on a single-name result we prove: a finite partial-fraction formula for the Laplace transform of the default probability under phase-type jumps. On daily CDX North American High-Yield and Investment-Grade panels, our drivers attain the lowest out-of-sample errors in a six-model field and reproduce the inverted spread curves of names heading into default, which a Lévy subordinator provably does not. At the index level, the two-parameter dependence structure closes $73\%$ to $89\%$ of the tranche pricing gap left by independent marginals with the dependence parameters frozen, and up to $95\%$ once re-marked to tranche quotes; our framework dominates a single-factor Gaussian copula and the affine intensity benchmark of Duffie--Gârleanu on both indices.

## 核心方法论

**方法：** 随机过程建模/校准
**策略方向：** 信用/多资产定价

## 关键发现

- We calibrate credit default swaps and index tranches with elastically stopped Lévy processes: each firm defaults when the running supremum of a latent, spectrally positive distress process crosses an independent exponential barrier.
- This yields a Cox construction with totally inaccessible default times, while retaining the interpretability and explicit formulas of a structural approach.
- Adding a single common compound Poisson jump factor to every firm's latent driver gives a parsimonious multi-credit model with simultaneous defaults, which is priced by an exact Wiener--Hopf Monte Carlo scheme.

## 实践要点

- 细节需阅读原文确认

## 相关概念

- [[strategy-prototypes]] — 策略原型
- [[risk-management]] — 风险管理
- [[factor-research]] — 因子研究框架
