# 🎯 RANGO EA — Quantitative Regime-Adaptive DCA Trading System for MT5

[![Platform](https://img.shields.io/badge/Platform-MetaTrader%205-blue.svg)](https://www.metatrader5.com/)
[![Language](https://img.shields.io/badge/Language-MQL5-orange.svg)]()
[![Strategy](https://img.shields.io/badge/Strategy-Regime--Adaptive%20DCA%20%7C%20Mean%20Reversion-green.svg)]()
[![Backtest](https://img.shields.io/badge/Strategy%20Tester-100%25%20Free%20Simulation-brightgreen.svg)]()
[![Telegram Support](https://img.shields.io/badge/Telegram-@trading__world__support-blue.svg)](https://t.me/trading_world_support)

> **Rango EA** is an advanced, institutional-grade automated trading system for **MetaTrader 5 (MT5)**. It replaces traditional "blind" Dollar-Cost Averaging (DCA) and dangerous Martingale grids with an intelligent **Regime-Dependent Permission Engine**, **Trend Danger Scoring (TDS)**, and **Momentum-Adaptive Grid Stretching**.

---

## 📌 Table of Contents

- [Overview](#-overview)
- [Why Rango is Different (The Anti-Blowup DCA)](#-why-rango-is-different-the-anti-blowup-dca)
- [Key Features](#-key-features)
- [System Architecture & Quantitative Models](#-system-architecture--quantitative-models)
- [Quick Start & Installation Guide](#-quick-start--installation-guide)
- [How to Activate (Free Backtesting & Live Setup)](#-how-to-activate-free-backtesting--live-setup)
- [Mobile Push Notifications Setup (MT5 App & MetaQuotes ID)](#-mobile-push-notifications-setup-mt5-app--metaquotes-id)
- [Input Parameters Reference](#-input-parameters-reference)
- [Recommended Trading Setup](#-recommended-trading-setup)
- [Frequently Asked Questions (FAQ)](#-frequently-asked-questions-faq)
- [Support & Community](#-support--community)
- [Risk Disclaimer](#-risk-disclaimer)

---

## 💡 Overview

Traditional grid and Martingale systems inevitably fail during prolonged, one-directional trend explosions. They blindly stack orders into strong adverse momentum until the account suffers a margin call.

**Rango EA was built from the ground up to solve this exact problem.**

Instead of blindly opening orders at fixed pip intervals, Rango incorporates a **Multi-Factor Quantitative Permission Gate**:
1. It continuously monitors market regime persistence using **Hurst Exponent**, **ADX**, and **ATR Expansion Ratios**.
2. If the market is trending strongly against you, **DCA order expansion is strictly blocked**.
3. Recovery orders are only permitted when quantitative metrics confirm that adverse trend momentum has decayed and **mean-reversion probability is mathematically elevated**.

```
                        ┌─────────────────────────────────┐
                        │   Market Price & Volatility     │
                        └────────────────┬────────────────┘
                                         │
                 ┌───────────────────────┴───────────────────────┐
                 ▼                                               ▼
   ┌───────────────────────────┐                   ┌───────────────────────────┐
   │ Trend Danger Score (TDS)  │                   │   Exhaustion Score (ES)   │
   │  - ADX Trend Strength     │                   │  - Distance from 200 EMA  │
   │  - Hurst Memory (H > 0.55)│                   │  - RSI Extremes / Bands   │
   │  - Volatility Acceleration│                   │  - Momentum Decay Wicks   │
   └─────────────┬─────────────┘                   └─────────────┬─────────────┘
                 │                                               │
                 └───────────────────────┬───────────────────────┘
                                         ▼
                        ┌─────────────────────────────────┐
                        │   DCA Readiness Score (DRS)     │
                        └────────────────┬────────────────┘
                                         │
        ┌────────────────────────────────┼────────────────────────────────┐
        ▼                                ▼                                ▼
┌───────────────┐                ┌───────────────┐                ┌───────────────┐
│  DCA BLOCKED  │                │   DCA WATCH   │                │   DCA READY   │
│ Hold Expansion│                │ Await Trigger │                │ Execute Level │
└───────────────┘                └───────────────┘                └───────────────┘
```

---

## ⚡ Why Rango is Different (The Anti-Blowup DCA)

| Feature | Conventional DCA / Martingale | **Rango EA** |
| :--- | :--- | :--- |
| **Averaging Trigger** | Fixed pip distances or timer | **Quantitative DCA Readiness Score (DRS)** |
| **Trending Protection** | None (keeps averaging into crash) | **Anti-Expansion Gate & Trend Danger Block** |
| **Grid Spacing** | Static / Fixed | **Adaptive Dynamic Stretching (Up to 300%)** |
| **Directional Gate** | Opens dual Buy/Sell blindly | **Directional Isolation (No counter-trend clutter)** |
| **Risk Boundary** | Unbounded risk | **Capped Stop Loss & Survival Mode Defense** |
| **Visual Telemetry** | Basic or none | **Real-Time HUD Dashboard & Smart Signal Arrows** |
| **Strategy Tester** | Often hidden or limited | **100% Free, Unrestricted Backtesting** |

---

## 🚀 Key Features

### 1. 🛡️ DCA Readiness & Regime Permission Engine
- **Trend Danger Score (TDS: 0–100)**: Evaluates trend strength, Hurst persistence, and volatility acceleration. If $TDS > 65$, new DCA entries are frozen.
- **Exhaustion Score (ES: 0–100)**: Quantifies statistical stretch from the 200 EMA, RSI overbought/oversold extremes, candle body deceleration, and rejection pinbars.
- **Anti-Expansion Gate**: Instantly blocks averaging if short-term ATR surges violently over long-term ATR ($ATR_5 / ATR_{30} > 1.35$) during news spikes or breakout cascades.
- **Emergency DCA Kill Switch**: Automatically halts grid expansions if adverse market conditions exceed critical safety limits ($TDS > 85$).

### 2. 🎯 Directional DCA Isolation
- When market is in a confirmed **Uptrend**, Buy DCA is blocked and only Sell DCA mean-reversion is evaluated at exhaustion.
- When market is in a confirmed **Downtrend**, Sell DCA is blocked and only Buy DCA is evaluated at bottom exhaustion.
- Opposing dual signals are only allowed in **Sideways / Range-bound** conditions.

### 3. 📈 M5 Adaptive Recovery Grid (`CAdaptiveRecovery`)
- **Momentum-Based Spacing Stretching**: Grid levels automatically widen when strong adverse momentum candles appear.
- **Post-Fill SL Stretching**: Stop Loss distances only stretch after a pending level is executed, keeping initial risk locked.
- **Risk Budget Allocation**: Lot sizing dynamically calculates required recovery volume while strictly enforcing the base volume cap to prevent exponential lot explosion.
- **Survival Mode**: If risk budget reaches maximum capacity, all further expansions halt to protect equity.

### 4. 📊 Professional On-Chart Dashboard & Telemetry
- Clean, high-contrast visual display showing:
  - Current Market Regime & Volatility state.
  - Live **TDS** (Trend Danger Score) and **DRS** (DCA Readiness Score).
  - Win/Loss statistics, current recovery stage, and total committed risk in currency.

### 5. 🏹 Smart Signal Arrows & Telegram Notifications
- Clean arrow markers rendered only on **state transitions** with customizable cooldowns and auto-expiration cleanup (no chart clutter).
- Full **Telegram Bot Integration**: Delivers real-time trade signals, DCA readiness alerts, and chart screenshots directly to your private channel or group.

### 6. 🧪 Unrestricted MT5 Strategy Tester
- Backtest, stress test, and optimize Rango on historical data across any broker, instrument, and timeframe with **zero restrictions**.

---

## 🛠️ Quick Start & Installation Guide

### Prerequisites
- **Platform**: MetaTrader 5 (Build 3800 or newer recommended)
- **OS**: Windows 10 / 11 or Windows Server (VPS)
- **Account Type**: Hedging account recommended (standard or ECN with low spread)

---

### Step-by-Step Installation

1. **Download `Rango.ex5`**:
   - Download the latest compiled `Rango.ex5` from the [Releases](../../releases) tab or the repository root.

2. **Copy to MT5 Directory**:
   - Open your MetaTrader 5 terminal.
   - Click `File` ➔ `Open Data Folder`.
   - Navigate to `MQL5` ➔ `Experts\`.
   - Paste `Rango.ex5` into this folder.

3. **Enable Algorithmic Trading & DLL Imports**:
   - In MT5, go to `Tools` ➔ `Options` (or press `Ctrl + O`).
   - Select the **Expert Advisors** tab.
   - Check ✅ **Allow algorithmic trading**.
   - Check ✅ **Allow DLL imports** *(Required for hardware licensing validation & volume identification)*.
   - Check ✅ **Allow WebRequest for listed URL** and add `https://api.telegram.org` *(If using Telegram notifications)*.

4. **Attach Rango to Chart**:
   - In the **Navigator** window (`Ctrl + N`), expand **Expert Advisors**.
   - Drag and drop **Rango** onto your desired chart (e.g., `XAUUSD`, `EURUSD`, `GBPUSD`, `BTCUSD`).
   - Recommended Timeframe: **M5** or **M15**.
   - In the EA settings popup, ensure **Allow live trading** is checked, configure your risk parameters, and click **OK**.

---

## 🔑 How to Activate (Free Backtesting & Live Setup)

Rango offers two operating tiers:

### 1. 🧪 100% Free Strategy Tester (Simulations & Backtesting)
- **Zero license key required.**
- Open the MT5 Strategy Tester (`Ctrl + R`), select `Rango.ex5`, choose your symbol and date range, and click **Start**.
- Backtest any historical period with complete feature access!

### 2. 💻 Live / Demo Chart Activation
When you first attach Rango to a live or demo chart:
1. Rango runs in **View-Only Demo Mode** (live dashboard, calculations, and signals are active).
2. It automatically generates your unique machine serial and exports it to your terminal's file folder:
   ```
   MQL5\Files\Rango_Request_Serial.txt
   ```
3. Copy your Serial (format: `RANGO-REQ-XXXX-XXXX-XXXX-XXXX`).
4. Send your serial to Telegram Support: **[@trading_world_support](https://t.me/trading_world_support)** to receive your **Activation Key** (`RANGO-ACT-...`).
5. Paste the key into the input parameter `InpLicenseKey` (or leave it in cache once validated).

---

## 📲 Mobile Push Notifications Setup (MT5 App & MetaQuotes ID)

Rango EA supports instant push notifications delivered directly to the **MetaTrader 5 Mobile App** (iOS & Android) using the native MQL5 `SendNotification` engine. This ensures ultra-fast, direct alerts on your phone without requiring WebRequest configurations or Telegram bots.

### Step 1: Find Your MetaQuotes ID on Mobile
* **Android**: Open the MetaTrader 5 app ➔ Tap the Menu (☰) ➔ Select **Messages** (or **Settings** ➔ **Messages**) ➔ Note down your **MetaQuotes ID** (an 8-character alphanumeric string, e.g., `1A2B3C4D`).
* **iOS (iPhone/iPad)**: Open the MetaTrader 5 app ➔ Go to **Settings** ➔ Select **Chat and Messages** ➔ Find your **My MetaQuotes ID** at the bottom of the screen.

### Step 2: Configure Push Notifications in Desktop MT5
1. On your desktop MT5 terminal, open `Tools` ➔ `Options` (or press `Ctrl + O`).
2. Navigate to the **Notifications** tab.
3. Check ✅ **Enable Push Notifications**.
4. ⚠️ **IMPORTANT (Anti-Spam Configuration)**:
   * **UNCHECK** ❌ **Notifications from the local terminal**
   * **UNCHECK** ❌ **Notifications from the trade server**
   > 💡 **Why uncheck these?** If checked, the MT5 broker trade server will send a push notification for every single micro-action (every pending order placed, modified, or closed by the EA grid). Unchecking these two options ensures you **only** receive Rango's clean, high-priority analytical signals (Trend & DCA alerts) without broker spam!
5. Enter your **MetaQuotes ID** in the **MetaQuotes ID** field (multiple IDs can be separated by commas).
6. Click the **Test** button. You should immediately receive a test notification on your mobile phone!
7. Click **OK** to save the settings.

### Step 3: Configure Notification Settings in Rango EA
In the EA's Input Settings:
* `EnableNotifications = true` — Enable push notification alerts.
* `InpNotifyDCASignal = true` — Receive real-time push alerts when quantitative metrics trigger a **DCA READY** state.
* `InpNotifyTrendSignal = true` — Receive push alerts when a confirmed **Trend Pullback Signal** occurs.
* `InpMetaQuotesID = "..."` — Enter your MetaQuotes ID (Optional reference / terminal linkage).

---

## ⚙️ Input Parameters Reference

### General Settings
| Parameter | Default | Description |
| :--- | :--- | :--- |
| `InpLicenseKey` | `""` | Activation Key provided by support (Leave blank once cached). |
| `InputMagicNumber` | `0` | Unique EA identifier (`0` = Auto-allocated based on symbol hash). |
| `Language` | `LANG_ENGLISH` | Dashboard & alert language (`LANG_ENGLISH` or `LANG_VIETNAMESE`). |
| `InpShowDashboard` | `true` | Display the real-time HUD dashboard on chart. |
| `AutoTrading` | `false` | Enable automated order execution (`true` = Auto, `false` = Signals only). |
| `SinglePositionMode` | `true` | Restrict EA to one active strategy cycle at a time. |
| `ATR_Multiplier` | `1.0` | Global ATR multiplier for dynamic distance calculations. |
| `MaxSpreadPoints` | `0` | Max allowed spread in points (`0` = Disabled). |

### Risk Management
| Parameter | Default | Description |
| :--- | :--- | :--- |
| `RiskMode` | `RISK_BY_VOLUME` | Risk sizing mode (`RISK_BY_PERCENT`, `RISK_BY_AMOUNT`, `RISK_BY_VOLUME`). |
| `RiskDefaulMinimum` | `0.01` | Base lot size or risk percentage. |
| `RiskRewardRatio` | `1.0` | Default Risk:Reward ratio for single-entry targets. |
| `MaxVolumeFixed` | `0` | Hard volume cap per single order (`0` = Unlimited). |

### DCA Permission Engine Settings
| Parameter | Default | Description |
| :--- | :--- | :--- |
| `InpEnableDCAPermission` | `true` | Master toggle for the DCA Permission Gate. |
| `InpMinDRS_Ready` | `65.0` | Minimum DCA Readiness Score required to authorize averaging ($0–100$). |
| `InpMaxTDS_Allowed` | `65.0` | Maximum Trend Danger Score allowed before DCA is blocked ($0–100$). |
| `InpEnableAntiExpansion` | `true` | Freeze DCA on sudden ATR expansion spikes ($ATR_5/ATR_{30} > 1.35$). |
| `InpEnableDCAKillSwitch` | `true` | Emergency stop freezing new orders if $TDS > 85$. |
| `InpShowDCAArrows` | `true` | Plot visual DCA authorization arrows on chart. |
| `InpDCAArrowCooldownBars` | `3` | Minimum bar spacing between consecutive DCA arrows. |

### Recovery Grid Settings *(Dev/Backtest mode; Locked as global constants in Release Mode)*
| Parameter | Default | Description |
| :--- | :--- | :--- |
| `InpEnableRecovery` | `false` | Enable multi-level adaptive recovery grid. |
| `InpRecoveryDivision` | `6` | Maximum number of recovery grid levels. |
| `RecoveryMultiplier` | `1.25` | Lot multiplier across consecutive recovery orders. |
| `RecoveryTarget` | `3.00` | Basket Take Profit target (% of account balance). |
| `RecoveryDrawdown` | `10.0` | Maximum allowable drawdown limit (% of balance). |
| `EnableBreakeven` | `true` | Move basket to Breakeven when profit trigger is reached. |
| `EnableAdaptiveRecovery` | `true` | Enable dynamic momentum-based grid level stretching. |

### Push Notifications & Alerts
| Parameter | Default | Description |
| :--- | :--- | :--- |
| `EnableNotifications` | `true` | Master toggle for notifications. |
| `InpNotifyDCASignal` | `true` | Send mobile push alert when DCA state transitions to READY. |
| `InpNotifyTrendSignal` | `false` | Send mobile push alert on confirmed trend pullback signals. |
| `InpMetaQuotesID` | `""` | MetaQuotes ID for direct push delivery to MT5 Mobile app. |

---

## 📈 Recommended Trading Setup

| Asset Class | Recommended Symbols | Optimal Timeframe | Minimum Balance (0.01 lot) | Recommended Leverage |
| :--- | :--- | :--- | :--- | :--- |
| **Precious Metals** | `XAUUSD` (Gold) | `M5` / `M15` | $1,000 – $2,000 | 1:100 to 1:500 |
| **Major Forex** | `EURUSD`, `GBPUSD`, `USDJPY` | `M5` / `M15` | $500 – $1,000 | 1:100 to 1:500 |
| **Crypto** | `BTCUSD`, `ETHUSD` | `M15` / `H1` | $2,000+ | 1:50 to 1:200 |

> 💡 **Pro-Tip**: For maximum stability during high-impact news events (CPI, NFP, FOMC), ensure `InpEnableAntiExpansion = true` and `EnableNewsAlert = true`.

---

## ❓ Frequently Asked Questions (FAQ)

<details>
<summary><b>Q1: Can I backtest Rango in the MT5 Strategy Tester for free?</b></summary>
<br>
<b>Yes, 100%!</b> Rango has a built-in Strategy Tester bypass that allows unrestricted backtesting, parameter optimization, and visual mode simulations without requiring an activation key.
</details>

<details>
<summary><b>Q2: Why does MT5 show "DLL imports must be enabled"?</b></summary>
<br>
Rango uses Windows standard system API (`kernel32.dll`) to read the local volume serial for hardware-locked activation. Please check <b>"Allow DLL imports"</b> in <code>Tools ➔ Options ➔ Expert Advisors</code>.
</details>

<details>
<summary><b>Q3: Where can I find my Machine Serial for live activation?</b></summary>
<br>
When you place Rango on any live/demo chart, check the file:
<code>MetaTrader 5 ➔ MQL5 ➔ Files ➔ Rango_Request_Serial.txt</code>.
Alternatively, the serial is printed directly in the MT5 <b>Experts Log tab</b>.
</details>

<details>
<summary><b>Q4: Does Rango work on a VPS?</b></summary>
<br>
<b>Yes!</b> Rango is optimized for low CPU usage and works seamlessly on any Windows VPS (24/7 continuous operation recommended).
</details>

<details>
<summary><b>Q5: How do I receive push alerts on my phone without being spammed?</b></summary>
<br>
Find your <b>MetaQuotes ID</b> inside the MT5 Mobile app (Android or iOS), enter it in desktop MT5 under <code>Tools ➔ Options ➔ Notifications</code>, and check <b>"Enable Push Notifications"</b>. 
<br><br>
⚠️ <b>Anti-Spam Tip:</b> Make sure to <b>UNCHECK</b> <i>"Notifications from the local terminal"</i> and <i>"Notifications from the trade server"</i> so you only receive Rango's high-value strategy signals instead of an alert for every single grid order modification!
</details>

---

## 💬 Support & Community

- ✈️ **Telegram Support & License Activation**: [@trading_world_support](https://t.me/trading_world_support)
- 📺 **YouTube Channel**: Subscribe for tutorial videos, parameter optimization guides, and live trading sessions.
- 🐛 **Issue Tracker**: If you find a bug or have a feature request, feel free to open an issue on this GitHub repository.

---

## ⚠️ Risk Disclaimer

Trading foreign exchange (Forex), contracts for difference (CFDs), metals, and cryptocurrencies carries a high level of risk and may not be suitable for all investors. The high degree of leverage can work against you as well as for you. Before deciding to trade or use automated trading systems, you should carefully consider your investment objectives, level of experience, and risk appetite.

Past performance is not indicative of future results. The software and materials provided in this repository are for educational and informational purposes. You are solely responsible for all trading decisions and financial outcomes on your live accounts.

---

<div align="center">
  <sub>Built with ❤️ by World Trading Lab. All Rights Reserved.</sub>
</div>
