---
title: Optimal Trading of Microstructure Mean Reversion
created: 2026-08-04
updated: 2026-08-04
type: concept
tags: [factor, 均值回归, 配对-套利, quant, 执行-做市, strategy]
sources: [Optimal Trading of Microstructure Mean Reversion]
confidence: medium
---

# Optimal Trading of Microstructure Mean Reversion

> 来源：[arXiv:2608.00885v1](https://arxiv.org/abs/2608.00885v1) | 作者：

## 基本信息

| 字段 | 值 |
|------|----|
| 发表 | 2026-08-01 |
| 方法 | 未识别 |
| 策略类型 | 均值回归, 配对/套利, 执行/做市 |
| 资产类别 | 未特定 |

## 摘要

At the scale of seconds the observed mid carries a stationary, mean-reverting error around a latent efficient price. We build an order book whose own flow produces that error and solve for the trading rule that maximises the long-run average profit rate net of the bid-ask spread. In a liquid large-tick asset the spread is one tick or two, and it is exactly the parity of the mid on the half-tick grid: tight at a half-integer, open at an integer. One coordinate therefore carries the problem: the g

## 核心方法论

**方法：** 未识别
**策略方向：** 均值回归, 配对/套利, 执行/做市

## 关键发现

- At the scale of seconds the observed mid carries a stationary, mean-reverting error around a latent efficient price.
- We build an order book whose own flow produces that error and solve for the trading rule that maximises the long-run average profit rate net of the bid-ask spread.
- In a liquid large-tick asset the spread is one tick or two, and it is exactly the parity of the mid on the half-tick grid: tight at a half-integer, open at an integer.

## 实践要点

- 细节需阅读原文确认

## 相关概念

- [[strategy-prototypes]] — 策略原型
- [[risk-management]] — 风险管理
- [[factor-research]] — 因子研究框架
