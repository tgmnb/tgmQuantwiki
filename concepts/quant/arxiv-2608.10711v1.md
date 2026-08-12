---
title: Optimal Pricing and Hedging of SOFR Derivatives
created: 2026-08-12
updated: 2026-08-12
type: concept
tags: [quant, strategy, 衍生品定价, 利率, 对冲]
sources: [Optimal Pricing and Hedging of SOFR Derivatives]
confidence: medium
---

# Optimal Pricing and Hedging of SOFR Derivatives

> 来源：[arXiv:2608.10711v1](https://arxiv.org/abs/2608.10711) | 作者：Teemu Pennanen, Waleed Taoum

## 基本信息

| 字段 | 值 |
|------|----|
| 发表 | 2026-08-11 |
| 方法 | 最优定价/对冲 |
| 策略类型 | 衍生品/利率 |
| 资产类别 | 未特定 |

## 摘要

Thousands of SOFR derivatives are available in exchanges and OTC, but the market remains illiquid and incomplete. Such a market is beyond the scope of classic risk-neutral approaches that imply linear pricing rules and, at best, approximate hedging strategies whose hedging error may be difficult to quantify. This paper develops an indifference pricing model which is consistent with observed derivative quotes, the agent's financial position and views about the uncertain future as well as risk preferences as described by a convex risk measure. In addition to prices and hedging strategies, the model gives an explicit description of the hedging error and the associated risk. The approach is illustrated numerically using hundreds of CME-listed derivatives to price and hedge unreplicable OTC SOFR derivatives. The indifference prices are computed in less than a minute on a regular PC. We find that the optimal hedging portfolios tend to be sparse but still provide good approximations of the derivative payouts.

## 核心方法论

**方法：** 最优定价/对冲
**策略方向：** 衍生品/利率

## 关键发现

- Thousands of SOFR derivatives are available in exchanges and OTC, but the market remains illiquid and incomplete.
- Such a market is beyond the scope of classic risk-neutral approaches that imply linear pricing rules and, at best, approximate hedging strategies whose hedging error may be difficult to quantify.
- This paper develops an indifference pricing model which is consistent with observed derivative quotes, the agent's financial position and views about the uncertain future as well as risk preferences as described by a convex risk measure.

## 实践要点

- 细节需阅读原文确认

## 相关概念

- [[strategy-prototypes]] — 策略原型
- [[risk-management]] — 风险管理
- [[factor-research]] — 因子研究框架
