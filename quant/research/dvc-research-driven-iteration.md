---
title: DVC 研究驱动迭代主页面
created: 2026-07-08
updated: 2026-07-08 (V1-A added)
type: research-index
tags: [quant, dvc, research-driven-iteration, RDI, double-volume-cut, master-index, v1b-confirm-path, v1a-weak-rebound]
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
| V1 | ideal_close_confirm | close>DVC close → DVC close 买入 | **理论上限** | +272.73% | 19.20% | 2014~2026-07-01 | ❌ 已降级 | 不可执行，仅作上限参考 |
| V2 | 实盘口径 touch/next/same | touch: high>=close; next: 收盘确认→次日开; same: 收盘确认→当日收盘 | **可执行** | 5.58%~15.65% | 74.66%~83.38% | 2014~2026-07-01 | ⚠️ 已证弱 | touch 收益尚可但 MDD 过大；next_open 收益过低 |
| V3 | 回踩 pb1d0/pb3d0 | close 确认后 1d/3d 回踩 DVC close | **可执行** | 10.29%~11.77% | 36.84%~55.46% | 2014~2026-07-01 | ⚠️ 可执行候选 | pb1d0 (B4) MDD 最低，推荐底座；D/E 验收后收益偏低 |
|| V4 | B4_R60 / pb1d0续研 | 小矩阵 C 模式改善 | C 模式 | ?? | ?? | 2014~2026-07-01 | 🔬 待严验/标待跑 | C 模式有改善迹象，D/E 严格验收尚未完成 |
|| V1-B | 确认路径型 DVC 机制画像 | 聚焦 confirm_d1 内部差异：右尾继续 vs 当日透支 | C 模式研究 | — | — | 2014-01 ~ 2026-07 | ✅ 已完成确认路径机制画像+分布统计+诊断框架 | 45%信号触发confirm_d1；gapup+confirm 的 ret_20d=4.59%；回踩后收益归零(0.29%)；市场环境是致胜维度(强势2.5x)；上影缩短预示右尾；当日大阳线透支后续；详见 v1b_confirm_path_report.md |
|| V1-A | 弱势反抽型 DVC 机制画像 | 聚焦弱势/超跌环境下的 DVC 反抽特征：MA25/MA60弱势、距MA60深度、前N日跌幅 | C 模式研究 | — | — | 2014-01 ~ 2026-07 | ✅ 已完成弱势反抽机制画像+形态交叉分布+tail诊断 | 22%信号(36.6K)在综合弱势条件下；弱势反抽候选60d胜率48.32%(vs全量43.40%)，60d>20% 18.92%；距MA60<-20%组60d>20% 20.28%，60d胜率50.39%；弱势+前5日超跌(prev<-10%)的5d胜率100%但仅12信号；详见 v1a_weak_rebound_report.md |

### 版本说明

- **V0 / A0**：纯倍量切（不加双红过滤，不加任何入场优化）。这是后续所有研究的基线起点。**当前主要研究任务**。
- **V1-A**：弱势反抽型 DVC 机制画像（同层并行研究）。聚焦「弱势/超跌环境下 DVC 信号是否呈现更强的反抽特性？反抽是短命的还是可能转主升浪？」。**已完成 C 模式画像，进入机制诊断阶段。**
- **V1-B**：确认路径型 DVC 机制画像（同层并行研究）。聚焦「confirm_d1 内部差异分析：gapup/回踩/市场环境/形态透支」。**已完成 C 模式画像，确认机制诊断完成。**
- **V1**：旧理想版「突破收盘买入」。已降级为理论上限，不作为推荐依据。
- **V2**：touch_immediate / close_confirm_next_open / close_confirm_same_close。收益率低或回撤大，不作为主推底座。
- **V3**：confirm_pullback_1d_0pct / confirm_pullback_3d_0pct。当前最优底座候选，但 D/E 验收后收益偏低。
- **V4**：B4_R60/pb1d0 小矩阵优化，C 模式有改善。尚未通过 D/E 严格验收，需重跑验证。

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

### 4.2 市场环境（含弱势反抽）
- 指数 MA（沪深300/中证1000 的 MA25/MA60 状态）
- 指数 MACD（周/月线红绿柱）
- 月线状态（上涨/下跌/震荡月 vs 倍量切信号频率与胜率）
- **弱势反抽机制画像**：✅ V1-A 已完成 — 见 [V1-A 弱势反抽报告](../dvc-rdi-v1-20260708/weak_rebound/v1a_weak_rebound_report.md) | [CSV](../dvc-rdi-v1-20260708/weak_rebound/v1a_weak_rebound_profile.csv)
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
|- **状态**：✅ V1-B 确认路径机制画像已完成 — 见 [V1-B 确认路径报告](../dvc-rdi-v1-20260708/confirm_path/v1b_confirm_path_report.md)
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
| A0 分布分析脚本 | `/home/tgm/quant/research/dvc-rdi-a0-20260708/shape/a0_long_horizon_distribution.py` | 分布统计+分组画像分析脚本 | A0 研究 |
|| V1-B 确认路径画像(CSV) | `/home/tgm/quant/research/dvc-rdi-v1-20260708/confirm_path/v1b_confirm_path_profile.csv` | 11路径×7周期全分布统计（p25/p50/p75/p90/p95/p99、右尾/左尾比例、偏度峰度） | V1-B 机制研究 |
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
| D/E 报告 | `/home/tgm/quant/research/dvc-executable-rerun-20260708/executable_rerun_report.md` | D/E 可执行回测完整报告 | D/E 可执行 |

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

### 当前进度：V1-A 弱势反抽机制画像 ✅ ｜ V1-B 确认路径机制画像 ✅ —— 两个同层并行研究均已完成

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

3. ✅ **市场环境画像** — 已完成（见 V1-B 确认路径报告，结合 regime 分析）
   - 强势/弱势/震荡 → confirm 收益差异 2.5x ✅
   - MACD 正/负 → 差异显著 ✅
   - 已通过 regime × confirm 交叉分析覆盖

#### 阶段 2：入场路径统一对比（各路径在 A0 上的 baseline）✅ V1-B 已完成

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

#### 阶段 3：方向决策 🔬

综合阶段 1 + 阶段 2 + V1-A/V1-B 的证据：

**V1-A 弱势反抽方向**:
- 综合弱势反抽候选在 20/60d 均有 2-4x 的均值改善（60d 均值 0.04 vs 全量 0.02）
- 距MA60 深度是反抽核心指标：<-20% 组 60d 胜率 50.39%，60d>20% 20.28%
- 形态交叉在弱势状态下分组差距小，弱势环境本身是主要 alpha 来源
- 弱势+超跌组合样本少但效应极强，值得进一步验证
- **方向价值：弱势环境可作为「权重乘数」或「过滤层」，叠加在其他入场路径上**

**V1-B 确认路径方向**:
- confirm_d1 + gapup 过滤是最有前景的入口方向（ret_3d=3.16%, ret_20d=4.59%）
- 回踩路径（pb1d0）已证伪——回踩后收益归零
- 市场环境是区分 confirm 质量的致胜维度（强势 4.43% vs 弱势 1.95% @ 20d）

**综合诊断**: 两个方向有互补性 —— V1-B 聚焦入口路径（怎么买），V1-A 聚焦环境条件（什么时候买）。如果用 V1-B 的 confirm_d1 入口 + V1-A 的弱势/MA60 环境过滤，可能形成更强的组合效果。**但此组合需要在 D/E 模式中验证，不在本 C 模式研究范围内。**

**待定：选出一个或组合方向升级为 V5 底座，再展开下一层（退出、仓位、执行竞争）研究。**

---

## 八、关联文档

- [倍量切双红系统性研究](./dvc-entry-methods-research.md) — 旧版主研究页面（需修正）
- [共享资金账户回测](./dvc-shared-account-backtest.md) — 理想版回测报告（需标注不可执行）
- [Round 4 可执行口径](./dvc-executable-intraday-round4-20260706.md) — D/E 小时线触发顺序
- [DVC 审计报告](/home/tgm/quant/scripts/dvc_audit_report.md) — 口径可信度全量审计
- [Wiki 修正草案](/home/tgm/quant/research/dvc-wiki-correction-draft-20260708.md) — 修正方案、分层金字塔、禁止事项
