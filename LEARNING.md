# Auto-Learning System - PRJX Liquidity Manager

## Overview

The auto-learning system enables the PRJX Liquidity Manager agent to continuously improve its decision-making by learning from every action, outcome, and market condition. It builds a knowledge base of patterns, mistakes, and successes that inform future decisions.

## Learning Architecture

```
┌─────────────────────────────────────────────────────────────────┐
│                    AUTO-LEARNING SYSTEM                         │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│  ┌─────────────┐    ┌─────────────┐    ┌─────────────┐         │
│  │   ACTION    │───▶│   OUTCOME   │───▶│  ANALYSIS   │         │
│  │  (Decision) │    │  (Result)   │    │  (Learning) │         │
│  └─────────────┘    └─────────────┘    └─────────────┘         │
│         │                  │                  │                 │
│         ▼                  ▼                  ▼                 │
│  ┌─────────────────────────────────────────────────────┐       │
│  │                    MEMORY BANK                       │       │
│  │  ┌──────────┐ ┌──────────┐ ┌──────────┐ ┌────────┐ │       │
│  │  │ Patterns │ │ Mistakes │ │Successes │ │ Models │ │       │
│  │  └──────────┘ └──────────┘ └──────────┘ └────────┘ │       │
│  └─────────────────────────────────────────────────────┘       │
│         │                                                       │
│         ▼                                                       │
│  ┌─────────────────────────────────────────────────────┐       │
│  │               DECISION IMPROVEMENT                   │       │
│  │     Better Entry │ Better Exit │ Better Risk        │       │
│  └─────────────────────────────────────────────────────┘       │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

## Learning Categories

### 1. Entry Learning
Learns optimal conditions for entering liquidity positions.

```yaml
entry_patterns:
  conditions_tracked:
    - pool_tvl_trend: [increasing, stable, decreasing]
    - volume_trend: [high, medium, low]
    - price_volatility: [high, medium, low]
    - apr_stability: [stable, volatile]
    - il_history: [low, medium, high]
    - time_of_day: [0-23]
    - day_of_week: [0-6]

  outcomes_measured:
    - realized_pnl: float
    - fee_earnings: float
    - time_in_range: percent
    - actual_il: percent
    - actual_apr: percent

  learning_output:
    - entry_confidence_score: 0.0-1.0
    - recommended_range_multiplier: float
    - optimal_position_size: float
    - expected_duration: hours
```

### 2. Exit Learning
Learns optimal timing and conditions for exiting positions.

```yaml
exit_patterns:
  conditions_tracked:
    - position_age: hours
    - current_il: percent
    - price_deviation: percent
    - volume_change: percent
    - apr_change: percent
    - overall_market_trend: [bull, bear, sideways]

  outcomes_measured:
    - exit_pnl: float
    - opportunity_cost: float
    - fees_collected: float
    - would_have_been_better: boolean  # Hindsight analysis

  learning_output:
    - exit_threshold_optimization: float
    - timing_adjustment: hours
    - partial_exit_recommendation: percent
```

### 3. Risk Learning
Learns to better assess and manage risk.

```yaml
risk_patterns:
  conditions_tracked:
    - pool_correlation: float
    - historical_volatility: float
    - liquidity_depth: float
    - concentration_risk: float

  outcomes_measured:
    - max_drawdown: percent
    - recovery_time: hours
    - worst_case_il: percent
    - stress_test_results: float

  learning_output:
    - dynamic_il_tolerance: percent
    - position_size_multiplier: float
    - emergency_threshold_adjustment: float
```

### 4. Pool-Specific Learning
Learns unique characteristics of each pool.

```yaml
pool_learning:
  pools:
    HYPE/USD₮0:
      characteristics:
        avg_volatility: learned
        typical_range: learned
        best_entry_times: learned
        fee_consistency: learned
      performance_history: []

    HYPE/kHYPE:
      characteristics:
        correlation_factor: learned
        il_patterns: learned
        rebalancing_frequency: learned
      performance_history: []
```

## Learning Algorithms

### Pattern Recognition
```
ALGORITHM: PatternRecognition(event)
├── Extract features from event
├── Compare with historical patterns
├── Calculate similarity scores
├── Identify matching patterns
├── Retrieve outcomes of similar events
└── Generate confidence score
```

### Mistake Analysis
```
ALGORITHM: MistakeAnalysis(failed_action)
├── Identify what went wrong
├── Root cause analysis
│   ├── Market conditions changed?
│   ├── Wrong timing?
│   ├── Wrong position size?
│   └── Wrong range selection?
├── Create anti-pattern rule
├── Update confidence weights
└── Store in mistakes.json
```

### Success Reinforcement
```
ALGORITHM: SuccessReinforcement(successful_action)
├── Identify what went right
├── Extract winning conditions
├── Increase weight for similar patterns
├── Update success metrics
└── Store in successes.json
```

### Continuous Improvement
```
ALGORITHM: ContinuousImprovement()
├── Periodically analyze all decisions
├── Calculate win rate trends
├── Identify improvement areas
├── Adjust decision thresholds
└── Update confidence models
```

## Learning Metrics

### Confidence Score Calculation
```
confidence = (successful_similar_actions / total_similar_actions) *
             recency_weight *
             relevance_weight *
             learning_maturity_factor

Where:
- recency_weight: more recent = higher weight
- relevance_weight: more feature match = higher weight
- learning_maturity_factor: improves with more data
```

### Learning Progress Indicators
```yaml
metrics:
  total_decisions: integer
  successful_decisions: integer
  failed_decisions: integer
  win_rate: percent
  improvement_trend: [improving, stable, declining]

  by_category:
    entry:
      confidence: 0.0-1.0
      sample_size: integer
      recent_win_rate: percent
    exit:
      confidence: 0.0-1.0
      sample_size: integer
      recent_win_rate: percent
    risk:
      confidence: 0.0-1.0
      sample_size: integer
      prediction_accuracy: percent
```

## Learning Thresholds

```yaml
thresholds:
  # Minimum data points before high confidence
  min_samples_for_confidence: 10

  # Confidence required for auto-execution
  auto_execute_confidence: 0.7

  # Confidence required for recommendation
  recommend_confidence: 0.5

  # Learning rate (how fast to adapt)
  learning_rate: 0.1

  # Forget rate (how fast old data loses relevance)
  forget_rate: 0.01

  # Pattern matching similarity threshold
  similarity_threshold: 0.8
```

## Learning Outputs

### Daily Learning Report
```
╔═══════════════════════════════════════════════════════════════╗
║                    DAILY LEARNING REPORT                       ║
╠═══════════════════════════════════════════════════════════════╣
║ Decisions Today: 12                                           ║
║ Success Rate: 75% (↑ 5% from yesterday)                       ║
║                                                                ║
║ NEW LEARNINGS:                                                ║
║ • HYPE/USD₮0 performs better with wider ranges (±8%)         ║
║ • Avoid entries during high volatility (>15%)                 ║
║ • Best exit timing: after 48-72 hours for short positions    ║
║                                                                ║
║ PATTERNS IDENTIFIED:                                          ║
║ • Price reversals often happen at 10% deviation              ║
║ • Volume spike = potential range break                       ║
║                                                                ║
║ MISTAKES AVOIDED:                                             ║
║ • Did not enter during correlating asset dump                ║
║ • Waited for better entry instead of FOMO                    ║
║                                                                ║
║ CONFIDENCE LEVELS:                                            ║
║ • Entry decisions: 78% (↑ 3%)                                 ║
║ • Exit decisions: 72% (↑ 1%)                                  ║
║ • Risk assessment: 85% (stable)                               ║
╚═══════════════════════════════════════════════════════════════╝
```

## Memory Management

### Data Retention
```yaml
retention:
  detailed_trades: 90 days       # Full trade details
  aggregated_patterns: forever    # Pattern summaries
  learning_weights: forever       # Learned weights
  raw_market_data: 30 days       # Raw data for analysis
  mistakes: forever              # Never forget mistakes
  successes: forever             # Never forget successes
```

### Memory Optimization
```yaml
optimization:
  # Compress old data
  compress_after_days: 30

  # Aggregate similar patterns
  pattern_clustering: true

  # Remove redundant entries
  deduplication: true

  # Keep only significant events
  significance_threshold: 0.1
```

## Transfer Learning

The agent can share learning across:

1. **Similar Pools**: Learning from HYPE/USD₮0 applies to HYPE/UBTC
2. **Similar Conditions**: Market crash patterns are universal
3. **Time Periods**: Weekly patterns repeat

```yaml
transfer_learning:
  enabled: true
  pool_similarity_threshold: 0.7
  condition_similarity_threshold: 0.8
```

## Learning Commands

```
"What have you learned about [POOL]?"
 → Shows pool-specific insights and patterns

"How confident are you about [decision]?"
 → Shows confidence score and reasoning

"Show your biggest mistakes"
 → Lists top mistakes and lessons learned

"Show your best trades"
 → Lists successful patterns

"Reset learning for [POOL]"
 → Clears pool-specific learning (keeps general)

"Export learning data"
 → Downloads learning database

"Import learning data"
 → Loads previous learning database
```

## Learning Safety

1. **Never delete mistakes** - They prevent repeat errors
2. **Require confirmation** for new pattern-based actions
3. **Gradual confidence building** - No overconfidence early
4. **Regular sanity checks** - Ensure learning isn't corrupted
5. **Human oversight option** - Can require human review periodically

---

The learning system makes the agent smarter with every trade, building institutional knowledge that compounds over time.
