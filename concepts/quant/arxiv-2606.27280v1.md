---
title: Exact and Deterministic Patch Descriptor Retrieval via Hierarchical Normalization
created: 2026-08-01
updated: 2026-08-01
type: concept
tags: [factor, strategy, quant, 执行-做市]
sources: [Exact and Deterministic Patch Descriptor Retrieval via Hierarchical Normalization]
confidence: medium
---

# Exact and Deterministic Patch Descriptor Retrieval via Hierarchical Normalization

> 来源：[arXiv:2606.27280v1](https://arxiv.org/abs/2606.27280v1) | 作者：

## 基本信息

| 字段 | 值 |
|------|----|
| 发表 | 2026-06-25 |
| 方法 | 未识别 |
| 策略类型 | 执行/做市 |
| 资产类别 | 未特定 |

## 摘要

We present a patch descriptor retrieval method that returns the exact nearest neighbour -- provably identical to exhaustive full-vector search -- while evaluating only a small fraction of the database, and does so deterministically: the same (database, query) pair always produces the same result, independent of run order, thread count, or hardware. This contrasts with approximate nearest-neighbour (ANN) approaches such as HNSW and IVF-PQ, which trade exactness for speed and may return different 

## 核心方法论

**方法：** 未识别
**策略方向：** 执行/做市

## 关键发现

- We present a patch descriptor retrieval method that returns the exact nearest neighbour -- provably identical to exhaustive full-vector search -- while evaluating only a small fraction of the database, and does so deterministically: the same (database, query) pair always produces the same result, independent of run order, thread count, or hardware.
- This contrasts with approximate nearest-neighbour (ANN) approaches such as HNSW and IVF-PQ, which trade exactness for speed and may return different.

## 实践要点

- 细节需阅读原文确认

## 相关概念

- [[strategy-prototypes]] — 策略原型
- [[risk-management]] — 风险管理
- [[factor-research]] — 因子研究框架
