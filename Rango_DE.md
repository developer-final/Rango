# 🎯 RANGO EA — Quantitatives, Regime-Adaptives DCA-Handelssystem für MT5

[![Platform](https://img.shields.io/badge/Platform-MetaTrader%205-blue.svg)](https://www.metatrader5.com/)
[![Language](https://img.shields.io/badge/Language-MQL5-orange.svg)]()
[![Strategy](https://img.shields.io/badge/Strategy-Regime--Adaptive%20DCA%20%7C%20Mean%20Reversion-green.svg)]()
[![Backtest](https://img.shields.io/badge/Strategy%20Tester-100%25%20Free%20Simulation-brightgreen.svg)]()
[![Telegram Support](https://img.shields.io/badge/Telegram-@trading__world__support-blue.svg)](https://t.me/trading_world_support)

> **Rango EA** ist ein hochentwickeltes, quantitatives Handelssystem auf institutionellem Niveau für **MetaTrader 5 (MT5)**. Es ersetzt herkömmliches „blindes“ Dollar-Cost-Averaging (DCA) und gefährliche Martingale-Gitter durch eine intelligente **Regime-Dependent Permission Engine**, **Trend Danger Scoring (TDS)** und **Statistical Exhaustion Scoring (ES)**.

---

## 📌 Inhaltsverzeichnis

- [Überblick](#-überblick)
- [Warum Rango anders ist (Das Anti-Blowup-DCA)](#-warum-rango-anders-ist-das-anti-blowup-dca)
- [Hauptfunktionen](#-hauptfunktionen)
- [Systemarchitektur & Quantitative Modelle](#-systemarchitektur--quantitative-modelle)
- [Schnellstart & Installationsanleitung](#-schnellstart--installationsanleitung)
- [Aktivierung (Kostenloser Strategy Tester & Live-Setup)](#-aktivierung-kostenloser-strategy-tester--live-setup)
- [Mobile Push-Benachrichtigungen einrichten (MT5 App & MetaQuotes ID)](#-mobile-push-benachrichtigungen-einrichten-mt5-app--metaquotes-id)
- [Eingabeparameter-Referenz (Input Parameters)](#-eingabeparameter-referenz-input-parameters)
- [Empfohlenes Trading-Setup](#-empfohlenes-trading-setup)
- [Häufig gestellte Fragen (FAQ)](#-häufig-gestellte-fragen-faq)
- [Support & Community](#-support--community)
- [Risikohinweis](#-risikohinweis)

---

## 💡 Überblick

Traditionelle Grid- und Martingale-Systeme scheitern unvermeidlich bei lang anhaltenden, starken Einweg-Trendausbrüchen. Sie eröffnen blind immer mehr Positionen gegen das Momentum, bis das Konto einen Margin Call erleidet.

**Rango EA wurde von Grund auf entwickelt, um genau dieses Problem zu lösen.**

Anstatt blind Aufträge in festen Pip-Abständen zu platzieren, nutzt Rango ein **Multi-Faktor Quantitatives Zulassungsgate (Permission Gate)**:
1. Es überwacht kontinuierlich die Persistenz des Marktregimes mittels **Hurst-Exponent**, **ADX** und **ATR-Expansionsverhältnissen**.
2. Wenn sich der Markt in einem starken Trend gegen Ihre Position befindet, wird die **DCA-Auftragserweiterung strikt blockiert**.
3. Signale und Einstiege werden erst dann genehmigt, wenn quantitative Kennzahlen bestätigen, dass das gegnerische Trendmomentum erschöpft ist und die **Wahrscheinlichkeit einer Mean-Reversion (Mittelwertrückkehr) mathematisch erhöht ist**.

```
                        ┌─────────────────────────────────┐
                        │    Marktpreis & Volatilität     │
                        └────────────────┬────────────────┘
                                         │
                 ┌───────────────────────┴───────────────────────┐
                 ▼                                               ▼
   ┌───────────────────────────┐                   ┌───────────────────────────┐
   │ Trend Danger Score (TDS)  │                   │   Exhaustion Score (ES)   │
   │  - ADX Trendstärke        │                   │  - Abstand zum 200 EMA    │
   │  - Hurst-Memory (H > 0.55)│                   │  - RSI-Extreme / Bänder   │
   │  - Volatilitätsbeschl.    │                   │  - Momentum-Erschöpfung   │
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
│Erweiterung aus│                │ Signal warten │                │ Signal ausf.  │
└───────────────┘                └───────────────┘                └───────────────┘
```

---

## ⚡ Warum Rango anders ist (Das Anti-Blowup-DCA)

| Funktion | Konventionelles DCA / Martingale | **Rango EA** |
| :--- | :--- | :--- |
| **Averaging-Trigger** | Feste Pip-Abstände oder Timer | **Quantitativer DCA Readiness Score (DRS)** |
| **Trend-Schutz** | Keiner (kauft/verkauft blind in den Crash) | **Anti-Expansions-Gate & Trend Danger Block** |
| **Timing-Präzision** | Blind / Feste Pips | **Quantitative Erschöpfung & Bestätigung der Umkehr** |
| **Richtungstrennung** | Öffnet Buy/Sell blind gleichzeitig | **Direktionale Isolation (Kein Gegentrend-Rauschen)** |
| **Risikogrenze** | Unbegrenztes Totalverlustrisiko | **Striktes Risikomanagement & dynamischer ATR-Schutz** |
| **Visuelle Telemetrie**| Kaum oder gar nicht vorhanden | **Echtzeit-HUD-Dashboard & smarte Signalpfeile** |
| **Strategy Tester** | Oft eingeschränkt oder blockiert | **100% kostenloser, uneingeschränkter Backtest** |

---

## 🚀 Hauptfunktionen

### 1. 🛡️ DCA-Zulassung & Marktregime-Engine
- **Trend Danger Score (TDS: 0–100)**: Bewertet Trendstärke, Hurst-Persistenz und Volatilitätsbeschleunigung. Wenn $TDS > 65$, werden neue DCA-Einstiege sofort eingefroren.
- **Exhaustion Score (ES: 0–100)**: Quantifiziert statistische Überdehnung vom 200 EMA, RSI-Überkauft/Überverkauft-Extreme, Kerzenkörper-Verlangsamung und Ablehnungs-Pinbars.
- **Anti-Expansion Gate**: Blockiert das Averaging sofort, wenn die kurzfristige ATR die langfristige ATR stark übersteigt ($ATR_5 / ATR_{30} > 1.35$) – beispielsweise bei News-Spikes oder Ausbrüchen.
- **Notfall-DCA-Kill-Switch**: Stoppt das Nachkaufen automatisch, wenn adverse Marktbedingungen kritische Sicherheitsgrenzen überschreiten ($TDS > 85$).

### 2. 🎯 Direktionale DCA-Isolation
- Bei bestätigtem **Aufwärtstrend (Uptrend)**: Buy-DCA wird blockiert und nur Sell-DCA Mean-Reversion an Erschöpfungspunkten bewertet.
- Bei bestätigtem **Abwärtstrend (Downtrend)**: Sell-DCA wird blockiert und nur Buy-DCA Mean-Reversion am Boden bewertet.
- Beidseitige Signale sind nur in **Seitwärts- / Range-Märkten** erlaubt.

### 3. 🛡️ Professionelles Risikomanagement & flexible Positionsgrößen
- **Mehrere Positionsgrößen-Modi**: Feste Lotgröße (Volume), fester Geldbetrag ($) oder prozentuales Kontorisiko (%).
- **Automatische SL/TP-Projektion**: Dynamische Abstandsberechnungen basierend auf der ATR höherer Zeiteinheiten und anpassbaren Risk:Reward-Verhältnissen.
- **Ausführungsschutz**: Spread-Filter, Validierung des Mindest-Stopp-Abstands des Brokers und Margin-Prüfung vor jeder Ausführung.

### 4. 📊 Professionelles On-Chart HUD Dashboard
- Übersichtliches High-Contrast-Display:
  - Aktuelles Marktregime und Volatilitätsstatus.
  - Live-Werte für **TDS** (Trend-Gefahr) und **DRS** (DCA-Bereitschaft).
  - Win/Loss-Statistiken, Trend-Pullback-Status und Live-Gewinnmetriken.

### 5. 🏹 Smarte Signalpfeile & Push-Benachrichtigungen
- Pfeile erscheinen nur bei **Zustandsübergängen** (mit Cooldown und automatischem Entfernen abgelaufener Pfeile).
- **Direkte MT5-Mobile-Push-Benachrichtigungen**: Sendet Handelssignale und DCA-READY-Warnungen via MetaQuotes ID in Echtzeit auf Ihr Smartphone.

### 6. 🧪 Uneingeschränkter MT5 Strategy Tester
- Backtests, Stresstests und Optimierungen auf historischen Daten bei jedem Broker und Zeiteinheit **ohne Einschränkungen**.

---

## 🛠️ Schnellstart & Installationsanleitung

### Voraussetzungen
- **Plattform**: MetaTrader 5 (Build 3800 oder neuer empfohlen)
- **Betriebssystem**: Windows 10 / 11 oder Windows Server (VPS)
- **Kontotyp**: Hedging-Konto empfohlen (Standard oder ECN mit engem Spread)

---

### Schritt-für-Schritt-Installation

1. **`Rango.ex5` herunterladen**:
   - Laden Sie die kompilierte Datei `Rango.ex5` aus dem Bereich [Releases](../../releases) oder dem Hauptverzeichnis herunter.

2. **In das MT5-Verzeichnis kopieren**:
   - Öffnen Sie Ihr MetaTrader 5 Terminal.
   - Klicken Sie auf `Datei (File)` ➔ `Dateiordner öffnen (Open Data Folder)`.
   - Navigieren Sie zu `MQL5` ➔ `Experts\`.
   - Fügen Sie `Rango.ex5` in diesen Ordner ein.

3. **Automatischen Handel & DLL-Importe aktivieren**:
   - Gehen Sie in MT5 auf `Extras (Tools)` ➔ `Optionen (Options)` (oder `Strg + O`).
   - Wählen Sie den Reiter **Experten (Expert Advisors)**.
   - Aktivieren Sie ✅ **Automatischen Handel erlauben (Allow algorithmic trading)**.
   - Aktivieren Sie ✅ **DLL-Importe erlauben (Allow DLL imports)** *(Erforderlich für Hardware-Lizenzierung)*.
   - Aktivieren Sie ✅ **WebRequest für folgende URLs erlauben** und fügen Sie `https://api.telegram.org` hinzu *(falls Telegram-Benachrichtigungen genutzt werden)*.

4. **Rango auf den Chart ziehen**:
   - Im Fenster **Navigator** (`Strg + N`) den Bereich **Experten** aufklappen.
   - Ziehen Sie **Rango** auf das gewünschte Chart (z. B. `XAUUSD`, `EURUSD`, `GBPUSD`, `BTCUSD`).
   - Empfohlene Zeiteinheit: **M5** oder **M15**.
   - Überprüfen Sie, ob **Live-Trading erlauben** aktiv ist, konfigurieren Sie Ihr Risiko und klicken Sie auf **OK**.

---

## 🔑 Aktivierung (Kostenloser Strategy Tester & Live-Setup)

Rango bietet zwei Betriebsmodi:

### 1. 🧪 100% Kostenloser Strategy Tester (Backtests & Simulationen)
- **Kein Lizenzschlüssel erforderlich.**
- Öffnen Sie den MT5 Strategy Tester (`Strg + R`), wählen Sie `Rango.ex5`, Symbol und Zeitraum und klicken Sie auf **Start**.
- Vollständiger Funktionszugriff ohne Limitierungen!

### 2. 💻 Live- / Demo-Chart-Aktivierung
Wenn Sie Rango zum ersten Mal an einen Live- oder Demo-Chart anhängen:
1. Rango startet im **Nur-Ansicht-Demomodus (View-Only Demo Mode)** (Dashboard und Signale sind aktiv).
2. Es generiert automatisch Ihre eindeutige Hardware-Seriennummer in der Datei:
   ```
   MQL5\Files\Rango_Request_Serial.txt
   ```
3. Kopieren Sie die Seriennummer (Format: `RANGO-REQ-XXXX-XXXX-XXXX-XXXX`).
4. Senden Sie diese an den Telegram-Support: **[@trading_world_support](https://t.me/trading_world_support)**, um Ihren **Aktivierungsschlüssel** zu erhalten (`RANGO-ACT-...`).
5. Fügen Sie den Schlüssel in den Parameter `InpLicenseKey` ein (wird nach erfolgreicher Validierung gespeichert).

---

## 📲 Mobile Push-Benachrichtigungen einrichten (MT5 App & MetaQuotes ID)

Rango EA unterstützt direkte Push-Benachrichtigungen an die **MetaTrader 5 Mobile App** (iOS & Android) über die native MQL5-Funktion `SendNotification` – blitzschnell und ohne komplexe WebRequest-Setups.

### Schritt 1: MetaQuotes ID auf dem Smartphone finden
* **Android**: MT5 App öffnen ➔ Menü (☰) ➔ **Nachrichten** (oder **Einstellungen** ➔ **Nachrichten**) ➔ 8-stellige **MetaQuotes ID** notieren (z. B. `1A2B3C4D`).
* **iOS (iPhone/iPad)**: MT5 App öffnen ➔ **Einstellungen** ➔ **Chat und Nachrichten** ➔ **Meine MetaQuotes ID** unten ablesen.

### Schritt 2: Push-Benachrichtigungen im Desktop-MT5 einrichten
1. Im Desktop-MT5 auf `Extras` ➔ `Optionen` (`Strg + O`) gehen.
2. Den Reiter **Benachrichtigungen (Notifications)** wählen.
3. ✅ **Push-Benachrichtigungen aktivieren** ankreuzen.
4. ⚠️ **WICHTIG (Anti-Spam-Einstellung)**:
   * **DEAKTIVIEREN** ❌ **Benachrichtigungen vom lokalen Terminal**
   * **DEAKTIVIEREN** ❌ **Benachrichtigungen vom Handelsserver**
   > 💡 **Warum deaktivieren?** Wenn diese aktiv sind, sendet der Broker für jede kleinste Orderänderung eine Push-Nachricht. Wenn Sie beide deaktivieren, erhalten Sie **nur** Rangos hochwertige Strategiesignale ohne Spam!
5. Tragen Sie Ihre **MetaQuotes ID** ein (mehrere IDs durch Kommas trennen).
6. Klicken Sie auf **Test**. Sie erhalten sofort eine Testnachricht auf Ihr Smartphone!
7. Klicken Sie auf **OK**.

### Schritt 3: Benachrichtigungsparameter im Rango EA anpassen
In den EA-Einstellungen:
* `EnableNotifications = true` — Push-Benachrichtigungen aktivieren.
* `InpNotifyDCASignal = true` — Benachrichtigung erhalten, wenn Status auf **DCA READY** wechselt.
* `InpNotifyTrendSignal = true` — Benachrichtigung bei bestätigtem Trend-Pullback-Signal.
* `InpMetaQuotesID = "..."` — Ihre MetaQuotes ID eintragen.

---

## ⚙️ Eingabeparameter-Referenz (Input Parameters)

### Allgemeine Einstellungen (General Settings)
| Parameter | Standard | Beschreibung |
| :--- | :--- | :--- |
| `InpLicenseKey` | `""` | Aktivierungsschlüssel vom Support (nach Caching leer lassen). |
| `InputMagicNumber` | `0` | Eindeutige Magic Number (`0` = Automatisch per Symbol-Hash). |
| `Language` | `LANG_ENGLISH` | Sprache für Dashboard & Benachrichtigungen (`LANG_ENGLISH` oder `LANG_VIETNAMESE`). |
| `InpShowDashboard` | `true` | Echtzeit-HUD-Dashboard auf dem Chart anzeigen. |
| `SinglePositionMode` | `true` | Beschränkt den EA auf einen Strategiezyklus gleichzeitig. |
| `ATR_Multiplier` | `1.0` | Globaler ATR-Multiplikator für dynamische Distanzen. |
| `MaxSpreadPoints` | `0` | Maximal erlaubter Spread in Punkten (`0` = Deaktiviert). |

### Risikomanagement (Risk Management)
| Parameter | Standard | Beschreibung |
| :--- | :--- | :--- |
| `RiskMode` | `RISK_BY_VOLUME` | Risikoberechnungsmodus (`RISK_BY_PERCENT`, `RISK_BY_AMOUNT`, `RISK_BY_VOLUME`). |
| `RiskDefaulMinimum` | `0.01` | Basis-Lotgröße oder Risikoprozentsatz. |
| `RiskRewardRatio` | `1.0` | Standard-CRV (Risk:Reward) für Einzeleinstiege. |
| `MaxVolumeFixed` | `0` | Maximale Lotgröße pro Einzelorder (`0` = Unbegrenzt). |

### DCA-Zulassungs-Engine (DCA Permission Engine Settings)
| Parameter | Standard | Beschreibung |
| :--- | :--- | :--- |
| `InpEnableDCAPermission` | `true` | Hauptschalter für das DCA-Zulassungsgate. |
| `InpMinDRS_Ready` | `65.0` | Mindest-DRS-Score für DCA-Freigabe ($0–100$). |
| `InpMaxTDS_Allowed` | `65.0` | Maximaler TDS-Gefahrenscore vor DCA-Blockade ($0–100$). |
| `InpEnableAntiExpansion` | `true` | DCA bei plötzlicher Volatilitätsexpansion einfrieren ($ATR_5/ATR_{30} > 1.35$). |
| `InpEnableDCAKillSwitch` | `true` | Notstopp für neue Aufträge bei $TDS > 85$. |
| `InpShowDCAArrows` | `true` | DCA-Genehmigungspfeile auf dem Chart einblenden. |
| `InpDCAArrowCooldownBars` | `3` | Mindestabstand in Kerzen zwischen DCA-Pfeilen. |

### Push-Benachrichtigungen & Alarme
| Parameter | Standard | Beschreibung |
| :--- | :--- | :--- |
| `EnableNotifications` | `true` | Hauptschalter für Benachrichtigungen. |
| `InpNotifyDCASignal` | `true` | Push-Alarm bei Wechsel in den Status READY. |
| `InpNotifyTrendSignal` | `false` | Push-Alarm bei Trend-Pullback-Signalen. |
| `InpMetaQuotesID` | `""` | MetaQuotes ID für direkte Übertragung an die MT5 App. |

---

## 📈 Empfohlenes Trading-Setup

| Asset-Klasse | Empfohlene Symbole | Optimale Zeiteinheit | Mindestkapital (0.01 Lot) | Empfohlener Hebel |
| :--- | :--- | :--- | :--- | :--- |
| **Edelmetalle** | `XAUUSD` (Gold) | `M1` / `M5` | $10,000+ | 1:2000 |
| **Hauptwährungspaare**| `EURUSD`, `GBPUSD`, `USDJPY`| `M5` / `M15` | $10,000+ | 1:2000 |
| **Krypto** | `BTCUSD`, `ETHUSD` | `M5` / `M15` | $10,000+ | 1:2000 |

> 💡 **Profi-Tipp**: Für maximale Stabilität bei wichtigen Wirtschaftsnachrichten (CPI, NFP, Zinsentscheide) sollten `InpEnableAntiExpansion = true` und `EnableNewsAlert = true` stets aktiv sein.

---

## ❓ Häufig gestellte Fragen (FAQ)

<details>
<summary><b>Q1: Kann ich Rango kostenlos im MT5 Strategy Tester backtesten?</b></summary>
<br>
<b>Ja, zu 100% kostenlos!</b> Rango verfügt über einen integrierten Tester-Bypass, der unbegrenzte Backtests, Parameteroptimierungen und visuelle Simulationen ohne Lizenzschlüssel ermöglicht.
</details>

<details>
<summary><b>Q2: Warum verlangt MT5 "DLL-Importe müssen aktiviert sein"?</b></summary>
<br>
Rango greift auf Standard-Windows-APIs (`kernel32.dll`) zu, um die Hardware-ID für die Lizenzierung auszulesen. Bitte aktivieren Sie <b>"DLL-Importe erlauben"</b> unter <code>Extras ➔ Optionen ➔ Experten</code>.
</details>

<details>
<summary><b>Q3: Wo finde ich meine Seriennummer für die Live-Aktivierung?</b></summary>
<br>
Nach dem Anhängen von Rango an einen Chart finden Sie die Datei unter:
<code>MetaTrader 5 ➔ MQL5 ➔ Files ➔ Rango_Request_Serial.txt</code>.
Zudem wird die Seriennummer im MT5-Reiter <b>Experten (Log)</b> ausgegeben.
</details>

<details>
<summary><b>Q4: Läuft Rango stabil auf einem VPS?</b></summary>
<br>
<b>Ja!</b> Rango ist extrem ressourcenschonend programmiert und läuft rund um die Uhr stabil auf jedem Windows-VPS.
</details>

<details>
<summary><b>Q5: Wie erhalte ich Signale auf mein Handy ohne Broker-Spam?</b></summary>
<br>
Tragen Sie Ihre <b>MetaQuotes ID</b> in der Desktop-Version unter <code>Extras ➔ Optionen ➔ Benachrichtigungen</code> ein und aktivieren Sie <b>"Push-Benachrichtigungen aktivieren"</b>.
<br><br>
⚠️ <b>Anti-Spam-Tipp:</b> <b>DEAKTIVIEREN</b> Sie unbedingt <i>"Benachrichtigungen vom lokalen Terminal"</i> und <i>"Benachrichtigungen vom Handelsserver"</i>, um nur die hochpräzisen Rango-Signale zu erhalten!
</details>

---

## 💬 Support & Community

- ✈️ **Telegram-Support & Lizenzierung**: [@trading_world_support](https://t.me/trading_world_support)
- 📺 **YouTube-Kanal**: Video-Tutorials, Optimierungsleitfäden und Live-Trading-Sessions.
- 🐛 **Issue Tracker**: Fehlerberichte und Feature-Wünsche können direkt im GitHub-Repository eingereicht werden.

---

## ⚠️ Risikohinweis

Der Handel mit Devisen (Forex), Differenzkontrakten (CFDs), Edelmetallen und Kryptowährungen birgt ein hohes Verlustrisiko und ist möglicherweise nicht für alle Anleger geeignet. Ein hoher Hebel kann sowohl zu Ihren Gunsten als auch zu Ihren Ungunsten wirken. Prüfen Sie vor dem Einsatz automatisierter Handelssysteme sorgfältig Ihre Anlageziele, Erfahrung und Risikobereitschaft.

Vergangene Wertentwicklungen sind keine Garantie für zukünftige Ergebnisse. Die in diesem Repository bereitgestellte Software dient ausschließlich Bildungs- und Informationszwecken. Sie tragen die volle Verantwortung für alle Handelsentscheidungen auf Ihren Konten.

---

<div align="center">
  <sub>Mit ❤️ entwickelt von World Trading Lab. Alle Rechte vorbehalten.</sub>
</div>
