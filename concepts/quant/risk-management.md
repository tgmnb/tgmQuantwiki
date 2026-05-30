---
title: Risk Management
created: 2026-04-23
updated: 2026-04-23
type: concept
tags: [quant, risk, position-sizing]
confidence: high
---

# Risk Management

## 核心理念

> **"始终先想风险，再想利润。"** — 生存比赚钱更重要。

风险管理是量化交易的最后一环，也是最重要的一环。再好的策略，没有严格的风险管理，也会因一次极端行情爆仓。

## 三层风险控制

```
总仓位风险 (< 2%/笔)
  └─ 单品种仓位 (凯利公式 / 固定比例 / ATR)
      └─ 止损 (技术止损 / ATR 止损 / 时间止损)
```

## 1. 固定比例仓位 (Fixed Fractional)

每笔交易承担固定比例的总资金风险。

```python
def fixed_fractional_position(account_size: float, risk_per_trade: float,
                               entry_price: float, stop_loss: float) -> float:
    """
    account_size: 账户权益
    risk_per_trade: 每笔风险比例 (如 0.02 = 2%)
    entry_price: 入场价格
    stop_loss: 止损价格
    """
    risk_amount = account_size * risk_per_trade
    risk_per_share = abs(entry_price - stop_loss)
    position_size = risk_amount / risk_per_share
    return position_size
```

## 2. ATR 仓位 (Volatility-Adjusted)

根据市场波动率调整仓位，波动大时仓位轻，波动小时仓位重。

```python
def atr_position_size(account_size: float, risk_per_trade: float,
                      entry_price: float, atr: float, atr_multiplier: float = 2) -> float:
    stop_loss_pips = atr * atr_multiplier
    risk_amount = account_size * risk_per_trade
    position_size = risk_amount / stop_loss_pips
    return position_size
```

## 3. 凯利公式 (Kelly Criterion)

最优仓位公式，但实践中建议用 **半凯利 (Kelly/2)** 以降低波动。

```python
def kelly_position(win_rate: float, avg_win: float, avg_loss: float) -> float:
    """
    Kelly % = W - (1-W) / (avg_win / avg_loss)
    W = 胜率, avg_win = 平均盈利, avg_loss = 平均亏损
    """
    if avg_loss == 0:
        return 0
    win_ratio = avg_win / avg_loss
    kelly = win_rate - ((1 - win_rate) / win_ratio)
    return max(0, kelly)  # 半凯利: kelly / 2
```

## 止损体系

| 类型 | 说明 | 适用场景 |
|------|------|----------|
| 技术止损 | 跌破支撑/均线/趋势线 | 趋势策略 |
| ATR 止损 | 价格变动超过 N 倍 ATR | 波动率适配 |
| 时间止损 | 入场后 N 根 K 线未盈利 | 所有策略 |
| 比例止损 | 亏损超过账户 X% | 紧急风控 |

```python
def calculate_stops(entry_price: float, atr: float,
                    regime: str, strategy_type: str) -> dict:
    stops = {}
    if strategy_type == "trend":
        stops["atr_2x"] = entry_price - 2 * atr
        stops["atr_3x"] = entry_price - 3 * atr
    elif strategy_type == "mean_reversion":
        stops["bollinger"] = entry_price - 2 * atr  # 窄止损
    return stops
```

## 最大回撤控制

```python
def check_drawdown_limits(current_equity: float, peak_equity: float,
                          max_drawdown_pct: float = 0.15) -> bool:
    drawdown = (peak_equity - current_equity) / peak_equity
    if drawdown > max_drawdown_pct:
        return False  # 触及红线，停止交易
    return True
```

## 风险管理决策树

```
新开仓前检查:
  1. 单笔风险 < 账户 2%?
  2. 总仓位风险 < 账户 10%?
  3. 当前回撤 < 15%?
  4. 不是新闻/事件窗口?
  └─ 全部通过 -> 可以开仓
```

## 相关概念

- [[strategy-prototypes]] — 策略必须有配套止损
- [[trading-regime]] — 不同 Regime 用不同仓位
- [[technical-analysis]] — 支撑阻力用于设置止损
