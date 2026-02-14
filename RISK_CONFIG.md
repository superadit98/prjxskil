# PRJX Liquidity Manager - Risk Configuration

## Overview

This document defines all risk parameters for the liquidity management system. Risk management is the cornerstone of sustainable DeFi liquidity provision.

---

## Risk Parameters

### Global Risk Settings

```json
{
  "risk_profile": "MODERATE",

  "portfolio_limits": {
    "max_total_exposure_usd": 50000,
    "max_single_position_usd": 15000,
    "max_single_position_percent": 30,
    "max_pool_exposure_percent": 50,
    "min_reserve_percent": 15,
    "max_correlated_exposure_percent": 40
  },

  "position_sizing": {
    "default_position_size_percent": 15,
    "min_position_size_usd": 500,
    "max_position_size_usd": 15000,
    "sizing_by_confidence": {
      "high": 25,
      "medium": 15,
      "low": 10
    }
  },

  "stop_loss": {
    "enabled": true,
    "default_percent": 15,
    "trailing_enabled": true,
    "trailing_trigger_percent": 10,
    "trailing_distance_percent": 5
  },

  "take_profit": {
    "enabled": true,
    "default_percent": 25,
    "partial_enabled": true,
    "partial_levels": [
      {"percent": 15, "exit_ratio": 0.3},
      {"percent": 25, "exit_ratio": 0.4},
      {"percent": 40, "exit_ratio": 0.3}
    ]
  },

  "impermanent_loss": {
    "alert_threshold_percent": 5,
    "warning_threshold_percent": 10,
    "critical_threshold_percent": 15,
    "auto_exit_threshold_percent": 20
  },

  "volatility": {
    "high_volatility_threshold": 15,
    "extreme_volatility_threshold": 30,
    "position_pause_in_extreme": true
  }
}
```

---

## Risk Profiles

### Conservative Profile
```
┌─────────────────────────────────────────────┐
│           CONSERVATIVE PROFILE              │
├─────────────────────────────────────────────┤
│ Max Single Position:     20%                │
│ Default Stop Loss:       10%                │
│ Default Take Profit:     15%                │
│ IL Alert Threshold:      5%                 │
│ Max Volatility Allowed:  Medium             │
│ Auto-Rebalance:          Disabled           │
│ Preferred Pools:         Stable/Stable      │
│ Range Strategy:          Wide               │
└─────────────────────────────────────────────┘
```

### Moderate Profile (Default)
```
┌─────────────────────────────────────────────┐
│           MODERATE PROFILE                  │
├─────────────────────────────────────────────┤
│ Max Single Position:     30%                │
│ Default Stop Loss:       15%                │
│ Default Take Profit:     25%                │
│ IL Alert Threshold:      10%                │
│ Max Volatility Allowed:  Medium-High        │
│ Auto-Rebalance:          Enabled            │
│ Preferred Pools:         Major pairs        │
│ Range Strategy:          Balanced           │
└─────────────────────────────────────────────┘
```

### Aggressive Profile
```
┌─────────────────────────────────────────────┐
│           AGGRESSIVE PROFILE                │
├─────────────────────────────────────────────┤
│ Max Single Position:     50%                │
│ Default Stop Loss:       25%                │
│ Default Take Profit:     50%                │
│ IL Alert Threshold:      15%                │
│ Max Volatility Allowed:  High               │
│ Auto-Rebalance:          Enabled            │
│ Preferred Pools:         All pairs          │
│ Range Strategy:          Narrow             │
└─────────────────────────────────────────────┘
```

---

## Stop Loss System

### Stop Loss Types

#### 1. Fixed Stop Loss
```
Exit when total loss reaches threshold.
Example: SL at 15% → Exit when position down 15%
```

#### 2. Trailing Stop Loss
```
Stop loss follows price upward, locks in profits.

Entry Price: $100
SL: 15% trailing

Price → $120 → SL moves to $102 (locks $2 profit)
Price → $110 → SL stays at $102 (triggers exit)
Price → $130 → SL moves to $110.50

Best for: Trending markets
```

#### 3. Time-Based Stop Loss
```
Exit if position underperforms for too long.

Example: If P&L < 5% after 7 days, exit.
Reason: Capital efficiency
```

#### 4. IL-Based Stop Loss
```
Exit if impermanent loss exceeds threshold.

Example: Exit if IL > 12%
Reason: Protect against divergence loss
```

### Stop Loss Calculation

```javascript
function calculateStopLoss(position, config) {
  const currentValue = position.current_value;
  const entryValue = position.entry_value;
  const pnl_percent = ((currentValue - entryValue) / entryValue) * 100;

  let triggered = false;
  let trigger_reason = null;

  // Fixed SL
  if (pnl_percent <= -config.stop_loss.default_percent) {
    triggered = true;
    trigger_reason = 'FIXED_SL';
  }

  // Trailing SL
  if (config.stop_loss.trailing_enabled) {
    const peakValue = position.peak_value;
    const trailingSL = peakValue * (1 - config.stop_loss.trailing_distance_percent / 100);
    if (currentValue <= trailingSL) {
      triggered = true;
      trigger_reason = 'TRAILING_SL';
    }
  }

  // IL-based SL
  if (position.impermanent_loss >= config.impermanent_loss.auto_exit_threshold_percent) {
    triggered = true;
    trigger_reason = 'IL_THRESHOLD';
  }

  return { triggered, trigger_reason };
}
```

---

## Take Profit System

### Take Profit Types

#### 1. Single Target
```
Exit entire position at target.
TP: 25% → Exit all when +25% reached
```

#### 2. Partial Targets (Recommended)
```
Scale out at multiple levels.

Level 1: +15% → Exit 30% of position
Level 2: +25% → Exit 40% of position
Level 3: +40% → Exit remaining 30%

Benefits:
- Lock in profits gradually
- Capture further upside
- Reduce regret from early exits
```

#### 3. Dynamic TP
```
Adjust TP based on conditions.

If volatility > 20%: TP = 35% (higher target)
If volatility < 10%: TP = 15% (quick profit)
If APR > 50%: TP = 40% (let winners run)
```

---

## Impermanent Loss Management

### IL Monitoring Levels

| IL Level | Action | Alert |
|----------|--------|-------|
| 0-5% | Normal operations | None |
| 5-10% | Increased monitoring | Yellow alert |
| 10-15% | Consider exit | Orange alert |
| 15-20% | Recommend exit | Red alert |
| >20% | Auto-exit | Critical alert |

### IL Protection Strategies

#### Strategy 1: Wide Range
```
Wider range = Lower IL risk
Trade-off: Lower fee earnings

Recommended for:
- High volatility pairs
- Uncertain market direction
- Conservative risk profile
```

#### Strategy 2: Correlated Pairs
```
Use pairs that move together.
Example: ETH/stETH, wBTC/tBTC
IL risk: Minimal
Trade-off: Lower APR typically
```

#### Strategy 3: Hedging
```
Hedge LP position with:
- Perpetual shorts
- Options
- Inverse LP position

Complex but effective.
```

---

## Volatility Management

### Volatility Levels

| Level | Daily Change | Action |
|-------|--------------|--------|
| Low | < 5% | Narrow range, maximize fees |
| Medium | 5-15% | Standard range |
| High | 15-30% | Wide range, smaller size |
| Extreme | > 30% | Pause new entries, monitor |

### Volatility Response Protocol

```
┌─────────────────────────────────────────────────────────────┐
│              VOLATILITY RESPONSE PROTOCOL                   │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  DETECT: Price moves > X% in 1 hour                        │
│     │                                                       │
│     ▼                                                       │
│  ANALYZE: Is this sustained or spike?                      │
│     │                                                       │
│     ├──► SPIKE ──► Monitor, no action                       │
│     │                                                       │
│     └──► SUSTAINED ──► Check positions                     │
│                              │                              │
│                              ▼                              │
│                    POSITION STATUS CHECK                    │
│                              │                              │
│         ┌────────────────────┼────────────────────┐        │
│         ▼                    ▼                    ▼        │
│     IN RANGE          NEAR EDGE            OUT OF RANGE   │
│         │                    │                    │        │
│         ▼                    ▼                    ▼        │
│    Widen range        Alert user          Urgent alert    │
│    if high vol        suggest action      suggest exit    │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

---

## Position Health Score

### Health Score Calculation

```javascript
function calculateHealthScore(position) {
  let score = 100;

  // In-range bonus (max +20)
  if (position.in_range) {
    score += 20;
  } else {
    score -= 30; // Out of range penalty
  }

  // IL impact (max -30)
  score -= position.il_percent * 1.5;

  // PnL impact (max ±20)
  score += Math.min(position.pnl_percent, 20);
  score -= Math.max(position.pnl_percent, -20);

  // Time in position (normalize)
  const optimalTime = 72; // hours
  const timeDiff = Math.abs(position.hold_time_hours - optimalTime);
  score -= timeDiff / 24; // Penalty for too long/short

  // Fee earnings bonus (max +10)
  const feeYield = (position.fees_earned / position.entry_value) * 100;
  score += Math.min(feeYield * 2, 10);

  return Math.max(0, Math.min(100, score));
}
```

### Health Score Interpretation

| Score | Status | Action |
|-------|--------|--------|
| 90-100 | Excellent | Maintain position |
| 70-89 | Good | Monitor normally |
| 50-69 | Fair | Consider adjustments |
| 30-49 | Poor | Review and possibly exit |
| 0-29 | Critical | Exit recommended |

---

## Risk Alerts

### Alert Configuration

```json
{
  "alerts": {
    "position_health": {
      "fair_threshold": 70,
      "poor_threshold": 50,
      "critical_threshold": 30
    },

    "price_movement": {
      "hourly_alert_percent": 5,
      "daily_alert_percent": 10,
      "weekly_alert_percent": 20
    },

    "apr_change": {
      "drop_alert_percent": 30,
      "surge_alert_percent": 50
    },

    "tvl_change": {
      "drop_alert_percent": 20,
      "surge_alert_percent": 50
    },

    "delivery": {
      "channels": ["telegram", "whatsapp"],
      "quiet_hours": {
        "enabled": true,
        "start": "23:00",
        "end": "07:00",
        "critical_override": true
      }
    }
  }
}
```

### Alert Templates

#### Health Alert
```
⚠️ POSITION HEALTH ALERT
━━━━━━━━━━━━━━━━━━━━━━━━━
Pool: {POOL}
Health Score: {SCORE}/100

Position Status:
├── Value: ${VALUE}
├── P&L: {PNL}%
├── IL: {IL}%
├── In Range: {IN_RANGE}

📊 AI Analysis:
{ANALYSIS}

🎯 Recommended Action:
{ACTION}

Confidence: {CONFIDENCE}%
```

#### Volatility Alert
```
🔥 VOLATILITY ALERT
━━━━━━━━━━━━━━━━━━━━━━━━━
Pool: {POOL}
Price Change: {CHANGE}% in {TIMEFRAME}

Your Position:
├── Range: {LOWER} - {UPPER}
├── Current Price: {PRICE}
├── Distance to Edge: {DISTANCE}%

⚠️ Action Required:
{ACTION}

Historical Similar Events:
{HISTORY}
```

---

## Emergency Protocols

### Emergency Stop
```
Triggered by:
- User command: "prjx emergency stop"
- Critical threshold breach
- API/Connection failure

Actions:
1. Halt all automated operations
2. Cancel pending transactions
3. Alert user immediately
4. Preserve current state
```

### Forced Exit
```
Triggered by:
- User command: "prjx emergency exit"
- Multiple critical alerts
- System malfunction

Actions:
1. Remove all liquidity
2. Collect all fees
3. Return to base currency
4. Send full report
```

---

## Risk Reporting

### Daily Risk Report
```
📊 DAILY RISK REPORT
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Date: {DATE}

Portfolio Risk Metrics:
├── Total Exposure: ${EXPOSURE}
├── Risk Score: {SCORE}/100
├── Max Drawdown: {DRAWDOWN}%
└── VaR (95%): ${VAR}

Position Breakdown:
┌─────────┬────────┬────────┬────────┐
│ Pool    │ Risk   │ IL     │ Health │
├─────────┼────────┼────────┼────────┤
│ POOL_1  │ LOW    │ 2.1%   │ 85     │
│ POOL_2  │ MEDIUM │ 5.5%   │ 72     │
│ POOL_3  │ HIGH   │ 8.2%   │ 58     │
└─────────┴────────┴────────┴────────┘

⚠️ Risk Alerts Today: {COUNT}
🎯 Actions Taken: {ACTIONS}

Recommendations:
{RECOMMENDATIONS}
```

---

## Configuration Commands

| Command | Description |
|---------|-------------|
| `prjx risk profile [name]` | Set risk profile |
| `prjx risk sl [percent]` | Set default stop loss |
| `prjx risk tp [percent]` | Set default take profit |
| `prjx risk il [percent]` | Set IL alert threshold |
| `prjx risk max [amount]` | Set max exposure |
| `prjx risk report` | Generate risk report |
| `prjx risk status` | Show current settings |
