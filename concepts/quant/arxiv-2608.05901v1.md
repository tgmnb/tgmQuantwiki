---
title: From Value Bounds to Policy-Distance and Active-Face Certificates: Same-Grid Duality for Constrained Dynamic Portfolios
created: 2026-08-08
updated: 2026-08-08
type: concept
tags: [quant, 组合优化, 对偶, 约束, 数值求解]
sources: [From Value Bounds to Policy-Distance and Active-Face Certificates: Same-Grid Duality for Constrained Dynamic Portfolios]
confidence: medium
---

# From Value Bounds to Policy-Distance and Active-Face Certificates: Same-Grid Duality for Constrained Dynamic Portfolios

> 来源：[arXiv:2608.05901v1](https://arxiv.org/abs/2608.05901) | 作者：Jeonggyu Huh

## 基本信息

| 字段 | 值 |
|------|----|
| 发表 | 2026-08-06 |
| 方法 | 强化学习/RL |
| 策略类型 | 未识别 |
| 资产类别 | 未特定 |

## 摘要

Neural and numerical policy solvers can produce feasible controls even when the optimal rule and its binding constraints are unavailable. A primal-dual bracket certifies value loss, but it does not locate the optimal policy or explain which constraints genuinely bind. We show that, on the same declared simulation grid, one bracket can support both conclusions. For polyhedral controls, an exact conditional budget identity rewrites the residual as a pathwise nonnegative terminal Fenchel defect plus date-by-constraint complementary-slackness terms. A canonical Doob compensation removes the budget martingale that obscures small residuals. Bellman-primitive curvature conditions then yield an occupancy-weighted policy region with the sharp O(sqrt(G)) radius, while a paired constraint relaxation lower-bounds the optimal multiplier and certifies a binding face. A finite-sample resolution theorem quantifies the path budget needed to certify a target policy tolerance or face. Locked one-asset and two-asset audits cover every external policy error and make no false face declaration. An exact-wrapper stress test remains tight through 50 assets, while a separate state-dependent pilot identifies learned-dual tightness as the high-dimensional bottleneck. Reference solutions enter only after certification.

## 核心方法论

**方法：** 强化学习/RL
**策略方向：** 未识别

## 关键发现

- Neural and numerical policy solvers can produce feasible controls even when the optimal rule and its binding constraints are unavailable.
- A primal-dual bracket certifies value loss, but it does not locate the optimal policy or explain which constraints genuinely bind.
- We show that, on the same declared simulation grid, one bracket can support both conclusions.

## 实践要点

- 细节需阅读原文确认

## 相关概念

- [[strategy-prototypes]] — 策略原型
- [[risk-management]] — 风险管理
- [[factor-research]] — 因子研究框架
