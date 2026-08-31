# 🎯 RANGO EA — 用于 MT5 的量化市场状态自适应 DCA 交易系统

[![Platform](https://img.shields.io/badge/Platform-MetaTrader%205-blue.svg)](https://www.metatrader5.com/)
[![Language](https://img.shields.io/badge/Language-MQL5-orange.svg)]()
[![Strategy](https://img.shields.io/badge/Strategy-Regime--Adaptive%20DCA%20%7C%20Mean%20Reversion-green.svg)]()
[![Backtest](https://img.shields.io/badge/Strategy%20Tester-100%25%20Free%20Simulation-brightgreen.svg)]()
[![Telegram Support](https://img.shields.io/badge/Telegram-@trading__world__support-blue.svg)](https://t.me/trading_world_support)

> **Rango EA** 是一款专为 **MetaTrader 5 (MT5)** 设计的机构级高级量化交易系统。它通过智能**市场状态依赖准入引擎 (Regime-Dependent Permission Engine)**、**趋势危险评分 (TDS)** 和 **统计力竭评分 (ES)**，彻底取代了传统的“盲目”马丁格尔 (Martingale) 网格与无风控加仓。

---

## 📌 目录

- [系统概述](#-系统概述)
- [为什么 Rango 独具优势（防爆仓 DCA）](#-为什么-rango-独具优势防爆仓-dca)
- [核心功能与特性](#-核心功能与特性)
- [系统架构与量化数学模型](#-系统架构与量化数学模型)
- [快速入门与安装指南](#-快速入门与安装指南)
- [激活流程（免费策略回测与实盘设置）](#-激活流程免费策略回测与实盘设置)
- [移动端推送通知设置（MT5 App 与 MetaQuotes ID）](#-移动端推送通知设置mt5-app-与-metaquotes-id)
- [输入参数详解 (Input Parameters)](#-输入参数详解-input-parameters)
- [推荐交易配置](#-推荐交易配置)
- [常见问题解答 (FAQ)](#-常见问题解答-faq)
- [技术支持与社区](#-技术支持与社区)
- [风险免责声明](#-风险免责声明)

---

<div align="center">

## 🎬 Video Tutorials

<a href="https://www.youtube.com/playlist?list=PLHwVdyPeKoh0">
  <img src="https://img.youtube.com/vi/a6Jrm4S1b4A/maxresdefault.jpg"
       alt="Watch the video tutorial playlist"
       width="800">
</a>

<br>

**▶️ [Watch the full playlist on YouTube](https://www.youtube.com/playlist?list=PLHwVdyPeKoh0)**

</div>

## 💡 系统概述

传统的网格与马丁格尔系统在遭遇持久的单边单向强趋势时必然会走向爆仓。它们盲目地在强烈的反向动能中不断加仓，直到账户触发强制平仓（Margin Call）。

**Rango EA 的诞生正是为了彻底解决这一痛点。**

Rango 不再依据固定点数间隔盲目下单，而是引入了**多因子量化准入闸门 (Multi-Factor Quantitative Permission Gate)**：
1. 持续监测市场状态持续性（利用 **赫斯特指数 Hurst Exponent**、**ADX** 和 **ATR 波动率扩张比率**）。
2. 如果市场出现强烈的反向单边趋势，**系统将严格冻结并阻断任何新的 DCA 加仓**。
3. 仅当量化指标确认逆向趋势动能已耗尽、且**均值回归概率在数学统计上达到显著高位**时，才允许入场与加仓。

```
                        ┌─────────────────────────────────┐
                        │        市场价格与波动率数据       │
                        └────────────────┬────────────────┘
                                         │
                 ┌───────────────────────┴───────────────────────┐
                 ▼                                               ▼
   ┌───────────────────────────┐                   ┌───────────────────────────┐
   │    趋势危险评分 (TDS)      │                   │      统计力竭评分 (ES)      │
   │  - ADX 趋势强度指标        │                   │  - 偏离 200 EMA 距离      │
   │  - 赫斯特记忆性 (H > 0.55) │                   │  - RSI 极端超买/超卖与布林带│
   │  - ATR 波动率加速度        │                   │  - 动能衰减引线 (Pinbar)   │
   └─────────────┬─────────────┘                   └─────────────┬─────────────┘
                 │                                               │
                 └───────────────────────┬───────────────────────┘
                                         ▼
                        ┌─────────────────────────────────┐
                        │      DCA 就绪评分 (DRS)         │
                        └────────────────┬────────────────┘
                                         │
        ┌────────────────────────────────┼────────────────────────────────┐
        ▼                                ▼                                ▼
┌───────────────┐                ┌───────────────┐                ┌───────────────┐
│  DCA BLOCKED  │                │   DCA WATCH   │                │   DCA READY   │
│   加仓严格阻断  │                │   等待触发信号 │                │   执行加仓信号 │
└───────────────┘                └───────────────┘                └───────────────┘
```

---

## ⚡ 为什么 Rango 独具优势（防爆仓 DCA）

| 功能特性 | 传统 DCA / 马丁格尔网格 | **Rango EA** |
| :--- | :--- | :--- |
| **加仓触发机制** | 固定点数距离或固定时间间隔 | **量化 DCA 就绪评分 (DRS)** |
| **单边趋势保护** | 无（逆势疯狂加仓直到爆仓） | **防波动扩张闸门与趋势危险阻断引擎** |
| **时机精准度** | 盲目入场 / 机械点数 | **动能衰竭与统计学均值回归反转确认** |
| **方向隔离过滤** | 盲目双向同时开单 | **定向隔离机制（严禁逆势加仓杂波）** |
| **风险控制边界** | 无限下行风险 | **严格资金管理与动态 ATR 止损保护** |
| **直观图表显示** | 无或仅有简单文本 | **实时 HUD 仪表盘与智能状态切换箭头** |
| **策略回测支持** | 往往受限或被收费隐藏 | **100% 免费、无限制策略测试器支持** |

---

## 🚀 核心功能与特性

### 1. 🛡️ DCA 准入与市场状态引擎
- **趋势危险评分 (TDS: 0–100)**：综合评估趋势强度、赫斯特持续性和波动加速度。当 $TDS > 65$ 时，自动冻结所有新 DCA 加仓。
- **统计力竭评分 (ES: 0–100)**：量化价格相对 200 EMA 的极端偏离、RSI 超买超卖、K 线实体减速与价格拒绝长引线。
- **防波动扩张闸门 (Anti-Expansion Gate)**：若短期 ATR 剧烈超越长期 ATR（$ATR_5 / ATR_{30} > 1.35$），在突发新闻或剧烈突破时瞬间锁死加仓。
- **紧急 DCA 熔断机制 (Kill Switch)**：当市场极端逆势超过安全阈值（$TDS > 85$）时，全自动终止加仓。

### 2. 🎯 定向 DCA 隔离机制
- 当市场处于确认的**上升趋势 (Uptrend)** 时：阻断所有 Buy DCA，仅在顶部动能衰竭时评估 Sell DCA 均值回归。
- 当市场处于确认的**下降趋势 (Downtrend)** 时：阻断所有 Sell DCA，仅在底部动能衰竭时评估 Buy DCA 均值回归。
- 双向对冲加仓仅在**震荡/盘整 (Sideways/Range)** 市场中开放。

### 3. 🛡️ 专业风险控制与多模式仓位计算
- **多维度仓位管理**：支持固定手数 (Volume)、固定风险金额 ($) 或账户净值风险百分比 (%)。
- **全自动 SL/TP 动态投射**：基于大周期 ATR 波动率与自定义风险收益比 (Risk:Reward) 动态计算目标。
- **执行安全保障**：点差过滤器、经纪商最小挂单距离校验以及交易前可用保证金充足性检查。

### 4. 📊 专业的图表 HUD 仪表盘与遥测
- 高对比度清爽 HUD 界面，实时显示：
  - 当前市场状态 (Market Regime) 与波动率环境。
  - 实时 **TDS**（趋势危险度）与 **DRS**（DCA 就绪度）。
  - 胜率统计、趋势回调状态与实时盈亏。

### 5. 🏹 智能信号箭头与移动端推送
- 仅在**状态转移**时绘制清晰的箭头指示，具备冷却过滤与过期自动清理机制（保持图表整洁）。
- **直接 MT5 移动端推送**：通过 MetaQuotes ID 将高价值交易信号即时发送至手机。

### 6. 🧪 100% 免费 MT5 策略测试器回测
- 允许在任何经纪商、任何品种和任何时间周期上进行历史回测、压力测试与参数优化，**毫无限制**。

---

## 🛠️ 快速入门与安装指南

### 环境要求
- **平台**：MetaTrader 5（建议 Build 3800 或更高版本）
- **操作系统**：Windows 10 / 11 或 Windows Server (VPS)
- **账户类型**：对冲账户 (Hedging)，推荐低点差标准或 ECN 账户

---

### 分步安装指南

1. **下载 `Rango.ex5`**：
   - 从 [Releases](../../releases) 页面或项目根目录下载最新的编译文件 `Rango.ex5`。

2. **复制到 MT5 目录**：
   - 打开 MetaTrader 5 终端。
   - 点击菜单 `文件 (File)` ➔ `打开数据文件夹 (Open Data Folder)`。
   - 进入路径 `MQL5` ➔ `Experts\`。
   - 将 `Rango.ex5` 粘贴到该文件夹中。

3. **启用算法交易与 DLL 导入**：
   - 在 MT5 中点击 `工具 (Tools)` ➔ `选项 (Options)`（快捷键 `Ctrl + O`）。
   - 切换到 **EA 交易 (Expert Advisors)** 选项卡。
   - 勾选 ✅ **允许算法交易 (Allow algorithmic trading)**。
   - 勾选 ✅ **允许 DLL 导入 (Allow DLL imports)** *(用于硬件特征绑定与授权验证)*。
   - 勾选 ✅ **允许 WebRequest** 并添加 `https://api.telegram.org` *(如使用 Telegram 通知)*。

4. **加载 Rango 到图表**：
   - 在 **导航 (Navigator)** 窗口 (`Ctrl + N`) 中展开 **EA 交易**。
   - 将 **Rango** 拖放至目标图表（如 `XAUUSD`, `EURUSD`, `GBPUSD`, `BTCUSD`）。
   - 推荐周期：**M5** 或 **M15**。
   - 在弹出的 EA 属性窗口中勾选 **允许实时交易**，配置风险参数后点击 **确定 (OK)**。

---

## 🔑 激活流程（免费策略回测与实盘设置）

Rango 提供两种运行层级：

### 1. 🧪 100% 免费策略测试器（模拟与回测）
- **无需任何授权密钥。**
- 打开 MT5 策略测试器 (`Ctrl + R`)，选择 `Rango.ex5`，设定品种与日期范围，点击 **开始 (Start)**。
- 完整体验所有策略特性与参数回测！

### 2. 💻 实盘 / 模拟盘图表激活
首次将 Rango 附加到实盘或模拟盘图表时：
1. Rango 以**仅查看演示模式 (View-Only Demo Mode)** 运行（HUD 仪表盘、量化计算与信号正常工作）。
2. EA 会自动生成机器唯一硬件序列号并导出到终端文件目录：
   ```
   MQL5\Files\Rango_Request_Serial.txt
   ```
3. 复制您的机器序列号（格式：`RANGO-REQ-XXXX-XXXX-XXXX-XXXX`）。
4. 将序列号发送至 Telegram 客服：**[@trading_world_support](https://t.me/trading_world_support)** 获取专属**激活密钥** (`RANGO-ACT-...`)。
5. 将密钥填入输入参数 `InpLicenseKey`（验证成功后会自动缓存）。

---

## 📲 移动端推送通知设置（MT5 App 与 MetaQuotes ID）

Rango EA 支持使用 MQL5 原生 `SendNotification` 引擎直接推送警报至 **MetaTrader 5 移动端应用**（iOS 与 Android）。无需配置 WebRequest 或外部机器人，即可实现零延迟极速推送。

### 第一步：在手机端获取 MetaQuotes ID
* **Android 安卓**：打开 MetaTrader 5 应用 ➔ 点击菜单栏 (☰) ➔ 选择 **信息 (Messages)**（或 **设置** ➔ **信息**）➔ 记下 **MetaQuotes ID**（8位字母数字，例如 `1A2B3C4D`）。
* **iOS 苹果**：打开 MetaTrader 5 应用 ➔ 进入 **设置 (Settings)** ➔ 选择 **聊天和信息 (Chat and Messages)** ➔ 在屏幕最下方找到 **我的 MetaQuotes ID**。

### 第二步：在桌面端 MT5 中配置推送
1. 在电脑端 MT5 点击 `工具 (Tools)` ➔ `选项 (Options)`（快捷键 `Ctrl + O`）。
2. 切换至 **通知 (Notifications)** 选项卡。
3. 勾选 ✅ **启用推送通知 (Enable Push Notifications)**。
4. ⚠️ **重要提示（防骚扰配置）**：
   * **取消勾选** ❌ **本地终端通知 (Notifications from the local terminal)**
   * **取消勾选** ❌ **交易服务器通知 (Notifications from the trade server)**
   > 💡 **为什么要取消这两项？** 如果勾选，经纪商服务器会在每次挂单、改单、平仓时都发送通知。取消这两项可确保您的手机**仅接收** Rango 高价值的策略分析信号（趋势与 DCA 就绪警报），彻底远离垃圾通知骚扰！
5. 在 **MetaQuotes ID** 文本框中输入您的手机 ID（多个 ID 用英文逗号隔开）。
6. 点击 **测试 (Test)** 按钮，手机将立即收到测试推送！
7. 点击 **确定 (OK)** 保存配置。

### 第三步：在 Rango EA 中配置通知参数
在 EA 输入参数中设置：
* `EnableNotifications = true` — 开启推送通知。
* `InpNotifyDCASignal = true` — 当量化指标转为 **DCA READY** 时接收实时推送。
* `InpNotifyTrendSignal = true` — 出现确认的 **趋势回调信号 (Trend Pullback)** 时接收推送。
* `InpMetaQuotesID = "..."` — 填入 MetaQuotes ID（可选参数 / 终端链接）。

---

## ⚙️ 输入参数详解 (Input Parameters)

### 常规设置 (General Settings)
| 参数名 | 默认值 | 参数说明 |
| :--- | :--- | :--- |
| `InpLicenseKey` | `""` | 客服提供的激活密钥（缓存后可留空）。 |
| `InputMagicNumber` | `0` | EA 独有魔术码 (`0` = 根据交易品种哈希自动分配)。 |
| `Language` | `LANG_ENGLISH` | 界面仪表盘与警报语言 (`LANG_ENGLISH` 或 `LANG_VIETNAMESE`)。 |
| `InpShowDashboard` | `true` | 在图表上显示实时 HUD 仪表盘。 |
| `SinglePositionMode` | `true` | 限制 EA 一次仅执行一个策略周期。 |
| `ATR_Multiplier` | `1.0` | 用于动态距离计算的全局 ATR 乘数。 |
| `MaxSpreadPoints` | `0` | 允许的最大点差过滤 (`0` = 禁用)。 |

### 资金与风险管理 (Risk Management)
| 参数名 | 默认值 | 参数说明 |
| :--- | :--- | :--- |
| `RiskMode` | `RISK_BY_VOLUME` | 风险计算模式 (`RISK_BY_PERCENT`, `RISK_BY_AMOUNT`, `RISK_BY_VOLUME`)。 |
| `RiskDefaulMinimum` | `0.01` | 基础交易手数或风险百分比。 |
| `RiskRewardRatio` | `1.0` | 单笔交易默认盈亏比 (Risk:Reward)。 |
| `MaxVolumeFixed` | `0` | 单笔订单最大固定手数上限 (`0` = 无限制)。 |

### DCA 准入引擎设置 (DCA Permission Engine Settings)
| 参数名 | 默认值 | 参数说明 |
| :--- | :--- | :--- |
| `InpEnableDCAPermission` | `true` | DCA 准入闸门总开关。 |
| `InpMinDRS_Ready` | `65.0` | 授权 DCA 加仓所需的最低就绪评分 DRS ($0–100$)。 |
| `InpMaxTDS_Allowed` | `65.0` | 阻断 DCA 前允许的最大趋势危险评分 TDS ($0–100$)。 |
| `InpEnableAntiExpansion` | `true` | 波动率突增时冻结加仓 ($ATR_5/ATR_{30} > 1.35$)。 |
| `InpEnableDCAKillSwitch` | `true` | 当 $TDS > 85$ 时触发紧急熔断停止加仓。 |
| `InpShowDCAArrows` | `true` | 在图表上绘制 DCA 准入信号箭头。 |
| `InpDCAArrowCooldownBars` | `3` | 连续 DCA 信号箭头之间的最小 K 线间隔。 |

### 推送通知设置 (Push Notifications & Alerts)
| 参数名 | 默认值 | 参数说明 |
| :--- | :--- | :--- |
| `EnableNotifications` | `true` | 通知总开关。 |
| `InpNotifyDCASignal` | `true` | 当 DCA 状态转为 READY 时发送手机推送。 |
| `InpNotifyTrendSignal` | `false` | 当出现趋势回调信号时发送手机推送。 |
| `InpMetaQuotesID` | `""` | 移动端 MetaQuotes ID。 |

---

## 📈 推荐交易配置

| 资产类别 | 推荐交易品种 | 最佳时间周期 | 最低资金要求 (0.01 手) | 推荐杠杆比例 |
| :--- | :--- | :--- | :--- | :--- |
| **贵金属** | `XAUUSD` (黄金) | `M1` / `M5` | $10,000+ | 1:2000 |
| **主要外汇** | `EURUSD`, `GBPUSD`, `USDJPY` | `M5` / `M15` | $10,000+ | 1:2000 |
| **加密货币** | `BTCUSD`, `ETHUSD` | `M5` / `M15` | $10,000+ | 1:2000 |

> 💡 **专业建议**：为在重磅数据发布（CPI、非农、美联储决议）期间获得最高稳定性，请务必保持 `InpEnableAntiExpansion = true` 与 `EnableNewsAlert = true`。

---

## ❓ 常见问题解答 (FAQ)

<details>
<summary><b>Q1: 我可以在 MT5 策略测试器中免费回测 Rango 吗？</b></summary>
<br>
<b>完全可以，100% 免费！</b> Rango 内置了策略测试器自动绕行机制，无需任何激活密钥即可进行无限制的历史回测、参数优化与可视化模拟。
</details>

<details>
<summary><b>Q2: 为什么 MT5 提示 "DLL imports must be enabled"？</b></summary>
<br>
Rango 使用 Windows 标准系统 API (`kernel32.dll`) 读取本地硬件序列号以进行机器绑定授权。请在 <code>工具 ➔ 选项 ➔ EA 交易</code> 中勾选 <b>"允许 DLL 导入"</b>。
</details>

<details>
<summary><b>Q3: 在哪里可以找到用于实盘激活的机器序列号？</b></summary>
<br>
当您将 Rango 加载到任何图表时，请查看生成的文件：
<code>MetaTrader 5 ➔ MQL5 ➔ Files ➔ Rango_Request_Serial.txt</code>。
此外，序列号也会直接输出在 MT5 的 <b>“EA 交易”日志选项卡</b> 中。
</details>

<details>
<summary><b>Q4: Rango 可以在 VPS 云服务器上运行吗？</b></summary>
<br>
<b>当然可以！</b> Rango 针对超低 CPU 占用进行了深度算法优化，可在任何 Windows VPS 上 7x24 小时全天候稳定运行。
</details>

<details>
<summary><b>Q5: 如何在手机上接收推送通知同时避免被垃圾信息轰炸？</b></summary>
<br>
在手机 MT5 应用中获取您的 <b>MetaQuotes ID</b>，填入电脑端 MT5 的 <code>工具 ➔ 选项 ➔ 通知</code> 中并勾选 <b>"启用推送通知"</b>。
<br><br>
⚠️ <b>防骚扰秘诀：</b> 务必 <b>取消勾选</b> <i>"本地终端通知"</i> 与 <i>"交易服务器通知"</i>，这样您的手机就只会收到 Rango 核心的高价值策略信号，而不会收到经纪商发送的海量琐碎成交通知！
</details>

---

## 💬 技术支持与社区

- ✈️ **Telegram 客服与授权激活**：[@trading_world_support](https://t.me/trading_world_support)
- 📺 **YouTube 官方频道**：订阅观看视频教程、参数优化指南和实盘交易演示。
- 🐛 **问题反馈**：如果您发现 Bug 或有功能建议，欢迎在 GitHub 仓库提交 Issue。

---

## ⚠️ 风险免责声明

外汇保证金 (Forex)、差价合约 (CFD)、贵金属和加密货币交易具有极高的资金风险，可能并不适合所有投资者。高杠杆既能放大收益，也能成倍放大亏损。在决定参与交易或使用自动化交易系统之前，您应当审慎评估自身的投资目标、交易经验水平以及风险承受能力。

历史表现不代表未来收益。本代码库中提供的软件与文档材料仅供学习研究和信息参考之用。您应对实盘账户中的所有交易决策与财务结果承担全部责任。

---

<div align="center">
  <sub>由 World Trading Lab 用心 ❤️ 打造。保留所有权利。</sub>
</div>
