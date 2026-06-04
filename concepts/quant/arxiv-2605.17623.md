---
title: Where the Quantum Lives in D-Wave Hybrid Portfolio Optimization: An Operational Decomposition Audit
created: 2026-06-04
updated: 2026-06-04
type: concept
tags: [factor, strategy, 组合优化, quant]
sources: [Where the Quantum Lives in D-Wave Hybrid Portfolio Optimization: An Operational Decomposition Audit]
confidence: medium
---

# Where the Quantum Lives in D-Wave Hybrid Portfolio Optimization: An Operational Decomposition Audit

> 来源：[arXiv:2605.17623](https://arxiv.org/abs/2605.17623) | 作者：Luis Lozano

## 基本信息

| 字段 | 值 |
|------|----|
| 发表 | 2026-06-04 |
| 方法 | 未识别 |
| 策略类型 | 组合优化 |
| 资产类别 | 未特定 |

## 摘要

We audit the operational decomposition of D-Wave&#39;s hybrid quantum-classical portfolio-optimization service on cardinality-constrained mean-variance-turnover instances spanning N=10 to 640, with the constraint-native LeapHybridCQM interface, the penalty-encoded LeapHybridBQM interface, and Gurobi MIQP and simulated-annealing classical anchors. We report all three SDK timing fields (t_run, t_charge, t_QPU) and define a candidate four-metric audit protocol for hybrid quantum-classical solvers. 

## 核心方法论

**方法：** 未识别
**策略方向：** 组合优化

## 关键发现

- We audit the operational decomposition of D-Wave&#39;s hybrid quantum-classical portfolio-optimization service on cardinality-constrained mean-variance-turnover instances spanning N=10 to 640, with the constraint-native LeapHybridCQM interface, the penalty-encoded LeapHybridBQM interface, and Gurobi MIQP and simulated-annealing classical anchors.
- We report all three SDK timing fields (t_run, t_charge, t_QPU) and define a candidate four-metric audit protocol for hybrid quantum-classical solvers.

## 实践要点

- 细节需阅读原文确认

## 相关概念

- [[strategy-prototypes]] — 策略原型
- [[risk-management]] — 风险管理
- [[factor-research]] — 因子研究框架
