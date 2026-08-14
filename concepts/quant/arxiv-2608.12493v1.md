---
title: Beyond the Skew-Stickiness Ratio: Transport Geometry of Spot-Driven Variance Surface Dynamics
created: 2026-08-14
updated: 2026-08-14
type: concept
tags: [quant, factor, 期权, 波动率曲面, 策略]
sources: [Beyond the Skew-Stickiness Ratio: Transport Geometry of Spot-Driven Variance Surface Dynamics]
confidence: medium
---

# Beyond the Skew-Stickiness Ratio: Transport Geometry of Spot-Driven Variance Surface Dynamics

> 来源：[arXiv:2608.12493v1](https://arxiv.org/abs/2608.12493) | 作者：Charlie Che, Pradeepta Das

## 基本信息

| 字段 | 值 |
|------|----|
| 发表 | 2026-08-12 |
| 方法 | 传输几何/运输流 |
| 策略类型 | 期权波动率建模 |
| 资产类别 | 期权/波动率 |

## 摘要

We develop a geometric theory of arbitrage-free implied variance surface dynamics. Smile dynamics are formulated as transport flows on the admissible class of static-arbitrage-free surfaces: spot movements generate transport vector fields, and the transport velocity field v(k) unifies all classical stickiness regimes. The skew-stickiness ratio (SSR) is the zeroth-order transport coefficient; higher-order coefficients govern ATM skew, curvature, and higher smile derivatives. A local jet transport corollary extends the theory to arbitrary log-moneyness and identifies v(k) nonparametrically from market data. Under explicit regularity conditions, the flow preserves butterfly and calendar arbitrage-freeness locally. Sticky-strike, sticky-delta, SSR, local volatility, and rough volatility all arise as special cases within this framework.   Empirically, we apply a sequential forward-substitution estimator to five years of SPX implied-volatility data across seven tenors from one month to two years. Three findings emerge. First, SPX exhibits super-skew behavior: the SSR coefficient declines monotonically from 1.44 at one month toward 1.01 at two years. Second, self-similar transport is rejected at all tenors; the skew-transport coefficient changes sign between the six- and nine-month tenors. Third, the velocity profile varies significantly with moneyness at intermediate tenors and evolves from U-shaped at short maturities to monotonically decreasing at long maturities. Out-of-sample, the full three-parameter model outperforms SSR on curvature dynamics by 17-21% at medium tenors.

## 核心方法论

**方法：** 传输几何/运输流
**策略方向：** 期权波动率建模

## 关键发现

- We develop a geometric theory of arbitrage-free implied variance surface dynamics.
- Smile dynamics are formulated as transport flows on the admissible class of static-arbitrage-free surfaces: spot movements generate transport vector fields, and the transport velocity field v(k) unifies all classical stickiness regimes.
- The skew-stickiness ratio (SSR) is the zeroth-order transport coefficient; higher-order coefficients govern ATM skew, curvature, and higher smile derivatives.

## 实践要点

- 策略报告了超额收益（需核实样本外表现）

## 相关概念

- [[strategy-prototypes]] — 策略原型
- [[risk-management]] — 风险管理
- [[factor-research]] — 因子研究框架
