# 🎯 RANGO EA — Sistema di Trading Quantitativo DCA Adattivo al Regime di Mercato per MT5

[![Platform](https://img.shields.io/badge/Piattaforma-MetaTrader%205-blue.svg)](https://www.metatrader5.com/)
[![Language](https://img.shields.io/badge/Linguaggio-MQL5-orange.svg)]()
[![Strategy](https://img.shields.io/badge/Strategia-Regime--Adaptive%20DCA%20%7C%20Mean%20Reversion-green.svg)]()
[![Backtest](https://img.shields.io/badge/Strategy%20Tester-100%25%20Simulazione%20Gratuita-brightgreen.svg)]()
[![Telegram Support](https://img.shields.io/badge/Supporto%20Telegram-@trading__world__support-blue.svg)](https://t.me/trading_world_support)

> **Rango EA** è un sistema di trading quantitativo avanzato di livello istituzionale per **MetaTrader 5 (MT5)**. Sostituisce il tradizionale Dollar-Cost Averaging (DCA) "cieco" e le pericolose griglie Martingala con un motore di autorizzazione intelligente **Regime-Dependent Permission Engine**, un punteggio di pericolo di trend **Trend Danger Scoring (TDS)** e un punteggio di esaurimento statistico **Statistical Exhaustion Scoring (ES)**.

---

## 📌 Indice

- [Panoramica](#-panoramica)
- [Perché Rango è Differente (Il DCA Anti-Bruciatura del Conto)](#-perché-rango-è-differente-il-dca-anti-bruciatura-del-conto)
- [Caratteristiche Principali](#-caratteristiche-principali)
- [Architettura del Sistema e Modelli Quantitativi](#-architettura-del-sistema-e-modelli-quantitativi)
- [Guida Rapida e Installazione](#-guida-rapida-e-installazione)
- [Come Attivare (Backtesting Gratuito e Configurazione Reale)](#-come-attivare-backtesting-gratuito-e-configurazione-reale)
- [Configurazione Notifiche Push Mobile (App MT5 e MetaQuotes ID)](#-configurazione-notifiche-push-mobile-app-mt5-e-metaquotes-id)
- [Riferimento Parametri di Input (Input Parameters)](#-riferimento-parametri-di-input-input-parameters)
- [Configurazione di Trading Consigliata](#-configurazione-di-trading-consigliata)
- [Domande Frequenti (FAQ)](#-domande-frequenti-faq)
- [Supporto e Community](#-supporto-e-community)
- [Dichiarazione di Non Responsabilità sui Rischi](#-dichiarazione-di-non-responsabilità-sui-rischi)

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

## 💡 Panoramica

I sistemi a griglia e Martingala tradizionali falliscono inevitabilmente durante prolungate e forti esplosioni di trend unidirezionali. Aprono ordini alla cieca contro un forte momentum avverso fino al raggiungimento del Margin Call.

**Rango EA è stato sviluppato da zero per risolvere definitivamente questo problema.**

Invece di aprire ordini a intervalli di pip fissi, Rango integra un **Gate di Autorizzazione Quantitativa Multifattoriale (Multi-Factor Quantitative Permission Gate)**:
1. Monitora costantemente la persistenza del regime di mercato tramite l'**Esponente di Hurst**, l'**ADX** e i **Rapporti di Espansione ATR**.
2. Se il mercato si muove in forte trend contro la posizione, l'**espansione degli ordini DCA viene rigorosamente bloccata**.
3. Segnali ed ingressi sono autorizzati solo quando le metriche quantitative confermano che il momentum avverso si è esaurito e la **probabilità di ritorno verso la media (Mean Reversion) è matematicamente elevata**.

```
                        ┌─────────────────────────────────┐
                        │   Prezzo di Mercato & Volatilità│
                        └────────────────┬────────────────┘
                                         │
                 ┌───────────────────────┴───────────────────────┐
                 ▼                                               ▼
   ┌───────────────────────────┐                   ┌───────────────────────────┐
   │ Punteggio Pericolo (TDS)  │                   │ Punteggio Esaurimento(ES) │
   │  - Forza del Trend ADX    │                   │  - Distanza dalla 200 EMA │
   │  - Memoria Hurst (H>0.55) │                   │  - Estremi RSI e Bande    │
   │  - Accelerazione Volat.   │                   │  - Pinbar di Rifiuto      │
   └─────────────┬─────────────┘                   └─────────────┬─────────────┘
                 │                                               │
                 └───────────────────────┬───────────────────────┘
                                         ▼
                        ┌─────────────────────────────────┐
                        │Punteggio Prontezza DCA (DRS)    │
                        └────────────────┬────────────────┘
                                         │
        ┌────────────────────────────────┼────────────────────────────────┐
        ▼                                ▼                                ▼
┌───────────────┐                ┌───────────────┐                ┌───────────────┐
│  DCA BLOCCATO │                │  DCA IN ATTESA│                │   DCA PRONTO  │
│Blocco Ordini  │                │ In Attesa Seg.│                │Esegui Segnale │
└───────────────┘                └───────────────┘                └───────────────┘
```

---

## ⚡ Perché Rango è Differente (Il DCA Anti-Bruciatura del Conto)

| Caratteristica | DCA Tradizionale / Martingala | **Rango EA** |
| :--- | :--- | :--- |
| **Innesco Mediata** | Pips fissi o timer temporale | **Punteggio Quantitativo di Prontezza DCA (DRS)** |
| **Protezione dal Trend** | Nessuna (continua a mediare nel crollo) | **Gate Anti-Espansione & Blocco Pericolo Trend** |
| **Precisione del Timing** | Alla cieca / Pip fissi | **Conferma Esaurimento Quantitativo e Reversione** |
| **Isolamento Direzionale**| Apertura Buy/Sell contemporanea | **Isolamento Direzionale (Nessun DCA contro-trend)** |
| **Limite di Rischio** | Rischio illimitato di azzeramento | **Gestione Rigorosa del Rischio & Stop ATR Dinamico** |
| **Telemetria Visiva** | Assente o minimale | **Dashboard HUD in Tempo Reale & Frecce Intelligenti** |
| **Tester di Strategia**| Spesso limitato o bloccato | **Backtest 100% Gratuito e Illimitato** |

---

## 🚀 Caratteristiche Principali

### 1. 🛡️ Motore di Autorizzazione DCA e Regime di Mercato
- **Trend Danger Score (TDS: 0–100)**: Valuta forza del trend, persistenza di Hurst e accelerazione della volatilità. Se $TDS > 65$, i nuovi ingressi DCA vengono bloccati.
- **Exhaustion Score (ES: 0–100)**: Quantifica la distanza statistica rispetto alla 200 EMA, estremi ipercomprato/ipervenduto RSI e pinbar di rifiuto del prezzo.
- **Gate Anti-Espansione**: Blocca istantaneamente il DCA se l'ATR a breve termine supera notevolmente l'ATR a lungo termine ($ATR_5 / ATR_{30} > 1.35$) durante news o breakout improvvisi.
- **Kill Switch di Emergenza DCA**: Arresto automatico d'emergenza se le condizioni avverse superano le soglie critiche di sicurezza ($TDS > 85$).

### 2. 🎯 Isolamento Direzionale DCA
- In **Trend Rialzista (Uptrend)** confermato: Il DCA Buy viene bloccato e si valuta solo il ritorno alla media Sell sui massimi di esaurimento.
- In **Trend Ribassista (Downtrend)** confermato: Il DCA Sell viene bloccato e si valuta solo il ritorno alla media Buy sui minimi di esaurimento.
- I segnali bidirezionali sono ammessi esclusivamente in mercati in **Fase Laterale / Range**.

### 3. 🛡️ Gestione Professionale del Rischio e Size Flessibile
- **Molteplici Modalità di Calcolo**: Volume fisso (Lotti), importo monetario fisso a rischio ($), o percentuale del capitale (%).
- **Calcolo Dinamico SL/TP**: Distanze dinamiche basate sull'ATR del timeframe superiore e rapporto Rischio:Rendimento personalizzabile.
- **Garanzie di Esecuzione**: Filtro spread, convalida distanza minima di stop del broker e controllo margine disponibile pre-trade.

### 4. 📊 Dashboard HUD Professionale su Grafico
- Interfaccia ad alto contrasto che mostra:
  - Stato attuale del Regime di Mercato e Volatilità.
  - Valori in tempo reale di **TDS** (Pericolo Trend) e **DRS** (Prontezza DCA).
  - Statistiche di vincita, stato dei pullback e profitto live.

### 5. 🏹 Frecce di Segnale Intelligenti & Notifiche Push
- Frecce generate unicamente nelle **transizioni di stato**, con cooldown e pulizia automatica dei vecchi segnali.
- **Notifiche Push Dirette su MT5 Mobile**: Ricevi segnali di trading e avvisi DCA READY direttamente sul tuo smartphone tramite MetaQuotes ID.

### 6. 🧪 Strategy Tester MT5 Senza Limitazioni
- Effettua backtest, stress test e ottimizzazioni su dati storici con qualsiasi broker e timeframe **senza alcun vincolo**.

---

## 🛠️ Guida Rapida e Installazione

### Prerequisiti
- **Piattaforma**: MetaTrader 5 (Build 3800 o successiva consigliata)
- **Sistema Operativo**: Windows 10 / 11 o Windows Server (VPS)
- **Tipo di Conto**: Conto Hedging raccomandato (Standard o ECN con spread ridotto)

---

### Installazione Passo dopo Passo

1. **Scaricare `Rango.ex5`**:
   - Scarica la versione compilata `Rango.ex5` dalla sezione [Releases](../../releases) o dalla cartella principale del repository.

2. **Copiare nella Directory MT5**:
   - Apri il terminale MetaTrader 5.
   - Clicca su `File` ➔ `Apri scheda dati (Open Data Folder)`.
   - Vai nel percorso `MQL5` ➔ `Experts\`.
   - Incolla il file `Rango.ex5` in questa cartella.

3. **Abilitare Trading Algoritmico e Import DLL**:
   - In MT5, vai su `Strumenti (Tools)` ➔ `Opzioni (Options)` (oppure `Ctrl + O`).
   - Seleziona la scheda **Consiglieri Esperti (Expert Advisors)**.
   - Spunta ✅ **Permetti trading algoritmico (Allow algorithmic trading)**.
   - Spunta ✅ **Permetti importazione DLL (Allow DLL imports)** *(Necessario per la licenza hardware)*.
   - Spunta ✅ **Permetti WebRequest per gli URL elencati** e aggiungi `https://api.telegram.org` *(se si usa Telegram)*.

4. **Applicare Rango al Grafico**:
   - Nella finestra **Navigatore** (`Ctrl + N`), espandi **Consiglieri Esperti**.
   - Trascina **Rango** sul grafico prescelto (es. `XAUUSD`, `EURUSD`, `GBPUSD`, `BTCUSD`).
   - Timeframe consigliato: **M5** o **M15**.
   - Assicurati che **Consenti trading reale** sia attivo, configura i parametri e premi **OK**.

---

## 🔑 Come Attivare (Backtesting Gratuito e Configurazione Reale)

Rango offre due modalità d'uso:

### 1. 🧪 Tester di Strategia 100% Gratuito (Simulazioni)
- **Nessuna chiave di licenza richiesta.**
- Apri lo Strategy Tester di MT5 (`Ctrl + R`), seleziona `Rango.ex5`, imposta il simbolo e l'intervallo temporale e clicca su **Avvio**.
- Esegui backtest completi con tutte le funzionalità attive!

### 2. 💻 Attivazione su Grafico Reale / Demo
Al primo inserimento di Rango su un grafico online:
1. Rango si avvia in **Modalità Demo Sola Lettura (View-Only Demo Mode)** (il pannello e i calcoli funzionano regolarmente).
2. Genera in automatico il codice seriale univoco del PC nel file:
   ```
   MQL5\Files\Rango_Request_Serial.txt
   ```
3. Copia il codice seriale (formato: `RANGO-REQ-XXXX-XXXX-XXXX-XXXX`).
4. Invia il seriale al supporto Telegram: **[@trading_world_support](https://t.me/trading_world_support)** per ricevere la tua **Chiave di Attivazione** (`RANGO-ACT-...`).
5. Inserisci la chiave nel parametro `InpLicenseKey` (verrà memorizzata in cache).

---

## 📲 Configurazione Notifiche Push Mobile (App MT5 e MetaQuotes ID)

Rango EA supporta notifiche push immediate sull'app **MetaTrader 5 Mobile** (iOS e Android) tramite la funzione nativa MQL5 `SendNotification`, senza necessità di bot esterni o configurazioni complesse.

### Passo 1: Trovare il proprio MetaQuotes ID su Mobile
* **Android**: Apri l'app MT5 ➔ Menu (☰) ➔ **Messaggi** (o **Impostazioni** ➔ **Messaggi**) ➔ Copia il tuo **MetaQuotes ID** (codice alfanumerico di 8 caratteri, es. `1A2B3C4D`).
* **iOS (iPhone/iPad)**: Apri l'app MT5 ➔ **Impostazioni** ➔ **Chat e Messaggi** ➔ Leggi il **Mio MetaQuotes ID** in basso.

### Passo 2: Configurare le Notifiche su MT5 Desktop
1. Su MT5 Desktop, vai in `Strumenti` ➔ `Opzioni` (`Ctrl + O`).
2. Seleziona la scheda **Notifiche (Notifications)**.
3. Spunta ✅ **Abilita notifiche Push**.
4. ⚠️ **IMPORTANTE (Impostazione Anti-Spam)**:
   * **DESELEZIONA** ❌ **Notifiche dal terminale locale**
   * **DESELEZIONA** ❌ **Notifiche dal server di trading**
   > 💡 **Perché deselezionarli?** Se attivi, il broker invierà notifiche per ogni minima modifica o ordine pendente. Deselezionandoli, riceverai **solo** i segnali di trading ad alta priorità di Rango, senza spam!
5. Inserisci il tuo **MetaQuotes ID** nel campo apposito (separa più ID con virgole).
6. Clicca su **Test**. Riceverai un messaggio di notifica istantaneo sul telefono!
7. Clicca su **OK** per confermare.

### Passo 3: Configurare i Parametri di Notifica in Rango EA
Nei parametri di input dell'EA:
* `EnableNotifications = true` — Abilita le notifiche push.
* `InpNotifyDCASignal = true` — Ricevi notifiche quando lo stato diventa **DCA READY**.
* `InpNotifyTrendSignal = true` — Ricevi notifiche su segnale di pullback di trend confermato.
* `InpMetaQuotesID = "..."` — Inserisci il tuo MetaQuotes ID.

---

## ⚙️ Riferimento Parametri di Input (Input Parameters)

### Impostazioni Generali (General Settings)
| Parametro | Predefinito | Descrizione |
| :--- | :--- | :--- |
| `InpLicenseKey` | `""` | Chiave di licenza fornita dal supporto (lasciare vuoto dopo la cache). |
| `InputMagicNumber` | `0` | Identificativo Magic Number (`0` = Assegnazione automatica per simbolo). |
| `Language` | `LANG_ENGLISH` | Lingua del pannello e notifiche (`LANG_ENGLISH` o `LANG_VIETNAMESE`). |
| `InpShowDashboard` | `true` | Mostra il pannello HUD sul grafico. |
| `SinglePositionMode` | `true` | Limita l'EA a un solo ciclo strategico attivo alla volta. |
| `ATR_Multiplier` | `1.0` | Moltiplicatore ATR globale per il calcolo delle distanze dinamiche. |
| `MaxSpreadPoints` | `0` | Filtro spread massimo in punti (`0` = Disabilitato). |

### Gestione del Rischio (Risk Management)
| Parametro | Predefinito | Descrizione |
| :--- | :--- | :--- |
| `RiskMode` | `RISK_BY_VOLUME` | Modalità calcolo size (`RISK_BY_PERCENT`, `RISK_BY_AMOUNT`, `RISK_BY_VOLUME`). |
| `RiskDefaulMinimum` | `0.01` | Lotto base o percentuale di rischio. |
| `RiskRewardRatio` | `1.0` | Rapporto Rischio:Rendimento predefinito per ordini singoli. |
| `MaxVolumeFixed` | `0` | Dimensione massima fissa per singolo ordine (`0` = Illimitato). |

### Motore di Permesso DCA (DCA Permission Engine Settings)
| Parametro | Predefinito | Descrizione |
| :--- | :--- | :--- |
| `InpEnableDCAPermission` | `true` | Interruttore generale del gate di autorizzazione DCA. |
| `InpMinDRS_Ready` | `65.0` | Punteggio minimo DRS richiesto per abilitare il DCA ($0–100$). |
| `InpMaxTDS_Allowed` | `65.0` | Punteggio massimo di pericolo TDS consentito prima del blocco ($0–100$). |
| `InpEnableAntiExpansion` | `true` | Blocca il DCA su brusche espansioni di ATR ($ATR_5/ATR_{30} > 1.35$). |
| `InpEnableDCAKillSwitch` | `true` | Arresto di emergenza per nuovi ordini se $TDS > 85$. |
| `InpShowDCAArrows` | `true` | Visualizza le frecce di autorizzazione DCA sul grafico. |
| `InpDCAArrowCooldownBars` | `3` | Distanza minima in candele tra frecce DCA consecutive. |

### Notifiche Push e Avvisi
| Parametro | Predefinito | Descrizione |
| :--- | :--- | :--- |
| `EnableNotifications` | `true` | Interruttore generale notifiche. |
| `InpNotifyDCASignal` | `true` | Notifica push su smartphone quando lo stato DCA diventa READY. |
| `InpNotifyTrendSignal` | `false` | Notifica push su segnale di pullback confermato. |
| `InpMetaQuotesID` | `""` | MetaQuotes ID per recapito diretto su MT5 mobile. |

---

## 📈 Configurazione di Trading Consigliata

| Classe di Asset | Simboli Consigliati | Timeframe Ottimale | Capitale Minimo (0.01 lotto) | Leva Consigliata |
| :--- | :--- | :--- | :--- | :--- |
| **Metalli Preziosi** | `XAUUSD` (Oro) | `M1` / `M5` | $10,000+ | 1:2000 |
| **Forex Major** | `EURUSD`, `GBPUSD`, `USDJPY` | `M5` / `M15` | $10,000+ | 1:2000 |
| **Criptovalute** | `BTCUSD`, `ETHUSD` | `M5` / `M15` | $10,000+ | 1:2000 |

> 💡 **Suggerimento Pro**: Per la massima stabilità durante eventi macroeconomici ad alto impatto (CPI, NFP, decisioni sui tassi), mantenere attivi `InpEnableAntiExpansion = true` e `EnableNewsAlert = true`.

---

## ❓ Domande Frequenti (FAQ)

<details>
<summary><b>Q1: Posso eseguire il backtest di Rango gratuitamente nel tester di MT5?</b></summary>
<br>
<b>Sì, al 100%!</b> Rango include un bypass automatico per lo Strategy Tester che permette di effettuare backtest, ottimizzazioni e simulazioni visive senza richiedere una chiave di attivazione.
</details>

<details>
<summary><b>Q2: Perché MT5 mostra "È necessario abilitare l'importazione delle DLL"?</b></summary>
<br>
Rango utilizza le API standard di Windows (`kernel32.dll`) per leggere il numero seriale dell'hardware a fini di licenza. Spunta <b>"Permetti importazione DLL"</b> in <code>Strumenti ➔ Opzioni ➔ Consiglieri Esperti</code>.
</details>

<details>
<summary><b>Q3: Dove trovo il seriale del mio computer per l'attivazione in reale?</b></summary>
<br>
Dopo aver applicato Rango a un grafico, controlla il file:
<code>MetaTrader 5 ➔ MQL5 ➔ Files ➔ Rango_Request_Serial.txt</code>.
Il seriale viene stampato anche nella scheda <b>Esperti (Experts Log)</b> di MT5.
</details>

<details>
<summary><b>Q4: Rango funziona correttamente su un VPS?</b></summary>
<br>
<b>Assolutamente sì!</b> Rango è ottimizzato per un utilizzo minimo della CPU e funziona ininterrottamente 24/7 su qualsiasi VPS Windows.
</details>

<details>
<summary><b>Q5: Come ricevere avvisi su smartphone evitando lo spam del broker?</b></summary>
<br>
Inserisci il tuo <b>MetaQuotes ID</b> in MT5 Desktop su <code>Strumenti ➔ Opzioni ➔ Notifiche</code> e attiva <b>"Abilita notifiche Push"</b>.
<br><br>
⚠️ <b>Consiglio Anti-Spam:</b> Assicurati di <b>DESELEZIONARE</b> <i>"Notifiche dal terminale locale"</i> e <i>"Notifiche dal server di trading"</i> per ricevere solo i segnali strategici di Rango!
</details>

---

## 💬 Supporto e Community

- ✈️ **Supporto Telegram e Attivazione Licenze**: [@trading_world_support](https://t.me/trading_world_support)
- 📺 **Canale YouTube**: Video tutorial, guide all'ottimizzazione e sessioni di trading dal vivo.
- 🐛 **Segnalazione Problemi**: Per segnalare bug o richiedere funzionalità, apri una Issue su questo repository GitHub.

---

## ⚠️ Dichiarazione di Non Responsabilità sui Rischi

Il trading su Forex, contratti per differenza (CFD), metalli e criptovalute comporta un elevato livello di rischio e potrebbe non essere adatto a tutti gli investitori. L'elevata leva finanziaria può operare sia a vostro favore che a vostro sfavore. Prima di decidere di fare trading o utilizzare sistemi automatizzati, valutate attentamente i vostri obiettivi finanziari, l'esperienza e la propensione al rischio.

I risultati passati non sono indicativi di rendimenti futuri. Il software e i materiali forniti in questo repository sono destinati esclusivamente a scopi educativi e informativi. Siete gli unici responsabili di ogni decisione e risultato finanziario sui vostri conti reali.

---

<div align="center">
  <sub>Sviluppato con ❤️ da World Trading Lab. Tutti i diritti riservati.</sub>
</div>
