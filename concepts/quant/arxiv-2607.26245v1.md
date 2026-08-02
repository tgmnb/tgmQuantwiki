---
title: OpenMarket: A Synchronized Polymarket-Binance Dataset for High-Frequency Prediction-Market Research
created: 2026-08-01
updated: 2026-08-01
type: concept
tags: [factor, strategy, quant, 执行-做市]
sources: [OpenMarket: A Synchronized Polymarket-Binance Dataset for High-Frequency Prediction-Market Research]
confidence: medium
---

# OpenMarket: A Synchronized Polymarket-Binance Dataset for High-Frequency Prediction-Market Research

> 来源：[arXiv:2607.26245v1](https://arxiv.org/abs/2607.26245v1) | 作者：

## 基本信息

| 字段 | 值 |
|------|----|
| 发表 | 2026-07-28 |
| 方法 | 未识别 |
| 策略类型 | 执行/做市 |
| 资产类别 | 未特定 |

## 摘要

OpenMarket began as an attempt to trade Polymarket's BTC 15-minute binary markets against Binance BTC/USDT order flow. The attempt did not produce a tradable edge: out-of-sample, a walk-forward logistic model over 43 microstructure features does not beat, and slightly underperforms, the probability already implied by Polymarket's own order book, and simulated trading nets -0.116 normalized payoff units per attempted trade under stated fee and slippage assumptions. We release the synchronized cor

## 核心方法论

**方法：** 未识别
**策略方向：** 执行/做市

## 关键发现

- OpenMarket began as an attempt to trade Polymarket's BTC 15-minute binary markets against Binance BTC/USDT order flow.
- The attempt did not produce a tradable edge: out-of-sample, a walk-forward logistic model over 43 microstructure features does not beat, and slightly underperforms, the probability already implied by Polymarket's own order book, and simulated trading nets -0.
- 116 normalized payoff units per attempted trade under stated fee and slippage assumptions.

## 实践要点

- 考虑了交易成本/滑点

## 相关概念

- [[strategy-prototypes]] — 策略原型
- [[risk-management]] — 风险管理
- [[factor-research]] — 因子研究框架
