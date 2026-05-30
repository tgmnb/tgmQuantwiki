---
title: raw/articles/2603.13252v1.md
created: 2026-05-01
updated: 2026-05-01
type: concept
tags: [执行-做市, factor, quant, strategy]
sources: [raw/articles/2603.13252v1.md]
confidence: medium
---

# When Alpha Breaks: Two-Level Uncertainty for Safe Deployment of Cross-Sectional Stock Rankers

> 来源：[arXiv:2603.13252v1](https://arxiv.org/abs/2603.13252v1) | 作者：

## 基本信息

| 字段 | 值 |
|------|----|
| 发表 | 2026-02-24 |
| 方法 | 未识别 |
| 策略类型 | 执行/做市 |
| 资产类别 | 股票 |

## 摘要

Cross-sectional ranking models are often deployed as if point predictions were sufficient: the model outputs scores and the portfolio follows the induced ordering. Under non-stationarity, rankers can fail during regime shifts. In the AI Stock Forecaster, a LightGBM ranker performs well overall at a 20-day horizon, yet the 2024 holdout coincides with an AI thematic rally and sector rotation that breaks the signal at longer horizons and weakens 20d. This motivates treating deployment as two decisi

## 核心方法论

**方法：** 未识别
**策略方向：** 执行/做市

## 关键发现

- Cross-sectional ranking models are often deployed as if point predictions were sufficient: the model outputs scores and the portfolio follows the induced ordering.
- Under non-stationarity, rankers can fail during regime shifts.
- In the AI Stock Forecaster, a LightGBM ranker performs well overall at a 20-day horizon, yet the 2024 holdout coincides with an AI thematic rally and sector rotation that breaks the signal at longer horizons and weakens 20d.

## 实践要点

- 细节需阅读原文确认

## 相关概念

- [[strategy-prototypes]] — 策略原型
- [[risk-management]] — 风险管理
- [[factor-research]] — 因子研究框架
