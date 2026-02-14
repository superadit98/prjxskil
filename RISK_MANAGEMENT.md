# Risk Management System - PRJX Liquidity Manager

## Overview

Comprehensive risk management framework that protects capital while maximizing returns. The system continuously monitors, assesses, and mitigates risks across all liquidity positions.

## Risk Framework

```
┌─────────────────────────────────────────────────────────────────┐
│                    RISK MANAGEMENT HIERARCHY                    │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│  LEVEL 1: PORTFOLIO RISK (Highest Priority)                    │
│  ├── Total Exposure Limits                                      │
│  ├── Portfolio Correlation                                      │
│  └── Maximum Drawdown Protection                               │
│                                                                  │
│  LEVEL 2: POSITION RISK                                         │
│  ├── Position Size Limits                                       │
│  ├── Single Pool Exposure                                       │
│  └── Stop-Loss / Take-Profit                                   │
│                                                                  │
│  LEVEL 3: POOL RISK                                             │
│  ├── Impermanent Loss Monitoring                               │
│  ├── Range Boundaries                                           │
│  └── Liquidity Depth                                            │
│                                                                  │
│  LEVEL 4: MARKET RISK                                           │
│  ├── Volatility Monitoring                                      │
│  ├── Trend Analysis                                             │
│  └── External Events                                            │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

## Risk Parameters

### Portfolio Level
```yaml
portfolio_risk:
  # Maximum total value at risk
  max_total_exposure: 50000           # USD

  # Maximum percentage of net worth
  max_portfolio_percentage: 30        # percent

  # Maximum correlated exposure
  max_correlated_exposure: 40         # percent

  # Maximum drawdown before pause
  max_drawdown: 20                    # percent

  # Daily loss limit
  max_daily_loss: 5                   # percent

  # Recovery mode threshold
  recovery_threshold: 15              # percent loss
```

### Position Level
```yaml
position_risk:
  # Maximum single position size
  max_position_size: 10000            # USD

  # Maximum percentage of portfolio
  max_position_percentage: 25         # percent

  # Stop-loss settings
  stop_loss:
    enabled: true
    default_percent: 15               # percent
    trailing: true                     # Move up with profit
    trailing_distance: 5               # percent

  # Take-profit settings
  take_profit:
    enabled: true
    default_percent: 50               # percent
    partial_exit: true                 # Take partial profits
    partial_percent: 50                # % to take at target

  # Position timeout
  max_hold_time: 720                   # hours (30 days)
  force_review: 168                    # hours (7 days)
```

### Pool Level
```yaml
pool_risk:
  # Impermanent loss tolerance
  max_il_tolerance: 10                 # percent

  # Range settings
  min_range_width: 5                   # percent from current price
  max_range_width: 50                  # percent from current price

  # Liquidity requirements
  min_pool_liquidity: 100000           # USD
  min_pool_volume_24h: 10000           # USD

  # APR thresholds
  min_apr: 15                          # percent
  max_apr_warning: 200                 # percent (suspicious if too high)
```

## Risk Monitoring

### Real-time Metrics
```yaml
monitoring:
  # Check intervals
  price_check_interval: 1              # minute
  position_check_interval: 5           # minutes
  portfolio_check_interval: 15         # minutes

  # Alert thresholds
  il_alert_threshold: 5                # percent
  out_of_range_alert: true
  price_movement_alert: 10             # percent in 1 hour

  # Health score calculation
  health_score_weights:
    in_range: 0.3
    il_within_tolerance: 0.3
    fees_vs_il: 0.2
    price_stability: 0.2
```

### Health Score Calculation
```
Health Score = (W1 × Range Score) + (W2 × IL Score) + (W3 × Fee Score) + (W4 × Stability Score)

Where:
- Range Score: 100 if in range, 0 if completely out
- IL Score: 100 - (IL% × 10), min 0
- Fee Score: (Fees earned / IL) × 100, capped at 100
- Stability Score: Based on price volatility, 0-100

Rating:
- 80-100: Excellent 🟢
- 60-79: Good 🟡
- 40-59: Fair 🟠
- 0-39: Poor 🔴
```

## Risk Actions

### Automatic Actions
```yaml
auto_actions:
  # When IL exceeds tolerance
  il_breach:
    - alert_user
    - calculate_optimal_exit
    - prepare_exit_transaction
    - execute_if_auto_enabled: true

  # When position out of range
  out_of_range:
    - alert_user
    - check_duration_out_of_range
    - recommend_rebalance_or_exit

  # When stop-loss triggered
  stop_loss:
    - immediate_alert
    - execute_exit: true
    - log_reason

  # When take-profit triggered
  take_profit:
    - alert_user
    - execute_partial_or_full_exit
    - record_success

  # When daily loss limit hit
  daily_loss_limit:
    - pause_all_trading
    - alert_user
    - require_manual_reset

  # When max drawdown hit
  max_drawdown:
    - emergency_pause_all
    - alert_user_urgent
    - force_review_required
```

### Emergency Procedures
```yaml
emergency:
  # Kill switch
  kill_switch:
    enabled: true
    command: "EMERGENCY STOP"
    action: pause_all_operations
    require_confirmation: false

  # Circuit breaker
  circuit_breaker:
    consecutive_losses: 3
    action: pause_and_review
    auto_resume: false

  # Flash crash protection
  flash_crash:
    price_drop_threshold: 20           # percent in 5 minutes
    action: pause_trading
    duration: 30                       # minutes
```

## Risk Reports

### Position Risk Report
```
╔═══════════════════════════════════════════════════════════════╗
║                    POSITION RISK REPORT                        ║
╠═══════════════════════════════════════════════════════════════╣
║ Position: HYPE/USD₮0                                          ║
║ Value: $5,000 | Entry: $2.50 | Current: $2.35                ║
║                                                                ║
║ RISK METRICS:                                                  ║
║ ┌─────────────────────┬─────────────┬─────────────────────┐   ║
║ │ Metric              │ Current     │ Status              │   ║
║ ├─────────────────────┼─────────────┼─────────────────────┤   ║
║ │ Impermanent Loss    │ 4.2%        │ 🟡 Warning          │   ║
║ │ Range Position      │ 85% in      │ 🟢 Good             │   ║
║ │ Health Score        │ 72/100      │ 🟡 Fair             │   ║
║ │ Stop-Loss Distance  │ -8.5%       │ 🟢 Safe             │   ║
║ │ Take-Profit Dist.   │ +35%        │ 🟢 On Track         │   ║
║ │ Hold Duration       │ 3 days      │ 🟢 Normal           │   ║
║ └─────────────────────┴─────────────┴─────────────────────┘   ║
║                                                                ║
║ WARNINGS:                                                      ║
║ ⚠️ IL approaching 5% threshold                                 ║
║ ⚠️ Price near lower range boundary                            ║
║                                                                ║
║ RECOMMENDATIONS:                                               ║
║ → Consider tightening stop-loss to -10%                       ║
║ → Monitor for potential rebalance                             ║
║ → IL expected to improve if price recovers                    ║
╚═══════════════════════════════════════════════════════════════╝
```

### Portfolio Risk Report
```
╔═══════════════════════════════════════════════════════════════╗
║                   PORTFOLIO RISK SUMMARY                       ║
╠═══════════════════════════════════════════════════════════════╣
║ Total Portfolio Value: $15,245                                 ║
║ Total Exposure: $10,000 (65.5% of portfolio)                  ║
║                                                                ║
║ EXPOSURE BREAKDOWN:                                            ║
║ ┌──────────────────┬─────────────┬─────────────┬───────────┐  ║
║ │ Pool             │ Exposure    │ % Portfolio │ Risk Lvl  │  ║
║ ├──────────────────┼─────────────┼─────────────┼───────────┤  ║
║ │ HYPE/USD₮0       │ $5,000      │ 32.8%       │ 🟡 Medium │  ║
║ │ HYPE/kHYPE       │ $3,000      │ 19.7%       │ 🟢 Low    │  ║
║ │ HYPE/UBTC        │ $2,000      │ 13.1%       │ 🟠 High   │  ║
║ └──────────────────┴─────────────┴─────────────┴───────────┘  ║
║                                                                ║
║ CORRELATION ANALYSIS:                                          ║
║ • HYPE pairs correlation: 75% (High)                          ║
║ • Effective independent exposure: $6,500                      ║
║                                                                ║
║ AGGREGATE RISK:                                                ║
║ • Average IL: 3.2%                                             ║
║ • Average Health: 78/100                                       ║
║ • Positions at Risk: 1/3                                       ║
║ • Stop-Loss at Risk: $0                                        ║
║                                                                ║
║ LIMITS STATUS:                                                 ║
║ ✓ Total Exposure: $10,000 / $50,000 (20%)                     ║
║ ✓ Daily Loss: +$245 / -$750 (within limit)                    ║
║ ✓ Max Position: $5,000 / $10,000 (50%)                        ║
║ ⚠ Correlated Exposure: 75% / 40% (EXCEEDED)                   ║
║                                                                ║
║ RECOMMENDATIONS:                                               ║
║ → Reduce HYPE concentration - add non-HYPE pair               ║
║ → Consider USD₮0/UBTC pool for diversification               ║
╚═══════════════════════════════════════════════════════════════╝
```

## Risk-based Decision Flow

### Entry Decision
```
┌─────────────────────────────────────────────────────────────────┐
│                     ENTRY DECISION FLOW                         │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│  1. CHECK PORTFOLIO LIMITS                                      │
│     ├── Total exposure < max?                                   │
│     ├── Daily loss within limit?                                │
│     └── Enough uncorrelated capacity?                          │
│           │                                                      │
│           ▼                                                      │
│  2. CHECK POSITION LIMITS                                       │
│     ├── Position size < max?                                    │
│     ├── Pool not overexposed?                                   │
│     └── Existing positions healthy?                            │
│           │                                                      │
│           ▼                                                      │
│  3. CHECK POOL RISK                                             │
│     ├── IL projection acceptable?                               │
│     ├── Pool liquidity sufficient?                              │
│     ├── APR sustainable?                                        │
│     └── Range appropriate for volatility?                      │
│           │                                                      │
│           ▼                                                      │
│  4. RISK SCORING                                                │
│     ├── Calculate risk score (0-100)                           │
│     ├── Score >= threshold?                                     │
│     └── Learning confirms decision?                            │
│           │                                                      │
│           ▼                                                      │
│  5. EXECUTE or REJECT                                           │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

### Exit Decision
```
┌─────────────────────────────────────────────────────────────────┐
│                      EXIT DECISION FLOW                         │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│  TRIGGER CHECK:                                                  │
│  ├── Stop-loss hit? → IMMEDIATE EXIT                           │
│  ├── Take-profit hit? → PARTIAL/FULL EXIT                      │
│  ├── IL exceeded? → EVALUATE EXIT                              │
│  ├── Out of range too long? → EVALUATE REBALANCE               │
│  ├── Position timeout? → REVIEW AND DECIDE                     │
│  └── Better opportunity? → EVALUATE SWITCH                     │
│                                                                  │
│  EXIT CALCULATION:                                               │
│  ├── Expected exit value                                        │
│  ├── Remaining fees to collect                                  │
│  ├── Opportunity cost of staying                                │
│  └── Learning-based timing optimization                        │
│                                                                  │
│  EXECUTION:                                                      │
│  ├── Full exit or partial?                                      │
│  ├── Immediate or staged?                                       │
│  └── Rebalance or stay out?                                    │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

## Risk Learning

The system learns risk patterns over time:

```yaml
risk_learning:
  # Patterns to learn
  patterns:
    - il_spike_preceders:          # Signs before IL spike
      - volume_drop
      - whale_movement
      - correlation_break

    - successful_risk_mitigation:  # What worked
      - early_exit_triggers
      - hedging_strategies
      - rebalance_timing

  # Risk model updates
  update_frequency: daily
  backtest_new_rules: true
  require_validation: true
```

## Risk Commands

```
"Check portfolio risk"
 → Full portfolio risk analysis

"Show position risk for [POOL]"
 → Detailed position risk report

"What's my current IL?"
 → Impermanent loss summary

"Set stop-loss X% for [POOL]"
 → Update stop-loss level

"Set take-profit X% for [POOL]"
 → Update take-profit level

"Emergency stop"
 → Immediately pause all operations

"Resume trading"
 → Resume after emergency stop

"Adjust risk parameters"
 → Modify risk settings
```

---

Risk management protects your capital while the AI works to maximize returns.
