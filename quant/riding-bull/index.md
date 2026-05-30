---
title: 骑牛登山—技术面大佬Wiki
created: 2026-05-27
updated: 2026-05-27
type: index
tags: [quant, strategy, riding-bull]
confidence: high
---

# 骑牛登山Wiki

> 某技术面大佬的完整知识沉淀。收录其教学视频、解盘观点、交易模式，以及观点验证追踪。

## 目录结构

- [[profiles/index]] — 大佬背景与风格概述
- [[teachings/index]] — 技术教学知识点（指标用法、形态识别、量价关系等）
- [[market-analysis/index]] — 解盘视频要点整理（按时间线）
- [[trading-patterns/index]] — 提炼出的交易模式（入场/离场/止损/仓位规则）
- [[views/pending-views]] — **观点待验证列表**（未验证的观点）
- [[views/verified-views]] — **已验证观点记录**（已兑现/未兑现）
- [[daily-reports/index]] — 每日日报存档
- [[trade-records/index]] — TGM基于模式的实际交易记录

## 使用流程

### 内容录入（人工→Hermes）
1. TGM看完视频，提炼观点和技术要点
2. 提交给Hermes，由Hermes整理到对应分类
3. 新观点自动追加到[[views/pending-views]]

### 每日验证（自动）
1. 每晚收盘数据更新后，触发验证脚本
2. 扫描观点待验证列表，逐一比对当日股价
3. 更新观点状态（已兑现/未兑现/待继续观察）

### 早报推送（自动）
1. 每天早上8:12，生成日报
2. 日报包含：今日已兑现观点 / 待兑现观点进展 / 盘中关注信号

### 交易前审查（人工→Hermes）
1. TGM计划入场前，报备交易计划
2. Hermes检索wiki中对应模式定义
3. 检查交易是否符合模式入场条件/仓位管理/止损规则
4. 返回审查结论

## 关联页面

- [[../index]] — Wiki首页
