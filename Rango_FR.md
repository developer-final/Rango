# 🎯 RANGO EA — Système de Trading DCA Quantitatif Adaptatif au Régime de Marché pour MT5

[![Platform](https://img.shields.io/badge/Plateforme-MetaTrader%205-blue.svg)](https://www.metatrader5.com/)
[![Language](https://img.shields.io/badge/Langage-MQL5-orange.svg)]()
[![Strategy](https://img.shields.io/badge/Strat%C3%A9gie-Regime--Adaptive%20DCA%20%7C%20Mean%20Reversion-green.svg)]()
[![Backtest](https://img.shields.io/badge/Testeur%20de%20Strat%C3%A9gie-100%25%20Simulation%20Gratuite-brightgreen.svg)]()
[![Telegram Support](https://img.shields.io/badge/Support%20Telegram-@trading__world__support-blue.svg)](https://t.me/trading_world_support)

> **Rango EA** est un système de trading quantitatif avancé de niveau institutionnel pour **MetaTrader 5 (MT5)**. Il remplace le moyennage à la baisse (DCA) traditionnel et les grilles Martingale dangereuses par un moteur d'autorisation intelligent **Regime-Dependent Permission Engine**, un score de danger de tendance **Trend Danger Scoring (TDS)** et un score d'épuisement statistique **Statistical Exhaustion Scoring (ES)**.

---

## 📌 Table des Matières

- [Vue d'ensemble](#-vue-densemble)
- [Pourquoi Rango est différent (Le DCA anti-explosion de compte)](#-pourquoi-rango-est-différent-le-dca-anti-explosion-de-compte)
- [Fonctionnalités Principales](#-fonctionnalités-principales)
- [Architecture du Système & Modèles Quantitatifs](#-architecture-du-système--modèles-quantitatifs)
- [Démarrage Rapide & Guide d'Installation](#-démarrage-rapide--guide-dinstallation)
- [Procédure d'Activation (Backtest Gratuit & Compte Réel)](#-procédure-dactivation-backtest-gratuit--compte-réel)
- [Configuration des Notifications Push Mobiles (MT5 App & MetaQuotes ID)](#-configuration-des-notifications-push-mobiles-mt5-app--metaquotes-id)
- [Référence des Paramètres d'Entrée (Input Parameters)](#-référence-des-paramètres-dentrée-input-parameters)
- [Configuration de Trading Recommandée](#-configuration-de-trading-recommandée)
- [Foire Aux Questions (FAQ)](#-foire-aux-questions-faq)
- [Support & Communauté](#-support--communauté)
- [Avertissement sur les Risques](#-avertissement-sur-les-risques)

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

## 💡 Vue d'ensemble

Les systèmes de grille et de Martingale traditionnels échouent inévitablement lors de fortes explosions de tendance unidirectionnelles. Ils accumulent aveuglément des ordres contre une dynamique adverse jusqu'à l'appel de marge (Margin Call).

**Rango EA a été conçu dès l'origine pour résoudre exactement ce problème.**

Au lieu d'ouvrir des positions à des intervalles de pips fixes, Rango intègre une **Porte d'Autorisation Quantitative Multi-Facteurs (Multi-Factor Quantitative Permission Gate)** :
1. Surveillance continue de la persistance du régime de marché grâce à **l'Exposant de Hurst**, **l'ADX** et les **Ratios d'expansion de l'ATR**.
2. Si le marché développe une forte tendance défavorable, **l'ouverture de nouveaux ordres DCA est strictement bloquée**.
3. Les signaux et entrées ne sont autorisés que lorsque les métriques quantitatives confirment l'épuisement du momentum adverse et que la **probabilité de retour à la moyenne (Mean Reversion) est mathématiquement maximisée**.

```
                        ┌─────────────────────────────────┐
                        │   Prix du Marché & Volatilité   │
                        └────────────────┬────────────────┘
                                         │
                 ┌───────────────────────┴───────────────────────┐
                 ▼                                               ▼
   ┌───────────────────────────┐                   ┌───────────────────────────┐
   │ Trend Danger Score (TDS)  │                   │   Exhaustion Score (ES)   │
   │  - Force de tendance ADX  │                   │  - Écart à la moyenne EMA │
   │  - Persistance de Hurst   │                   │  - Extrêmes RSI & Bandes  │
   │  - Accélération de l'ATR  │                   │  - Pinbars d'épuisement   │
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
│  DCA BLOQUÉ   │                │ EN ATTENTE    │                │  DCA PRÊT     │
│Gel des Ordres │                │Attente Signal │                │Exécution Ordre│
└───────────────┘                └───────────────┘                └───────────────┘
```

---

## ⚡ Pourquoi Rango est différent (Le DCA anti-explosion de compte)

| Caractéristique | DCA Traditionnel / Martingale | **Rango EA** |
| :--- | :--- | :--- |
| **Déclencheur d'Averaging** | Distance en pips fixe ou temps | **Score Quantitatif de Disponibilité DCA (DRS)** |
| **Protection de Tendance** | Aucune (renforce dans le krach) | **Porte Anti-Expansion & Blocage du Danger de Tendance** |
| **Précision du Timing** | Aveugle / Pips fixes | **Confirmation d'Épuisement & Rebond Statistique** |
| **Isolation Directionnelle** | Achats/Ventes simultanés sans filtre | **Isolation Directionnelle (Aucun DCA à contre-tendance)** |
| **Gestion du Risque** | Risque illimité de liquidation | **Gestion Stricte du Risque & Protection Stop ATR Dynamique**|
| **Télémétrie Visuelle** | Basique ou inexistante | **Tableau de Bord HUD en Temps Réel & Flèches Intelligentes**|
| **Testeur de Stratégie** | Souvent bridé ou payant | **Backtest 100% Gratuit & Illimité** |

---

## 🚀 Fonctionnalités Principales

### 1. 🛡️ Moteur d'Autorisation DCA & Détection du Régime de Marché
- **Trend Danger Score (TDS : 0–100)** : Évalue la force de la tendance, la mémoire de Hurst et l'accélération de la volatilité. Si $TDS > 65$, les nouveaux ordres DCA sont immédiatement gelés.
- **Exhaustion Score (ES : 0–100)** : Quantifie l'extension statistique par rapport à l'EMA 200, les extrêmes de surachat/survente du RSI et les mèches de rejet de prix.
- **Porte Anti-Expansion** : Bloque instantanément le DCA si l'ATR court terme explose par rapport à l'ATR long terme ($ATR_5 / ATR_{30} > 1.35$) lors des annonces économiques ou cassures brutales.
- **Kill Switch DCA d'Urgence** : Arrêt d'urgence automatique si les conditions défavorables dépassent les seuils de sécurité ($TDS > 85$).

### 2. 🎯 Isolation Directionnelle du DCA
- En **Tendance Haussière (Uptrend)** confirmée : Le DCA à l'Achat est bloqué et seul le retour à la moyenne en Vente est évalué lors de l'épuisement haussier.
- En **Tendance Baissière (Downtrend)** confirmée : Le DCA à la Vente est bloqué et seul le retour à la moyenne à l'Achat est évalué sur creux d'épuisement.
- Les signaux bidirectionnels ne sont autorisés qu'en phase de **Range / Consolidation**.

### 3. 🛡️ Gestion Professionnelle du Risque & Dimensionnement Flexible
- **Modes de Calcul du Lot Multiples** : Volume fixe (Lots), montant monétaire à risque fixe ($), ou pourcentage du capital (%).
- **Projection Automatisée SL/TP** : Calculs dynamiques basés sur l'ATR supérieur et ratio Risque:Rendement personnalisable.
- **Sécurités d'Exécution** : Filtre de spread, vérification des distances minimales de stop du courtier et validation de la marge disponible.

### 4. 📊 Tableau de Bord HUD Professionnel sur Graphique
- Interface visuelle claire et contrastée :
  - État actuel du régime de marché et de la volatilité.
  - Valeurs en direct du **TDS** (Danger Tendance) et du **DRS** (Disponibilité DCA).
  - Statistiques de performance, état des pullbacks et profits en direct.

### 5. 🏹 Flèches de Signal Intelligentes & Notifications Push
- Flèches tracées uniquement lors des **transitions d'état**, avec temps de recharge et suppression automatique des anciens signaux.
- **Notifications Push Mobiles MT5 Directes** : Envoi instantané des alertes et signaux DCA READY directement sur votre smartphone via MetaQuotes ID.

### 6. 🧪 Testeur de Stratégie MT5 Illimité
- Testez, optimisez et éprouvez Rango sur les données historiques de n'importe quel courtier et unité de temps **sans aucune restriction**.

---

## 🛠️ Démarrage Rapide & Guide d'Installation

### Prérequis
- **Plateforme** : MetaTrader 5 (Build 3800 ou plus récent recommandé)
- **Système d'exploitation** : Windows 10 / 11 ou Windows Server (VPS)
- **Type de compte** : Compte Hedging recommandé (Standard ou ECN à faible spread)

---

### Installation Étape par Étape

1. **Télécharger `Rango.ex5`** :
   - Téléchargez le fichier compilé `Rango.ex5` depuis l'onglet [Releases](../../releases) ou la racine du dépôt.

2. **Copier dans le Répertoire MT5** :
   - Ouvrez votre terminal MetaTrader 5.
   - Cliquez sur `Fichier (File)` ➔ `Ouvrir le dossier des données (Open Data Folder)`.
   - Allez dans `MQL5` ➔ `Experts\`.
   - Collez `Rango.ex5` dans ce dossier.

3. **Activer le Trading Algorithmique & les Importations DLL** :
   - Dans MT5, allez dans `Outils (Tools)` ➔ `Options` (ou `Ctrl + O`).
   - Sélectionnez l'onglet **Expert Advisors**.
   - Cochez ✅ **Autoriser le trading algorithmique (Allow algorithmic trading)**.
   - Cochez ✅ **Autoriser les importations DLL (Allow DLL imports)** *(Requis pour la validation de licence matérielle)*.
   - Cochez ✅ **Autoriser WebRequest pour les URL listées** et ajoutez `https://api.telegram.org` *(si vous utilisez Telegram)*.

4. **Attacher Rango au Graphique** :
   - Dans le **Navigateur** (`Ctrl + N`), déroulez **Expert Advisors**.
   - Glissez-déposez **Rango** sur le graphique souhaité (ex. : `XAUUSD`, `EURUSD`, `GBPUSD`, `BTCUSD`).
   - Période recommandée : **M5** ou **M15**.
   - Assurez-vous que **Autoriser le trading en direct** est coché, configurez vos paramètres et cliquez sur **OK**.

---

## 🔑 Procédure d'Activation (Backtest Gratuit & Compte Réel)

Rango propose deux niveaux d'utilisation :

### 1. 🧪 Testeur de Stratégie 100% Gratuit (Simulations & Backtests)
- **Aucune clé de licence requise.**
- Ouvrez le testeur de stratégie MT5 (`Ctrl + R`), sélectionnez `Rango.ex5`, choisissez votre symbole et cliquez sur **Démarrer**.
- Accédez à l'ensemble des fonctionnalités sans restriction !

### 2. 💻 Activation sur Graphique Réel / Démo
Lors de la première utilisation de Rango sur un graphique live ou démo :
1. Rango démarre en **Mode Démo Consultation Seule (View-Only Demo Mode)** (le tableau de bord et les calculs fonctionnent).
2. L'EA génère automatiquement votre numéro de série machine et l'exporte dans :
   ```
   MQL5\Files\Rango_Request_Serial.txt
   ```
3. Copiez votre numéro de série (format : `RANGO-REQ-XXXX-XXXX-XXXX-XXXX`).
4. Envoyez-le au Support Telegram : **[@trading_world_support](https://t.me/trading_world_support)** pour recevoir votre **Clé d'Activation** (`RANGO-ACT-...`).
5. Collez la clé dans le paramètre d'entrée `InpLicenseKey` (mise en mémoire cache automatique).

---

## 📲 Configuration des Notifications Push Mobiles (MT5 App & MetaQuotes ID)

Rango EA prend en charge les notifications instantanées directes vers l'application **MetaTrader 5 Mobile** (iOS & Android) grâce au moteur natif MQL5 `SendNotification`, garantissant une réception ultra-rapide sans dépendre de bots externes.

### Étape 1 : Récupérer votre MetaQuotes ID sur Mobile
* **Android** : Ouvrir MT5 ➔ Menu (☰) ➔ **Messages** (ou **Paramètres** ➔ **Messages**) ➔ Noter votre **MetaQuotes ID** (chaîne de 8 caractères, ex. : `1A2B3C4D`).
* **iOS (iPhone/iPad)** : Ouvrir MT5 ➔ **Paramètres** ➔ **Chat et Messages** ➔ Trouver **Mon MetaQuotes ID** en bas de l'écran.

### Étape 2 : Configurer les Notifications sur MT5 Desktop
1. Sur MT5 PC, aller dans `Outils` ➔ `Options` (`Ctrl + O`).
2. Ouvrir l'onglet **Notifications**.
3. Cocher ✅ **Activer les notifications push (Enable Push Notifications)**.
4. ⚠️ **IMPORTANT (Configuration Anti-Spam)** :
   * **DÉCOCHER** ❌ **Notifications du terminal local**
   * **DÉCOCHER** ❌ **Notifications du serveur de trading**
   > 💡 **Pourquoi décocher ces options ?** Si elles sont cochées, le courtier enverra une notification pour chaque micro-action (modification d'ordre, exécution). En les décochant, vous **ne recevez que** les signaux à haute valeur ajoutée de Rango !
5. Saisissez votre **MetaQuotes ID** dans le champ prévu (plusieurs IDs séparés par des virgules).
6. Cliquez sur **Tester**. Vous devriez recevoir une notification test immédiatement sur votre mobile !
7. Cliquez sur **OK** pour enregistrer.

### Étape 3 : Configurer les Paramètres dans Rango EA
Dans les réglages de l'EA :
* `EnableNotifications = true` — Activer les notifications push.
* `InpNotifyDCASignal = true` — Recevoir une alerte quand le statut passe à **DCA READY**.
* `InpNotifyTrendSignal = true` — Recevoir une alerte sur signal de pullback de tendance confirmé.
* `InpMetaQuotesID = "..."` — Entrer votre MetaQuotes ID.

---

## ⚙️ Référence des Paramètres d'Entrée (Input Parameters)

### Paramètres Généraux (General Settings)
| Paramètre | Défaut | Description |
| :--- | :--- | :--- |
| `InpLicenseKey` | `""` | Clé d'activation fournie par le support (laisser vide après mise en cache). |
| `InputMagicNumber` | `0` | Identifiant Magic Number (`0` = Attribution auto selon le hash du symbole). |
| `Language` | `LANG_ENGLISH` | Langue du tableau de bord & alertes (`LANG_ENGLISH` ou `LANG_VIETNAMESE`). |
| `InpShowDashboard` | `true` | Afficher le tableau de bord HUD sur le graphique. |
| `SinglePositionMode` | `true` | Restreindre l'EA à un seul cycle stratégique à la fois. |
| `ATR_Multiplier` | `1.0` | Multiplicateur ATR global pour les calculs dynamiques. |
| `MaxSpreadPoints` | `0` | Filtre de spread maximal en points (`0` = Désactivé). |

### Gestion du Risque (Risk Management)
| Paramètre | Défaut | Description |
| :--- | :--- | :--- |
| `RiskMode` | `RISK_BY_VOLUME` | Mode de calcul du lot (`RISK_BY_PERCENT`, `RISK_BY_AMOUNT`, `RISK_BY_VOLUME`). |
| `RiskDefaulMinimum` | `0.01` | Taille de lot de base ou pourcentage de risque. |
| `RiskRewardRatio` | `1.0` | Ratio Risque:Rendement par défaut pour les ordres simples. |
| `MaxVolumeFixed` | `0` | Plafond de volume fixe par ordre unique (`0` = Illimité). |

### Paramètres du Moteur DCA (DCA Permission Engine Settings)
| Paramètre | Défaut | Description |
| :--- | :--- | :--- |
| `InpEnableDCAPermission` | `true` | Interrupteur principal de la porte d'autorisation DCA. |
| `InpMinDRS_Ready` | `65.0` | Score DRS minimal requis pour autoriser le DCA ($0–100$). |
| `InpMaxTDS_Allowed` | `65.0` | Score TDS maximal toléré avant blocage du DCA ($0–100$). |
| `InpEnableAntiExpansion` | `true` | Geler le DCA lors des pics d'expansion ATR ($ATR_5/ATR_{30} > 1.35$). |
| `InpEnableDCAKillSwitch` | `true` | Arrêt d'urgence de tout nouvel ordre si $TDS > 85$. |
| `InpShowDCAArrows` | `true` | Afficher les flèches d'autorisation DCA sur le graphique. |
| `InpDCAArrowCooldownBars` | `3` | Espacement minimal en barres entre flèches consécutives. |

### Notifications & Alertes
| Paramètre | Défaut | Description |
| :--- | :--- | :--- |
| `EnableNotifications` | `true` | Interrupteur principal des notifications. |
| `InpNotifyDCASignal` | `true` | Alerte push lorsque le statut DCA passe à READY. |
| `InpNotifyTrendSignal` | `false` | Alerte push lors de signaux de tendance confirmés. |
| `InpMetaQuotesID` | `""` | MetaQuotes ID pour la réception mobile. |

---

## 📈 Configuration de Trading Recommandée

| Classe d'Actifs | Symboles Recommandés | Unité de Temps Optimale | Capital Minimum (0.01 lot) | Effet de Levier Recommandé |
| :--- | :--- | :--- | :--- | :--- |
| **Métaux Précieux** | `XAUUSD` (Or) | `M1` / `M5` | $10,000+ | 1:2000 |
| **Forex Majeur** | `EURUSD`, `GBPUSD`, `USDJPY` | `M5` / `M15` | $10,000+ | 1:2000 |
| **Crypto-monnaies**| `BTCUSD`, `ETHUSD` | `M5` / `M15` | $10,000+ | 1:2000 |

> 💡 **Conseil Pro** : Pour une stabilité optimale lors des annonces économiques majeures (CPI, NFP, taux directeurs), gardez toujours `InpEnableAntiExpansion = true` et `EnableNewsAlert = true`.

---

## ❓ Foire Aux Questions (FAQ)

<details>
<summary><b>Q1 : Puis-je tester Rango gratuitement dans le testeur MT5 ?</b></summary>
<br>
<b>Oui, à 100% !</b> Rango intègre un contournement automatique du testeur de stratégie, permettant d'effectuer des backtests, des optimisations et des simulations visuelles sans clé de licence.
</details>

<details>
<summary><b>Q2 : Pourquoi MT5 indique-t-il "Les importations DLL doivent être activées" ?</b></summary>
<br>
Rango utilise l'API standard Windows (`kernel32.dll`) pour lire le numéro de série matériel afin de valider la licence. Cochez <b>"Autoriser les importations DLL"</b> dans <code>Outils ➔ Options ➔ Expert Advisors</code>.
</details>

<details>
<summary><b>Q3 : Où trouver le numéro de série de ma machine pour l'activation réelle ?</b></summary>
<br>
Après avoir appliqué Rango à un graphique, consultez le fichier :
<code>MetaTrader 5 ➔ MQL5 ➔ Files ➔ Rango_Request_Serial.txt</code>.
Le numéro de série est également affiché dans l'onglet <b>Journal Experts</b> de MT5.
</details>

<details>
<summary><b>Q4 : Rango fonctionne-t-il sur un serveur VPS ?</b></summary>
<br>
<b>Oui !</b> Rango est optimisé pour une consommation CPU minimale et fonctionne en continu 24/7 sur n'importe quel VPS Windows.
</details>

<details>
<summary><b>Q5 : Comment recevoir les signaux sur mobile sans spam ?</b></summary>
<br>
Renseignez votre <b>MetaQuotes ID</b> dans MT5 PC sous <code>Outils ➔ Options ➔ Notifications</code> et cochez <b>"Activer les notifications push"</b>.
<br><br>
⚠️ <b>Astuce Anti-Spam :</b> Veillez à <b>DÉCOCHER</b> <i>"Notifications du terminal local"</i> et <i>"Notifications du serveur de trading"</i> pour ne recevoir que les alertes stratégiques de Rango !
</details>

---

## 💬 Support & Communauté

- ✈️ **Support Telegram & Activation de Licence** : [@trading_world_support](https://t.me/trading_world_support)
- 📺 **Chaîne YouTube** : Tutoriels vidéo, guides d'optimisation et sessions de trading en direct.
- 🐛 **Suivi des Problèmes** : Pour signaler un bug ou proposer une amélioration, ouvrez un ticket sur ce dépôt GitHub.

---

## ⚠️ Avertissement sur les Risques

Le trading de devises (Forex), de contrats sur la différence (CFD), de métaux et de crypto-monnaies comporte un niveau de risque élevé et peut ne pas convenir à tous les investisseurs. L'effet de levier élevé peut jouer en votre faveur comme en votre défaveur. Avant de décider de trader ou d'utiliser des systèmes automatisés, évaluez attentivement vos objectifs d'investissement, votre niveau d'expérience et votre tolérance au risque.

Les performances passées ne préjugent pas des résultats futurs. Les logiciels et documents fournis dans ce dépôt sont destinés à des fins éducatives. Vous êtes seul responsable de vos décisions de trading sur vos comptes réels.

---

<div align="center">
  <sub>Développé avec ❤️ par World Trading Lab. Tous droits réservés.</sub>
</div>
