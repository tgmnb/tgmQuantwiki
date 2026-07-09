---
title: DVC 研究驱动迭代主页面
created: 2026-07-08
updated: 2026-07-09 (final strategy matrix completed — DVC A0 abandoned as standalone strategy)
type: research-index
tags: [quant, dvc, research-driven-iteration, RDI, double-volume-cut, master-index, oracle-taxonomy, final-verdict]
status: active
---

# DVC 研究驱动迭代主页面

> **创建日期**：2026-07-08
> **仓库状态**：❌ `/home/tgm/quant` 非 git 仓库 — 当前无 commit/branch 绑定。版本回溯依赖手动备份和目录时间戳。
> **此页职责**：统一维护所有 DVC 版本的研究状态、决策记录、产物索引，确保每个版本可复现、可调取。

---

## 一、方法论：研究驱动迭代（Research-Driven Iteration, RDI）

> 摒弃「试错驱动开发」模式，转为「研究驱动迭代」：**同层并行研究 → 证据积累 → 机制诊断 → 决出底座 → 展开下一层**。

### 核心原则

| # | 原则 | 说明 |
|:-:|:----|:-----|
| 1 | **同层并行研究** | 同一层的多个机制方向（入场路径、退出路径、仓位管理等）应**并行**研究，不预设单一路径为「正解」。等各方向画像完毕再决策。 |
| 2 | **先画像/归因/证据，再决策** | 每个机制方向先做诊断 — C 模式研究（画像、归因、证据积累），再进入 D/E 可执行验收。C 与 D/E 是不同阶段，不可混淆。 |
| 3 | **机制升级优于调参** | 在信号逻辑/入场方法/退出规则层面做改进（如 touch→pullback），优于在单一方案内调整 NAV/HOLD 等参数。参数最优解在底座变更后需重新验证。 |
| 4 | **每个版本绑定快照** | 每个版本必须有：代码快照（脚本路径 + git ref 或备份标记）、回测产物路径、数据日期、决策记录。做到「任一版本可调取、可复现」。 |
| 5 | **C 模式 = 研究诊断，D/E = 可执行验收** | C 模式（日终全知排序）仅用于因子研究、上限估算、方向筛选，**不得**作为实盘预期引用。D/E 模式才是可执行验收标准。 |

### 迭代流程

```
同一层并行研究（入场·退出·环境·仓位）
        │
        ▼
  各方向 C 模式画像完成
        │
        ▼
  机制诊断 + 证据对比
        │
        ▼
  选出一个方向升级为下一版底座
        │
        ▼
  新底座展开下一层研究（如：回踩底座上叠加退出优化）
        │
        ▼    ← 回到顶层，重复
```

---

## 二、当前已知教训（从过往研究积累）

### 2.1 核心教训：`close > DVC close` 但按 DVC close 价买 = 不可执行

旧理想版「突破收盘买入」的逻辑：
```
条件：收盘价 > DVC 信号日收盘价(P)
买入价：高开→开盘价，N 字型→P 价（DVC close）
```

**问题**：收盘后既无法用 DVC close 限价单成交（已收盘），也无法用当日开盘价（时间已过）。这是一个**不可执行的理论上限**。

旧 wiki 页面（`dvc-entry-methods-research.md`、`dvc-shared-account-backtest.md`）未标注此缺陷就将 73.71%+ 年化作为推荐结论输出 — **此问题已在 2026-07-08 审计中发现并确认**。

### 2.2 touch / next / same / pullback / D-E 数据结论

**实盘口径退化（V3 引擎，high-based nshape + 0.8% 买入滑点 + 整手约束）**：

| 入场方式 | 年化 | MDD | CAGR保留率 | 状态 |
|---------|------|-----|-----------|------|
| ideal_close_confirm（旧理想版） | +272.73% | 19.20% | — | ❌ 理论上限 |
| touch_immediate（触价追买） | +15.65% | 74.66% | 5.7% | ⚠️ 可执行但回撤大 |
| close_confirm_next_open（尾盘→次日开） | +5.58% | 83.38% | 2.0% | ✅ 可执行但收益低 |
| close_confirm_same_close（尾盘确认买当日） | -7.66% | 90.52% | -2.8% | ✅ 可执行（亏损） |
| confirm_pullback_1d_0pct（B4版） | +10.29% | 36.84% | — | ✅ 推荐底座 |
| confirm_pullback_3d_0pct（AB0版） | +11.77% | 55.46% | — | ⚠️ 备选 |

**关键结论**：
- 旧所有研究版 100-200% 年化在实盘口径下全部缩水到 5-15%
- CAGR 保留率仅 2%-5.7%（理想→可执行）
- touch_immediate 虽然收益相对最高，但 MDD 74.66% 不可接受
- pb1d0 回踩方案以最低的 MDD（36.84%）提供 10%+ 年化，当前最优底座

### 2.3 D/E 小时线触发顺序（独立路径）

Round 4 的 D/E 模式（按小时线 trigger_ts 排序）提供了更高的收益（D_TRIGGER_ORDER_TOP4 年化 174-220%），但：
- 使用 V1 runner（close-based nshape + 无买入滑点 + 无整手），数值偏大
- 尚未与 touch/pullback 降级口径交叉验证
- **是独立的研究线**，不代表旧理想版的可执行版本

### 2.4 禁止事项

1. **❌ 不得引用 ideal 作为推荐预期** — 必须标注「理论上限，不可执行」
2. **❌ 不得把 high>=DVC close 和 close>DVC close 混用** — 触价 vs 收盘确认是不同信号逻辑
3. **❌ 不得跳过 baseline 直接报告「最优」** — 新方法必须与 ideal / touch / next_open 对比
4. **❌ 不得用理想版参数直接套用给可执行版** — 路径不同，参数最优解可能完全不同

---

## 三、版本表

| 版本 | 名称 | 入场方法 | 口径 | 年化 | MDD | 数据日期 | 状态 | 决策 |
|:----:|:----|:---------|:----:|:---:|:---:|:--------:|:----:|:----:|
| V0 | A0 纯倍量切形态画像 | 纯 DVC：阳线+中阳>=2%+切MA5/10+倍量>=2x | C模式研究 | — | — | 2014-01-02 ~ 2026-07-02 | ✅ 已完成形态画像+20/60d分布补充 | 166K信号，5,634股票；纯 DVC 无正向统计优势（fwd_1d胜率45%，均值0.16%）；形态特征与收益弱相关（r最高0.07~0.08）；
  - **20/60d 补充发现**（2026-07-08）：1d均值0.16%→20d均值0.27%→60d均值1.76%，胜率从45%→44%→43%；右尾(>20%)从1d 0.1%→60d 16.6%，左尾(<-20%)从0.0%→14.9%；20d偏度3.70（右尾极厚，少数大赢家）；无上影组1d胜率52.95%最高，弱势市场环境60d均值2.94%最高；详见 a0_long_horizon_report.md |
| A0-oracle | Oracle 未来函数标签体系 | 基于 fwd_ret_60d/20d/10d 的学术归档 | C 模式（学术归档） | — | — | 2014-01 ~ 2026-07 | ✅ 已完成 | 四类标签：long_trend 12.7%, short_rebound 5.5%, failure 9.9%, noise_flat 71.9%；数据不支持简单 V1-A/V1-B 二元分割；详见 oracle_taxonomy_report.md |
| V1 | ideal_close_confirm | close>DVC close → DVC close 买入 | **理论上限** | +272.73% | 19.20% | 2014~2026-07-01 | ❌ 已降级 | 不可执行，仅作上限参考 |
| V2 | 实盘口径 touch/next/same | touch: high>=close; next: 收盘确认→次日开; same: 收盘确认→当日收盘 | **可执行** | 5.58%~15.65% | 74.66%~83.38% | 2014~2026-07-01 | ⚠️ 已证弱 | touch 收益尚可但 MDD 过大；next_open 收益过低 |
| V3 | 回踩 pb1d0/pb3d0 | close 确认后 1d/3d 回踩 DVC close | **可执行** | 10.29%~11.77% | 36.84%~55.46% | 2014~2026-07-01 | ⚠️ 可执行候选 | pb1d0 (B4) MDD 最低，推荐底座；D/E 验收后收益偏低 |
|| V4 | B4_R60 / pb1d0续研 | 小矩阵 C 模式改善 | C 模式 | ?? | ?? | 2014~2026-07-01 | 🔬 待严验/标待跑 | C 模式有改善迹象，D/E 严格验收尚未完成 |
|| V1-B | 确认路径型 DVC 机制画像 | 聚焦 confirm_d1 内部差异：右尾继续 vs 当日透支 | C 模式研究 | — | — | 2014-01 ~ 2026-07 | ⚠️ 已完成候选观察，已被 oracle taxonomy 取代 | 45%信号触发confirm_d1；gapup+confirm 的 ret_20d=4.59%；回踩后收益归零(0.29%)；市场环境是致胜维度(强势2.5x)；上影缩短预示右尾；当日大阳线透支后续；详见 v1b_confirm_path_report.md |
|| V1-A | 弱势反抽型 DVC 机制画像 | 聚焦弱势/超跌环境下的 DVC 反抽特征：MA25/MA60弱势、距MA60深度、前N日跌幅 | C 模式研究 | — | — | 2014-01 ~ 2026-07 | ⚠️ 已完成候选观察，已被 oracle taxonomy 取代 | 22%信号(36.6K)在综合弱势条件下；弱势反抽候选60d胜率48.32%(vs全量43.40%)，60d>20% 18.92%；距MA60<-20%组60d>20% 20.28%，60d胜率50.39%；弱势+前5日超跌(prev<-10%)的5d胜率100%但仅12信号；详见 v1a_weak_rebound_report.md |

### 版本说明

- **V0 / A0**：纯倍量切（不加双红过滤，不加任何入场优化）。这是后续所有研究的基线起点。
- **A0-oracle**：Oracle 未来函数标签体系（学术归档）。基于 fwd_ret_60d/20d/10d 对 A0 信号做回顾性分类：long_trend 12.7%、short_rebound 5.5%、failure 9.9%、noise_flat 71.9%。**仅用于学术归档，不构成策略推荐。当前阶段基准。**
- **V1-A**：弱势反抽型 DVC 机制画像（同层并行研究，已被 oracle taxonomy 降级为前置观察）。聚焦「弱势/超跌环境下 DVC 信号是否呈现更强的反抽特性？」。**已完成 C 模式画像，但已被 oracle taxonomy 取代为阶段基准——数据不支持将其与 V1-B 二元组合成机制定论。**
- **V1-B**：确认路径型 DVC 机制画像（同层并行研究，已被 oracle taxonomy 降级为前置观察）。聚焦「confirm_d1 内部差异分析：gapup/回踩/市场环境/形态透支」。**已完成 C 模式画像，但已被 oracle taxonomy 取代为阶段基准——不能作为方向裁决依据。**
- **V1**：旧理想版「突破收盘买入」。已降级为理论上限，不作为推荐依据。
- **V2**：touch_immediate / close_confirm_next_open / close_confirm_same_close。收益率低或回撤大，不作为主推底座。
- **V3**：confirm_pullback_1d_0pct / confirm_pullback_3d_0pct。当前最优底座候选，但 D/E 验收后收益偏低。
- **V4**：B4_R60/pb1d0 小矩阵优化，C 模式有改善。尚未通过 D/E 严格验收，需重跑验证。

---

## 三.B Oracle 未来函数标签体系（学术归档）

> **仅用于学术归档，不构成策略推荐。** Oracle 标签使用未来实际价格信息回顾性分类 A0 信号，目的是先定义客观的未来结果类型，再反查各类信号的差异特征，为后续代理机制设计提供依据。

### 标签定义与分布

| Oracle 类型 | 定义 | 数量 | 占比 |
|:-----------|:-----|:-----|:----:|
| **long_trend** | 持续主升浪（fwd_ret_60d≥30% 或 fwd_ret_20d≥20% 且 fwd_ret_60d≥10%） | 20,947 | **12.7%** |
| **short_rebound** | 短期强反弹不持续（fwd_ret_10d≥10% 且非 long_trend，fwd_ret_60d<10%） | 9,031 | **5.5%** |
| **failure** | 显著亏损（fwd_ret_20d≤-20% 或 fwd_ret_60d≤-30%） | 16,437 | **9.9%** |
| **noise_flat** | 其余（无显著方向） | 118,831 | **71.9%** |

### 关键发现

1. **short_rebound 不是 long_trend 的弱化版**
   - short_rebound 的 5-10 日收益最高（均值 +17.2%），远高于 long_trend 的 +10.4%
   - short_rebound 的 MFE 均值 25.1% > long_trend 的 18.8%——近端爆发力更强
   - 核心差异在持续性：long_trend 60 日收益继续扩张（+53.1%），short_rebound 60 日归零（-2.9%）
   - 可用的分离线索：10 日收益绝对值 + MFE 与最终收益的 gap

2. **failure 大阳线陷阱**
   - failure 组 body_pct 最大（7.0%）、turnover 最高（¥469M）、dist_ma60 最高
   - 更大的阳线和更高的成交额反而是风险信号
   - D5 确认率仅 58%（long_trend 84%），确认延迟是最大的非未来区分器

3. **K 线形状区分力弱**
   - body_pct、volume_ratio、shadow 等传统 K 线指标在各 oracle 类型间差异很小（|d| < 0.2）
   - 确认率（D5/D3 confirm）是最大的无偏信号质量近似指标

4. **数据不支持简单 V1-A/V1-B 二元分割**
   - 不存在显式的「弱势反抽 vs 确认动量」二元对立
   - short_rebound 和 long_trend 在初始特征上连续，本质差异在持续性
   - V1-A（弱势反抽）和 V1-B（确认路径）均已被 oracle taxonomy 降级为前置观察，不能作为机制定论

### 产物

| 文件 | 路径 |
|:----|:-----|
| Oracle 标记数据 | `../dvc-rdi-a0-20260708/oracle_taxonomy/a0_oracle_labeled.parquet` |
| 分类汇总 | `../dvc-rdi-a0-20260708/oracle_taxonomy/a0_oracle_class_summary.csv` |
| 特征对比 | `../dvc-rdi-a0-20260708/oracle_taxonomy/a0_oracle_feature_diff.csv` |
| 完整报告 | `../dvc-rdi-a0-20260708/oracle_taxonomy/a0_oracle_taxonomy_report.md` |
| Proxy 代理特征矩阵 | `../dvc-rdi-a0-20260708/proxy_features/proxy_feature_matrix.parquet` | 165K×143 矩阵，140 个代理特征（80MB） |
| Proxy 特征重要性 | `../dvc-rdi-a0-20260708/proxy_features/proxy_feature_importance.csv` | 661 条对比 × Cohen's d / AUC / KS |
| Proxy 分箱 Lift | `../dvc-rdi-a0-20260708/proxy_features/proxy_feature_bucket_lift.csv` | 1100 条分箱富集分析 |
| Proxy 特征元数据 | `../dvc-rdi-a0-20260708/proxy_features/proxy_feature_metadata.csv` | 140 特征属性 + 等待天数 + 缺失率 |
| Proxy 完整报告 | `../dvc-rdi-a0-20260708/proxy_features/proxy_feature_report.md` | 522 行完整画像 + 候选代理汇总 |
| Proxy 构建脚本 | `../dvc-rdi-a0-20260708/proxy_features/build_proxy_feature_library.py` | 可复现完整流程 |
| 机制裁决报告 | `../dvc-rdi-a0-20260708/mechanism_selection/mechanism_selection_report.md` | 最终机制裁决 |
| 早期路径阈值表 | `../dvc-rdi-a0-20260708/mechanism_selection/early_path_thresholds.csv` | 各阈值下 oracle 类型分布 |
| 陷阱联合升力表 | `../dvc-rdi-a0-20260708/mechanism_selection/trap_combo_lift.csv` | 波动率体量陷阱单变量+联合分层 |
| 机制评分卡 | `../dvc-rdi-a0-20260708/mechanism_selection/mechanism_scorecard.csv` | 两条机制评分对比 |
| 机制验证脚本 | `../dvc-rdi-a0-20260708/mechanism_selection/run_mechanism_validation.py` | 可复现机制验证 |
| **Checkpoint 暂存** | `../dvc-rdi-a0-20260708/checkpoints/20260709_mechanism_selection_checkpoint.md` | 2026-07-09 机制选择暂存：候选记录 + 待补研究清单 |

---

## 三.C 最终机制裁决（已暂存）

> **⚠️ 本条目为 2026-07-08 阶段性结论，已于 2026-07-09 被 checkpoint 暂存为非最终候选。详见 [Checkpoint](../dvc-rdi-a0-20260708/checkpoints/20260709_mechanism_selection_checkpoint.md)。**
>
> **裁决日期**：2026-07-08
> **算法**：基于 oracle 标签评估两条候选代理机制的区分力、实盘可知性、等待成本，给出可裁决证据。
> **输入**：proxy_feature_matrix (165K×143); proxy_feature_importance (661条对比); oracle_labeled (165K×86)

### 候选机制

| 机制 | 核心特征 | 等待天数 |
|:----|:---------|:--------:|
| **A) 早期路径确认** | confirm_d1/d3/d5, never_triggered, ret_1d/ret_3d | T+1~T+5 |
| **B) 波动率体量陷阱** | body_pct, turnover, dist_ma60_pct, atr_pct_14, volatility_20d | T+0 |

### 验证核心数据

| 指标 | 机制A | 机制B |
|:----|:-----|:-----|
| 区分力 | **强** — D5确认率 failure 58% vs LT 84% vs SR 94%; 从未触发率 failure 42% vs LT 16% vs SR 6% | **中等偏弱** — body_pct d=0.18; 联合3特征 P80 fail lift=2.02x但样本<4% |
| failure过滤率 | 42% (复合过滤 never_triggered==0 + confirm_d5==True) | <15% (单变量); 联合条件过于激进 |
| LT/SR分离 | 不充分 — ret_3d Δ=2.28pp; D5 confirm Δ=11pp | 极弱 — d<0.2 |
| 实盘可知性 | T+1~T+5已知 | T+0已知 |
| 未来函数风险 | 无 | 无 |

### 最终裁决

| 项目 | 决策 |
|:----|:-----|
| **主机制** | **早期路径确认机制** — 候选暂存，待补研究后裁决 |
| **辅助机制** | **波动率体量陷阱** — 作为早期路径的前置 T+0 预警层 |
| **淘汰机制** | 无（两条机制非互斥，可组合使用） |
| **下一步** | ⏸️ **机制候选已暂存，待补研究完成后再决定是否进入策略构建** |
| **LT/SR分离** | 进入策略层后附加收益速率衰减监测 |

### 机制组合顺序

```
T+0（信号日）
  └─ 波动率体量陷阱检查（body_pct>P80 + turnover>P80 + dist_ma60_pct>P80 → "陷阱警示"）
T+1（收盘）  
  └─ ret_1d 检查（ret_1d < 0% → 快速降仓，误杀LT约40%）
T+5（收盘）
  └─ confirm_d5 + never_triggered 检查（confirm_d5==False → 否决，过滤42% failure）
```

### 产物清单

| 文件 | 路径 |
|:----|:-----|
| 完整裁决报告 | `../dvc-rdi-a0-20260708/mechanism_selection/mechanism_selection_report.md` |
| 早期路径阈值表 | `../dvc-rdi-a0-20260708/mechanism_selection/early_path_thresholds.csv` |

### 三.E 最终策略矩阵：先入场+止盈止损/让利润奔跑（2026-07-09）

> **验证日期**：2026-07-09
> **方法论**：放弃 S1/S2 的"等待确认再入场"思路，改为"先入场(T+1 open) + 退出机制管理风险". 基于上一轮发现: DVC 最佳获利窗口在 T+1~T+3, T+6 入场延迟吃掉所有收益.

**核心问题**: 即使使用最优退出机制(TP/SL/移动止盈/让利润奔跑)和最强过滤(月MACD中红/大红), 能否将 DVC A0 信号转化为可执行策略?

**测试范围**: 57 个策略组合 × 165K 信号 × 12 年

#### 关键发现

1. **DVC A0 信号基底太弱**: T+1 open 入场 5d 均值仅 +0.38%, 中位数 -0.31%, 胜率 47.1%. 无统计显著的正向选择能力.

2. **退出机制无法逆转信号缺陷**: 最优退出策略(F2_F_let_run_10%_20d)将均值提升到 +2.28%, 但中位数仍为 -0.36%.

3. **E1 (T+2 ret_1d>-1% 条件入场) 是唯一中位数明确的策略**: 均值 +1.85%, 中位数 +0.86%, 胜率 56.0%. 但本质是"排除首日大跌"的通用动量过滤器, 非 DVC 特有.

4. **TP 策略 (C_tp_5%/10%/20%) 的"高收益"是数学假象**: 给股票 20 天等待反弹, 72.5% 会在 20 天内触及 +5% TP. 中位数接近 TP 阈值说明绝大多数收益来自"等待反弹"而非 DVC 选股.

5. **组合层 MDD 过高**: 最佳策略 MDD 也超过 -60%, 无法实战.

#### 最终裁决

**❌ 放弃 DVC A0 独立策略化. 证据充分:**
- 三路独立研究路径(等待确认/先入场退出/形态分析)指向同一结论
- 没有任何退出机制/过滤组合能产生可靠的实战级正预期
- 信号层均值 +0.38% ~ +2.28%, 交易成本 ~1% 单边, 净收益所剩无几
- 最佳策略中位数仍接近零或为负

#### 产物清单

| 文件 | 路径 | 说明 |
|:----|:-----|:-----|
| 完整报告 | `../dvc-rdi-a0-20260709/final_strategy/final_strategy_report.md` | 修正版最终裁决报告 |
| 策略矩阵(CSV) | `../dvc-rdi-a0-20260709/final_strategy/final_strategy_matrix.csv` | 57 策略 × 30+ 指标 |
| 组合结果(CSV) | `../dvc-rdi-a0-20260709/final_strategy/final_strategy_portfolio.csv` | Top10 策略组合回测结果 |
| 脚本 | `../dvc-rdi-a0-20260709/final_strategy/run_final_strategy.py` | 可复现完整回测脚本 |

#### 建议的资源重定向

1. **放弃 DVC A0 作为独立信号** — 信号基底的正向选择能力太弱
2. **E1 过滤器可作为通用工具** — "排除首日大跌"的动量过滤器可能适用于其他信号
3. **月MACD环境过滤可保留** — 作为多因子框架中的一个条件
4. **未来有 DVC 新变种时可重新测试** — 但必须从 A 层验证正预期开始
| 陷阱联合升力表 | `../dvc-rdi-a0-20260708/mechanism_selection/trap_combo_lift.csv` |
| 机制评分卡 | `../dvc-rdi-a0-20260708/mechanism_selection/mechanism_scorecard.csv` |
| 验证脚本 | `../dvc-rdi-a0-20260708/mechanism_selection/run_mechanism_validation.py` |

---

## 三.D 最小可执行策略验证（2026-07-09）

> **验证日期**：2026-07-09
> **脚本**：`/home/tgm/quant/research/dvc-rdi-a0-20260709/strategy_test/run_strategy_test.py`
> **报告**：`/home/tgm/quant/research/dvc-rdi-a0-20260709/strategy_test/strategy_test_report.md`

基于机制裁决构建 S0/S1/S2 三个最小可执行策略的信号层回测：

| 策略 | 入场 | 过滤 | 信号数 | 5d均值% | 5d胜率% |
|------|------|------|:------:|:-------:|:-------:|
| S0_baseline | T+1 open | 无 | 165,246 | +0.26 | 46.9% |
| S1_early_path | T+6 open | month_macd非大绿 + ret_1d>-1% + confirm_d5 | 81,337 | +0.22 | 47.0% |
| S2_trap_veto | T+6 open | S1 + atr≤P80 | 65,428 | +0.19 | 47.3% |

**裁决**：⚠️ 机制转策略部分成立。过滤有效（S1合格信号T+1入场+2.82% vs S0 +0.26%），但T+6入场延迟成本-2.60pp几乎抵消所有增益。不建议直接进入D/E层。详见产物索引和完整报告。

---

## 四、并行研究方向 Backlog

以下方向从**纯倍量切（A0）**开始并行画像，不预设单一路径为正确答案。

### 4.1 形态结构
- 实体/影线比例对后续走势的影响
- 涨幅大小（2% 阈值是否最优）
- 量比（2倍阈值是否最优，量比分布画像）
- 切 MA 方式（同时切 MA5/MA10 vs 只切其一 vs 不切）
- **状态**：✅ 已完成 — [A0 形态画像报告](../dvc-rdi-a0-20260708/shape/a0_shape_report.md) | [CSV](../dvc-rdi-a0-20260708/shape/a0_shape_profile.csv)
  - 关键发现：纯 DVC 信号无正向统计优势（fwd_1d 胜率仅 45%、均值 0.16%）
  - 形态特征与收益相关性弱（r 最高 0.07~0.08），最强的正向特征为当日涨幅/实体涨幅/收盘位置
  - 上影比例与收益负相关（r=-0.054），无上影线股票 fwd_1d 胜率 51.6%
  - 切断更多 MA → 正向收益略高（切 MA5+10+25+60 胜率 47.6%）
  - 倍量比与收益无单调关系
- ✅ **20/60d 分布补充已完成** — [20/60日分布报告](../dvc-rdi-a0-20260708/shape/a0_long_horizon_report.md) | [CSV](../dvc-rdi-a0-20260708/shape/a0_long_horizon_distribution.csv)
  - **总体**: 1d均值0.16%→20d均值0.27%→60d均值1.76%，胜率44.98%→44.13%→43.40%
  - **尾部特征**: 右尾(>20%)从1d 0.13%→60d 16.58%，左尾(<-20%)从0.04%→14.90%；20d偏度3.70（右尾极厚）
  - **无上影组**: 1d均值1.12%（全组最高），1d胜率52.95%，但20/60d优势收窄（均值0.41%/1.25%）
  - **弱势市场(距MA60<-10%)**: 20d均值1.66%，20d胜率50.22%，60d均值2.94% — 超跌反弹信号最强
  - **前期动量反转**: 前5日涨幅最高分位的20d均值-0.63%（负收益），最低分位+0.36% — 高动量反转、低动量回复
  - **当日涨幅极端**: 高涨幅(分位8/9)短期(1d)好但长期(20d)均值趋近于0 — 大阳线可能是短期透支
|- ✅ **Oracle 未来函数标签体系已完成** — 见 [Oracle Taxonomy 报告](../dvc-rdi-a0-20260708/oracle_taxonomy/a0_oracle_taxonomy_report.md) | [CSV](../dvc-rdi-a0-20260708/oracle_taxonomy/a0_oracle_class_summary.csv)
|  - **当前阶段基准**：基于未来收益回顾性分类（long_trend 12.7%, short_rebound 5.5%, failure 9.9%, noise_flat 71.9%）
|  - 数据不支持简单 V1-A/V1-B 二元分割；短期反弹不是主升浪弱化版
|  - ✅ 代理特征库研究已基于 oracle 类别完成（140 个代理特征） — 见下方下一步计划章节
|- ✅ **Proxy 代理特征库画像已完成** — 140 个代理特征（110 数值 + 9 分类 + 21 布尔），见 [Proxy 完整报告](../dvc-rdi-a0-20260708/proxy_features/proxy_feature_report.md)
|  - T+0 技术指标整体区分力弱，最强为 atr_pct_14（Fail vs NF AUC=0.667）
|  - T+1~T+3 早期路径确认明显更强（ret_1d AUC=0.700, ret_3d AUC=0.796）
|  - 传统 K 线 / KDJ / MACD 柱 / 上影 / 量比——不要过度迷信

### 4.2 市场环境（含弱势反抽）
- 指数 MA（沪深300/中证1000 的 MA25/MA60 状态）
- 指数 MACD（周/月线红绿柱）
- 月线状态（上涨/下跌/震荡月 vs 倍量切信号频率与胜率）
- **弱势反抽机制画像**：⚠️ V1-A 已完成 — 见 [V1-A 弱势反抽报告](../dvc-rdi-v1-20260708/weak_rebound/v1a_weak_rebound_report.md) | [CSV](../dvc-rdi-v1-20260708/weak_rebound/v1a_weak_rebound_profile.csv)（已被 oracle taxonomy 降级为前置观察，不能作为机制定论；详见 Oracle Taxonomy 章节）
  - 关键发现：
    1. 综合弱势反抽候选（most_below_ma60 + dist_ma60<0）占 22% 信号，60d 胜率 48.32% vs 全量 43.40%，60d>20% 18.92%
    2. 距MA60 深度是反抽强度的核心指标：<-20% 组的 60d 胜率 50.39%，20d>20% 11.07%
    3. 弱势反抽主要是「短反抽」但有一定比例转主升浪：60d>20% 18.92%，60d>50% 5.00%
    4. 形态交叉（上影/MA切断/涨幅分位/量比分位）在弱势状态下分组差距小，弱势环境本身是主要因子
    5. 弱势+前5日超跌组合信号极少（仅12个），但反抽极强（5d胜率100%，20d>20% 75%）

### 4.3 入场路径
|- touch（触价追买）
|- close confirm（收盘确认）
|- pullback（回踩 DVC close）
|- next open（次日开盘）
|- **状态**：⚠️ V1-B 确认路径机制画像已完成 — 见 [V1-B 确认路径报告](../dvc-rdi-v1-20260708/confirm_path/v1b_confirm_path_report.md)（已被 oracle taxonomy 降级为前置观察，不能作为机制定论；详见 Oracle Taxonomy 章节）
|  - 关键发现：confirm_d1 是收益最高路径（ret_3d=3.16%, WR=70.4%），gapup 贡献 52% 收益、确认质量贡献 48%
|  - 回踩后 confirm 收益归零（0.29% @ 20d），证明回踩是信号失败标志
|  - 市场环境是区分 confirm 质量的致胜维度（强势 4.43% vs 弱势 1.95% @ 20d）
|  - 上影缩短 + 当日涨幅适中 + MA5/10 切断是最佳 confirm 形态组合

### 4.4 执行竞争
- D/E 小时线触发顺序
- TOPN（买前 N 个）
- 同日触发顺序优先级
- **状态**：🟡 Round 4 有探索，需与 A0 基线关联

### 4.5 退出路径
- HOLD N（固定持有 N 天）
- 时间止损（N 天后无论盈亏卖出）
- 动能退出（跌破 MA5/MA10/止盈线）
- 指数退出（INDEX_MA25_EXIT）
- **状态**：🔬 待系统研究

### 4.6 资金/仓位
- NAV 比例（固定%）
- 满仓/限仓
- 信号密度与仓位匹配
- **状态**：🔬 待系统研究

---

## 五、版本绑定模板

每个版本必须记录以下字段：

| 字段 | 说明 | 示例 |
|:----|:----|:-----|
| `version_id` | 版本标识符 | `V3_pb1d0` |
| `hypothesis` | 研究假设 | 「收盘确认后1日内回踩DVC close可提升入场精度」 |
| `code_ref` | 代码路径/脚本名 | `dvc_iterative_experiment_round3_execution.py` |
| `data_date` | 数据截止日期 | `2026-07-01` |
| `script_path` | 脚本完整路径 | `/home/tgm/quant/scripts/dvc_iterative_experiment_round3_execution.py` |
| `output_path` | 回测产物目录/文件 | `/home/tgm/quant/research/dvc-entry-method-comparison-20260708/` |
| `git_ref` | commit hash 或 tag 或标注 | `❌ 非 git repo — 依赖手动备份` |
| `status` | 状态枚举 | `active` / `deprecated` / `archived` / `pending_validation` |
| `key_metrics` | 核心指标 | `年化=10.29%, MDD=36.84%, Sharpe=0.425, 胜率=47.64%` |
| `decision` | 决策记录 | 「选为当前推荐底座，待 D/E 严格验收」 |

---

## 六、已有产物索引

### 6.1 主要回测产物

| 产物 | 路径 | 内容 | 口径 |
|:----|:-----|:-----|:----:|
| 入场方法对比 | `/home/tgm/quant/research/dvc-entry-method-comparison-20260708/entry_method_comparison.csv` | 5种入场方式对比（ideal / touch / next_open / same_close / touch_then_next） | V3 引擎 |
| 入场方法对比(HTML) | `/home/tgm/quant/research/dvc-entry-method-comparison-20260708/entry_method_comparison.html` | 同上，可视化 | V3 引擎 |
| 年度收益 | `/home/tgm/quant/research/dvc-entry-method-comparison-20260708/entry_method_yearly_returns.csv` | 各入场方式的逐年收益分解 | V3 引擎 |
| 净值曲线 | `/home/tgm/quant/research/dvc-entry-method-comparison-20260708/entry_method_equity_curves.png` | 5种入场方式的净值曲线对比 | V3 引擎 |
| 回踩入场对比(CSV) | `/home/tgm/quant/research/dvc-entry-method-comparison-20260708/pullback/pullback_entry_comparison.csv` | pb1d/pb3d/pb5d + 0%/3% 回踩对比 | V3 引擎 |
| 回踩入场对比(MD) | `/home/tgm/quant/research/dvc-entry-method-comparison-20260708/pullback/pullback_entry_comparison.md` | 同上，Markdown 报告 | V3 引擎 |
| 回踩净值曲线 | `/home/tgm/quant/research/dvc-entry-method-comparison-20260708/pullback/pullback_entry_equity_curves.png` | 回踩方案净值曲线 | V3 引擎 |
| A0 形态画像(CSV) | `/home/tgm/quant/research/dvc-rdi-a0-20260708/shape/a0_shape_profile.csv` | 166K 纯 DVC 信号 × 24 列（倍量比、实体涨幅、上影/下影比例、MA切断类型、距MA60、前N日涨幅、fwd_ret_1d~10d） | A0 研究 |
| A0 形态画像(MD) | `/home/tgm/quant/research/dvc-rdi-a0-20260708/shape/a0_shape_report.md` | 形态画像报告：forward returns 统计、分箱对比、MA切断分析、特征相关性、机制发现 | A0 研究 |
| A0 20/60d分布(CSV) | `/home/tgm/quant/research/dvc-rdi-a0-20260708/shape/a0_long_horizon_distribution.csv` | 275行分组分布统计：7个维度×5周期×全分布指标（p1/p5/.../p99、右尾/左尾比例、偏度峰度） | A0 研究 |
| A0 20/60d全量信号(CSV) | `/home/tgm/quant/research/dvc-rdi-a0-20260708/shape/a0_shape_profile_with_long.csv` | 166K信号 + fwd_ret_1d~60d含20d/60d | A0 研究 |
|| A0 分布分析脚本 | `/home/tgm/quant/research/dvc-rdi-a0-20260708/shape/a0_long_horizon_distribution.py` | 分布统计+分组画像分析脚本 | A0 研究 |
|| A0 Oracle 标记数据 | `/home/tgm/quant/research/dvc-rdi-a0-20260708/oracle_taxonomy/a0_oracle_labeled.parquet` | 165K+信号×各类别标签 + MFE/MAE/确认路径 | A0 Oracle 学术归档 |
|| A0 Oracle 分类汇总 | `/home/tgm/quant/research/dvc-rdi-a0-20260708/oracle_taxonomy/a0_oracle_class_summary.csv` | 4类×全分布统计量（均值/中位/胜率/p25-p99/std） | A0 Oracle 学术归档 |
|| A0 Oracle 特征对比 | `/home/tgm/quant/research/dvc-rdi-a0-20260708/oracle_taxonomy/a0_oracle_feature_diff.csv` | 259条 oracle 类型间特征对比（Cohen's d + KS） | A0 Oracle 学术归档 |
||| A0 Oracle 完整报告 | `/home/tgm/quant/research/dvc-rdi-a0-20260708/oracle_taxonomy/a0_oracle_taxonomy_report.md` | 标签体系 + 分布统计 + 反查差异 + 机制发现 | A0 Oracle 学术归档 |
||| A0 Proxy 代理特征矩阵 | `/home/tgm/quant/research/dvc-rdi-a0-20260708/proxy_features/proxy_feature_matrix.parquet` | 165K×143 矩阵，140 个代理特征（80MB） | A0 代理特征研究 |
||| A0 Proxy 特征重要性 | `/home/tgm/quant/research/dvc-rdi-a0-20260708/proxy_features/proxy_feature_importance.csv` | 661 条对比结果 × Cohen's d / AUC / KS | A0 代理特征研究 |
||| A0 Proxy 分箱 Lift | `/home/tgm/quant/research/dvc-rdi-a0-20260708/proxy_features/proxy_feature_bucket_lift.csv` | 1100 条分箱 lift 记录 | A0 代理特征研究 |
||| A0 Proxy 类型统计 | `/home/tgm/quant/research/dvc-rdi-a0-20260708/proxy_features/proxy_feature_type_stats.csv` | 440 条 oracle 类型 × 特征统计 | A0 代理特征研究 |
||| A0 Proxy 特征元数据 | `/home/tgm/quant/research/dvc-rdi-a0-20260708/proxy_features/proxy_feature_metadata.csv` | 140 特征属性 + 等待天数 + 缺失率 | A0 代理特征研究 |
||| A0 Proxy 完整报告 | `/home/tgm/quant/research/dvc-rdi-a0-20260708/proxy_features/proxy_feature_report.md` | 522 行：核心发现 + 候选代理汇总 + 后续方向 | A0 代理特征研究 |
||| A0 Proxy 构建脚本 | `/home/tgm/quant/research/dvc-rdi-a0-20260708/proxy_features/build_proxy_feature_library.py` | 可复现完整构建流程 | A0 代理特征研究 |
||| V1-B 确认路径画像(CSV) | `/home/tgm/quant/research/dvc-rdi-v1-20260708/confirm_path/v1b_confirm_path_profile.csv` | 11路径×7周期全分布统计（p25/p50/p75/p90/p95/p99、右尾/左尾比例、偏度峰度） | V1-B 机制研究 |
|| V1-B 确认路径画像(MD) | `/home/tgm/quant/research/dvc-rdi-v1-20260708/confirm_path/v1b_confirm_path_report.md` | confirm 内部差异分析：gapup/回踩/市场环境/形态/透支诊断框架 | V1-B 机制研究 |
|| V1-B 确认路径增强数据 | `/home/tgm/quant/research/dvc-rdi-v1-20260708/confirm_path/v1b_confirm_path_enhanced.parquet` | 166K行×37列，含路径标志+形态+衍生标志 | V1-B 机制研究 |
|| V1-A 弱势反抽画像(CSV) | `/home/tgm/quant/research/dvc-rdi-v1-20260708/weak_rebound/v1a_weak_rebound_profile.csv` | 59组×64列全分布统计（5/10/20/60d × 均值/中位/胜率/p25-p99/右尾左尾/偏度峰度） | V1-A 机制研究 |
|| V1-A 弱势反抽画像(MD) | `/home/tgm/quant/research/dvc-rdi-v1-20260708/weak_rebound/v1a_weak_rebound_report.md` | 弱势反抽机制画像报告：9维度画像+形态交叉+右尾诊断+机制发现 | V1-A 机制研究 |
|| V1-A 分析脚本 | `/home/tgm/quant/research/dvc-rdi-v1-20260708/weak_rebound/v1a_weak_rebound_analysis.py` | V1-A 弱势反抽分析脚本（857行，含画像+绘图+MD生成） | V1-A 机制研究 |
| 新路径测试(CSV) | `/home/tgm/quant/research/dvc-new-path-tests-20260708/new_path_tests.csv` | B4_R60 等新路径 C 模式测试 | C 模式 |
| 新路径测试(HTML) | `/home/tgm/quant/research/dvc-new-path-tests-20260708/new_path_tests.html` | 同上，可视化 | C 模式 |
| 新路径净值曲线 | `/home/tgm/quant/research/dvc-new-path-tests-20260708/new_path_tests_equity_curves.png` | 新路径方案净值曲线 | C 模式 |
| D/E 回踩验证 | `/home/tgm/quant/research/dvc-executable-rerun-20260708/pullback_pb1d0_DE_results.csv` | pb1d0 D/E 模式回踩结果 | D/E 可执行 |
| D/E 回踩验证(pb3d0) | `/home/tgm/quant/research/dvc-executable-rerun-20260708/pullback_pb3d0_DE_results.csv` | pb3d0 D/E 模式回踩结果 | D/E 可执行 |
| D/E 整合 | `/home/tgm/quant/research/dvc-executable-rerun-20260708/pullback_DE_consolidated.csv` | D/E 回踩方案整合 | D/E 可执行 |
| 重跑矩阵状态 | `/home/tgm/quant/research/dvc-executable-rerun-20260708/rerun_matrix_status.csv` | 所有策略 D/E 重跑状态追踪 | D/E 可执行 |
|| D/E 报告 | `/home/tgm/quant/research/dvc-executable-rerun-20260708/executable_rerun_report.md` | D/E 可执行回测完整报告 | D/E 可执行 |
||| 策略测试信号结果 | `/home/tgm/quant/research/dvc-rdi-a0-20260709/strategy_test/strategy_signal_results.csv` | S0/S1/S2 信号层收益分布（4持有期×16统计量） | 信号层回测 |
||| 策略测试报告(MD) | `/home/tgm/quant/research/dvc-rdi-a0-20260709/strategy_test/strategy_test_report.md` | 完整策略验证报告（含延迟分析、Oracle保留、裁决） | 信号层回测 |
|||| 策略测试脚本 | `/home/tgm/quant/research/dvc-rdi-a0-20260709/strategy_test/run_strategy_test.py` | 可复现策略测试（含组合模拟） | 信号层回测 |
|||| 最终策略矩阵报告(MD) | `/home/tgm/quant/research/dvc-rdi-a0-20260709/final_strategy/final_strategy_report.md` | 修正版最终裁决: ❌ 放弃 DVC A0 独立策略化 | 最终策略 |
|||| 最终策略矩阵(CSV) | `/home/tgm/quant/research/dvc-rdi-a0-20260709/final_strategy/final_strategy_matrix.csv` | 57 策略 × 30+ 指标 (信号层) | 最终策略 |
|||| 最终组合回测(CSV) | `/home/tgm/quant/research/dvc-rdi-a0-20260709/final_strategy/final_strategy_portfolio.csv` | Top10 策略组合层结果 | 最终策略 |
|||| 最终策略脚本 | `/home/tgm/quant/research/dvc-rdi-a0-20260709/final_strategy/run_final_strategy.py` | 可复现 57 策略矩阵回测脚本 | 最终策略 |

### 6.2 审计与文档

| 文件 | 路径 | 说明 |
|:----|:-----|:-----|
| DVC 审计报告 | `/home/tgm/quant/scripts/dvc_audit_report.md` | 2026-07-08 全量审计：口径可信度、路径评估、修正建议 |
| Wiki 修正草案 | `/home/tgm/quant/research/dvc-wiki-correction-draft-20260708.md` | 旧 wiki 页面的修正方案、分层金字塔、禁止事项、后续待验证方向 |

### 6.3 已有 Wiki 页面

| 页面 | 路径 | 说明 | 需修正? |
|:----|:-----|:-----|:-------:|
| 倍量切双红系统性研究 | `/home/tgm/wiki/quant/research/dvc-entry-methods-research.md` | 5种入场方法研究，突破收盘买入未标注理论上限 | ✅ 需标注 |
| 共享资金账户回测 | `/home/tgm/wiki/quant/research/dvc-shared-account-backtest.md` | 73.71% 年化未标注不可执行 | ✅ 需标注 |
| Round 4 可执行口径 | `/home/tgm/wiki/quant/research/dvc-executable-intraday-round4-20260706.md` | D/E 小时线触发顺序，结论有效但未与降级口径交叉验证 | ⚠️ 需补充 |

### 6.4 Round 实验目录

| 目录 | 路径 | 日期 | 说明 |
|:----|:-----|:----|:-----|
| Round 1 | `/home/tgm/quant/research/dvc-iteration-round1-20260706/` | 07-06 | C 模式 baseline，69~123% 年化 |
| Round 2 | `/home/tgm/quant/research/dvc-iteration-round2-20260706/` | 07-06 | C 模式 + NSHAPE_ONLY，95~171% |
| Round 3 | `/home/tgm/quant/research/dvc-iteration-round3-20260706/` | 07-06 | C 模式 + TOP4 + MA60排序，130~235% |
| Round 4 | `/home/tgm/quant/research/dvc-iteration-round4-execution-20260706/` | 07-06 | C(研究)/D(可执行)/E(可执行) 模式分拆，D/E 156~248% |

---

## 七、下一步计划

### 当前阶段基准：A0-oracle 未来函数标签体系（学术归档）

**阶段 1：A0 基线建立（C 模式）**

1. ✅ **纯倍量切信号画像** — 已完成（见 [A0 形态画像报告](../dvc-rdi-a0-20260708/shape/a0_shape_report.md)）
   - 信号数量、频率、时间分布 ✅
   - 信号后 N 天收益分布（均值、中位数、胜率、盈亏比）✅
   - 最优持有期分析 ✅

2. ✅ **形态结构因子画像** — 已完成（同上）
   - 实体/影线 → 分组回测 ✅
   - 涨幅大小 → 分组回测 ✅
   - 量比 → 分组回测 ✅
   - 切 MA 方式 → 分组回测 ✅

3. ✅ **市场环境画像** — 已完成（参见 V1-B 产物索引，该报告中的 regime 分析已纳入 oracle taxonomy 框架参考）
   - 强势/弱势/震荡 → confirm 收益差异 2.5x ✅
   - MACD 正/负 → 差异显著 ✅
   - 已通过 regime × confirm 交叉分析覆盖

#### 阶段 2：入场路径统一对比（各路径在 A0 上的 baseline）✅ 已有产物索引（V1-B 前置观察）

在 A0 统一基线（同一信号池、同一引擎、同一数据区间）上对比：
- touch_immediate — 已确认 ret_3d=0.92%, MDD 过大
- close_confirm_next_open — confirm_d1 ret_3d=3.16%, gapup 贡献 52%
- confirm_pullback_1d_0pct — 确认回踩后收益归零 (0.29% @ 20d)
- 新增 gapup+confirm 组合路径分析 ✅

**关键结果**:
| 路径 | ret_20d% | 胜率% | 可执行性 |
|:-----|:--------:|:-----:|:--------:|
| confirm_only（无回踩） | **6.69** | 62.1 | ❌ 事后判 |
| gapup+confirm | **4.59** | 55.2 | ⚠️ 开盘执行 |
| confirm_d1 全部 | **3.33** | 53.2 | ⚠️ 尾盘执行 |
| no_gapup+confirm | **2.19** | 51.4 | ✅ 尾盘执行 |
| pb1d0 | **0.04** | 44.9 | ✅ 可执行但无效 |

#### ✅ 代理特征库研究已完成 — 140 个代理特征画像

在 oracle taxonomy 四类标签基础上，构建了 140 个代理特征（110 数值 + 9 分类 + 21 布尔）并评估各特征对四种 oracle 类型的区分力。详见 [Proxy 完整报告](../dvc-rdi-a0-20260708/proxy_features/proxy_feature_report.md)。

**核心结论**：

1. **T+0 传统技术指标没有单变量强分离力**：
   - 最强 T+0 特征为 `atr_pct_14`，Fail vs noise_flat AUC=0.667；`volatility_20d` AUC=0.648
   - 传统 K 线（body_pct、upper_shadow_pct）、KDJ、MACD 柱、量比（volume_ratio）等单变量区分力极弱（|d| < 0.20）
   - **不要过度迷信 MACD/KDJ/上影/量比**——这些指标在 oracle 类型间高度重叠

2. **T+1~T+3 早期路径确认明显更强**：
   - SR vs Fail: `ret_1d` AUC=0.700，`ret_3d` AUC=0.796（3 天可锁定方向）
   - LT vs Fail: `ret_3d` AUC=0.716，`ret_5d` AUC=0.771
   - **信号后的收益确认是当前已知最强的 oracle 类型区分特征**

3. **波动率/体量陷阱方向突出**：
   - failure 组具有更高的波动率、更大的实体、更远的均线偏离、更高的成交额
   - `atr_pct_14` × `turnover` × `body_pct` 联合使用有潜力
   - ADX 未计算（跳过），列为待补

**产物**：

| 产物 | 路径 |
|:----|:-----|
| 完整特征矩阵 | `../dvc-rdi-a0-20260708/proxy_features/proxy_feature_matrix.parquet` |
| 特征重要性 | `../dvc-rdi-a0-20260708/proxy_features/proxy_feature_importance.csv` |
| 分箱 Lift 表 | `../dvc-rdi-a0-20260708/proxy_features/proxy_feature_bucket_lift.csv` |
| 类型统计 | `../dvc-rdi-a0-20260708/proxy_features/proxy_feature_type_stats.csv` |
| 特征元数据 | `../dvc-rdi-a0-20260708/proxy_features/proxy_feature_metadata.csv` |
| 完整报告 | `../dvc-rdi-a0-20260708/proxy_features/proxy_feature_report.md` |
| 构建脚本 | `../dvc-rdi-a0-20260708/proxy_features/build_proxy_feature_library.py` |

#### ✅ 最终策略矩阵验证已完成（2026-07-09）— 裁决：放弃 DVC A0 独立策略化

经过 57 个策略组合 × 165K 信号 × 12 年的完整测试：
- 基线 (T+1 open, 5d hold): mean +0.38%, median -0.31%, wr 47.1%
- 最优过滤+退出 (F2_F_let_run): mean +2.28%, median -0.36%
- 唯一中位数正策略 (E1_time_5d): mean +1.85%, median +0.86%, wr 56.0% — 但本质是通用动量过滤, 非 DVC 特有
- TP 策略的"高收益"是数学假象（给股票 20 天等反弹）
- 所有策略组合 MDD > -60%

**❌ 最终裁决: DVC A0 信号基底太弱, 无法独立策略化.** 建议资源重定向.

#### 下一步方向

1. **放弃 DVC A0 作为独立信号源** — 165K 信号 × 12 年 × 5632 只股票已提供充分统计显著性, 无需继续测试
2. **E1 过滤器("排除首日大跌")可作为通用工具** — 独立于 DVC, 适用于其他信号框架
3. **月MACD环境过滤可保留** — 作为多因子框架中的一个条件, 非独立策略
4. **如果未来有新的 DVC 变种**(不同倍率/实体要求/成交量定义)可重新测试 — 但必须从 A 层验证正预期开始
5. **研究资源应转向更有前景的方向** — 其他信号源或复合框架

---

## 八、关联文档

- [倍量切双红系统性研究](./dvc-entry-methods-research.md) — 旧版主研究页面（需修正）
- [共享资金账户回测](./dvc-shared-account-backtest.md) — 理想版回测报告（需标注不可执行）
- [Round 4 可执行口径](./dvc-executable-intraday-round4-20260706.md) — D/E 小时线触发顺序
- [DVC 审计报告](/home/tgm/quant/scripts/dvc_audit_report.md) — 口径可信度全量审计
- [Wiki 修正草案](/home/tgm/quant/research/dvc-wiki-correction-draft-20260708.md) — 修正方案、分层金字塔、禁止事项
- [最终策略矩阵报告](/home/tgm/quant/research/dvc-rdi-a0-20260709/final_strategy/final_strategy_report.md) — 2026-07-09 最终裁决: 放弃 DVC A0 独立策略化
