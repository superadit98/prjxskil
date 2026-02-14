# PRJX Liquidity Manager - AI Agent with Auto-Learning

## Metadata
- **Name**: prjx-liquidity-manager
- **Version**: 1.0.0
- **Author**: OpenClaw Community
- **Category**: DeFi, Liquidity Management, Trading
- **Tags**: defi, liquidity, prjx, hyperliquid, trading, automation, ai-agent
- **Requires**: browser_tool, http_tool, calculate_tool, notification_tool

## Description

AI-powered liquidity management agent for PRJX (Project X) DEX with full automation, auto-learning capabilities, and paper trading mode. This agent manages your liquidity positions on prjx.com with intelligent decision-making that improves over time through experience.

## Capabilities

### Core Operations
- Add/Remove/Modify liquidity positions on PRJX V3 pools
- Automatic fee collection and compounding
- Position rebalancing based on market conditions
- Multi-pool portfolio management

### Risk Management
- Real-time impermanent loss (IL) monitoring
- Position health scoring and alerts
- Dynamic stop-loss and take-profit execution
- Exposure limit enforcement
- Maximum drawdown protection

### Auto-Learning System
- Learns from successful and failed trades
- Adapts strategies based on market conditions
- Builds pattern recognition for price movements
- Improves decision-making over time
- Maintains memory of past experiences

### Paper Trading Mode
- Simulate all operations before real execution
- Test strategies with virtual funds
- Compare paper vs real performance
- Validate learning before live trading

### Automation Features
- Scheduled monitoring and rebalancing
- Price-based triggers and alerts
- Automatic position adjustments
- Performance reporting to messaging platforms

## Tools Required

```yaml
tools:
  - name: browser_tool
    purpose: Interact with prjx.com website
    actions: [navigate, click, input, screenshot, wait]

  - name: http_tool
    purpose: API calls for on-chain data
    actions: [get, post]

  - name: calculate_tool
    purpose: Financial calculations
    actions: [il_calc, apr_calc, pnl_calc, position_value]

  - name: notification_tool
    purpose: Send alerts and reports
    actions: [send_message, send_report, send_alert]
```

## Configuration

```yaml
config:
  # Trading Mode
  trading_mode: paper  # paper | live | hybrid

  # Risk Parameters
  max_position_size: 10000      # USD
  max_total_exposure: 50000     # USD
  max_il_tolerance: 5           # percent
  stop_loss_percent: 15         # percent
  take_profit_percent: 50       # percent

  # Pool Preferences
  preferred_pools:
    - HYPE/USD₮0
    - HYPE/kHYPE
    - HYPE/UBTC
  min_liquidity: 100000         # USD
  min_apr: 20                   # percent

  # Automation Settings
  auto_rebalance: true
  auto_compound: true
  rebalance_threshold: 10       # percent price movement
  compound_interval: 24         # hours

  # Learning Settings
  learning_enabled: true
  learning_mode: continuous     # continuous | batch
  min_confidence: 0.7           # minimum confidence for auto-execute

  # Notification Channels
  notifications:
    telegram: true
    whatsapp: false
    discord: false
    email: false

  # Schedule
  monitoring_interval: 15       # minutes
  report_schedule: "0 9 * * *"  # daily at 9 AM
```

## Commands

### Position Management
```
"Add liquidity $X to [POOL] with range +-Y%"
"Remove X% from [POOL] position"
"Show all my positions"
"Rebalance position [POOL]"
"Collect fees from all positions"
```

### Monitoring
```
"Check portfolio health"
"Show IL for all positions"
"What's my current APR?"
"Alert me when [POOL] price hits X"
```

### Paper Trading
```
"Enable paper trading mode"
"Run simulation with $X over Y days"
"Compare paper vs real performance"
"Validate strategy before execution"
```

### Learning
```
"Show learning progress"
"What have you learned about [POOL]?"
"Reset learning memory"
"Export learning data"
```

## Decision Framework

### Entry Decision Process
1. **Market Analysis**
   - Check pool TVL trend (increasing = good)
   - Check 24h volume (high volume = more fees)
   - Check price volatility (determine range width)
   - Check APR sustainability

2. **Risk Assessment**
   - Calculate expected IL range
   - Assess correlation between paired assets
   - Check current exposure vs limits
   - Evaluate recent performance patterns

3. **Learning Integration**
   - Query memory for similar conditions
   - Check success rate of past entries
   - Apply learned adjustments
   - Confidence scoring

4. **Execution**
   - If confidence >= threshold: auto-execute
   - If confidence < threshold: request confirmation
   - Log decision reasoning for learning

### Exit Decision Process
1. **Trigger Check**
   - Stop-loss price reached?
   - Take-profit target hit?
   - Position out of range?
   - IL exceeded tolerance?

2. **Learning Integration**
   - Check similar exit scenarios
   - Optimal timing analysis
   - Partial vs full exit decision

3. **Execution**
   - Calculate optimal exit amount
   - Execute removal
   - Collect remaining fees
   - Log outcome for learning

## Memory Structure

The agent maintains several memory files:

```
memory/
├── trades.json          # All executed trades
├── learning.json        # Learned patterns and insights
├── performance.json     # Performance metrics
├── mistakes.json        # Failed decisions and lessons
├── successes.json       # Successful patterns
└── market_memory.json   # Market condition history
```

## Safety Rules

1. **Never exceed configured limits**
2. **Always log decisions before execution**
3. **Paper trade new strategies first**
4. **Require confirmation for unusual operations**
5. **Maintain audit trail of all actions**
6. **Auto-disable on consecutive losses**
7. **Emergency stop capability always available**

## Error Handling

- Network errors: Retry with exponential backoff
- Transaction failures: Log and analyze for learning
- Unexpected prices: Pause and reassess
- Learning degradation: Reset to safe defaults

## Performance Metrics Tracked

- Total PnL (Realized + Unrealized)
- Fee earnings
- Impermanent Loss
- APR achieved vs expected
- Win rate
- Average hold time
- Learning improvement score

## Initial Prompts

When first activated, the agent will:

1. Ask about your wallet setup
2. Confirm notification preferences
3. Set risk parameters based on your capital
4. Start in paper trading mode by default
5. Begin learning from day one

---

## Quick Start

```
"I want to start managing liquidity on PRJX. My budget is $5000.
 I want full automation with paper trading first.
 Set moderate risk - max 10% IL tolerance.
 Notify me on Telegram for all major decisions."
```

The agent will then:
1. Initialize paper trading account
2. Scan available pools
3. Identify best opportunities
4. Propose initial positions
5. Execute after your approval
6. Begin continuous monitoring
7. Start learning from every decision
