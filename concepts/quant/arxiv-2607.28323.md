---
title: Optimal Execution with Passive Market Impact
created: 2026-08-01
updated: 2026-08-01
type: concept
tags: [quant, factor, 执行-做市, strategy]
sources: [Optimal Execution with Passive Market Impact]
confidence: medium
---

# Optimal Execution with Passive Market Impact

> 来源：[arXiv:2607.28323](https://arxiv.org/abs/2607.28323) | 作者：Alexander Barzykin, Robert Boyce, Eyal Neuman等

## 基本信息

| 字段 | 值 |
|------|----|
| 发表 | 2026-07-31 |
| 方法 | 未识别 |
| 策略类型 | 执行/做市 |
| 资产类别 | 未特定 |

## 摘要

We derive a mesoscopic model for optimal execution with limit orders that incorporates microstructural features of passive price impact. Our framework is based on two empirical observables: the approximately exponential decay of limit-order fill probabilities with distance from the midprice, and the short-term linear response of price changes to order flow imbalance. Combining these ingredients, we obtain a reduced-form passive impact rate that decays exponentially with quote distance. The model describes passive execution at a tactical level, where fills arise from a sequence of quote adjustments that balance execution probability, adverse selection, and opportunity cost. We formulate and solve an optimal liquidation problem in which the trader controls the aggressiveness of passive sell quotes. This generates a trade-off between higher fill intensity and larger accumulated impact on the one hand, and lower impact but greater non-execution risk on the other. Empirical calibration using NASDAQ equities and public FX supports the empirical foundations of the model. We also analyse extensions with heterogeneous decay rates, transient impact, and target execution schedules.

## 核心方法论

**方法：** 未识别
**策略方向：** 执行/做市

## 关键发现

- We derive a mesoscopic model for optimal execution with limit orders that incorporates microstructural features of passive price impact.
- Our framework is based on two empirical observables: the approximately exponential decay of limit-order fill probabilities with distance from the midprice, and the short-term linear response of price changes to order flow imbalance.
- Combining these ingredients, we obtain a reduced-form passive impact rate that decays exponentially with quote distance.

## 实践要点

- 细节需阅读原文确认

## 相关概念

- [[strategy-prototypes]] — 策略原型
- [[risk-management]] — 风险管理
- [[factor-research]] — 因子研究框架
