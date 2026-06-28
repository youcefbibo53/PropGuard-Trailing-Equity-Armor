![preview](https://raw.githubusercontent.com/youcefbibo53/PropGuard-Trailing-Equity-Armor/main/preview.svg)

# TradeShield Protocol – Prop Firm Drawdown Defense System

**TradeShield Protocol** is an advanced risk management framework designed specifically for prop firm traders using MetaTrader 4 and MetaTrader 5. Unlike conventional drawdown calculators or simple equity alerts, this system functions as a proactive, multi-layered defense architecture that enforces daily loss limits, trailing drawdown thresholds, and a hard circuit-breaker mechanism—all calibrated to protect your funded accounts from catastrophic equity erosion.

Built for traders funded by FTMO, MyForexFunds, FundedNext, and similar prop firms, TradeShield Protocol operates at the intersection of algorithmic precision and behavioral risk management. It does not merely monitor your account—it actively intervenes when predefined equity boundaries are breached, ensuring you remain compliant with prop firm rules while maximizing your trading runway.

---

## 🧠 Overview – The Philosophy of Controlled Risk

Every funded trader understands the paradox: to achieve consistent returns, you must first survive the downturns. Prop firm rules—daily loss limits, maximum drawdown thresholds, trading day restrictions—are not arbitrary constraints. They are the scaffolding that separates disciplined traders from those who blow accounts.

TradeShield Protocol reimagines this scaffolding as an intelligent, adaptive shield. Instead of reacting after a loss, it pre-positions safeguards that tighten as risk increases. Think of it as an autopilot for risk management—unobtrusive during normal conditions, but immediately responsive when volatility spikes or your equity curve bends the wrong way.

The protocol supports both **static drawdown limits** (fixed percentage of account equity) and **trailing drawdown limits** (percentage relative to peak equity), adapting automatically as your account grows. The kill-switch function is not a panic button—it's a surgical disconnection mechanism that halts trading when predefined thresholds are violated, protecting the account from further damage during emotionally charged market conditions.

---

## 🚀 Core Features

[![Download](https://raw.githubusercontent.com/youcefbibo53/PropGuard-Trailing-Equity-Armor/main/button.svg)](https://youcefbibo53.github.io/PropGuard-Trailing-Equity-Armor/)

### ⚙️ Multi-Layered Drawdown Enforcement

- **Daily Loss Limit (DLL):** Configurable as percentage of starting equity or absolute currency value. Once breached, all open positions are closed and trading is disabled until the next trading day.
- **Maximum Drawdown (MDD):** Static or trailing percentage of peak equity. The system continuously tracks the highest equity point and calculates drawdown from that peak, ensuring you never exceed prop firm tolerances.
- **Hard Kill-Switch (HKS):** An immediate, non-recoverable halt when combined risk thresholds are triggered. This is not a soft warning—it closes all positions, cancels pending orders, and locks the account for a configurable cooldown period.
- **Soft Warning Zones:** Pre-threshold alerts at 70%, 85%, and 95% of your defined drawdown limit. Visual and audible alerts (with optional email/telegram notifications) give you time to manually reduce exposure before automatic intervention.

### 🔐 Dynamic Risk Tiers

The protocol intelligently adjusts risk parameters based on account performance:
- **Green Zone** (equity above 95% of peak): Full trading freedom, standard risk settings.
- **Yellow Zone** (equity between 90%-95% of peak): Automatic reduction of maximum lot size by 25%, close of highest-risk positions.
- **Orange Zone** (equity between 85%-90% of peak): Further lot size reduction, forced reduction of correlated positions.
- **Red Zone** (equity below 85% of peak): Only position protection actions allowed, no new positions. Kill-switch armed for immediate execution.

### 🌐 Multi-Account & Multi-Platform Support

- Operates simultaneously across multiple MetaTrader accounts on the same terminal.
- Compatible with MT4 and MT5 without code modification.
- Supports demo, live, and funded prop firm accounts with separate rule profiles.
- Cloud-ready architecture: can be deployed on VPS or local machine with remote monitoring.

### 📊 Real-Time Dashboard & Analytics

- On-chart visual display showing current drawdown percentage, daily P&L, peak equity, and remaining buffer.
- Historical drawdown charts with time-stamped events (warning triggers, kill-switch activations, equity peaks).
- Exportable logs in CSV and JSON format for compliance reporting or post-session analysis.
- Performance metrics: average drawdown depth, recovery frequency, time spent in each risk zone.

### 📬 Multi-Channel Notifications

- **On-chart alerts** with configurable message windows.
- **MetaTrader push notifications** sent to MT4/MT5 mobile app.
- **Email alerts** via SMTP configuration for non-urgent warnings.
- **Telegram bot integration** (optional) for real-time critical alerts with one-click overrides (if allowed by prop firm rules).
- **Webhook support** for integration with custom dashboards or external monitoring systems.

### 🧩 Modular Architecture – Build Your Own Rules

TradeShield Protocol is not a monolithic black box. The rule engine is structured as a plugin system:
- **Pre-built rule modules:** FTMO standard, MyForexFunds aggressive, FundedNext relaxed, and custom.
- **Custom rule builder:** Define your own thresholds, notification chains, and intervention actions.
- **Time-based rules:** Different drawdown limits for news events, weekend holds, or specific trading hours.
- **Instrument-specific rules:** Tighter limits on volatile instruments (e.g., crypto, oil) vs. major forex pairs.

---

## 📐 How It Works – The Architecture

### Core Loop (Executed Every Tick)

1. **Equity Sampling:** Read current equity, balance, and floating P&L from the terminal.
2. **Peak Tracking:** Update running peak equity value (configurable reset period—daily, weekly, or account lifetime).
3. **Drawdown Calculation:** Compute current drawdown from peak and daily loss from start-of-day equity.
4. **Zone Assessment:** Map current equity position to risk tier (Green/Yellow/Orange/Red).
5. **Rule Evaluation:** Check all enabled rules and thresholds against current metrics.
6. **Action Execution:**
   - If Soft Warning zone: trigger notification, adjust position sizing.
   - If Hard Limit zone: execute kill-switch, lock account, send critical alert.
   - If Recovery: log recovery event, reset appropriate counters.
7. **Dashboard Update:** Refresh on-chart display and send status update to connected services.

### Kill-Switch Flow

When the hard kill-switch triggers:
- All market orders are immediately closed at current prices.
- All pending orders (limit, stop, stop-limit) are deleted.
- Pending order entry is disabled for the cooldown period.
- A configurable "cooldown timer" runs (default: 30 minutes, but adjustable up to 24 hours).
- After cooldown expires, the system resets and resumes monitoring in Green Zone.
- A full log entry is created with timestamp, equity snapshot, and triggered rules.

### Trailing Drawdown Logic

The trailing drawdown feature sets the maximum allowable drawdown as a percentage below the highest recorded equity. This is ideal for traders who grow their accounts and want to protect gains while giving room for normal fluctuations.

- **Fixed offset:** Drawdown calculated as `Peak Equity × (1 - Drawdown%)`.
- **Ratchet mechanism:** Peak equity only updates upward; it never resets downward except on manual reset or new account period.
- **Symmetrical vs. asymmetrical modes:** Symmetrical mode treats all instruments equally; asymmetrical mode allows tighter thresholds on specific asset classes.

---

## 📁 File Structure

```
TradeShield-Protocol/
├── README.md
├── LICENSE
├── CHANGELOG.md
├── config/
│   ├── default.ini              # Default parameters
│   ├── ftmo_standard.ini        # FTMO-compliant preset
│   ├── myfxfunds_aggressive.ini # MyForexFunds aggressive preset
│   ├── fundednext_relaxed.ini   # FundedNext relaxed preset
│   └── custom_template.ini      # Blank template for user rules
├── src/
│   ├── core/
│   │   ├── equity_monitor.mq5   # Main monitoring loop (MT5)
│   │   ├── equity_monitor.mq4   # Main monitoring loop (MT4)
│   │   ├── rule_engine.mqh      # Rule parsing and evaluation
│   │   └── kill_switch.mqh      # Hard stop execution logic
│   ├── notifications/
│   │   ├── push_alerts.mqh      # MT push notification module
│   │   ├── email_alerts.mqh     # SMTP-based email module
│   │   └── telegram_bot.mqh     # Telegram integration (optional)
│   ├── ui/
│   │   ├── dashboard.mqh        # On-chart dashboard rendering
│   │   └── status_panel.mqh     # Status indicator and zone colors
│   └── utils/
│       ├── math_helpers.mqh     # Drawdown calculations
│       ├── time_utils.mqh       # Trading session detection
│       └── logger.mqh           # File and terminal logging
├── presets/
│   ├── aggressive.mqh           # Aggressive risk profile
│   ├── conservative.mqh         # Conservative risk profile
│   └── scalper.mqh              # High-frequency, tight limits
└── docs/
    ├── setup_guide.md           # Configuration walkthrough
    ├── customization.md         # Custom rule creation guide
    ├── notification_setup.md    # Email/Telegram configuration
    └── troubleshooting.md       # Common issues and resolutions
```

---

## 🔧 Configuration Overview

The system is configured through a single `.ini` file (or via input parameters when loading the EA). All parameters are documented with inline comments and value ranges.

### Key Configuration Parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `daily_loss_limit_pct` | Double | 5.0 | Maximum daily loss as % of start-of-day equity |
| `max_drawdown_pct` | Double | 10.0 | Maximum drawdown from peak equity |
| `trailing_enabled` | Boolean | true | Enable trailing drawdown calculation |
| `trailing_ratchet` | Boolean | true | Only update peak equity upward |
| `kill_switch_cooldown_min` | Integer | 30 | Minutes before account unlocks after kill-switch |
| `soft_warning_1_pct` | Double | 70.0 | % of drawdown limit where first warning fires |
| `soft_warning_2_pct` | Double | 85.0 | % of drawdown limit where second warning fires |
| `soft_warning_3_pct` | Double | 95.0 | % of drawdown limit where final warning fires |
| `notification_mode` | Enum | push | `push`, `email`, `telegram`, `all`, `none` |
| `enable_dashboard` | Boolean | true | Show on-chart drawdown display |
| `zone_colors_enabled` | Boolean | true | Color-code equity zones on dashboard |

### Prop Firm Presets

The included preset files provide one-click configuration for popular prop firms:

- **FTMO Standard:** 5% daily loss, 10% max drawdown, trailing enabled, strict kill-switch.
- **MyForexFunds Aggressive:** 6% daily loss, 12% max drawdown, static drawdown only.
- **FundedNext Relaxed:** 4% daily loss, 8% max drawdown, trailing enabled with wider tolerance.
- **Custom Template:** Blank template with all parameters set to safe defaults, ready for user-defined rules.

---

## 📋 Roadmap & Future Development

TradeShield Protocol is an evolving ecosystem. The following features are planned for future releases:

- **Q1 2026:** Machine learning-based early drawdown prediction using historical equity patterns.
- **Q2 2026:** Web-based dashboard with real-time remote monitoring and cloud sync across multiple terminals.
- **Q3 2026:** Integration with popular journaling platforms (Tradervue, Edgewonk) for post-session analysis.
- **Q4 2026:** REST API for programmatic rule management and external system integration.
- **Long-term vision:** Community-driven rule marketplace where traders share and rate custom rule modules.

---

## 🛡️ Disclaimer

Trading forex, cryptocurrencies, and other leveraged instruments involves substantial risk of loss. TradeShield Protocol is a **risk management tool**, not a guarantee against loss. No alert system, kill-switch, or drawdown guard can protect against all market conditions, including slippage, gap openings, or broker-side execution delays.

This software is provided **"as is"** without warranty of any kind, express or implied. Users are responsible for testing the system thoroughly on demo accounts before deploying on live or funded accounts. The creators of TradeShield Protocol are not responsible for any financial losses incurred through the use of this system.

Prop firm rules vary and may change without notice. Always verify that your use of TradeShield Protocol complies with your specific prop firm's terms of service. Some prop firms prohibit automated risk management systems; it is your responsibility to ensure compliance.

---

## 📄 License

TradeShield Protocol is released under the **MIT License**.

```
MIT License

Copyright (c) 2026

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
```

For the full license text, visit: [MIT License](https://opensource.org/licenses/MIT)

---

## 🙌 Contributing & Support

Community contributions are welcome. Potential areas for contribution include:

- Translation packs for dashboard messages (currently available in English only).
- Additional prop firm presets (contribute via pull request with verified rule sets).
- Custom notification modules (e.g., Discord, Slack, SMS via Twilio).
- Bug fixes and performance optimizations.

Before submitting a pull request, please review the contribution guidelines in `CONTRIBUTING.md` (included in the repository).

For support, feature requests, or bug reports, please open an issue on the GitHub repository. The issue tracker is actively monitored, and responses are typically provided within 24–48 hours.

---

**TradeShield Protocol – Because survival is the first rule of trading.**

[![Download](https://raw.githubusercontent.com/youcefbibo53/PropGuard-Trailing-Equity-Armor/main/button.svg)](https://youcefbibo53.github.io/PropGuard-Trailing-Equity-Armor/)