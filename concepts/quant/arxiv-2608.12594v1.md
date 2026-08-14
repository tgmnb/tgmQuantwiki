---
title: What Makes a Peer? Valuation-Anchored Similarity in Private Markets
created: 2026-08-14
updated: 2026-08-14
type: concept
tags: [quant, 估值, 私募, 相似度]
sources: [What Makes a Peer? Valuation-Anchored Similarity in Private Markets]
confidence: medium
---

# What Makes a Peer? Valuation-Anchored Similarity in Private Markets

> 来源：[arXiv:2608.12594v1](https://arxiv.org/abs/2608.12594) | 作者：Sebastian Frank, Jingrao Lyu, Max Jarmey等

## 基本信息

| 字段 | 值 |
|------|----|
| 发表 | 2026-08-12 |
| 方法 | 集成树监督相似度 |
| 策略类型 | 私募估值 |
| 资产类别 | 未特定 |

## 摘要

As more investors contemplate private markets and contend with limited transparency, sparse disclosures, and infrequent transactions, identifying economically meaningful peer companies for comparison is a fundamental challenge for valuation, due diligence, portfolio construction, and risk management. We propose an ensemble tree-based supervised similarity learning framework that defines company similarity through the lens of market valuation rather than static feature matching or semantic descriptions. Specifically, we train a CatBoost gradient-boosted decision tree model on observed private company valuations and derive a valuation-aware similarity metric from importance-weighted leaf-node co-occurrences across the ensemble. The similarity metric captures shared valuation drivers while accommodating nonlinear relationships, mixed data types, and pervasive missing data common in private markets. Using a global private-market universe of approximately 270,000 companies, including more than 53,000 firms with observed or derivable post-money valuations spanning multiple industries, geographies, and deal stages, we demonstrate that the proposed similarity framework improves upon traditional distance-based and text-embedding-based approaches in downstream k-nearest-neighbor valuation tasks in the evaluated industry groups, while retaining case-based explainability.

## 核心方法论

**方法：** 集成树监督相似度
**策略方向：** 私募估值

## 关键发现

- As more investors contemplate private markets and contend with limited transparency, sparse disclosures, and infrequent transactions, identifying economically meaningful peer companies for comparison is a fundamental challenge for valuation, due diligence, portfolio construction, and risk management.
- We propose an ensemble tree-based supervised similarity learning framework that defines company similarity through the lens of market valuation rather than static feature matching or semantic descriptions.
- Specifically, we train a CatBoost gradient-boosted decision tree model on observed private company valuations and derive a valuation-aware similarity metric from importance-weighted leaf-node co-occurrences across the ensemble.

## 实践要点

- 细节需阅读原文确认

## 相关概念

- [[strategy-prototypes]] — 策略原型
- [[risk-management]] — 风险管理
- [[factor-research]] — 因子研究框架
