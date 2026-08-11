---
title: When Cross-Venue Agreement Is Not Price Discovery: Disclosure Frontiers for 24/7 Equity-Perpetual Oracles
created: 2026-08-11
updated: 2026-08-11
type: concept
tags: [quant, 加密货币, 价格发现, 预言机, 永续合约]
sources: [When Cross-Venue Agreement Is Not Price Discovery: Disclosure Frontiers for 24/7 Equity-Perpetual Oracles]
confidence: medium
---

# When Cross-Venue Agreement Is Not Price Discovery: Disclosure Frontiers for 24/7 Equity-Perpetual Oracles

> 来源：[arXiv:2608.09188v1](https://arxiv.org/abs/2608.09188) | 作者：Donghwa Seo, Doohwi Cha, Seunghan Son等

## 基本信息

| 字段 | 值 |
|------|----|
| 发表 | 2026-08-10 |
| 方法 | 不动点/算子建模 |
| 策略类型 | 加密市场微观结构 |
| 资产类别 | 股票, 加密货币 |

## 摘要

Crypto-listed equity perpetuals trade while the primary cash market is closed, yet still need a mark for margin, funding, and liquidation. We model the closed-window mark as the fixed point of an oracle operator with two blocks: external anchoring and self/peer derivative reference. From marks and proxies alone the two are observationally equivalent: every reduced form admits infinitely many topology decompositions, and a path-law argument extends this to the full mark dynamics, so lead-lag and information-share estimators have power equal to size. Disclosure breaks the tie -- disclosed diagonal adjustment identifies the normalized topology, and disclosed support with forbidden anchors gives a row-level test that identifies, falsifies, or leaves a positive-dimensional class under a rank condition and finite-sample tolerance. Empirically, a disclosed OKX row survives pre-open falsification while a pure-external baseline shows the test's limited power, and an eight-week deep-closed panel with cash-reopen validation bounds the live-external content of closure variance. Cross-venue agreement is not price discovery unless disclosure or the cash reopen breaks the equivalence class.

## 核心方法论

**方法：** 不动点/算子建模
**策略方向：** 加密市场微观结构

## 关键发现

- Crypto-listed equity perpetuals trade while the primary cash market is closed, yet still need a mark for margin, funding, and liquidation.
- We model the closed-window mark as the fixed point of an oracle operator with two blocks: external anchoring and self/peer derivative reference.
- From marks and proxies alone the two are observationally equivalent: every reduced form admits infinitely many topology decompositions, and a path-law argument extends this to the full mark dynamics, so lead-lag and information-share estimators have power equal to size.

## 实践要点

- 细节需阅读原文确认

## 相关概念

- [[strategy-prototypes]] — 策略原型
- [[risk-management]] — 风险管理
- [[factor-research]] — 因子研究框架
