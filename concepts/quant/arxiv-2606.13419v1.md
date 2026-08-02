---
title: Realtime price impact detection
created: 2026-08-01
updated: 2026-08-01
type: concept
tags: [factor, strategy, quant, 执行-做市]
sources: [Realtime price impact detection]
confidence: medium
---

# Realtime price impact detection

> 来源：[arXiv:2606.13419v1](https://arxiv.org/abs/2606.13419v1) | 作者：

## 基本信息

| 字段 | 值 |
|------|----|
| 发表 | 2026-06-11 |
| 方法 | 未识别 |
| 策略类型 | 执行/做市 |
| 资产类别 | 未特定 |

## 摘要

An important question for an algo trader working an order is to understand if their actions are moving the market against them -- i.e., causing market impact. The conventional answer usually is one of two: (i) monitor price slippage in real-time, potentially reducing adverse activity with increased slippage, or (ii) do away with dynamic trading adjustments and rely on semi-static rules based on ex-post estimates of slippage over a large sample of events.   Realtime monitoring fails because relia

## 核心方法论

**方法：** 未识别
**策略方向：** 执行/做市

## 关键发现

- An important question for an algo trader working an order is to understand if their actions are moving the market against them -- i.
- The conventional answer usually is one of two: (i) monitor price slippage in real-time, potentially reducing adverse activity with increased slippage, or (ii) do away with dynamic trading adjustments and rely on semi-static rules based on ex-post estimates of slippage over a large sample of events.
- Realtime monitoring fails because relia.

## 实践要点

- 考虑了交易成本/滑点

## 相关概念

- [[strategy-prototypes]] — 策略原型
- [[risk-management]] — 风险管理
- [[factor-research]] — 因子研究框架
