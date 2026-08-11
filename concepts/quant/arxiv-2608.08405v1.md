---
title: Robustness or Crowding: Experimental Design for Trading Strategy Capacity
created: 2026-08-11
updated: 2026-08-11
type: concept
tags: [quant, 策略容量, alpha衰减, 拥挤度, 实验设计]
sources: [Robustness or Crowding: Experimental Design for Trading Strategy Capacity]
confidence: medium
---

# Robustness or Crowding: Experimental Design for Trading Strategy Capacity

> 来源：[arXiv:2608.08405v1](https://arxiv.org/abs/2608.08405) | 作者：Alejandro Rodriguez Dominguez, Miquel Noguer i Alonso

## 基本信息

| 字段 | 值 |
|------|----|
| 发表 | 2026-08-09 |
| 方法 | 因果推断/实验设计 |
| 策略类型 | 策略容量评估 |
| 资产类别 | 未特定 |

## 摘要

How much capital a trading strategy can absorb before its edge disappears is a causal question about how much is deployed, but it is answered with observational proxies that rest on incompatible assumptions. We ask what experiment would answer it instead, and show that two features of the problem interact to constrain any answer. Deployed capital erodes the edge gradually, so a trial of fixed length measures less than the eventual effect; and parallel implementations of one strategy trade the same securities, so they are not independent units. Comparing implementations on the same date removes market-wide shocks, which is what makes the comparison credible. But the crowding created by the strategy's own accumulated position is common to those implementations too, and an arbitrary date effect absorbs it exactly: the comparison that makes the experiment robust is the one that prevents it from measuring the crowding capacity is about. A same-date design recovers one implementation's private response at the prevailing level of aggregate positioning, and reaching the aggregate effect requires either implementations with deliberately different exposure to that position or variation in it over time. We characterise what each route identifies and what it costs, establish how far a fixed holding period understates the eventual effect and how to correct for it, and show what a finite set of deployment levels can and cannot reveal. A calibration on a purpose-built panel illustrates the resulting design rules and prices a study that would follow them.

## 核心方法论

**方法：** 因果推断/实验设计
**策略方向：** 策略容量评估

## 关键发现

- How much capital a trading strategy can absorb before its edge disappears is a causal question about how much is deployed, but it is answered with observational proxies that rest on incompatible assumptions.
- We ask what experiment would answer it instead, and show that two features of the problem interact to constrain any answer.
- Deployed capital erodes the edge gradually, so a trial of fixed length measures less than the eventual effect; and parallel implementations of one strategy trade the same securities, so they are not independent units.

## 实践要点

- 细节需阅读原文确认

## 相关概念

- [[strategy-prototypes]] — 策略原型
- [[risk-management]] — 风险管理
- [[factor-research]] — 因子研究框架
