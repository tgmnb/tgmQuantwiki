---
title: raw/articles/2604.22625v1.md
created: 2026-05-01
updated: 2026-05-01
type: concept
tags: [执行-做市, 配对-套利, quant, strategy, 组合优化, factor]
sources: [raw/articles/2604.22625v1.md]
confidence: medium
---

# FlashFolio: A GPU-Accelerated Solver for Portfolio Optimization

> 来源：[arXiv:2604.22625v1](https://arxiv.org/abs/2604.22625v1) | 作者：

## 基本信息

| 字段 | 值 |
|------|----|
| 发表 | 2026-04-24 |
| 方法 | 未识别 |
| 策略类型 | 配对/套利, 组合优化, 执行/做市 |
| 资产类别 | 未特定 |

## 摘要

We present FlashFolio, a GPU-accelerated solver for single-period and multi-period portfolio optimization with factor-based risk modeling, bid-offer spread costs, and nonlinear market impact. These models are widely used in portfolio construction and optimal execution, but become computationally challenging at large scale, especially in the multi-period setting. We benchmark FlashFolio against MOSEK on instances constructed from realistic market inputs. FlashFolio delivers consistent runtime imp

## 核心方法论

**方法：** 未识别
**策略方向：** 配对/套利, 组合优化, 执行/做市

## 关键发现

- We present FlashFolio, a GPU-accelerated solver for single-period and multi-period portfolio optimization with factor-based risk modeling, bid-offer spread costs, and nonlinear market impact.
- These models are widely used in portfolio construction and optimal execution, but become computationally challenging at large scale, especially in the multi-period setting.
- We benchmark FlashFolio against MOSEK on instances constructed from realistic market inputs.

## 实践要点

- 细节需阅读原文确认

## 相关概念

- [[strategy-prototypes]] — 策略原型
- [[risk-management]] — 风险管理
- [[factor-research]] — 因子研究框架
