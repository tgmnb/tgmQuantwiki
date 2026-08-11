---
title: On a Simple Relationship Between Order Imbalance, Skew and Width in Over-The-Counter Trading
created: 2026-08-11
updated: 2026-08-11
type: concept
tags: [quant, 做市, 订单失衡, OTC, 微观结构]
sources: [On a Simple Relationship Between Order Imbalance, Skew and Width in Over-The-Counter Trading]
confidence: medium
---

# On a Simple Relationship Between Order Imbalance, Skew and Width in Over-The-Counter Trading

> 来源：[arXiv:2608.07690v1](https://arxiv.org/abs/2608.07690) | 作者：Peter Cotton

## 基本信息

| 字段 | 值 |
|------|----|
| 发表 | 2026-08-07 |
| 方法 | 马尔可夫决策/稳态分析 |
| 策略类型 | 做市/报价 |
| 资产类别 | 未特定 |

## 摘要

We consider a market maker who can only obtain and dispose of inventory by responding to a sequence of sealed-bid enquiries, and whose customers arrive with imbalanced intent: sellers more often than buyers, or the reverse. Under the assumption that the best competing response is exponentially distributed around a commonly discerned fair price, we observe a symmetry in the steady state solution that compresses the imbalanced problem onto the perfectly balanced one. Order imbalance is absorbed, exactly, by a translation of the market maker's skew, a widening of her quotes, and a multiplication of her effective cost of carry. The adjustment is simple even though the solution it adjusts is not, and it involves no free parameter beyond the observable market width. The exponential assumption is needed only locally, at the quotes actually made, and the width that enters is the locally observed one. Among the consequences: a market maker with zero inventory should still skew; skew responds to imbalance at first order whereas width responds only at second order; and the popular "constant width, linear skew" heuristic is recovered as the small-skew solution in the special case of balanced flow and quadratic holding cost.

## 核心方法论

**方法：** 马尔可夫决策/稳态分析
**策略方向：** 做市/报价

## 关键发现

- We consider a market maker who can only obtain and dispose of inventory by responding to a sequence of sealed-bid enquiries, and whose customers arrive with imbalanced intent: sellers more often than buyers, or the reverse.
- Under the assumption that the best competing response is exponentially distributed around a commonly discerned fair price, we observe a symmetry in the steady state solution that compresses the imbalanced problem onto the perfectly balanced one.
- Order imbalance is absorbed, exactly, by a translation of the market maker's skew, a widening of her quotes, and a multiplication of her effective cost of carry.

## 实践要点

- 细节需阅读原文确认

## 相关概念

- [[strategy-prototypes]] — 策略原型
- [[risk-management]] — 风险管理
- [[factor-research]] — 因子研究框架
