# 🎯 RANGO EA — Ilościowy System Transakcyjny DCA Adaptacyjny do Reżimu Rynku dla MT5

[![Platform](https://img.shields.io/badge/Platforma-MetaTrader%205-blue.svg)](https://www.metatrader5.com/)
[![Language](https://img.shields.io/badge/J%C4%99zyk-MQL5-orange.svg)]()
[![Strategy](https://img.shields.io/badge/Strategia-Regime--Adaptive%20DCA%20%7C%20Mean%20Reversion-green.svg)]()
[![Backtest](https://img.shields.io/badge/Tester%20Strategii-100%25%20Darmowa%20Symulacja-brightgreen.svg)]()
[![Telegram Support](https://img.shields.io/badge/Wsparcie%20Telegram-@trading__world__support-blue.svg)](https://t.me/trading_world_support)

> **Rango EA** to zaawansowany ilościowy system transakcyjny klasy instytucjonalnej stworzony dla **MetaTrader 5 (MT5)**. Zastępuje tradycyjne, „ślepe” uśrednianie kosztów (DCA) i niebezpieczne siatki Martingale inteligentnym silnikiem uprawnień **Regime-Dependent Permission Engine**, wskaźnikiem zagrożenia trendem **Trend Danger Scoring (TDS)** oraz wskaźnikiem wyczerpania statystycznego **Statistical Exhaustion Scoring (ES)**.

---

## 📌 Spis Treści

- [Przegląd](#-przegląd)
- [Dlaczego Rango jest inny (DCA chroniące przed wyzerowaniem konta)](#-dlaczego-rango-jest-inny-dca-chroniące-przed-wyzerowaniem-konta)
- [Główne Funkcje](#-główne-funkcje)
- [Architektura Systemu i Modele Ilościowe](#-architektura-systemu-i-modele-ilościowe)
- [Szybki Start i Przewodnik Instalacji](#-szybki-start-i-przewodnik-instalacji)
- [Aktywacja (Darmowy Tester Strategii i Konfiguracja Live)](#-aktywacja-darmowy-tester-strategii-i-konfiguracja-live)
- [Konfiguracja Powiadomień Push w Telefonie (Aplikacja MT5 i MetaQuotes ID)](#-konfiguracja-powiadomień-push-w-telefonie-aplikacja-mt5-i-metaquotes-id)
- [Wykaz Parametrów Wejściowych (Input Parameters)](#-wykaz-parametrów-wejściowych-input-parameters)
- [Zalecana Konfiguracja Handlu](#-zalecana-konfiguracja-handlu)
- [Często Zadawane Pytania (FAQ)](#-często-zadawane-pytania-faq)
- [Wsparcie i Społeczność](#-wsparcie-i-społeczność)
- [Zrzeczenie się Odpowiedzialności z Tytułu Ryzyka](#-zrzeczenie-się-odpowiedzialności-z-tytułu-ryzyka)

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

## 💡 Przegląd

Tradycyjne systemy siatkowe (grid) oraz Martingale nieuchronnie zawodzą podczas długotrwałych, silnych jednokierunkowych eksplozji trendu. Ślepo dokładają pozycje pod silny impet przeciwko pozycji, aż konto otrzyma wezwanie do uzupełnienia depozytu (Margin Call).

**Rango EA został zaprojektowany od podstaw, aby definitywnie rozwiązać ten problem.**

Zamiast ślepo otwierać zlecenia w stałych odstępach pipsów, Rango wykorzystuje **Wieloczynnikową Ilościową Bramkę Uprawnień (Multi-Factor Quantitative Permission Gate)**:
1. Nieprzerwanie monitoruje trwałość reżimu rynkowego przy użyciu **Wykładnika Hursta (Hurst Exponent)**, **ADX** oraz **Wskaźników ekspansji ATR**.
2. Jeżeli rynek znajduje się w silnym trendzie przeciwko Twojej pozycji, **otwieranie kolejnych pozycji DCA jest ściśle blokowane**.
3. Sygnały i wejścia są autoryzowane tylko wtedy, gdy wskaźniki ilościowe potwierdzą wyczerpanie niekorzystnego impetu, a **prawdopodobieństwo powrotu do średniej (Mean Reversion) jest matematycznie wysokie**.

```
                        ┌─────────────────────────────────┐
                        │    Cena Rynkowa i Zmienność     │
                        └────────────────┬────────────────┘
                                         │
                 ┌───────────────────────┴───────────────────────┐
                 ▼                                               ▼
   ┌───────────────────────────┐                   ┌───────────────────────────┐
   │Ocena Zagrożenia Trendu(TDS│                   │  Ocena Wyczerpania (ES)   │
   │  - Siła trendu wg ADX     │                   │  - Dystans od 200 EMA     │
   │  - Pamięć Hursta (H > 0.55│                   │  - Ekstrema RSI i Wstęgi  │
   │  - Przyspieszenie Zmienno.│                   │  - Świece wygasania pędu  │
   └─────────────┬─────────────┘                   └─────────────┬─────────────┘
                 │                                               │
                 └───────────────────────┬───────────────────────┘
                                         ▼
                        ┌─────────────────────────────────┐
                        │   Ocena Gotowości DCA (DRS)     │
                        └────────────────┬────────────────┘
                                         │
        ┌────────────────────────────────┼────────────────────────────────┐
        ▼                                ▼                                ▼
┌───────────────┐                ┌───────────────┐                ┌───────────────┐
│ DCA ZABLOKOWAN│                │ OBSERWACJA DCA│                │   DCA GOTOWY  │
│Wstrzymaj Zlec.│                │Czekaj na Sygn.│                │Wykonaj Sygnał │
└───────────────┘                └───────────────┘                └───────────────┘
```

---

## ⚡ Dlaczego Rango jest inny (DCA chroniące przed wyzerowaniem konta)

| Cecha | Konwencjonalne DCA / Martingale | **Rango EA** |
| :--- | :--- | :--- |
| **Wyzwalacz Uśredniania**| Stały dystans pipsów lub czas | **Ilościowy Wskaźnik Gotowości DCA (DRS)** |
| **Ochrona Przed Trendem**| Brak (dokłada pozycje aż do krachu) | **Bramka Anty-Ekspansyjna i Blokada Zagrożenia Trendem**|
| **Precyzja Czasowa** | Ślepe wejścia / Stałe pipsy | **Ilościowe Wyczerpanie i Potwierdzenie Odwrócenia** |
| **Izolacja Kierunkowa** | Otwiera Buy/Sell bezkrytycznie | **Izolacja Kierunkowa (Brak uśredniania pod prąd)** |
| **Granica Ryzyka** | Nieograniczone ryzyko likwidacji | **Ścisłe Zarządzanie Ryzykiem i Dynamiczny Stop ATR** |
| **Telemetria Wizualna** | Podstawowa lub brak | **Panel HUD w Czasie Rzeczywistym i Inteligentne Strzałki**|
| **Tester Strategii** | Często zablokowany lub płatny | **100% Darmowy, Nieograniczony Backtest** |

---

## 🚀 Główne Funkcje

### 1. 🛡️ Silnik Uprawnień DCA i Rozpoznawanie Reżimu Rynku
- **Trend Danger Score (TDS: 0–100)**: Ocenia siłę trendu, trwałość Hursta i przyspieszenie zmienności. Gdy $TDS > 65$, nowe pozycje DCA są natychmiast zamrażane.
- **Exhaustion Score (ES: 0–100)**: Mierzy statystyczne odchylenie od średniej 200 EMA, poziomy wykupienia/wyprzedania RSI oraz świece odrzucenia ceny.
- **Bramka Anty-Ekspansyjna (Anti-Expansion Gate)**: Natychmiast blokuje uśrednianie, gdy krótkoterminowy ATR gwałtownie przekracza długoterminowy ($ATR_5 / ATR_{30} > 1.35$) podczas publikacji danych makroekonomicznych.
- **Awaryjny Kill Switch DCA**: Automatyczne zatrzymanie otwierania nowych pozycji, gdy ryzyko przekroczy granice bezpieczeństwa ($TDS > 85$).

### 2. 🎯 Kierunkowa Izolacja DCA (Directional Isolation)
- W potwierdzonym **Trendzie Wzrostowym (Uptrend)**: Zlecenia Buy DCA są zablokowane i badany jest wyłącznie powrót do średniej dla Sell DCA na szczycie wyczerpania.
- W potwierdzonym **Trendzie Spadkowym (Downtrend)**: Zlecenia Sell DCA są zablokowane i badany jest wyłącznie powrót do średniej dla Buy DCA na dołku wyczerpania.
- Zlecenia obukierunkowe są dozwolone jedynie w **Trendzie Bocznym / Konsolidacji (Range-bound)**.

### 3. 🛡️ Profesjonalne Zarządzanie Ryzykiem i Elastyczny Wolumen
- **Wiele Trybów Doboru Lotów**: Stały wolumen (Lot), stała kwota ryzyka ($) lub procent ryzyka z kapitału (%).
- **Automatyczna Projekcja SL/TP**: Dynamiczne kalkulacje w oparciu o ATR wyższego interwału i konfigurowalny stosunek Zysku do Ryzyka (Risk:Reward).
- **Zabezpieczenia Egzekucji**: Filtr spreadu, weryfikacja minimalnego dystansu stopów brokera oraz kontrola wolnego depozytu przed transakcją.

### 4. 📊 Profesjonalny Panel HUD na Wykresie
- Czytelny interfejs o wysokim kontraście prezentujący:
  - Aktualny reżim rynku i stan zmienności.
  - Wartości na żywo wskaźników **TDS** (Zagrożenie Trendem) i **DRS** (Gotowość DCA).
  - Statystyki zyskowności, stan korekty oraz bieżący zysk.

### 5. 🏹 Inteligentne Strzałki Sygnałowe i Powiadomienia Push
- Strzałki są rysowane wyłącznie przy **zmianie stanów rynkowych**, z czasem odnowienia i automatycznym usuwaniem starych sygnałów.
- **Bezpośrednie Powiadomienia Push w Aplikacji MT5 Mobile**: Błyskawiczne przesyłanie sygnałów i alertów DCA READY na telefon za pomocą MetaQuotes ID.

### 6. 🧪 Pełny Dostęp do Testera Strategii MT5
- Przeprowadzaj backtesty, testy warunków skrajnych i optymalizacje parametrów na danych historycznych u dowolnego brokera **bez żadnych ograniczeń**.

---

## 🛠️ Szybki Start i Przewodnik Instalacji

### Wymagania Wstępne
- **Platforma**: MetaTrader 5 (zalecany Build 3800 lub nowszy)
- **System Operacyjny**: Windows 10 / 11 lub Windows Server (VPS)
- **Typ Konta**: Konto z hedgingiem (Standard lub ECN z niskim spreadem)

---

### Instalacja Krok po Kroku

1. **Pobierz `Rango.ex5`**:
   - Pobierz najnowszy skompilowany plik `Rango.ex5` z zakładki [Releases](../../releases) lub katalogu głównego.

2. **Skopiuj do Katalogu MT5**:
   - Otwórz terminal MetaTrader 5.
   - Kliknij `Plik (File)` ➔ `Otwórz Folder Danych (Open Data Folder)`.
   - Przejdź do folderu `MQL5` ➔ `Experts\`.
   - Wklej plik `Rango.ex5` do tego folderu.

3. **Włącz Handel Algorytmiczny i Import DLL**:
   - W MT5 przejdź do `Narzędzia (Tools)` ➔ `Opcje (Options)` (skrót `Ctrl + O`).
   - Wybierz zakładkę **Strategie (Expert Advisors)**.
   - Zaznacz ✅ **Zezwalaj na handel automatyczny (Allow algorithmic trading)**.
   - Zaznacz ✅ **Zezwalaj na import DLL (Allow DLL imports)** *(Wymagane do weryfikacji licencji sprzętowej)*.
   - Zaznacz ✅ **Zezwalaj na WebRequest dla wymienionych adresów URL** i dodaj `https://api.telegram.org` *(w przypadku korzystania z Telegrama)*.

4. **Przeciągnij Rango na Wykres**:
   - W oknie **Nawigator (Navigator)** (`Ctrl + N`) rozwiń **Doradcy Eksperci**.
   - Przeciągnij **Rango** na wybrany wykres (np. `XAUUSD`, `EURUSD`, `GBPUSD`, `BTCUSD`).
   - Zalecany interwał: **M5** lub **M15**.
   - Upewnij się, że opcja **Zezwalaj na handel w czasie rzeczywistym** jest zaznaczona, skonfiguruj parametry ryzyka i kliknij **OK**.

---

## 🔑 Aktywacja (Darmowy Tester Strategii i Konfiguracja Live)

Rango oferuje dwa tryby działania:

### 1. 🧪 100% Darmowy Tester Strategii (Symulacje i Backtesting)
- **Klucz licencyjny nie jest wymagany.**
- Otwórz Tester Strategii MT5 (`Ctrl + R`), wybierz `Rango.ex5`, wskaż instrument i zakres dat, a następnie kliknij **Start**.
- Testuj historię z pełnym dostępem do wszystkich funkcji!

### 2. 💻 Aktywacja na Rachunku Rzeczywistym / Demo
Po dodaniu Rango do wykresu po raz pierwszy:
1. Rango uruchamia się w **Trybie Demonstracyjnym Tylko do Odczytu (View-Only Demo Mode)** (panel HUD i kalkulacje działają poprawnie).
2. Automatycznie generuje unikalny numer seryjny sprzętu do pliku:
   ```
   MQL5\Files\Rango_Request_Serial.txt
   ```
3. Skopiuj swój numer seryjny (format: `RANGO-REQ-XXXX-XXXX-XXXX-XXXX`).
4. Wyślij numer seryjny do wsparcia na Telegramie: **[@trading_world_support](https://t.me/trading_world_support)**, aby otrzymać **Klucz Aktywacyjny** (`RANGO-ACT-...`).
5. Wklej klucz do parametru wejściowego `InpLicenseKey` (zostanie automatycznie zapisany w pamięci podręcznej).

---

## 📲 Konfiguracja Powiadomień Push w Telefonie (Aplikacja MT5 i MetaQuotes ID)

Rango EA obsługuje bezpośrednie powiadomienia push do aplikacji **MetaTrader 5 Mobile** (iOS i Android) za pośrednictwem natywnej funkcji MQL5 `SendNotification`.

### Krok 1: Sprawdź swój MetaQuotes ID w Telefonie
* **Android**: Otwórz aplikację MT5 ➔ Menu (☰) ➔ **Wiadomości** (lub **Ustawienia** ➔ **Wiadomości**) ➔ Zanotuj 8-znakowy **MetaQuotes ID** (np. `1A2B3C4D`).
* **iOS (iPhone/iPad)**: Otwórz aplikację MT5 ➔ **Ustawienia** ➔ **Czat i wiadomości** ➔ Znajdź **Mój identyfikator MetaQuotes ID** na dole ekranu.

### Krok 2: Skonfiguruj Powiadomienia w MT5 na Komputerze
1. W stacjonarnym terminalu MT5 otwórz `Narzędzia` ➔ `Opcje` (`Ctrl + O`).
2. Wybierz zakładkę **Powiadomienia (Notifications)**.
3. Zaznacz ✅ **Włącz powiadomienia Push**.
4. ⚠️ **WAŻNE (Konfiguracja Anty-Spam)**:
   * **ODZNACZ** ❌ **Powiadomienia z lokalnego terminala**
   * **ODZNACZ** ❌ **Powiadomienia z serwera transakcyjnego**
   > 💡 **Dlaczego należy je odznaczyć?** Jeśli pozostaną aktywne, serwer brokera wyśle powiadomienie o każdej drobnej modyfikacji zlecenia. Ich odznaczenie gwarantuje, że otrzymasz **wyłącznie** wartościowe sygnały strategiczne Rango bez spamu!
5. Wpisz swój **MetaQuotes ID** w wyznaczonym polu (wiele identyfikatorów rozdziel przecinkami).
6. Kliknij przycisk **Test**. Na Twój telefon natychmiast przyjdzie powiadomienie testowe!
7. Kliknij **OK**, aby zapisać ustawienia.

### Krok 3: Ustawienia Powiadomień w Rango EA
W parametrach wejściowych EA:
* `EnableNotifications = true` — Włącz powiadomienia push.
* `InpNotifyDCASignal = true` — Otrzymuj alerty w czasie rzeczywistym, gdy stan zmieni się na **DCA READY**.
* `InpNotifyTrendSignal = true` — Otrzymuj alerty o potwierdzonym sygnale korekty w trendzie.
* `InpMetaQuotesID = "..."` — Wpisz swój MetaQuotes ID.

---

## ⚙️ Wykaz Parametrów Wejściowych (Input Parameters)

### Ustawienia Ogólne (General Settings)
| Parametr | Domyślnie | Opis |
| :--- | :--- | :--- |
| `InpLicenseKey` | `""` | Klucz aktywacyjny od wsparcia (pozostaw pusty po zapisaniu). |
| `InputMagicNumber` | `0` | Unikalny Magic Number (`0` = Automatyczny przydział wg hasha symbolu). |
| `Language` | `LANG_ENGLISH` | Język panelu i alertów (`LANG_ENGLISH` lub `LANG_VIETNAMESE`). |
| `InpShowDashboard` | `true` | Wyświetlaj panel HUD na wykresie. |
| `SinglePositionMode` | `true` | Ograniczenie EA do jednego aktywnego cyklu strategicznego na raz. |
| `ATR_Multiplier` | `1.0` | Globalny mnożnik ATR do dynamicznego wyliczania dystansów. |
| `MaxSpreadPoints` | `0` | Maksymalny dozwolony filtr spreadu w punktach (`0` = Wyłączony). |

### Zarządzanie Ryzykiem (Risk Management)
| Parametr | Domyślnie | Opis |
| :--- | :--- | :--- |
| `RiskMode` | `RISK_BY_VOLUME` | Tryb doboru lota (`RISK_BY_PERCENT`, `RISK_BY_AMOUNT`, `RISK_BY_VOLUME`). |
| `RiskDefaulMinimum` | `0.01` | Bazowy wolumen lota lub procent ryzyka. |
| `RiskRewardRatio` | `1.0` | Domyślny stosunek Zysku do Ryzyka (Risk:Reward) dla pojedynczych wejść. |
| `MaxVolumeFixed` | `0` | Maksymalny limit wolumenu na pojedyncze zlecenie (`0` = Bez limitu). |

### Ustawienia Silnika Uprawnień DCA (DCA Permission Engine Settings)
| Parametr | Domyślnie | Opis |
| :--- | :--- | :--- |
| `InpEnableDCAPermission` | `true` | Główny włącznik bramki uprawnień DCA. |
| `InpMinDRS_Ready` | `65.0` | Minimalny wynik gotowości DRS wymagany do uśredniania ($0–100$). |
| `InpMaxTDS_Allowed` | `65.0` | Maksymalny wynik zagrożenia TDS przed zablokowaniem DCA ($0–100$). |
| `InpEnableAntiExpansion` | `true` | Zamrożenie DCA przy gwałtownym skoku zmienności ($ATR_5/ATR_{30} > 1.35$). |
| `InpEnableDCAKillSwitch` | `true` | Awaryjne zatrzymanie nowych zleceń przy $TDS > 85$. |
| `InpShowDCAArrows` | `true` | Rysowanie strzałek uprawnień DCA na wykresie. |
| `InpDCAArrowCooldownBars` | `3` | Minimalny odstęp w świecach pomiędzy strzałkami DCA. |

### Powiadomienia Push i Alerty
| Parametr | Domyślnie | Opis |
| :--- | :--- | :--- |
| `EnableNotifications` | `true` | Główny włącznik powiadomień. |
| `InpNotifyDCASignal` | `true` | Wysyłaj alert mobilny przy przejściu statusu DCA do READY. |
| `InpNotifyTrendSignal` | `false` | Wysyłaj alert mobilny przy potwierdzonych sygnałach korekty. |
| `InpMetaQuotesID` | `""` | Identyfikator MetaQuotes ID dla aplikacji mobilnej. |

---

## 📈 Zalecana Konfiguracja Handlu

| Klasa Aktywów | Zalecane Instrumenty | Optymalny Interwał | Minimalny Depozyt (0.01 lota) | Zalecana Dźwignia |
| :--- | :--- | :--- | :--- | :--- |
| **Metale Szlachetne** | `XAUUSD` (Złoto) | `M1` / `M5` | $10,000+ | 1:2000 |
| **Główne Pary Walutowe** | `EURUSD`, `GBPUSD`, `USDJPY` | `M5` / `M15` | $10,000+ | 1:2000 |
| **Kryptowaluty** | `BTCUSD`, `ETHUSD` | `M5` / `M15` | $10,000+ | 1:2000 |

> 💡 **Wskazówka Eksperta**: Aby uzyskać maksymalną stabilność podczas kluczowych wydarzeń makroekonomicznych (CPI, NFP, decyzje o stopach procentowych), upewnij się, że opcje `InpEnableAntiExpansion = true` oraz `EnableNewsAlert = true` są włączone.

---

## ❓ Często Zadawane Pytania (FAQ)

<details>
<summary><b>Q1: Czy mogę bezpłatnie testować Rango w testerze strategii MT5?</b></summary>
<br>
<b>Tak, w 100% za darmo!</b> Rango posiada wbudowany mechanizm omijania weryfikacji w testerze strategii, co pozwala na nieograniczony backtesting, optymalizację parametrów i symulacje w trybie wizualnym bez konieczności podawania klucza aktywacyjnego.
</details>

<details>
<summary><b>Q2: Dlaczego MT5 wyświetla komunikat "Należy zezwolić na import DLL"?</b></summary>
<br>
Rango korzysta ze standardowego API Windows (`kernel32.dll`) do odczytu numeru seryjnego sprzętu w celach licencyjnych. Zaznacz opcję <b>"Zezwalaj na import DLL"</b> w <code>Narzędzia ➔ Opcje ➔ Strategie</code>.
</details>

<details>
<summary><b>Q3: Gdzie znajdę numer seryjny mojego komputera do aktywacji live?</b></summary>
<br>
Po umieszczeniu Rango na dowolnym wykresie sprawdź plik:
<code>MetaTrader 5 ➔ MQL5 ➔ Files ➔ Rango_Request_Serial.txt</code>.
Ponadto numer seryjny jest drukowany bezpośrednio w zakładce <b>Dziennik Ekspertów (Experts Log)</b> w MT5.
</details>

<details>
<summary><b>Q4: Czy Rango działa stabilnie na serwerze VPS?</b></summary>
<br>
<b>Zdecydowanie tak!</b> Rango został zoptymalizowany pod kątem minimalnego zużycia procesora i działa płynnie 24/7 na dowolnym serwerze Windows VPS.
</details>

<details>
<summary><b>Q5: Jak otrzymywać sygnały na telefon bez uciążliwego spamu?</b></summary>
<br>
Wprowadź swój <b>MetaQuotes ID</b> w stacjonarnym MT5 w menu <code>Narzędzia ➔ Opcje ➔ Powiadomienia</code> i zaznacz <b>"Włącz powiadomienia Push"</b>.
<br><br>
⚠️ <b>Wskazówka Anty-Spam:</b> Pamiętaj, aby <b>ODZNACZYĆ</b> opcje <i>"Powiadomienia z lokalnego terminala"</i> i <i>"Powiadomienia z serwera transakcyjnego"</i>, aby otrzymywać wyłącznie kluczowe sygnały analityczne Rango!
</details>

---

## 💬 Wsparcie i Społeczność

- ✈️ **Wsparcie Telegram i Aktywacja Licencji**: [@trading_world_support](https://t.me/trading_world_support)
- 📺 **Kanał YouTube**: Samouczki wideo, poradniki optymalizacji parametrów i sesje handlowe na żywo.
- 🐛 **Zgłaszanie Problemów**: Jeśli znajdziesz błąd lub masz propozycję nowej funkcji, otwórz Issue w tym repozytorium GitHub.

---

## ⚠️ Zrzeczenie się Odpowiedzialności z Tytułu Ryzyka

Handel na rynku Forex, kontraktami CFD, metalami szlachetnymi i kryptowalutami wiąże się z wysokim poziomem ryzyka i może nie być odpowiedni dla wszystkich inwestorów. Wysoka dźwignia finansowa może działać zarówno na Twoją korzyść, jak i na Twoją niekorzyść. Przed podjęciem decyzji o handlu lub zastosowaniu zautomatyzowanych systemów transakcyjnych należy dokładnie ocenić swoje cele inwestycyjne, poziom doświadczenia oraz tolerancję na ryzyko.

Wyniki osiągnięte w przeszłości nie stanowią gwarancji przyszłych zysków. Oprogramowanie i materiały udostępnione w tym repozytorium służą wyłącznie celom edukacyjnym i informacyjnym. Ponosisz wyłączną odpowiedzialność za wszystkie decyzje handlowe i wyniki finansowe na swoich rachunkach rzeczywistych.

---

<div align="center">
  <sub>Stworzone z ❤️ przez World Trading Lab. Wszelkie prawa zastrzeżone.</sub>
</div>
