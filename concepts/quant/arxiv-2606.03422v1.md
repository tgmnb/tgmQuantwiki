---
title: HonestAffinity: Leak-Aware Evaluation of Protein and Pocket Priors for Binding Affinity Prediction
created: 2026-06-05
updated: 2026-06-05
type: concept
tags: [factor, quant, 神经网络-dl, strategy]
sources: [HonestAffinity: Leak-Aware Evaluation of Protein and Pocket Priors for Binding Affinity Prediction]
confidence: medium
---

# HonestAffinity: Leak-Aware Evaluation of Protein and Pocket Priors for Binding Affinity Prediction

> 来源：[arXiv:2606.03422v1](https://arxiv.org/abs/2606.03422v1) | 作者：

## 基本信息

| 字段 | 值 |
|------|----|
| 发表 | 2026-06-02 |
| 方法 | 神经网络/DL |
| 策略类型 | 未识别 |
| 资产类别 | 未特定 |

## 摘要

Sequence-based deep learning offers a scalable alternative to structure-based scoring for protein-ligand binding affinity prediction. However, progress is hard to interpret when architectural priors are evaluated on canonical PDBbind-style splits that leak similarity classes across folds. We present HonestAffinity, a compact 1D-input predictor to isolate two priors under a leak-aware protocol: frozen ESM-2 (650M) protein embeddings and a learned binary pocket-position marker. We evaluate a multi

## 核心方法论

**方法：** 神经网络/DL
**策略方向：** 未识别

## 关键发现

- Sequence-based deep learning offers a scalable alternative to structure-based scoring for protein-ligand binding affinity prediction.
- However, progress is hard to interpret when architectural priors are evaluated on canonical PDBbind-style splits that leak similarity classes across folds.
- We present HonestAffinity, a compact 1D-input predictor to isolate two priors under a leak-aware protocol: frozen ESM-2 (650M) protein embeddings and a learned binary pocket-position marker.

## 实践要点

- 细节需阅读原文确认

## 相关概念

- [[strategy-prototypes]] — 策略原型
- [[risk-management]] — 风险管理
- [[factor-research]] — 因子研究框架
