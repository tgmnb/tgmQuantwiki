---
title: Learning Market Making with Closing Auctions
created: 2026-08-01
updated: 2026-08-01
type: concept
tags: [执行-做市, quant, factor, 强化学习-rl, strategy]
sources: [Learning Market Making with Closing Auctions]
confidence: medium
---

# Learning Market Making with Closing Auctions

> 来源：[arXiv:2601.17247](https://arxiv.org/abs/2601.17247) | 作者：Julius Graf, Thibaut Mastrolia

## 基本信息

| 字段 | 值 |
|------|----|
| 发表 | 2026-07-31 |
| 方法 | 强化学习/RL |
| 策略类型 | 执行/做市 |
| 资产类别 | 未特定 |

## 摘要

In this work, we investigate a market making execution problem on a trading session in which a continuous phase on a limit order book is followed by a closing auction. Whereas standard optimal market making models typically rely on terminal inventory penalties to manage end-of-day risk, ignoring the significant liquidity events available in closing auctions, we propose a deep reinforcement learning framework, consisting of a Deep Q-Network and its continuous-control actor-critic extensions (DDPG, TD3 and SAC), that explicitly incorporates this mechanism. We introduce a market making framework designed to explicitly anticipate the closing auction, continuously refining the projected clearing price as the trading session evolves. We develop a generative stochastic market model to simulate the trading session and to emulate the market. Our theoretical model and these deep reinforcement learning methods are applied on the generator in two settings: (1) when the mid price follows a rough Heston model with generative data from this stochastic model; and (2) when the mid price corresponds to historical data of assets from the S&amp;P 500 index and the performance of our algorithm is compared with stylized reference benchmarks from optimal market making.

## 核心方法论

**方法：** 强化学习/RL
**策略方向：** 执行/做市

## 关键发现

- In this work, we investigate a market making execution problem on a trading session in which a continuous phase on a limit order book is followed by a closing auction.
- Whereas standard optimal market making models typically rely on terminal inventory penalties to manage end-of-day risk, ignoring the significant liquidity events available in closing auctions, we propose a deep reinforcement learning framework, consisting of a Deep Q-Network and its continuous-control actor-critic extensions (DDPG, TD3 and SAC), that explicitly incorporates this mechanism.
- We introduce a market making framework designed to explicitly anticipate the closing auction, continuously refining the projected clearing price as the trading session evolves.

## 实践要点

- 基于历史回测，警惕过拟合风险

## 相关概念

- [[strategy-prototypes]] — 策略原型
- [[risk-management]] — 风险管理
- [[factor-research]] — 因子研究框架
