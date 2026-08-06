---
title: Portfolio Allocation under Heterogeneous Scales and Multifractality
created: 2026-08-06
updated: 2026-08-06
type: concept
tags: [quant, factor, 组合优化, 风险, 多重分形]
sources: [Portfolio Allocation under Heterogeneous Scales and Multifractality]
confidence: medium
---

# Portfolio Allocation under Heterogeneous Scales and Multifractality

> 来源：[arXiv:2608.04987v1](https://arxiv.org/abs/2608.04987) | 作者：Shinji Kakinaka, Ken Umeno

## 基本信息

| 字段 | 值 |
|------|----|
| 发表 | 2026-08-05 |
| 方法 | 多重分形分析/MFCCA |
| 策略类型 | 组合优化 |
| 资产类别 | 未特定 |

## 摘要

Cross-correlations between financial signals are neither scale-free nor amplitude-independent: they vary with the time scale over which they are measured and with the magnitude of the fluctuations that dominate the average. We exploit this structure to construct a portfolio allocation model in which the risk functional is the signed fluctuation function of multifractal cross-correlation analysis (MFCCA), indexed by a scale $s$ and a fluctuation order $q$. Unlike MFDCCA-type criteria, which rectify local detrended covariances before aggregation, MFCCA retains their sign, so that co-moving and counter-moving components contribute to risk with opposite signs; for $q=2$ the resulting quadratic form coincides with the detrended fluctuation function of the portfolio series itself, recovering the mean--variance criterion as a scale-dependent limit. Using two-component ARFIMA and Markov-switching multifractal processes, we show that prescribed multiscale and multifractal dependence is transmitted into the optimal weights, and that sign preservation contributes more to the reduction of tail risk than aggregation over fluctuation orders. Applied to financial multi-assets, the criterion lowers drawdown, Value-at-Risk, and expected shortfall relative to the mean--variance benchmark at every required return, in and out of sample, without any loss in realized portfolio return. The construction maps signed multiscale interaction structures onto resource-allocation decisions, and applies to any complex system whose components interact across heterogeneous scales with amplitude-dependent coupling.

## 核心方法论

**方法：** 多重分形分析/MFCCA
**策略方向：** 组合优化

## 关键发现

- Cross-correlations between financial signals are neither scale-free nor amplitude-independent: they vary with the time scale over which they are measured and with the magnitude of the fluctuations that dominate the average.
- We exploit this structure to construct a portfolio allocation model in which the risk functional is the signed fluctuation function of multifractal cross-correlation analysis (MFCCA), indexed by a scale $s$ and a fluctuation order $q$.
- Unlike MFDCCA-type criteria, which rectify local detrended covariances before aggregation, MFCCA retains their sign, so that co-moving and counter-moving components contribute to risk with opposite signs; for $q=2$ the resulting quadratic form coincides with the detrended fluctuation function of the portfolio series itself, recovering the mean--variance criterion as a scale-dependent limit.

## 实践要点

- 细节需阅读原文确认

## 相关概念

- [[strategy-prototypes]] — 策略原型
- [[risk-management]] — 风险管理
- [[factor-research]] — 因子研究框架
