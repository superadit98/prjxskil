# Memory System - PRJX Liquidity Manager

## Overview

The memory system stores all experiences, patterns, decisions, and outcomes. It enables the AI agent to learn from past actions and improve decision-making over time.

## Memory Architecture

```
┌─────────────────────────────────────────────────────────────────┐
│                       MEMORY ARCHITECTURE                       │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│   SHORT-TERM MEMORY (Session)                                   │
│   ├── Current session decisions                                 │
│   ├── Temporary calculations                                    │
│   └── Active monitoring data                                    │
│                                                                  │
│   WORKING MEMORY (Recent)                                       │
│   ├── Last 24 hours decisions                                   │
│   ├── Active positions state                                    │
│   └── Pending alerts/tasks                                      │
│                                                                  │
│   LONG-TERM MEMORY (Persistent)                                 │
│   ├── All trades history                                        │
│   ├── Learned patterns                                          │
│   ├── Mistakes & lessons                                        │
│   └── Success patterns                                          │
│                                                                  │
│   COLLECTIVE MEMORY (Pool-Specific)                             │
│   ├── Pool behavior patterns                                    │
│   ├── Pool-specific learnings                                   │
│   └── Historical performance                                    │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

## Memory Files Structure

```
memory/
├── trades.json              # All executed trades
├── learning.json            # Learned patterns and insights
├── performance.json         # Performance metrics over time
├── mistakes.json            # Failed decisions and lessons
├── successes.json           # Successful patterns
├── market_memory.json       # Market condition history
├── pool_knowledge.json      # Pool-specific knowledge
└── session.json             # Current session data
```

## Data Schemas

### trades.json
```json
{
  "trades": [
    {
      "id": "TRD-2024-001",
      "timestamp": "2024-01-15T10:30:00Z",
      "type": "ADD_LIQUIDITY",
      "pool": "HYPE/USD₮0",
      "details": {
        "amount": 5000,
        "amount_token0": 1000,
        "amount_token1": 2500,
        "range_lower": 2.35,
        "range_upper": 2.65,
        "price_at_entry": 2.50
      },
      "reasoning": {
        "trigger": "OPPORTUNITY_DETECTED",
        "confidence": 0.85,
        "factors": [
          "TVL increasing 5%",
          "Volume spike detected",
          "APR above 80%"
        ]
      },
      "outcome": {
        "status": "OPEN",
        "current_value": 5200,
        "fees_earned": 45.50,
        "il": -12.30,
        "pnl": 233.20
      },
      "learning_tags": ["good_entry", "optimal_range"]
    }
  ]
}
```

### learning.json
```json
{
  "patterns": [
    {
      "id": "PTRN-001",
      "type": "ENTRY_SIGNAL",
      "conditions": {
        "tvl_trend": "increasing",
        "volume_trend": "high",
        "price_volatility": "low"
      },
      "outcome": "PROFITABLE",
      "success_rate": 0.78,
      "sample_size": 45,
      "last_updated": "2024-01-15T00:00:00Z",
      "learned_rules": [
        "Enter when TVL increasing + volume high",
        "Avoid entries during high volatility",
        "Wider ranges work better in volatile conditions"
      ]
    }
  ],
  "insights": [
    {
      "id": "INS-001",
      "pool": "HYPE/USD₮0",
      "insight": "Best entry times are 2-4 AM UTC",
      "confidence": 0.72,
      "sample_size": 30
    }
  ]
}
```

### mistakes.json
```json
{
  "mistakes": [
    {
      "id": "MSTK-001",
      "timestamp": "2024-01-10T14:00:00Z",
      "action": "ADD_LIQUIDITY",
      "pool": "HYPE/kHYPE",
      "what_went_wrong": "Entered during high volatility spike",
      "outcome": {
        "loss": -150,
        "il_peak": -8.5,
        "duration_out_of_range": "12 hours"
      },
      "root_cause": "Did not check volatility threshold",
      "lesson_learned": "Always verify volatility < 15% before entry",
      "rule_created": "IF volatility > 15% THEN delay_entry(1h)",
      "avoided_count": 5
    }
  ],
  "anti_patterns": [
    {
      "id": "ANTI-001",
      "pattern": "FOMO entry during price spike",
      "consequence": "Usually leads to immediate IL",
      "avoidance_rule": "Wait 30 minutes after >10% price move"
    }
  ]
}
```

### successes.json
```json
{
  "successes": [
    {
      "id": "SUCC-001",
      "timestamp": "2024-01-12T08:00:00Z",
      "action": "ADD_LIQUIDITY",
      "pool": "HYPE/USD₮0",
      "what_went_right": "Perfect entry timing and range selection",
      "outcome": {
        "profit": 245,
        "fees_earned": 120,
        "time_in_range": "95%",
        "max_il": "1.2%"
      },
      "factors": [
        "Entered during low volatility",
        "Range matched historical patterns",
        "Pool had increasing TVL"
      ],
      "pattern_extracted": "Entry with ±10% range works best for HYPE/USD₮0",
      "reuse_count": 8
    }
  ],
  "winning_patterns": [
    {
      "id": "WP-001",
      "pattern": "Wide range in volatile pools",
      "description": "±20% range for pools with >30% volatility",
      "win_rate": 0.82,
      "avg_profit": 180
    }
  ]
}
```

### pool_knowledge.json
```json
{
  "pools": {
    "HYPE/USD₮0": {
      "characteristics": {
        "avg_volatility": 12.5,
        "typical_apr": 85,
        "best_range_width": 10,
        "correlation": 0.85,
        "liquidity_depth": "HIGH"
      },
      "learned_behaviors": [
        "Performs well during Asian trading hours",
        "Sensitive to HYPE ecosystem news",
        "Quick recovery after dumps"
      ],
      "optimal_settings": {
        "entry_confidence_threshold": 0.75,
        "range_multiplier": 1.0,
        "rebalance_frequency": "daily"
      },
      "performance_history": {
        "total_trades": 45,
        "win_rate": 0.73,
        "avg_pnl": 125,
        "max_il_seen": 6.2
      }
    },
    "HYPE/kHYPE": {
      "characteristics": {
        "avg_volatility": 18.2,
        "typical_apr": 120,
        "best_range_width": 15,
        "correlation": 0.92,
        "liquidity_depth": "MEDIUM"
      },
      "learned_behaviors": [
        "High correlation - lower IL expected",
        "Good for stable yield farming",
        "kHYPE staking rewards affect pool"
      ],
      "optimal_settings": {
        "entry_confidence_threshold": 0.70,
        "range_multiplier": 1.2,
        "rebalance_frequency": "every_2_days"
      }
    }
  }
}
```

### performance.json
```json
{
  "summary": {
    "total_trades": 156,
    "winning_trades": 112,
    "losing_trades": 44,
    "win_rate": 0.718,
    "total_pnl": 4520.50,
    "total_fees_earned": 2890.30,
    "total_il": -1450.20,
    "avg_trade_duration": "4.2 days"
  },
  "by_pool": {
    "HYPE/USD₮0": {
      "trades": 65,
      "win_rate": 0.75,
      "pnl": 2150.00
    },
    "HYPE/kHYPE": {
      "trades": 48,
      "win_rate": 0.69,
      "pnl": 1520.50
    }
  },
  "timeline": [
    {
      "date": "2024-01-15",
      "trades": 3,
      "pnl": 145.20,
      "win_rate": 1.0
    }
  ],
  "learning_progress": {
    "week_1": {
      "win_rate": 0.55,
      "confidence": 0.50
    },
    "week_2": {
      "win_rate": 0.65,
      "confidence": 0.65
    },
    "current": {
      "win_rate": 0.72,
      "confidence": 0.78
    }
  }
}
```

## Memory Operations

### Store Decision
```
FUNCTION store_decision(decision)
├── Generate unique ID
├── Timestamp
├── Store reasoning
├── Link to similar past decisions
└── Update pattern matching index
```

### Retrieve Similar Cases
```
FUNCTION get_similar_cases(current_conditions)
├── Extract feature vector
├── Search memory for similar conditions
├── Rank by similarity score
├── Retrieve outcomes
└── Return top N matches with confidence
```

### Learn from Outcome
```
FUNCTION learn_from_outcome(decision_id, outcome)
├── Retrieve original decision
├── Compare expected vs actual outcome
├── IF successful:
│   ├── Store in successes.json
│   ├── Update winning patterns
│   └── Increase pattern weight
├── IF failed:
│   ├── Store in mistakes.json
│   ├── Analyze root cause
│   ├── Create anti-pattern rule
│   └── Decrease pattern weight
└── Update performance metrics
```

### Query Memory
```
FUNCTION query_memory(query_type, params)
CASE query_type:
  "similar_conditions":
    RETURN get_similar_cases(params.conditions)

  "pool_performance":
    RETURN pool_knowledge[params.pool]

  "mistakes_to_avoid":
    RETURN relevant_mistakes(params.pool, params.action)

  "success_patterns":
    RETURN relevant_successes(params.pool, params.action)

  "learning_progress":
    RETURN performance.learning_progress
```

## Memory Maintenance

### Automatic Cleanup
```yaml
cleanup:
  # Compress old detailed data
  compress_after_days: 30

  # Keep only significant patterns
  significance_threshold: 0.05

  # Archive old data
  archive_after_days: 90
  archive_location: "memory/archive/"

  # Remove duplicates
  dedup_interval: "daily"
```

### Backup Schedule
```yaml
backup:
  # Incremental backup
  incremental: "hourly"

  # Full backup
  full: "daily"

  # Retention
  keep_hourly: 24
  keep_daily: 30
  keep_monthly: 12

  # Cloud sync (optional)
  cloud_sync: false
  cloud_endpoint: ""
```

## Memory Queries (Examples)

### For Entry Decision
```json
{
  "query": "similar_entries",
  "conditions": {
    "pool": "HYPE/USD₮0",
    "tvl_trend": "increasing",
    "volatility": "medium"
  },
  "returns": [
    "success_rate",
    "avg_pnl",
    "recommended_range",
    "warnings"
  ]
}
```

### For Exit Decision
```json
{
  "query": "similar_exits",
  "conditions": {
    "pool": "HYPE/USD₮0",
    "current_il": 5.2,
    "position_age_hours": 72,
    "out_of_range": true
  },
  "returns": [
    "optimal_exit_timing",
    "recovery_probability",
    "similar_outcomes"
  ]
}
```

## Memory Stats Command

```
"Show memory stats"

Output:
╔═══════════════════════════════════════════════════════════════╗
║                     MEMORY STATISTICS                          ║
╠═══════════════════════════════════════════════════════════════╣
║ STORAGE:                                                       ║
║ • Trades: 156 entries (2.3 MB)                                ║
║ • Patterns: 48 entries (156 KB)                               ║
║ • Mistakes: 23 entries (89 KB)                                ║
║ • Successes: 67 entries (201 KB)                              ║
║ • Pool Knowledge: 8 pools (45 KB)                             ║
║                                                                ║
║ LEARNING METRICS:                                              ║
║ • Total Decisions: 234                                         ║
║ • Patterns Identified: 48                                      ║
║ • Anti-Patterns Created: 12                                    ║
║ • Learning Maturity: 78%                                       ║
║                                                                ║
║ TOP POOL KNOWLEDGE:                                            ║
║ • HYPE/USD₮0: 65 trades, 75% win rate                         ║
║ • HYPE/kHYPE: 48 trades, 69% win rate                         ║
║ • HYPE/UBTC: 23 trades, 65% win rate                          ║
║                                                                ║
║ RECENT LEARNINGS (Last 7 days):                               ║
║ • Wide ranges work better in volatile conditions              ║
║ • Avoid entries during correlation breakdown                  ║
║ • Fee compounding increases APR by 15%                        ║
╚═══════════════════════════════════════════════════════════════╝
```

---

The memory system is the foundation of intelligent decision-making, enabling continuous improvement through experience.
