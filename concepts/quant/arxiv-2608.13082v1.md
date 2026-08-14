---
title: LOB-ID: Evaluating Synthetic Market Data by Inception Distances
created: 2026-08-14
updated: 2026-08-14
type: concept
tags: [quant, 盘口, 生成模型, 评估]
sources: [LOB-ID: Evaluating Synthetic Market Data by Inception Distances]
confidence: medium
---

# LOB-ID: Evaluating Synthetic Market Data by Inception Distances

> 来源：[arXiv:2608.13082v1](https://arxiv.org/abs/2608.13082) | 作者：Andreea Bacalum, Zhuohan Wang, Ollie Olby等

## 基本信息

| 字段 | 值 |
|------|----|
| 发表 | 2026-08-13 |
| 方法 | Inception距离嵌入 |
| 策略类型 | 合成数据评估 |
| 资产类别 | 未特定 |

## 摘要

Generative models of limit orderbook (LOB) data have advanced rapidly, but their evaluation often focuses on stylised facts and selected market statistics. These measures provide useful diagnostics but may not capture the joint temporal and cross-level structure of order-book trajectories. We introduce LOB-ID, an embedding-based framework that adapts the Fréchet Inception Distance (FID) and Monge Inception Distance (MIND) to LOB data. To obtain domain-specific embeddings, we train the DeepLOB architecture on four months of Level-2 order-book data for five equities. We show that LOB-ID is stable across time, instruments, and embedding checkpoints, and rises monotonically under controlled distortions. We then construct a moment-matching attack against FID and a deep-book perturbation that evades statistic-based evaluation. MIND remains substantially more sensitive to both distortions. Finally, we score five generative LOB models, spanning stochastic baselines and deep learning approaches, and find that LOB-ID ranks them in line with the joint temporal and cross-level structure each captures by construction.

## 核心方法论

**方法：** Inception距离嵌入
**策略方向：** 合成数据评估

## 关键发现

- Generative models of limit orderbook (LOB) data have advanced rapidly, but their evaluation often focuses on stylised facts and selected market statistics.
- These measures provide useful diagnostics but may not capture the joint temporal and cross-level structure of order-book trajectories.
- We introduce LOB-ID, an embedding-based framework that adapts the Fréchet Inception Distance (FID) and Monge Inception Distance (MIND) to LOB data.

## 实践要点

- 细节需阅读原文确认

## 相关概念

- [[strategy-prototypes]] — 策略原型
- [[risk-management]] — 风险管理
- [[factor-research]] — 因子研究框架
