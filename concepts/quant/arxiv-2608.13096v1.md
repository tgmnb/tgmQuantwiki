---
title: FlowLOB: Efficient and Controllable Limit Order Book Generation with Flow Matching
created: 2026-08-14
updated: 2026-08-14
type: concept
tags: [quant, 盘口, 生成模型, 微观结构, 执行]
sources: [FlowLOB: Efficient and Controllable Limit Order Book Generation with Flow Matching]
confidence: medium
---

# FlowLOB: Efficient and Controllable Limit Order Book Generation with Flow Matching

> 来源：[arXiv:2608.13096v1](https://arxiv.org/abs/2608.13096) | 作者：Zhuohan Wang, Andreea Bacalum, Ollie Olby等

## 基本信息

| 字段 | 值 |
|------|----|
| 发表 | 2026-08-13 |
| 方法 | Flow Matching |
| 策略类型 | 盘口模拟/执行仿真 |
| 资产类别 | 未特定 |

## 摘要

Limit order book (LOB) simulators are most useful to practitioners when they combine realistic market dynamics, computationally efficient sampling, controllable scenario generation, and the ability to generalize beyond the instruments seen during training---properties that existing agent-based and deep generative simulators provide only partially. We present \textbf{FlowLOB}, a conditional \textbf{flow}-matching generator of \textbf{LOB} trajectories, trained on multiple Hong Kong Exchange (HKEX) symbols at three sampling frequencies ($0.1$s, $1$s, $10$s) in tick-relative representation that transfers to unseen instruments. Because flow and diffusion models admit a common formulation, we train both with identical data, architecture, and budget, and sample both through the same fixed-step ODE solvers, yielding a controlled comparison of sampling efficiency and fidelity. Flow matching attains its best quality with only $10$ ODE-solver steps, whereas diffusion needs many more function evaluations to approach the same fidelity. At this efficient operating point, FlowLOB improves realism over baselines, two learned and two agent-based models, in most distributional metrics at the two finer sampling frequencies. We evaluate counterfactual controllability with a distributional test that asks whether changing a scenario condition moves the generated statistic toward the corresponding real tail regime; FlowLOB satisfies this criterion in most tested settings. Both realism and control effects transfer zero-shot on a held-out symbol. We additionally conduct ablation studies on the network architecture and the learning rate.

## 核心方法论

**方法：** Flow Matching
**策略方向：** 盘口模拟/执行仿真

## 关键发现

- Limit order book (LOB) simulators are most useful to practitioners when they combine realistic market dynamics, computationally efficient sampling, controllable scenario generation, and the ability to generalize beyond the instruments seen during training---properties that existing agent-based and deep generative simulators provide only partially.
- We present \textbf{FlowLOB}, a conditional \textbf{flow}-matching generator of \textbf{LOB} trajectories, trained on multiple Hong Kong Exchange (HKEX) symbols at three sampling frequencies ($0.
- 1$s, $1$s, $10$s) in tick-relative representation that transfers to unseen instruments.

## 实践要点

- 细节需阅读原文确认

## 相关概念

- [[strategy-prototypes]] — 策略原型
- [[risk-management]] — 风险管理
- [[factor-research]] — 因子研究框架
