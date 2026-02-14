# Paper Trading System - PRJX Liquidity Manager

## Overview

Paper trading allows you to test strategies, validate AI decisions, and learn from simulations without risking real funds. All operations are executed in a virtual environment that mirrors real market conditions.

## Features

### Virtual Portfolio
- Simulated wallet with configurable starting balance
- Real-time price feeds from actual market
- Accurate fee calculations based on pool data
- Impermanent loss simulation
- Position tracking and PnL calculation

### Strategy Testing
- Backtest strategies against historical data
- Forward-test new strategies in real-time
- Compare multiple strategies simultaneously
- A/B test different parameters

### Validation Pipeline
- All new AI decisions go through paper trading first
- Minimum testing period before live execution
- Performance thresholds for strategy graduation
- Automatic rollback if performance degrades

## Architecture

```
┌─────────────────────────────────────────────────────────────────┐
│                    PAPER TRADING SYSTEM                         │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│   ┌──────────────┐      ┌──────────────┐      ┌──────────────┐ │
│   │  REAL MARKET │─────▶│ PRICE FEED   │─────▶│  SIMULATION  │ │
│   │    DATA      │      │   ENGINE     │      │    ENGINE    │ │
│   └──────────────┘      └──────────────┘      └──────────────┘ │
│                                                       │         │
│                                                       ▼         │
│   ┌──────────────┐      ┌──────────────┐      ┌──────────────┐ │
│   │  AI DECISION │─────▶│   STRATEGY   │─────▶│   PAPER      │ │
│   │    ENGINE    │      │   VALIDATOR  │      │  EXECUTION   │ │
│   └──────────────┘      └──────────────┘      └──────────────┘ │
│                                │                     │         │
│                                ▼                     ▼         │
│                         ┌──────────────┐      ┌──────────────┐ │
│                         │  GRADUATION  │◀─────│   RESULTS    │ │
│                         │    GATE      │      │   TRACKER    │ │
│                         └──────────────┘      └──────────────┘ │
│                                │                               │
│                                ▼                               │
│                         ┌──────────────┐                       │
│                         │    LIVE      │                       │
│                         │   TRADING    │                       │
│                         └──────────────┘                       │
└─────────────────────────────────────────────────────────────────┘
```

## Configuration

```yaml
paper_trading:
  enabled: true

  # Virtual account settings
  initial_balance: 10000        # USD
  balance_currency: USD

  # Simulation settings
  realistic_slippage: true
  realistic_gas_fees: true      # Even though PRJX is 0% fees
  realistic_timing: true        # Add realistic delays

  # Testing requirements
  min_test_period: 168          # hours (1 week)
  min_trades_required: 10
  min_win_rate: 0.55
  min_sharpe_ratio: 1.0
  max_drawdown: 0.15            # 15%

  # Graduation criteria
  auto_graduate: true
  graduation_threshold: 0.7     # confidence score

  # Comparison settings
  parallel_live_comparison: true
  track_divergence: true
```

## Paper Trading Modes

### Mode 1: Pure Paper (Default)
All operations are simulated. No real transactions.

```yaml
mode: paper
execution: virtual_only
wallet: simulated
notification: all_decisions
```

### Mode 2: Shadow Mode
Paper trade alongside live positions for comparison.

```yaml
mode: shadow
execution: virtual_parallel_to_live
wallet: both (paper + real)
notification: divergence_alerts
```

### Mode 3: Hybrid Mode
Paper trade validates, then executes live if confident.

```yaml
mode: hybrid
execution: paper_first_then_live
wallet: both
notification: pre_execution_summary
graduation_required: true
```

### Mode 4: Live Mode
All operations are real (paper trading disabled).

```yaml
mode: live
execution: real_only
wallet: real
notification: execution_confirmations
```

## Simulation Accuracy

### Price Simulation
```python
# Real-time price with realistic variance
simulated_price = real_price + random.normal(0, slippage_variance)

# Slippage calculation
slippage = min(max_slippage, trade_size / pool_liquidity * price_impact_factor)

# Execution price
execution_price = simulated_price * (1 + slippage)
```

### Fee Simulation
```python
# PRJX has 0% fees, but pool fees still apply
pool_fee_rate = get_pool_fee(pool_address)
estimated_fee = trade_value * pool_fee_rate

# Impermanent loss
il = calculate_il(
    initial_price_ratio=current_ratio,
    current_price_ratio=final_ratio,
    formula="v3_concentrated"
)
```

### Timing Simulation
```yaml
timing:
  decision_delay: 2-5 seconds    # AI thinking time
  execution_delay: 3-10 seconds  # Transaction time
  confirmation_delay: 5-15 seconds
  random_variance: ±20%
```

## Strategy Testing Framework

### Backtesting
Test strategies against historical data.

```yaml
backtest:
  start_date: "2024-01-01"
  end_date: "2024-12-31"
  initial_capital: 10000

  metrics:
    - total_return
    - sharpe_ratio
    - max_drawdown
    - win_rate
    - profit_factor
    - average_trade_duration
```

### Forward Testing
Test strategies in real-time simulation.

```yaml
forward_test:
  duration: 30 days
  starting_capital: 5000

  checkpoints:
    - daily_pnl
    - position_health
    - risk_metrics
    - comparison_to_benchmark
```

### A/B Testing
Compare multiple strategies.

```yaml
ab_test:
  strategies:
    A:
      range_multiplier: 1.5
      rebalance_threshold: 10%
    B:
      range_multiplier: 2.0
      rebalance_threshold: 15%

  split_ratio: 50/50
  duration: 14 days
  winner_criteria: higher_sharpe_ratio
```

## Paper Trading Reports

### Daily Summary
```
╔═══════════════════════════════════════════════════════════════╗
║                  PAPER TRADING - DAILY SUMMARY                 ║
╠═══════════════════════════════════════════════════════════════╣
║ Portfolio Value: $10,245.67 (+2.46%)                          ║
║ Virtual Balance: $5,245.67 | In Positions: $5,000.00          ║
║                                                                ║
║ POSITIONS:                                                     ║
║ ┌──────────────┬─────────┬─────────┬─────────┬──────────────┐║
║ │ Pool         │ Value   │ Fees    │ IL      │ Status       │║
║ ├──────────────┼─────────┼─────────┼─────────┼──────────────┤║
║ │ HYPE/USD₮0   │ $2,500  │ +$45.23 │ -$12.50 │ In Range ✓   │║
║ │ HYPE/kHYPE   │ $2,500  │ +$38.91 │ -$8.20  │ In Range ✓   │║
║ └──────────────┴─────────┴─────────┴─────────┴──────────────┘║
║                                                                ║
║ PERFORMANCE:                                                   ║
║ • Total Fees Earned: $84.14                                    ║
║ • Total IL: -$20.70                                            ║
║ • Net PnL: +$245.67 (+2.46%)                                   ║
║ • Win Rate: 75%                                                ║
║                                                                ║
║ DECISIONS TODAY:                                               ║
║ ✓ Added liquidity to HYPE/USD₮0 (confidence: 82%)             ║
║ ✓ Held HYPE/kHYPE position (within parameters)                ║
║ ✗ Avoided exit (would have missed +$15)                       ║
║                                                                ║
║ STRATEGY STATUS:                                               ║
║ Confidence Level: 78% (Ready for graduation at 85%)           ║
║ Days Tested: 5 of 7 minimum                                    ║
╚═══════════════════════════════════════════════════════════════╝
```

### Graduation Report
```
╔═══════════════════════════════════════════════════════════════╗
║                 🎓 STRATEGY GRADUATION REPORT                  ║
╠═══════════════════════════════════════════════════════════════╣
║ Strategy: Conservative Range Liquidity                         ║
║ Status: ✅ APPROVED FOR LIVE TRADING                          ║
║                                                                ║
║ PAPER TRADING RESULTS:                                         ║
║ ┌─────────────────────┬─────────────┬─────────────────────┐   ║
║ │ Metric              │ Result      │ Requirement         │   ║
║ ├─────────────────────┼─────────────┼─────────────────────┤   ║
║ │ Test Duration       │ 14 days     │ ≥ 7 days ✓          │   ║
║ │ Total Trades        │ 23          │ ≥ 10 ✓              │   ║
║ │ Win Rate            │ 73.9%       │ ≥ 55% ✓             │   ║
║ │ Sharpe Ratio        │ 1.45        │ ≥ 1.0 ✓             │   ║
║ │ Max Drawdown        │ 8.2%        │ ≤ 15% ✓             │   ║
║ │ Final Confidence    │ 0.87        │ ≥ 0.7 ✓             │   ║
║ └─────────────────────┴─────────────┴─────────────────────┘   ║
║                                                                ║
║ LIVE TRADING RECOMMENDATIONS:                                  ║
║ • Start with 50% of target position size                      ║
║ • Maintain paper trading shadow for 7 more days               ║
║ • Alert on >5% divergence between paper and live              ║
║                                                                ║
║ [ACTIVATE LIVE TRADING] [CONTINUE PAPER] [VIEW DETAILS]       ║
╚═══════════════════════════════════════════════════════════════╝
```

## Commands

### Basic Commands
```
"Enable paper trading mode"
 → Switches to paper trading (no real money)

"Disable paper trading mode"
 → Switches to live trading (real money at risk)

"What's my paper trading balance?"
 → Shows virtual portfolio value

"Show paper trading history"
 → Lists all simulated trades
```

### Testing Commands
```
"Run backtest on [STRATEGY] from [DATE] to [DATE]"
 → Historical simulation

"Test strategy: [DESCRIPTION]"
 → Creates and tests new strategy

"Compare paper vs live performance"
 → Shows divergence analysis

"Graduate strategy to live trading"
 → Moves validated strategy to production
```

### Validation Commands
```
"Is this strategy ready for live?"
 → Checks graduation criteria

"What's the confidence score?"
 → Shows current confidence level

"How many more tests needed?"
 → Shows remaining requirements
```

## Safety Features

### Automatic Safeguards
1. **Default Paper Mode**: All new agents start in paper trading
2. **Minimum Testing**: Cannot go live without meeting criteria
3. **Divergence Alerts**: Notified when paper/live diverge significantly
4. **Rollback Capability**: Can revert to paper if live underperforms
5. **Position Limits**: Paper trading tests within defined limits

### Manual Overrides
```yaml
overrides:
  # Force live (USE WITH CAUTION)
  force_live: false          # Requires explicit confirmation

  # Extend paper testing
  extend_paper: true         # Keep paper trading active

  # Emergency stop
  emergency_stop: true       # Pause all trading
```

## Data Persistence

```yaml
data_storage:
  paper_positions: data/paper_positions.json
  trade_history: data/paper_trades.json
  performance: data/paper_performance.json
  strategies: data/paper_strategies.json

  sync_interval: 5 minutes   # Save to disk
  backup_retention: 30 days  # Keep backups
```

## Transition Checklist

Before moving from Paper to Live:

- [ ] Minimum test period completed
- [ ] Minimum number of trades executed
- [ ] Win rate above threshold
- [ ] Max drawdown within limits
- [ ] Sharpe ratio acceptable
- [ ] Confidence score above threshold
- [ ] Risk parameters reviewed
- [ ] Wallet connected and funded
- [ ] Notifications configured
- [ ] Emergency procedures understood

---

Paper trading ensures every strategy is battle-tested before risking real capital.
