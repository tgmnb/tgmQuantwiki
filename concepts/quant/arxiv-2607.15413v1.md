---
title: FSZ: Breaking the Prediction-Throughput Trade-off in GPU Lossy Compression
created: 2026-07-20
updated: 2026-07-20
type: concept
tags: [strategy, 执行-做市, quant, factor]
sources: [FSZ: Breaking the Prediction-Throughput Trade-off in GPU Lossy Compression]
confidence: medium
---

# FSZ: Breaking the Prediction-Throughput Trade-off in GPU Lossy Compression

> 来源：[arXiv:2607.15413v1](https://arxiv.org/abs/2607.15413v1) | 作者：

## 基本信息

| 字段 | 值 |
|------|----|
| 发表 | 2026-07-16 |
| 方法 | 未识别 |
| 策略类型 | 执行/做市 |
| 资产类别 | 未特定 |

## 摘要

Existing fast GPU error-bounded lossy compressors have achieved high throughput through pure-GPU single-kernel designs, but their compression ratios remain limited because they typically apply a fixed first-order predictor on independent blocks. We propose FSZ, a GPU error-bounded lossy compressor that redesigns the prediction stage with three mutually reinforcing algorithmic innovations to achieve both higher compression ratios and higher throughput within a single CUDA kernel: (1) cross-block 

## 核心方法论

**方法：** 未识别
**策略方向：** 执行/做市

## 关键发现

- Existing fast GPU error-bounded lossy compressors have achieved high throughput through pure-GPU single-kernel designs, but their compression ratios remain limited because they typically apply a fixed first-order predictor on independent blocks.
- We propose FSZ, a GPU error-bounded lossy compressor that redesigns the prediction stage with three mutually reinforcing algorithmic innovations to achieve both higher compression ratios and higher throughput within a single CUDA kernel: (1) cross-block.

## 实践要点

- 细节需阅读原文确认

## 相关概念

- [[strategy-prototypes]] — 策略原型
- [[risk-management]] — 风险管理
- [[factor-research]] — 因子研究框架
