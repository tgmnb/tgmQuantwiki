---
title: Are Three Matrices All You Need To Beat the Market? Observable Matrix Dynamics for Portfolio Optimization
created: 2026-08-01
updated: 2026-08-01
type: concept
tags: [动量-趋势跟踪, 组合优化, quant, factor, strategy]
sources: [Are Three Matrices All You Need To Beat the Market? Observable Matrix Dynamics for Portfolio Optimization]
confidence: medium
---

# Are Three Matrices All You Need To Beat the Market? Observable Matrix Dynamics for Portfolio Optimization

> 来源：[arXiv:2607.27461](https://arxiv.org/abs/2607.27461) | 作者：Igor Halperin

## 基本信息

| 字段 | 值 |
|------|----|
| 发表 | 2026-07-31 |
| 方法 | 未识别 |
| 策略类型 | 动量/趋势跟踪, 组合优化 |
| 资产类别 | 期权/波动率 |

## 摘要

We present a simple framework for dynamic portfolio management that uses nothing but daily prices, trading volumes, and market capitalizations. Its state is three fixed-size matrices built from the price history: the distance matrix of the return correlations and the transition matrices of two Markov chains that rank the S\&amp;P 500 names monthly by trailing return and by trailing volatility. These three matrices rest on the price history alone, the same information Markowitz mean-variance optimization draws on, but they replace its expected-return vector and covariance matrix. Our method requires no matrix inversion, works on outlier-robust cross-sectional ranks, and is dynamic rather than single-period. Empirically the volatility rank is forecastable one step ahead while the return rank stays close to unforecastable. A portfolio built on the forecasts, a market-neutral momentum long-short blended with an opportunistic long-only sleeve, beats the market on two non-overlapping out-of-sample test sets, January 2022 to December 2024 and January 2025 to July 2026, at Sharpes of $1.06$ and $1.32$ against the market&#39;s $0.78$ and $1.14$, respectively, net of a five-basis-point trading cost and marked to market daily. It also outperforms the classical minimum-variance and maximum-diversification portfolios. Diversifying the long sleeve by residual distance adds a further edge on both periods, lifting the Sharpe to $1.08$ and $1.44$ and the annualized return from $18\%$ to $20\%$ and from $44\%$ to $56\%$, respectively. A convex information-leader overlay separately insures the market-neutral sleeve, buying convexity and a shallower drawdown at a small cost in return, the Sharpe unchanged.

## 核心方法论

**方法：** 未识别
**策略方向：** 动量/趋势跟踪, 组合优化

## 关键发现

- We present a simple framework for dynamic portfolio management that uses nothing but daily prices, trading volumes, and market capitalizations.
- Its state is three fixed-size matrices built from the price history: the distance matrix of the return correlations and the transition matrices of two Markov chains that rank the S\&amp;P 500 names monthly by trailing return and by trailing volatility.
- These three matrices rest on the price history alone, the same information Markowitz mean-variance optimization draws on, but they replace its expected-return vector and covariance matrix.

## 实践要点

- 策略报告了超额收益（需核实样本外表现）
- 使用了风险调整收益评估（Sharpe等）

## 相关概念

- [[strategy-prototypes]] — 策略原型
- [[risk-management]] — 风险管理
- [[factor-research]] — 因子研究框架
