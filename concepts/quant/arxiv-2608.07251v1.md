---
title: Reading Copom's Tone: A Weighted LLM Framework for Hawkish-Dovish Sentiment, Forward Guidance, and Uncertainty
created: 2026-08-10
updated: 2026-08-10
type: concept
tags: [quant, 情绪, 宏观, NLP, LLM, 央行沟通]
sources: [Reading Copom's Tone: A Weighted LLM Framework for Hawkish-Dovish Sentiment, Forward Guidance, and Uncertainty]
confidence: medium
---

# Reading Copom's Tone: A Weighted LLM Framework for Hawkish-Dovish Sentiment, Forward Guidance, and Uncertainty

> 来源：[arXiv:2608.07251v1](https://arxiv.org/abs/2608.07251) | 作者：Gabriel de Macedo Santos

## 基本信息

| 字段 | 值 |
|------|----|
| 发表 | 2026-08-07 |
| 方法 | LLM/NLP情绪分析 |
| 策略类型 | 宏观情绪 |
| 资产类别 | 未特定 |

## 摘要

This paper documents an applied natural-language-processing framework for measuring the tone of Brazilian Monetary Policy Committee (Copom) statements. The project is explicitly inspired by iSent, Itaú's Central Bank sentiment classifier, particularly its sentence-level division of official communication into hawkish, dovish, neutral, and out-of-context classes. The implementation extends that idea in three directions. First, an LLM identifies short hawkish and dovish expressions and assigns each a 0-to-1 intensity weight. Second, the document index combines sentence counts with document-specific average signal intensities, producing a bounded score from -1 to 1. Third, a separate full-document layer measures forward-guidance direction, guidance explicitness, uncertainty level, and change in uncertainty. The empirical sample is restricted to communications dated August 2016 or later and contains 80 statements and 1,498 classified sentences from August 31, 2016 through August 5, 2026. Across this sample, 33.3% of sentences are hawkish, 18.0% dovish, 42.1% neutral, and 6.5% out of context. The average document score is +0.107, while the most hawkish reading is +0.570 in August 2021. The latest statement, dated August 5, 2026, scores +0.232, with eight hawkish, two dovish, and nine neutral sentences. Its structural overlay is more nuanced: guidance is directionally ambiguous but partly explicit, while uncertainty is classified as central and higher than at the prior meeting. Tone and the guidance-direction score have a contemporaneous Pearson correlation of 0.719. These are descriptive outputs, not a validated forecast of Selic decisions or DI returns. The main contribution is therefore methodological: a transparent, incremental, auditable system that separates rhetorical tone from policy guidance and uncertainty.

## 核心方法论

**方法：** LLM/NLP情绪分析
**策略方向：** 宏观情绪

## 关键发现

- This paper documents an applied natural-language-processing framework for measuring the tone of Brazilian Monetary Policy Committee (Copom) statements.
- The project is explicitly inspired by iSent, Itaú's Central Bank sentiment classifier, particularly its sentence-level division of official communication into hawkish, dovish, neutral, and out-of-context classes.
- The implementation extends that idea in three directions.

## 实践要点

- 细节需阅读原文确认

## 相关概念

- [[strategy-prototypes]] — 策略原型
- [[risk-management]] — 风险管理
- [[factor-research]] — 因子研究框架
