---
title: Can Large Language Models Execute Parent Orders?
created: 2026-08-01
updated: 2026-08-01
type: concept
tags: [quant, factor, 执行-做市, strategy]
sources: [Can Large Language Models Execute Parent Orders?]
confidence: medium
---

# Can Large Language Models Execute Parent Orders?

> 来源：[arXiv:2607.28410](https://arxiv.org/abs/2607.28410) | 作者：Zane Shen, Xinli Xu, Guangyi Zhang等

## 基本信息

| 字段 | 值 |
|------|----|
| 发表 | 2026-07-31 |
| 方法 | 未识别 |
| 策略类型 | 执行/做市 |
| 资产类别 | 股票 |

## 摘要

Parent-order execution is a core problem in algorithmic trading, where the goal is to split a large order into smaller orders while reducing execution costs. Existing approaches either rely on pre-specified market assumptions that may not hold in practice, or require task-specific training that limits adaptability to new settings. To overcome these limitations, we present the first systematic study of large language models (LLMs) for parent-order execution. This extends the use of LLMs in finance from what to trade to how to execute. We propose PACE (Plan-Ahead Controlled Execution), a hierarchical framework that decomposes parent-order execution into long-horizon planning and short-horizon execution, requiring neither explicit market assumptions nor task-specific training. Experiments on Shenzhen Stock Exchange Level-1 data show that PACE outperforms TWAP, Almgren-Chriss, and learning-based baselines, exceeding the strongest baseline by 0.65 bps. Behavioral analysis reveals that LLMs make execution decisions differently from human investors: higher model confidence predicts better performance rather than worse returns, and the model trades earlier rather than procrastinating toward the deadline. These findings suggest that LLMs can complement human traders in execution decisions.

## 核心方法论

**方法：** 未识别
**策略方向：** 执行/做市

## 关键发现

- Parent-order execution is a core problem in algorithmic trading, where the goal is to split a large order into smaller orders while reducing execution costs.
- Existing approaches either rely on pre-specified market assumptions that may not hold in practice, or require task-specific training that limits adaptability to new settings.
- To overcome these limitations, we present the first systematic study of large language models (LLMs) for parent-order execution.

## 实践要点

- 策略报告了超额收益（需核实样本外表现）

## 相关概念

- [[strategy-prototypes]] — 策略原型
- [[risk-management]] — 风险管理
- [[factor-research]] — 因子研究框架
