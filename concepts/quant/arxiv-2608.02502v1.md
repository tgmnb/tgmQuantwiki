---
title: CMuon: Accelerating and Stabilizing Diffusion Transformer Training via Chunked Momentum Orthogonalization
created: 2026-08-04
updated: 2026-08-04
type: concept
tags: [factor, transformer-llm, 动量-趋势跟踪, quant, strategy]
sources: [CMuon: Accelerating and Stabilizing Diffusion Transformer Training via Chunked Momentum Orthogonalization]
confidence: medium
---

# CMuon: Accelerating and Stabilizing Diffusion Transformer Training via Chunked Momentum Orthogonalization

> 来源：[arXiv:2608.02502v1](https://arxiv.org/abs/2608.02502v1) | 作者：

## 基本信息

| 字段 | 值 |
|------|----|
| 发表 | 2026-08-03 |
| 方法 | Transformer/LLM |
| 策略类型 | 动量/趋势跟踪 |
| 资产类别 | 未特定 |

## 摘要

Diffusion Transformers (DiTs) have achieved state-of-the-art (SOTA) performance in visual generative modeling, yet their training remains computationally prohibitive. While the recently proposed Momentum Orthogonalization (Muon) optimizer offers a promising alternative to AdamW, its direct application to DiTs yields suboptimal late-stage convergence. In this paper, we identify the root cause of this bottleneck: standard DiT architectures fuse functionally distinct weights (e.g., within AdaLN and

## 核心方法论

**方法：** Transformer/LLM
**策略方向：** 动量/趋势跟踪

## 关键发现

- Diffusion Transformers (DiTs) have achieved state-of-the-art (SOTA) performance in visual generative modeling, yet their training remains computationally prohibitive.
- While the recently proposed Momentum Orthogonalization (Muon) optimizer offers a promising alternative to AdamW, its direct application to DiTs yields suboptimal late-stage convergence.
- In this paper, we identify the root cause of this bottleneck: standard DiT architectures fuse functionally distinct weights (e.

## 实践要点

- 细节需阅读原文确认

## 相关概念

- [[strategy-prototypes]] — 策略原型
- [[risk-management]] — 风险管理
- [[factor-research]] — 因子研究框架
