# PRJX Liquidity Manager - Trading Strategies

## Overview

This document outlines the strategies used by the AI for liquidity management decisions. Strategies are selected and combined based on market conditions, pool characteristics, and learned patterns.

---

## Strategy Framework

### Strategy Selection Matrix

```
┌─────────────────────────────────────────────────────────────────┐
│                    STRATEGY SELECTION MATRIX                    │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│                    VOLATILITY                                   │
│              LOW         MEDIUM        HIGH                     │
│           ┌──────────┬──────────┬──────────┐                   │
│     BULL  │ NARROW   │ BALANCED │ WIDE     │                   │
│           │ +FEES    │ +TREND   │ +SWING   │                   │
│ TREND     ├──────────┼──────────┼──────────┤                   │
│   NEUTRAL │ NARROW   │ BALANCED │ WIDE     │                   │
│           │ +FEES    │ +STD     │ +HEDGE   │                   │
│           ├──────────┼──────────┼──────────┤                   │
│     BEAR  │ CONSERV  │ DEFENSIVE│ CASH    │                   │
│           │ +SAFE    │ +EXIT    │ +WAIT    │                   │
│           └──────────┴──────────┴──────────┘                   │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

---

## Core Strategies

### Strategy 1: Fee Harvester (Low Volatility)

**Best For:** Stable or low-volatility markets

**Concept:** Maximize fee earnings with narrow ranges

```
CONFIGURATION:
├── Range Width: ±3-5% from current price
├── Position Size: 20-30% of allocation
├── Rebalance Frequency: When price exits range
├── Expected APR Boost: 1.5-2x pool average
├── IL Risk: Low (narrow range)
└── Time Horizon: 1-7 days

ENTRY CONDITIONS:
├── Volatility < 10% daily
├── Price consolidating (range-bound)
├── High trading volume in pool
└── No major news/events expected

EXIT CONDITIONS:
├── Volatility increases > 15%
├── Price breaks consolidation
├── Target fee earnings reached
└── IL exceeds 5%

EXAMPLE:
Pool: HYPE/USD₮0
Current Price: $0.92
Volatility: 8% daily
Strategy: NARROW ±4%
Range: $0.88 - $0.96
Expected: Higher fees, lower IL
```

### Strategy 2: Trend Follower (Directional)

**Best For:** Clear market trends

**Concept:** Position range in direction of trend

```
CONFIGURATION:
├── Range Width: ±8-12% from current price
├── Position Bias: Asymmetric (favor trend direction)
├── Position Size: 15-25% of allocation
├── Rebalance: Trail the trend
├── Expected Return: Trend + Fees
├── IL Risk: Medium
└── Time Horizon: 3-14 days

BULL TREND SETUP:
├── Range: Current -5% to +15%
├── More buffer on upside
├── Trail upward as price rises
└── Widen if momentum strong

BEAR TREND SETUP:
├── Range: Current -15% to +5%
├── More buffer on downside
├── Trail downward as price falls
└── Consider exit if reversal

ENTRY CONDITIONS:
├── Clear trend established (EMA crossover)
├── Volume confirming trend
├── Volatility moderate
└── No reversal signals

EXIT CONDITIONS:
├── Trend reversal confirmed
├── Target profit reached
├── Stop loss triggered
└── Volatility extreme
```

### Strategy 3: Wide Range Income (Conservative)

**Best For:** Uncertain markets, capital preservation

**Concept:** Wide ranges for safety, accept lower returns

```
CONFIGURATION:
├── Range Width: ±20-30% from current price
├── Position Size: 30-40% of allocation
├── Rebalance Frequency: Rarely (monthly)
├── Expected APR: 0.5-0.8x pool average
├── IL Risk: Very Low
└── Time Horizon: 14-30+ days

ENTRY CONDITIONS:
├── Uncertain market direction
├── High volatility expected
├── Long-term hold mindset
└── Capital preservation priority

EXIT CONDITIONS:
├── Major market structure change
├── Better opportunity elsewhere
├── Need liquidity
└── Portfolio rebalancing

EXAMPLE:
Pool: HYPE/kHYPE
Current Price: $1.00
Volatility: Unknown/High
Strategy: WIDE ±25%
Range: $0.75 - $1.25
Expected: Safe, steady fees
```

### Strategy 4: Rebalancer (Active Management)

**Best For:** Active management, optimize returns

**Concept:** Continuous adjustment of ranges

```
CONFIGURATION:
├── Initial Range: ±10% from current price
├── Position Size: 15-20% of allocation
├── Rebalance Trigger: Price within 2% of edge
├── Rebalance Method: Close + reopen new range
├── Expected APR: 1.2-1.5x pool average
├── IL Risk: Medium (frequent moves)
└── Time Horizon: Continuous

REBALANCE RULES:
├── Price near upper edge → Shift range up
├── Price near lower edge → Shift range down
├── Volatility increase → Widen range
├── Volatility decrease → Narrow range
└── Fee earnings high → Maintain position

COST CONSIDERATION:
├── Gas costs per rebalance: ~$5-10
├── Min earnings between rebalances: $20
├── Max rebalances per day: 2
└── Track rebalance ROI

ENTRY CONDITIONS:
├── Moderate volatility (10-20%)
├── Active trading in pool
├── Gas costs reasonable
└── User available to confirm (if required)

EXIT CONDITIONS:
├── Gas costs exceed benefits
├── Too many rebalances needed
├── Better static strategy available
└── User preference change
```

### Strategy 5: Hedged Position (Advanced)

**Best For:** Risk-averse, experienced users

**Concept:** Offset IL with hedge position

```
CONFIGURATION:
├── LP Position: Standard strategy
├── Hedge Size: 50-100% of LP value
├── Hedge Type: Short perp / inverse LP
├── Hedge Ratio: Dynamic based on IL
├── Expected Return: LP fees - hedge cost
├── IL Risk: Hedged (minimal)
└── Time Horizon: Any

HEDGE METHODS:

1. Perpetual Short
   ├── Short token on perp DEX
   ├── Size = LP exposure
   └── Cost = Funding rate

2. Inverse LP
   ├── LP in opposite pair
   └── Complex but effective

3. Options
   ├── Buy puts on volatile token
   └── Premium cost

EXAMPLE:
LP Position: $1000 in HYPE/USD₮0
Hedge: Short $500 HYPE on perp
IL Exposure: Halved
Net Return: Fees - funding

ENTRY CONDITIONS:
├── User understands hedging
├── Access to hedge instruments
├── Hedge costs < expected fees
└── Advanced risk management needed

EXIT CONDITIONS:
├── Hedge costs too high
├── LP position closed
├── Hedge no longer needed
└── Market conditions changed
```

---

## Strategy Combinations

### Balanced Portfolio Approach

Combine strategies for optimal risk-adjusted returns:

```
PORTFOLIO ALLOCATION EXAMPLE:
┌─────────────────────────────────────────────┐
│           STRATEGIC ALLOCATION               │
├─────────────────────────────────────────────┤
│                                              │
│  40% - Fee Harvester (Low vol pairs)        │
│  │    └── Stable income, low risk           │
│                                              │
│  30% - Trend Follower (Major pairs)         │
│  │    └── Growth potential, medium risk     │
│                                              │
│  20% - Wide Range (Volatile pairs)          │
│  │    └── Capital preservation              │
│                                              │
│  10% - Cash Reserve                          │
│  │    └── Opportunity fund                  │
│                                              │
└─────────────────────────────────────────────┘
```

---

## Entry Strategies

### Entry Timing Rules

```
OPTIMAL ENTRY CONDITIONS:
├── Price at support level (bounce expected)
├── Low volatility period
├── High funding rates (mean reversion)
├── Volume spike (entry before)
└── Technical breakout confirmed

AVOID ENTRY WHEN:
├── Price at resistance (rejection likely)
├── Extreme volatility (>30% daily)
├── Major news pending
├── Pool TVL dropping rapidly
└── APR unsustainably high (risk indicator)
```

### Entry Size Calculation

```javascript
function calculateEntrySize(opportunity, portfolio, confidence) {
  let baseSize = portfolio.total * config.default_position_percent / 100;

  // Adjust by confidence
  if (confidence >= 80) {
    baseSize *= 1.2; // Increase for high confidence
  } else if (confidence < 60) {
    baseSize *= 0.5; // Decrease for low confidence
  }

  // Adjust by pool risk
  if (opportunity.volatility > 20) {
    baseSize *= 0.7; // Reduce for volatile pools
  }

  // Ensure limits
  baseSize = Math.min(baseSize, config.max_single_position);
  baseSize = Math.max(baseSize, config.min_position_size);

  // Check remaining capacity
  const availableExposure = portfolio.max_exposure - portfolio.current_exposure;
  baseSize = Math.min(baseSize, availableExposure);

  return Math.round(baseSize);
}
```

---

## Exit Strategies

### Planned Exit Types

#### 1. Take Profit Exit
```
Trigger: P&L reaches target
Action: Remove liquidity, realize gains
Variations:
├── Full exit at target
├── Partial exit (scale out)
└── Trailing exit (lock profits)
```

#### 2. Stop Loss Exit
```
Trigger: Loss exceeds threshold
Action: Exit immediately, preserve capital
Priority: HIGHEST (execute without confirmation)
```

#### 3. Time-Based Exit
```
Trigger: Position held too long without profit
Action: Review and likely exit
Purpose: Capital efficiency
```

#### 4. Opportunity Exit
```
Trigger: Better opportunity found
Action: Exit current, enter new
Condition: New expected return > current + switch cost
```

#### 5. Risk Exit
```
Trigger: Risk parameters exceeded
Action: Reduce or close position
Examples:
├── IL too high
├── Pool metrics deteriorating
├── Correlation breakdown
└── Smart money exiting
```

---

## Rebalancing Strategies

### When to Rebalance

```
REBALANCE TRIGGERS:
├── Price exits range
├── Volatility change > 50%
├── APR change > 30%
├── Portfolio drift > 10%
└── Risk score change > 20 points

REBALANCE COST-BENEFIT:
Expected benefit = New fees - Old fees - Gas cost
Rebalance if benefit > threshold
```

### Rebalancing Methods

#### Method 1: Close and Reopen
```
1. Remove all liquidity
2. Collect fees
3. Add liquidity at new range

Pros: Clean, simple
Cons: Gas cost, miss fees during transition
```

#### Method 2: Dual Position
```
1. Keep existing position
2. Add new position at new range
3. Remove old position later

Pros: No gap in fee earning
Cons: More capital needed
```

#### Method 3: Gradual Shift
```
1. Remove 25% of liquidity
2. Add to new range
3. Repeat over time

Pros: Smooth transition
Cons: Complex tracking
```

---

## Special Situations

### High Volatility Events

```
BEFORE EVENT (e.g., major announcement):
├── Reduce position sizes
├── Widen ranges significantly
├── Set tighter stop losses
├── Increase monitoring frequency
└── Prepare for quick action

DURING EVENT:
├── Monitor continuously
├── Do not panic trade
├── Wait for trend confirmation
├── Follow pre-set rules
└── Document for learning

AFTER EVENT:
├── Assess new market structure
├── Adjust strategies accordingly
├── Learn from outcomes
└── Update patterns
```

### Market Regime Changes

```
BULL → BEAR TRANSITION:
├── Tighten stop losses
├── Shift to defensive strategies
├── Reduce total exposure
├── Favor stable pairs
└── Cash position increases

BEAR → BULL TRANSITION:
├── Gradual position building
├── Test with small sizes first
├── Focus on high-volume pairs
├── Wider ranges initially
└── Scale up with confirmation
```

---

## Strategy Performance Tracking

### Key Metrics

| Metric | Description | Target |
|--------|-------------|--------|
| Win Rate | % of profitable positions | > 60% |
| Avg P&L | Average return per position | > 5% |
| Sharpe Ratio | Risk-adjusted return | > 1.5 |
| Max Drawdown | Largest peak-to-trough | < 15% |
| Fee Yield | Annualized fee earnings | > Pool avg |
| IL Ratio | IL as % of fees earned | < 50% |
| Hold Time | Average position duration | Variable |

### Strategy Attribution

```
Track which strategy contributed to each outcome:
├── Winning trade attribution
├── Losing trade attribution
├── Strategy-specific win rates
├── Time-based performance
└── Market condition correlation
```

---

## AI Strategy Selection

### Decision Flow

```
START
  │
  ├── Analyze Market Conditions
  │   ├── Trend (BULL/NEUTRAL/BEAR)
  │   ├── Volatility (LOW/MEDIUM/HIGH)
  │   └── Sentiment (GREED/NEUTRAL/FEAR)
  │
  ├── Filter Applicable Strategies
  │   └── Based on conditions
  │
  ├── Check Learned Patterns
  │   ├── Similar conditions in memory
  │   └── Historical outcomes
  │
  ├── Score Each Strategy
  │   ├── Expected return
  │   ├── Risk level
  │   └── Pattern match confidence
  │
  ├── Select Best Strategy
  │   └── Highest score + confidence
  │
  └── Execute
      └── Log for learning
```

### Strategy Override

User can always override AI selection:
```
User: "Use Fee Harvester strategy for HYPE/USD₮0"

AI: Strategy override confirmed.
Using: FEE_HARVESTER
Parameters:
├── Range: ±4%
├── Size: $1000
├── Expected APR: +45%
└── Risk: LOW

Proceed? [CONFIRM]
```

---

## Strategy Commands

| Command | Description |
|---------|-------------|
| `prjx strategy list` | Show all available strategies |
| `prjx strategy recommend [pool]` | Get AI strategy recommendation |
| `prjx strategy set [name]` | Set preferred strategy |
| `prjx strategy performance` | Show strategy performance |
| `prjx strategy backtest [name]` | Backtest strategy historically |
