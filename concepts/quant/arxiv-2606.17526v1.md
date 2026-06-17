---
title: MGUP: A Momentum-Gradient Alignment Update Policy for Stochastic Optimization
created: 2026-06-17
updated: 2026-06-17
type: concept
tags: [强化学习-rl, factor, 动量-趋势跟踪, quant, strategy]
sources: [MGUP: A Momentum-Gradient Alignment Update Policy for Stochastic Optimization]
confidence: medium
---

# MGUP: A Momentum-Gradient Alignment Update Policy for Stochastic Optimization

> 来源：[arXiv:2606.17526v1](https://arxiv.org/abs/2606.17526v1) | 作者：

## 基本信息

| 字段 | 值 |
|------|----|
| 发表 | 2026-06-16 |
| 方法 | 强化学习/RL |
| 策略类型 | 动量/趋势跟踪 |
| 资产类别 | 未特定 |

## 摘要

Efficient optimization is essential for training large language models. Although intra-layer selective updates have been explored, a general mechanism that enables fine-grained control while ensuring convergence guarantees is still lacking. To bridge this gap, we propose \textbf{MGUP}, a novel mechanism for selective updates. \textbf{MGUP} augments standard momentum-based optimizers by applying larger step-sizes to a selected fixed proportion of parameters in each iteration, while applying small

## 核心方法论

**方法：** 强化学习/RL
**策略方向：** 动量/趋势跟踪

## 关键发现

- Efficient optimization is essential for training large language models.
- Although intra-layer selective updates have been explored, a general mechanism that enables fine-grained control while ensuring convergence guarantees is still lacking.
- To bridge this gap, we propose \textbf{MGUP}, a novel mechanism for selective updates.

## 实践要点

- 细节需阅读原文确认

## 相关概念

- [[strategy-prototypes]] — 策略原型
- [[risk-management]] — 风险管理
- [[factor-research]] — 因子研究框架
