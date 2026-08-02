---
title: FinanceHarness: Autonomous Financial Deep Research Framework
created: 2026-08-01
updated: 2026-08-01
type: concept
tags: [集成方法, 执行-做市, quant, factor, strategy]
sources: [FinanceHarness: Autonomous Financial Deep Research Framework]
confidence: medium
---

# FinanceHarness: Autonomous Financial Deep Research Framework

> 来源：[arXiv:2607.27853](https://arxiv.org/abs/2607.27853) | 作者：Yijia Xiao, Rujun Han, Yanfei Chen等

## 基本信息

| 字段 | 值 |
|------|----|
| 发表 | 2026-07-31 |
| 方法 | 集成方法 |
| 策略类型 | 执行/做市 |
| 资产类别 | 未特定 |

## 摘要

Powered by advances in LLMs and autonomous agents, deep research has become one of the most widely adopted agentic products. However, most deep research systems write general-purpose reports, which are inadequate for financial deep research. Financial research demands specialized knowledge to analyze historical patterns and forecast upcoming events. Automating financial deep research therefore requires both a layered harness to drive the research agent and a verifiable, point-in-time benchmark that prevents leakage of future information. We present FinanceHarness, a harness that runs finance-oriented tools and practitioner-guided workflows, automating financial deep research end to end: environment and data construction, the agent execution loop, and reward modeling. We further propose FinanceGym, comprising thesis-driven research questions and rubrics that combine pre-cutoff and post-cutoff criteria. Professional expert validation yields an 82% pass rate. Even leading LLMs and agents score below 40% on the rubrics, showing that FinanceGym is challenging and leaves substantial headroom. With the same open-weight backbone, FinanceHarness improves the overall rubric score from 25.3% to 32.4%. FinanceHarness is available at this https URL .

## 核心方法论

**方法：** 集成方法
**策略方向：** 执行/做市

## 关键发现

- Powered by advances in LLMs and autonomous agents, deep research has become one of the most widely adopted agentic products.
- However, most deep research systems write general-purpose reports, which are inadequate for financial deep research.
- Financial research demands specialized knowledge to analyze historical patterns and forecast upcoming events.

## 实践要点

- 基于历史回测，警惕过拟合风险

## 相关概念

- [[strategy-prototypes]] — 策略原型
- [[risk-management]] — 风险管理
- [[factor-research]] — 因子研究框架
