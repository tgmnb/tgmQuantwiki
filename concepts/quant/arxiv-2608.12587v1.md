---
title: DYSANOS Generative Dynamic Smooth Arbitrage-free Non-parametric Option Surfaces
created: 2026-08-14
updated: 2026-08-14
type: concept
tags: [quant, 期权, 生成模型, 无套利]
sources: [DYSANOS Generative Dynamic Smooth Arbitrage-free Non-parametric Option Surfaces]
confidence: medium
---

# DYSANOS Generative Dynamic Smooth Arbitrage-free Non-parametric Option Surfaces

> 来源：[arXiv:2608.12587v1](https://arxiv.org/abs/2608.12587) | 作者：Hans Buehler, Blanka Horvath, Anastasis Kratsios

## 基本信息

| 字段 | 值 |
|------|----|
| 发表 | 2026-08-12 |
| 方法 | 生成模型/SANOS |
| 策略类型 | 期权曲面生成 |
| 资产类别 | 期权/波动率 |

## 摘要

This article presents with DYSANOS the first generative market model for smooth SANOS option surfaces for all strikes and expiries which are free of static arbitrage. Our model is designed to generate entire paths of daily spot and option prices for years in the future.   We present a robust and useful if somewhat simplistic baseline hidden state generative model in the form of an AR(1) model. We discuss model setup, data pipeline, and training and investigate numerical resence of dynamic arbitrage. We illustrate model performance on Option Metrics' IvyDB S\&P Index data from 2020 to~2025 and compare it to a pure implied-vol PCA model.

## 核心方法论

**方法：** 生成模型/SANOS
**策略方向：** 期权曲面生成

## 关键发现

- This article presents with DYSANOS the first generative market model for smooth SANOS option surfaces for all strikes and expiries which are free of static arbitrage.
- Our model is designed to generate entire paths of daily spot and option prices for years in the future.
- We present a robust and useful if somewhat simplistic baseline hidden state generative model in the form of an AR(1) model.

## 实践要点

- 细节需阅读原文确认

## 相关概念

- [[strategy-prototypes]] — 策略原型
- [[risk-management]] — 风险管理
- [[factor-research]] — 因子研究框架
