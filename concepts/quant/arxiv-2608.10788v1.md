---
title: The Triadic Stress Index in Financial Markets
created: 2026-08-12
updated: 2026-08-12
type: concept
tags: [quant, factor, 市场风险, 压力指数]
sources: [The Triadic Stress Index in Financial Markets]
confidence: medium
---

# The Triadic Stress Index in Financial Markets

> 来源：[arXiv:2608.10788v1](https://arxiv.org/abs/2608.10788) | 作者：Alberto Acedo

## 基本信息

| 字段 | 值 |
|------|----|
| 发表 | 2026-08-11 |
| 方法 | 指数构建 |
| 策略类型 | 风险监控 |
| 资产类别 | 期货/商品, 加密货币 |

## 摘要

The Triadic Stress Index (TSI) takes a network index whose four factors were first observed in soil microbiome co-occurrence networks and applies it, without alteration, to the correlation network of financial assets.   We test it on five markets spanning 2006-2026 (equities including banking crises and the AI sector, cryptocurrencies, commodities, foreign exchange and sovereign debt), against three independent definitions of a crisis episode, at a fixed alarm budget, out of sample, with block-bootstrap intervals and a Holm correction across the family of tests. The benchmarks are the Absorption Ratio, the industry standard used by MSCI and central banks; the effective rank and the Vendi score, the sharpest spectral measures available; Ollivier-Ricci curvature; and the global and local balance indices of signed correlation networks.   Three comparisons favour the index. It carries a per-node decomposition, diag(A^3), naming which asset is carrying the concentration with no parameter to select, and scores 0.97-0.99 against 0.33-0.84 for the only published per-node alternative, whereas spectral attribution must first choose how many components to read and collapses under a standard but wrong choice. Its alarms are the cleanest of anything tested, 4.0% of them with no matching episode against 14.7% for the effective rank and roughly 59% for the Absorption Ratio. And it beats the Absorption Ratio on detection by 0.273 in F1 out of sample, p<0.0005.   The remaining comparisons are ties. Against the effective rank and the Vendi score the index ties in every scheme and both samples, and the margin over the Absorption Ratio narrows under the strictest labelling. On real matrices the far simpler node degree reproduces the attribution. A lead-lag analysis puts the peak cross-correlation at zero lag: this is a coincident state index, not a forecast.

## 核心方法论

**方法：** 指数构建
**策略方向：** 风险监控

## 关键发现

- The Triadic Stress Index (TSI) takes a network index whose four factors were first observed in soil microbiome co-occurrence networks and applies it, without alteration, to the correlation network of financial assets.
- We test it on five markets spanning 2006-2026 (equities including banking crises and the AI sector, cryptocurrencies, commodities, foreign exchange and sovereign debt), against three independent definitions of a crisis episode, at a fixed alarm budget, out of sample, with block-bootstrap intervals and a Holm correction across the family of tests.
- The benchmarks are the Absorption Ratio, the industry standard used by MSCI and central banks; the effective rank and the Vendi score, the sharpest spectral measures available; Ollivier-Ricci curvature; and the global and local balance indices of signed correlation networks.

## 实践要点

- 细节需阅读原文确认

## 相关概念

- [[strategy-prototypes]] — 策略原型
- [[risk-management]] — 风险管理
- [[factor-research]] — 因子研究框架
