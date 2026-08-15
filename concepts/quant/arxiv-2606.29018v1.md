---
title: Liquidity-Based Audit of Algorithmic Trading Strategies
created: 2026-08-15
updated: 2026-08-15
type: concept
tags: [quant, market-microstructure, liquidity, execution, algo-trading]
sources: [Liquidity-Based Audit of Algorithmic Trading Strategies]
confidence: medium
---

# Liquidity-Based Audit of Algorithmic Trading Strategies

> 来源：[arXiv:2606.29018v1](https://arxiv.org/abs/2606.29018) | 作者：Irene Aldridge

## 基本信息

| 字段 | 值 |
|------|----|
| 发表 | 2026-06-27 |
| 方法 | 多期遗憾分解 (multi-period regret decomposition); Kyle (1985) 知情交易者/做市商二分法; Roll (1984) 隐含价差 |
| 策略类型 | 流动性审计/算法交易策略分类 |
| 资产类别 | 股票 |

## 摘要

We show that net demand for liquidity by algo strategies is identifiable from its trade and price history alone, with no knowledge of its signal or optimization problem. An exact multi-period regret decomposition implies that the sign of this statistic classifies a linear strategy as a net liquidity consumer or provider, recovering the Kyle (1985) informed-trader/market-maker dichotomy from observables alone. Under an AR(1) cost process, the same statistic equals the product of strategy size and the squared Roll (1984) implied spread, making the correction a direct proxy for prevailing illiquidity. Extending to endogenous price impact and aggregating across N correlated strategies yields a liquidity-balance condition whose violation produces welfare loss scaling as N squared, a closed-form fire-sale externality. We calibrate to CRSP equity data (2016-2025), tracking implied spreads through the COVID-19 and 2022 rate-shock episodes, with an estimator computable in O(Tnd) time.

## 核心方法论

**方法：** 多期遗憾分解 (multi-period regret decomposition); Kyle (1985) 知情交易者/做市商二分法; Roll (1984) 隐含价差
**策略方向：** 流动性审计/算法交易策略分类

## 关键发现

- We show that net demand for liquidity by algo strategies is identifiable from its trade and price history alone, with no knowledge of its signal or optimization problem.
- An exact multi-period regret decomposition implies that the sign of this statistic classifies a linear strategy as a net liquidity consumer or provider, recovering the Kyle (1985) informed-trader/market-maker dichotomy from observables alone.
- Under an AR(1) cost process, the same statistic equals the product of strategy size and the squared Roll (1984) implied spread, making the correction a direct proxy for prevailing illiquidity.

## 实践要点

- 细节需阅读原文确认

## 相关概念

- [[strategy-prototypes]] — 策略原型
- [[risk-management]] — 风险管理
- [[factor-research]] — 因子研究框架
