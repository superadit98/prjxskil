# 🦞 PRJX Liquidity Manager - OpenClaw Skill

## 📋 Overview

**PRJX Liquidity Manager** adalah skill OpenClaw yang mengotomatisasi manajemen liquidity di PRJX (Project X) DEX dengan fitur:

- 🤖 **Full AI Automation** - AI mengambil keputusan dan eksekusi sendiri
- 🧠 **Auto-Learning** - Belajar dari kesalahan dan kesuksesan
- 📊 **Paper Trading** - Simulasi tanpa risiko uang nyata
- 🛡️ **Risk Management** - Stop-loss, take-profit, IL monitoring
- 📱 **Multi-Channel Alerts** - Notifikasi ke Telegram/WhatsApp/Discord

---

## 🚀 Quick Start

### Prasyarat
1. OpenClaw terinstall di komputer/server Anda
2. Wallet dengan dana di HyperEVM (HYPE, USD₮0, dll)
3. Akun Telegram untuk notifikasi (opsional)

### Instalasi Skill

```bash
# Clone atau copy skill ke folder OpenClaw
cp -r prjx-liquidity-skill ~/.openclaw/skills/

# Restart OpenClaw
openclaw restart
```

### Konfigurasi Awal

Katakan kepada OpenClaw:
```
"Aktifkan skill PRJX Liquidity Manager.
 Budget saya $5000.
 Mode paper trading dulu.
 Notifikasi ke Telegram.
 Risk moderate - max IL 10%."
```

---

## 📁 Struktur File

```
prjx-liquidity-skill/
├── SKILL.md              # Main skill definition
├── LEARNING.md           # Auto-learning system
├── PAPER_TRADING.md      # Paper trading mode
├── RISK_MANAGEMENT.md    # Risk management rules
├── MEMORY.md             # Memory system docs
├── README.md             # This file
│
├── memory/               # Persistent memory storage
│   ├── trades.json       # Trade history
│   ├── learning.json     # Learned patterns
│   ├── mistakes.json     # Mistakes & lessons
│   ├── successes.json    # Success patterns
│   ├── pool_knowledge.json
│   └── performance.json
│
├── logs/                 # Log files
│   └── decisions.log
│
└── data/                 # Runtime data
    ├── paper_positions.json
    └── config.json
```

---

## 🎯 Fitur Utama

### 1. Liquidity Management

| Fitur | Deskripsi |
|-------|-----------|
| **Add Liquidity** | Tambah posisi dengan optimal range otomatis |
| **Remove Liquidity** | Tarik posisi penuh atau parsial |
| **Rebalance** | Adjust range saat harga bergerak |
| **Collect Fees** | Klaim fee yang belum diklaim |

### 2. Auto-Learning System

```
┌────────────────────────────────────────────────┐
│          AUTO-LEARNING FLOW                     │
├────────────────────────────────────────────────┤
│                                                 │
│  Decision ──▶ Execute ──▶ Observe Outcome      │
│      │                         │                │
│      ▼                         ▼                │
│  ┌─────────────────────────────────────┐       │
│  │           MEMORY STORAGE            │       │
│  │  • What worked (Successes)          │       │
│  │  • What failed (Mistakes)           │       │
│  │  • Patterns identified              │       │
│  └─────────────────────────────────────┘       │
│      │                                          │
│      ▼                                          │
│  ┌─────────────────────────────────────┐       │
│  │      IMPROVED DECISIONS             │       │
│  │  Higher Confidence │ Better Timing  │       │
│  └─────────────────────────────────────┘       │
│                                                 │
└────────────────────────────────────────────────┘
```

### 3. Paper Trading Mode

| Mode | Deskripsi |
|------|-----------|
| **Paper** | Semua operasi simulasi, tidak ada uang nyata |
| **Shadow** | Paper trade parallel dengan live trading |
| **Hybrid** | Validasi di paper dulu, baru eksekusi live |
| **Live** | Operasi nyata dengan uang sungguhan |

### 4. Risk Management

```
┌─────────────────────────────────────────────────┐
│            RISK PROTECTION LAYERS               │
├─────────────────────────────────────────────────┤
│                                                  │
│  🔴 PORTFOLIO LEVEL                             │
│     • Max Total Exposure: $50,000               │
│     • Max Daily Loss: 5%                        │
│     • Max Drawdown: 20%                         │
│                                                  │
│  🟠 POSITION LEVEL                              │
│     • Max Position: $10,000                     │
│     • Stop-Loss: 15%                            │
│     • Take-Profit: 50%                          │
│                                                  │
│  🟡 POOL LEVEL                                  │
│     • Max IL Tolerance: 10%                     │
│     • Min Pool Liquidity: $100,000              │
│     • Min APR: 15%                              │
│                                                  │
│  🟢 MARKET LEVEL                                │
│     • Volatility Monitoring                     │
│     • Correlation Checks                        │
│     • Trend Analysis                            │
│                                                  │
└─────────────────────────────────────────────────┘
```

---

## 💬 Contoh Perintah

### Position Management
```
"Add liquidity $1000 ke HYPE/USD₮0 dengan range +-5%"
"Show semua posisi saya"
"Remove 50% dari posisi HYPE/kHYPE"
"Rebalance posisi yang out of range"
"Collect semua fees yang belum diklaim"
```

### Monitoring
```
"Berapa IL saya sekarang?"
"Check portfolio health"
"Alert jika HYPE turun 10%"
"Show APR untuk semua posisi"
```

### Paper Trading
```
"Enable paper trading mode"
"Run simulation dengan $5000 selama 7 hari"
"Compare paper vs live performance"
"Graduate strategy ke live trading"
```

### Learning
```
"What have you learned about HYPE/USD₮0?"
"Show your biggest mistakes"
"How confident are you about this decision?"
"Show learning progress"
```

### Risk Management
```
"Set stop-loss 10% untuk semua posisi"
"Set take-profit 30% untuk HYPE/kHYPE"
"Emergency stop"
"Resume trading"
"Adjust risk to aggressive mode"
```

---

## 📊 Dashboard & Reports

### Daily Report
```
╔═══════════════════════════════════════════════════════════════╗
║                    DAILY PORTFOLIO REPORT                      ║
╠═══════════════════════════════════════════════════════════════╣
║ 📅 Date: 2024-01-15                                           ║
║                                                                ║
║ 💰 PORTFOLIO VALUE: $5,245.67 (+2.46%)                        ║
║                                                                ║
║ 📊 POSITIONS:                                                  ║
║ ┌──────────────┬─────────┬─────────┬─────────┬──────────────┐║
║ │ Pool         │ Value   │ Fees    │ IL      │ Health       │║
║ ├──────────────┼─────────┼─────────┼─────────┼──────────────┤║
║ │ HYPE/USD₮0   │ $2,500  │ +$45.23 │ -$12.50 │ 🟢 78/100    │║
║ │ HYPE/kHYPE   │ $2,500  │ +$38.91 │ -$8.20  │ 🟢 82/100    │║
║ └──────────────┴─────────┴─────────┴─────────┴──────────────┘║
║                                                                ║
║ 📈 TODAY'S PERFORMANCE:                                        ║
║ • Fees Earned: $84.14                                          ║
║ • Net PnL: +$126.44                                            ║
║ • Decisions: 3 (All successful)                               ║
║                                                                ║
║ 🧠 LEARNING UPDATE:                                            ║
║ • New pattern identified: Wide ranges in volatile pools       ║
║ • Win rate improved: 72% → 75%                                ║
║ • Confidence: 78%                                              ║
╚═══════════════════════════════════════════════════════════════╝
```

---

## ⚙️ Konfigurasi

### File: data/config.json

```json
{
  "trading_mode": "paper",
  "risk_level": "moderate",

  "limits": {
    "max_total_exposure": 50000,
    "max_position_size": 10000,
    "max_il_tolerance": 10,
    "stop_loss_percent": 15,
    "take_profit_percent": 50
  },

  "automation": {
    "auto_rebalance": true,
    "auto_compound": true,
    "auto_execute_confidence": 0.7
  },

  "notifications": {
    "telegram": {
      "enabled": true,
      "bot_token": "YOUR_BOT_TOKEN",
      "chat_id": "YOUR_CHAT_ID"
    },
    "alert_levels": ["critical", "warning", "info"]
  },

  "learning": {
    "enabled": true,
    "mode": "continuous",
    "min_confidence": 0.7
  },

  "monitoring": {
    "interval_minutes": 15,
    "report_schedule": "0 9 * * *"
  }
}
```

### Risk Levels

| Level | Max Exposure | Stop Loss | IL Tolerance | Auto Execute |
|-------|-------------|-----------|--------------|--------------|
| **Conservative** | $25,000 | 10% | 5% | 0.85+ |
| **Moderate** | $50,000 | 15% | 10% | 0.70+ |
| **Aggressive** | $100,000 | 25% | 20% | 0.60+ |

---

## 🔧 Troubleshooting

### Skill Tidak Muncul
```bash
# Check skill location
ls ~/.openclaw/skills/prjx-liquidity-skill/

# Check OpenClaw logs
tail -f ~/.openclaw/logs/main.log

# Restart OpenClaw
openclaw restart
```

### Wallet Tidak Terdeteksi
```
"Connect my wallet to PRJX"
"Check wallet connection status"
```

### Learning Tidak Berjalan
```
"Enable learning mode"
"Check learning status"
"Reset learning memory"
```

### Paper Trading Tidak Akurat
```
"Recalibrate paper trading"
"Sync paper with live prices"
"Check simulation accuracy"
```

---

## 🛡️ Safety Features

1. **Default Paper Mode** - Semua baru mulai di paper trading
2. **Minimum Testing** - Harus test minimal 7 hari sebelum live
3. **Confidence Threshold** - Hanya eksekusi jika confidence >= 70%
4. **Emergency Stop** - Command "EMERGENCY STOP" untuk pause semua
5. **Circuit Breaker** - Auto pause setelah 3 consecutive losses
6. **Max Drawdown Protection** - Auto pause jika drawdown > 20%

---

## 📈 Expected Performance

Berdasarkan backtest dan paper trading:

| Metric | Conservative | Moderate | Aggressive |
|--------|-------------|----------|------------|
| Monthly Return | 5-10% | 10-20% | 20-40% |
| Max Drawdown | 5% | 10% | 20% |
| Win Rate | 75%+ | 65%+ | 55%+ |
| Sharpe Ratio | 2.0+ | 1.5+ | 1.0+ |

---

## 🆘 Support

### Commands Bantuan
```
"Help me with PRJX skill"
"Show available commands"
"Explain how learning works"
"What is impermanent loss?"
```

### Debug Commands
```
"Show debug info"
"Export all memory data"
"Show last 10 decisions"
"Recalculate all positions"
```

---

## 📜 Changelog

### v1.0.0 (Initial Release)
- Full AI automation
- Auto-learning system
- Paper trading mode
- Risk management framework
- Multi-channel notifications
- Pool-specific knowledge base

---

## 📄 License

MIT License - Free to use and modify

---

## 🙏 Credits

- OpenClaw Team - Platform
- PRJX (Project X) - DEX Platform
- Community Contributors

---

**🦞 Happy Liquidity Managing!**

*"The AI that actually does things with your liquidity."*
