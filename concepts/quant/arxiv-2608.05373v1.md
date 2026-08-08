---
title: Velocity- and Regime-Aware Detection of Intraday Options Market Manipulation, with Explainable Attribution
created: 2026-08-08
updated: 2026-08-08
type: concept
tags: [quant, strategy, 期权, 市场操纵检测, 监管, 微观结构]
sources: [Velocity- and Regime-Aware Detection of Intraday Options Market Manipulation, with Explainable Attribution]
confidence: medium
---

# Velocity- and Regime-Aware Detection of Intraday Options Market Manipulation, with Explainable Attribution

> 来源：[arXiv:2608.05373v1](https://arxiv.org/abs/2608.05373) | 作者：Alex Chen, Maria Hybinette

## 基本信息

| 字段 | 值 |
|------|----|
| 发表 | 2026-08-05 |
| 方法 | 未识别 |
| 策略类型 | 未识别 |
| 资产类别 | 期权/波动率 |

## 摘要

Intraday market manipulation is hard to detect because its footprint is brief, buried in millions of quotes, and statistically similar to ordinary volatility. Detectors reach high recall only by flagging so many other days that measured precision collapses, producing alerts no regulator can act on. We show that this manipulation leaves a distinctive dynamic signature: a pump-and-crash pattern visible in the velocity of market state, rather than its level.   We build a minute-level detection pipeline, strictly partitioned in time, based on smoothed state velocity: option-Delta velocity for index options and price velocity for equities. We explain every alert with SHAP attribution. We hold the test period strictly out-of-sample and fix all thresholds before evaluation. On the locked Indian BANKNIFTY index-options test, the plain autoencoder recovers 10 of 10 regulator-identified manipulation days.   Conditioning detection on market regimes inferred by a hidden Markov model yields an instructive negative result. The regimes are descriptively distinct, but using them trades recall for precision. Under the closed-world assumption that unlabeled days are normal, precision remains near 25%.   The same dynamic appears in thinly traded U.S. equities (SEC v. Patel). The shape of the signature survives the transfer; its velocity magnitude does not. A pump-reversal shape score ranks the complaint's alleged manipulation days with AUC 0.91 (ARQQ) and 0.81 (ACY). On the ARQQ worked example, the score peaks inside the complaint's documented minute window.   Finally, exact SHAP attribution over every alert shows that unconfirmed alerts share the regulator-identified days' attribution profile (cosine similarity 0.99). The precision ceiling is consistent with incomplete enforcement labels rather than detector failure. What transfers across markets and instrument types is the dynamic signature itself.

## 核心方法论

**方法：** 未识别
**策略方向：** 未识别

## 关键发现

- Intraday market manipulation is hard to detect because its footprint is brief, buried in millions of quotes, and statistically similar to ordinary volatility.
- Detectors reach high recall only by flagging so many other days that measured precision collapses, producing alerts no regulator can act on.
- We show that this manipulation leaves a distinctive dynamic signature: a pump-and-crash pattern visible in the velocity of market state, rather than its level.

## 实践要点

- 细节需阅读原文确认

## 相关概念

- [[strategy-prototypes]] — 策略原型
- [[risk-management]] — 风险管理
- [[factor-research]] — 因子研究框架
