# 🎯 RANGO EA — MT5용 시장 상태 적응형 퀀트 DCA 자동매매 시스템

[![Platform](https://img.shields.io/badge/Platform-MetaTrader%205-blue.svg)](https://www.metatrader5.com/)
[![Language](https://img.shields.io/badge/Language-MQL5-orange.svg)]()
[![Strategy](https://img.shields.io/badge/Strategy-Regime--Adaptive%20DCA%20%7C%20Mean%20Reversion-green.svg)]()
[![Backtest](https://img.shields.io/badge/Strategy%20Tester-100%25%20Free%20Simulation-brightgreen.svg)]()
[![Telegram Support](https://img.shields.io/badge/Telegram-@trading__world__support-blue.svg)](https://t.me/trading_world_support)

> **Rango EA**는 **MetaTrader 5 (MT5)** 플랫폼을 위해 개발된 기관급 고급 퀀트 트레이딩 시스템입니다. 위험한 마틴게일(Martingale) 그리드 및 무분별한 단순 물타기(DCA) 방식을 대체하여, 지능형 **시장 국면 종속 허가 엔진 (Regime-Dependent Permission Engine)**, **추세 위험도 점수 (TDS)**, **통계적 소진 점수 (ES)**를 적용합니다.

---

## 📌 목차

- [시스템 개요](#-시스템-개요)
- [Rango의 차별점 (계좌 파산 방지형 DCA)](#-rango의-차별점-계좌-파산-방지형-dca)
- [핵심 기능](#-핵심-기능)
- [시스템 아키텍처 및 퀀트 수식 모델](#-시스템-아키텍처-및-퀀트-수식-모델)
- [빠른 시작 및 설치 가이드](#-빠른-시작-및-설치-가이드)
- [라이선스 활성화 방법 (무료 백테스팅 및 실계좌 설정)](#-라이선스-활성화-방법-무료-백테스팅-및-실계좌-설정)
- [모바일 푸시 알림 설정 (MT5 앱 및 MetaQuotes ID)](#-모바일-푸시-알림-설정-mt5-앱-및-metaquotes-id)
- [입력 매개변수 상세 안내 (Input Parameters)](#-입력-매개변수-상세-안내-input-parameters)
- [권장 트레이딩 설정](#-권장-트레이딩-설정)
- [자주 묻는 질문 (FAQ)](#-자주-묻는-질문-faq)
- [기술 지원 및 커뮤니티](#-기술-지원-및-커뮤니티)
- [투자 위험 고지](#-투자-위험-고지)

---

## 💡 시스템 개요

전통적인 그리드 및 마틴게일 시스템은 강력하고 지속적인 일방향 추세 폭발 시 필연적으로 계좌 청산(마진콜)으로 이어집니다. 반대 방향 모멘텀에 맹목적으로 주문을 중첩하기 때문입니다.

**Rango EA는 이 근본적인 문제를 완벽하게 해결하기 위해 설계되었습니다.**

고정 핍 간격으로 무작정 진입하는 대신, Rango는 **다요소 퀀트 허가 게이트 (Multi-Factor Quantitative Permission Gate)**를 적용합니다:
1. **허스트 지수 (Hurst Exponent)**, **ADX**, **ATR 변동성 확장 비율**을 활용하여 시장 국면 지속성을 실시간 모니터링합니다.
2. 시장이 포지션 반대 방향으로 강력한 추세를 형성하면, **DCA 추가 진입이 엄격히 차단(Block)됩니다**.
3. 반대 추세 모멘텀이 통계적으로 완전히 소진되고 **평균 회귀(Mean-Reversion) 확률이 수학적으로 극대화된 시점**에만 주문 실행이 허가됩니다.

```
                        ┌─────────────────────────────────┐
                        │        시장 가격 및 변동성       │
                        └────────────────┬────────────────┘
                                         │
                 ┌───────────────────────┴───────────────────────┐
                 ▼                                               ▼
   ┌───────────────────────────┐                   ┌───────────────────────────┐
   │    추세 위험 점수 (TDS)   │                   │    통계적 소진 점수 (ES)   │
   │  - ADX 추세 강도          │                   │  - 200 EMA 대비 이격 거리 │
   │  - 허스트 지속성 (H>0.55) │                   │  - RSI 과매수/과매도 극단 │
   │  - ATR 변동성 가속도      │                   │  - 모멘텀 감속 핀바 캔들  │
   └─────────────┬─────────────┘                   └─────────────┬─────────────┘
                 │                                               │
                 └───────────────────────┬───────────────────────┘
                                         ▼
                        ┌─────────────────────────────────┐
                        │      DCA 준비도 점수 (DRS)      │
                        └────────────────┬────────────────┘
                                         │
        ┌────────────────────────────────┼────────────────────────────────┐
        ▼                                ▼                                ▼
┌───────────────┐                ┌───────────────┐                ┌───────────────┐
│  DCA BLOCKED  │                │   DCA WATCH   │                │   DCA READY   │
│ 추가진입 차단 │                │  트리거 대기  │                │  신호 실행    │
└───────────────┘                └───────────────┘                └───────────────┘
```

---

## ⚡ Rango의 차별점 (계좌 파산 방지형 DCA)

| 기능 항목 | 기존 DCA / 마틴게일 그리드 | **Rango EA** |
| :--- | :--- | :--- |
| **추가 진입 트리거** | 고정 핍 간격 또는 시간 타이머 | **퀀트 DCA 준비도 점수 (DRS)** |
| **추세 폭발 보호** | 없음 (폭락/폭등 시에도 무차별 진입) | **변동성 확장 방지 게이트 & 추세 위험 차단기** |
| **타이밍 정밀도** | 맹목적 진입 / 기계적 핍스 | **모멘텀 소진 및 통계적 반전 확인 후 진입** |
| **방향성 격리** | 양방향 매수/매도 무차별 체결 | **방향성 격리 구조 (역추세 물타기 원천 배제)** |
| **리스크 한계** | 무제한 파산 위험 | **엄격한 리스크 관리 & ATR 기반 동적 손절 보호** |
| **차트 시각화** | 없음 또는 단순 텍스트 | **실시간 HUD 대시보드 및 스마트 상태전환 화살표** |
| **전략 테스터** | 사용 제한 또는 유료화 | **100% 완전 무료, 무제한 백테스팅 지원** |

---

## 🚀 핵심 기능

### 1. 🛡️ DCA 허가 및 시장 상태 판별 엔진
- **추세 위험 점수 (TDS: 0–100)**: 추세 강도, 허스트 지속성, 변동성 가속도를 평가합니다. $TDS > 65$일 경우 신규 DCA 추가 진입이 즉시 동결됩니다.
- **통계적 소진 점수 (ES: 0–100)**: 200 EMA 이격도, RSI 과매수/과매도 극단값, 캔들 실체 감속 및 가격 거부 핀바를 정량화합니다.
- **변동성 급확장 방지 게이트 (Anti-Expansion Gate)**: 주요 뉴스 발표 등으로 단기 ATR이 장기 ATR을 급격히 초과할 경우 ($ATR_5 / ATR_{30} > 1.35$), 추가 진입을 즉각 차단합니다.
- **긴급 DCA 킬스위치 (Kill Switch)**: 극단적 역추세 상황 ($TDS > 85$) 발생 시 신규 진입을 전면 차단합니다.

### 2. 🎯 방향성 DCA 격리 (Directional Isolation)
- 명확한 **상승 추세 (Uptrend)**: 매수 DCA를 차단하고, 상단 모멘텀 소진 시 매도 DCA 평균 회귀만 평가합니다.
- 명확한 **하락 추세 (Downtrend)**: 매도 DCA를 차단하고, 하단 모멘텀 소진 시 매수 DCA 평균 회귀만 평가합니다.
- 양방향 신호는 **횡보/박스권 (Sideways/Range)** 장세에서만 허용됩니다.

### 3. 🛡️ 전문적 리스크 관리 및 유연한 포지션 사이징
- **다양한 계약수 산정 모드**: 고정 랏수 (Volume), 고정 손실 금액 ($), 또는 계좌 잔고 대비 리스크 비율 (%) 지원.
- **자동 SL/TP 투사**: 상위 타임프레임 ATR 및 사용자 정의 손익비(Risk:Reward)를 기반으로 동적 거리 계산.
- **체결 안정성 가드**: 스프레드 필터, 브로커 최소 스탑 거리 검증 및 주문 전 증거금 유효성 검사.

### 4. 📊 고성능 온차트 HUD 대시보드
- 시인성이 뛰어난 다크 모드 인터페이스:
  - 현재 시장 국면 (Market Regime) 및 변동성 상태.
  - 실시간 **TDS** (추세 위험도) 및 **DRS** (DCA 준비도).
  - 승률 통계, 추세 되돌림(Pullback) 상태 및 실시간 수익률.

### 5. 🏹 스마트 신호 화살표 및 모바일 푸시 알림
- 차트 난잡함을 방지하기 위해 **상태 전환 시점에만** 화살표를 표시하며, 쿨다운 및 자동 만료 삭제 기능 탑재.
- **MT5 모바일 앱 즉시 푸시 알림**: MetaQuotes ID를 통해 매매 신호 및 DCA READY 알림을 스마트폰으로 직접 전송.

### 6. 🧪 100% 무료 MT5 전략 테스터 백테스트 지원
- 모든 브로커, 종목, 타임프레임에서 과거 데이터 백테스팅, 스트레스 테스트 및 파라미터 최적화를 **완전 무료**로 수행 가능.

---

## 🛠️ 빠른 시작 및 설치 가이드

### 시스템 요구사항
- **플랫폼**: MetaTrader 5 (Build 3800 이상 권장)
- **운영체제**: Windows 10 / 11 또는 Windows Server (VPS)
- **계좌 유형**: 헤징(Hedging) 계좌 권장 (스프레드가 낮은 표준 또는 ECN)

---

### 단계별 설치 방법

1. **`Rango.ex5` 다운로드**:
   - [Releases](../../releases) 탭 또는 저장소 루트에서 최신 컴파일된 `Rango.ex5`를 다운로드합니다.

2. **MT5 폴더로 복사**:
   - MetaTrader 5 터미널을 실행합니다.
   - `파일 (File)` ➔ `데이터 폴더 열기 (Open Data Folder)`를 클릭합니다.
   - `MQL5` ➔ `Experts\` 폴더로 이동합니다.
   - `Rango.ex5` 파일을 해당 폴더에 붙여넣습니다.

3. **자동매매 및 DLL 가져오기 허용**:
   - MT5 상단 메뉴 `도구 (Tools)` ➔ `옵션 (Options)` (단축키 `Ctrl + O`)으로 이동합니다.
   - **Expert Advisors** 탭을 선택합니다.
   - ✅ **알고리즘 트레이딩 허용 (Allow algorithmic trading)** 체크.
   - ✅ **DLL 가져오기 허용 (Allow DLL imports)** 체크 *(하드웨어 라이선스 인증에 필수)*.
   - ✅ **목록에 있는 URL에 대해 WebRequest 허용**에 `https://api.telegram.org` 추가 *(텔레그램 알림 사용 시)*.

4. **차트에 Rango 적용**:
   - **내비게이터 (Navigator)** 창 (`Ctrl + N`)에서 **Expert Advisors** 항목을 확장합니다.
   - **Rango**를 원하는 차트(예: `XAUUSD`, `EURUSD`, `GBPUSD`, `BTCUSD`)로 드래그 앤 드롭합니다.
   - 권장 타임프레임: **M5** 또는 **M15**.
   - EA 설정 팝업에서 **실제 거래 허용**을 확인하고 리스크 변수를 설정한 뒤 **확인(OK)**을 누릅니다.

---

## 🔑 라이선스 활성화 방법 (무료 백테스팅 및 실계좌 설정)

Rango는 두 가지 방식으로 동작합니다:

### 1. 🧪 100% 무료 전략 테스터 (시뮬레이션 및 백테스트)
- **라이선스 키가 필요하지 않습니다.**
- MT5 전략 테스터 (`Ctrl + R`)를 열고 `Rango.ex5`, 종목, 기간을 설정한 후 **시작 (Start)**을 누릅니다.
- 모든 기능을 제약 없이 자유롭게 검증할 수 있습니다!

### 2. 💻 실계좌 / 데모 차트 활성화
Rango를 실계좌 또는 데모 차트에 처음 적용할 때:
1. Rango는 **조회 전용 데모 모드 (View-Only Demo Mode)**로 실행됩니다 (HUD 대시보드, 퀀트 계산 및 신호는 정상 작동).
2. PC 고유 하드웨어 시리얼 번호가 자동으로 생성되어 파일로 출력됩니다:
   ```
   MQL5\Files\Rango_Request_Serial.txt
   ```
3. 시리얼 번호를 복사합니다 (형식: `RANGO-REQ-XXXX-XXXX-XXXX-XXXX`).
4. 텔레그램 지원팀 **[@trading_world_support](https://t.me/trading_world_support)**으로 시리얼을 전송하여 **활성화 키(Activation Key)** (`RANGO-ACT-...`)를 발급받습니다.
5. 입력 매개변수 `InpLicenseKey`에 키를 입력합니다 (인증 완료 시 캐시에 영구 저장됩니다).

---

## 📲 모바일 푸시 알림 설정 (MT5 앱 및 MetaQuotes ID)

Rango EA는 MQL5 자체 `SendNotification` 기능을 사용하여 **MetaTrader 5 모바일 앱**(iOS 및 Android)으로 지연 없이 즉각적인 푸시 알림을 지원합니다. 복잡한 WebRequest나 외부 봇 없이 안전하게 수신할 수 있습니다.

### 1단계: 모바일 앱에서 MetaQuotes ID 확인
* **Android (안드로이드)**: MT5 앱 실행 ➔ 메뉴 (☰) ➔ **메시지** (또는 **설정** ➔ **메시지**) ➔ 8자리 **MetaQuotes ID** 확인 (예: `1A2B3C4D`).
* **iOS (아이폰/아이패드)**: MT5 앱 실행 ➔ **설정** ➔ **채팅 및 메시지** ➔ 하단에 표시된 **My MetaQuotes ID** 확인.

### 2단계: 데스크톱 MT5에서 푸시 알림 설정
1. PC용 MT5 터미널에서 `도구 (Tools)` ➔ `옵션 (Options)` (`Ctrl + O`)을 엽니다.
2. **알림 (Notifications)** 탭을 선택합니다.
3. ✅ **푸시 알림 사용 (Enable Push Notifications)** 체크.
4. ⚠️ **중요 (스팸 방지 설정)**:
   * **체크 해제** ❌ **로컬 터미널의 알림 (Notifications from the local terminal)**
   * **체크 해제** ❌ **거래 서버의 알림 (Notifications from the trade server)**
   > 💡 **왜 해제해야 하나요?** 이 두 항목이 켜져 있으면 증권사 서버에서 모든 미세 체결/주문 수정마다 알림을 보내게 됩니다. 체크를 해제하면 불필요한 스팸 없이 Rango의 핵심 전략 신호(추세 및 DCA 알림)만 깔끔하게 수신할 수 있습니다!
5. **MetaQuotes ID** 입력란에 스마트폰 ID를 입력합니다 (여러 개인 경우 쉼표로 구분).
6. **테스트 (Test)** 버튼을 클릭하여 스마트폰으로 테스트 메시지가 오는지 확인합니다.
7. **확인 (OK)**을 눌러 설정을 저장합니다.

### 3단계: Rango EA 알림 매개변수 설정
EA 입력 설정에서 다음을 지정합니다:
* `EnableNotifications = true` — 푸시 알림 기능 활성화.
* `InpNotifyDCASignal = true` — 지표가 **DCA READY** 상태가 될 때 실시간 푸시 수신.
* `InpNotifyTrendSignal = true` — 확정된 추세 되돌림 신호 발생 시 푸시 수신.
* `InpMetaQuotesID = "..."` — MetaQuotes ID (옵션 입력값).

---

## ⚙️ 입력 매개변수 상세 안내 (Input Parameters)

### 일반 설정 (General Settings)
| 매개변수 | 기본값 | 설명 |
| :--- | :--- | :--- |
| `InpLicenseKey` | `""` | 지원팀에서 발급받은 라이선스 키 (캐시 저장 후 공란 가능). |
| `InputMagicNumber` | `0` | EA 고유 매직 넘버 (`0` = 종목 해시값 기반 자동 부여). |
| `Language` | `LANG_ENGLISH` | 대시보드 및 알림 언어 (`LANG_ENGLISH` 또는 `LANG_VIETNAMESE`). |
| `InpShowDashboard` | `true` | 차트 상 실시간 HUD 대시보드 표시 여부. |
| `SinglePositionMode` | `true` | 한 번에 단일 전략 사이클만 실행하도록 제한. |
| `ATR_Multiplier` | `1.0` | 동적 거리 계산을 위한 글로벌 ATR 승수. |
| `MaxSpreadPoints` | `0` | 최대 허용 스프레드 필터 (포인트 단위, `0` = 비활성화). |

### 리스크 관리 (Risk Management)
| 매개변수 | 기본값 | 설명 |
| :--- | :--- | :--- |
| `RiskMode` | `RISK_BY_VOLUME` | 리스크 계산 방식 (`RISK_BY_PERCENT`, `RISK_BY_AMOUNT`, `RISK_BY_VOLUME`). |
| `RiskDefaulMinimum` | `0.01` | 기본 랏수 또는 리스크 백분율. |
| `RiskRewardRatio` | `1.0` | 단일 진입 목표 기본 손익비 (Risk:Reward). |
| `MaxVolumeFixed` | `0` | 단일 주문당 최대 고정 랏수 상한 (`0` = 무제한). |

### DCA 허가 엔진 설정 (DCA Permission Engine Settings)
| 매개변수 | 기본값 | 설명 |
| :--- | :--- | :--- |
| `InpEnableDCAPermission` | `true` | DCA 허가 게이트 활성화 마스터 스위치. |
| `InpMinDRS_Ready` | `65.0` | DCA 추가 진입을 허가하기 위한 최소 DRS 준비도 점수 ($0–100$). |
| `InpMaxTDS_Allowed` | `65.0` | DCA를 차단하기 전 허용되는 최대 추세 위험 점수 TDS ($0–100$). |
| `InpEnableAntiExpansion` | `true` | 변동성 급증 시 DCA 동결 ($ATR_5/ATR_{30} > 1.35$). |
| `InpEnableDCAKillSwitch` | `true` | $TDS > 85$ 초과 시 추가 주문 비상 차단. |
| `InpShowDCAArrows` | `true` | 차트에 DCA 허가 신호 화살표 표시. |
| `InpDCAArrowCooldownBars` | `3` | 연속된 DCA 화살표 간 최소 캔들 간격. |

### 푸시 알림 설정 (Push Notifications & Alerts)
| 매개변수 | 기본값 | 설명 |
| :--- | :--- | :--- |
| `EnableNotifications` | `true` | 알림 기능 마스터 스위치. |
| `InpNotifyDCASignal` | `true` | DCA 상태가 READY로 전환 시 스마트폰 푸시 알림 전송. |
| `InpNotifyTrendSignal` | `false` | 추세 풀백 신호 발생 시 푸시 알림 전송. |
| `InpMetaQuotesID` | `""` | 모바일 MT5 앱 다이렉트 푸시용 MetaQuotes ID. |

---

## 📈 권장 트레이딩 설정

| 자산군 | 권장 종목 | 최적 타임프레임 | 권장 최소 증거금 (0.01 랏) | 권장 레버리지 |
| :--- | :--- | :--- | :--- | :--- |
| **귀금속** | `XAUUSD` (골드) | `M1` / `M5` | $10,000+ | 1:2000 |
| **주요 외환** | `EURUSD`, `GBPUSD`, `USDJPY` | `M5` / `M15` | $10,000+ | 1:2000 |
| **암호화폐** | `BTCUSD`, `ETHUSD` | `M5` / `M15` | $10,000+ | 1:2000 |

> 💡 **전문가 팁**: 주요 경제 지표 발표 시(CPI, 비농업고용지수, FOMC 등) 안전한 운용을 위해 `InpEnableAntiExpansion = true` 및 `EnableNewsAlert = true` 설정을 항상 켜두는 것을 권장합니다.

---

## ❓ 자주 묻는 질문 (FAQ)

<details>
<summary><b>Q1: MT5 전략 테스터에서 무료로 백테스팅이 가능한가요?</b></summary>
<br>
<b>네, 100% 완전 무료입니다!</b> Rango는 전략 테스터 자동 바이패스 기능을 내장하고 있어 라이선스 키 없이도 무제한 백테스트, 파라미터 최적화, 비주얼 모드 시뮬레이션을 실행할 수 있습니다.
</details>

<details>
<summary><b>Q2: 왜 MT5에서 "DLL 가져오기를 허용해야 합니다"라는 메시지가 나오나요?</b></summary>
<br>
Rango는 Windows 표준 시스템 API (`kernel32.dll`)를 호출하여 하드웨어 고유 번호를 인식하고 안전한 라이선스를 확인합니다. <code>도구 ➔ 옵션 ➔ Expert Advisors</code>에서 <b>"DLL 가져오기 허용"</b>에 체크해 주세요.
</details>

<details>
<summary><b>Q3: 실계좌 활성화를 위한 머신 시리얼 번호는 어디서 찾을 수 있나요?</b></summary>
<br>
차트에 Rango를 적용한 후 다음 파일을 확인하세요:
<code>MetaTrader 5 ➔ MQL5 ➔ Files ➔ Rango_Request_Serial.txt</code>.
또한 MT5의 <b>전문가(Experts) 로그 탭</b>에도 시리얼이 바로 출력됩니다.
</details>

<details>
<summary><b>Q4: VPS(가상 서버)에서 안정적으로 구동되나요?</b></summary>
<br>
<b>매우 안정적입니다!</b> Rango는 초저전력 및 저CPU 점유율로 최적화되어 있어 모든 Windows VPS에서 24시간 365일 중단 없이 매끄럽게 작동합니다.
</details>

<details>
<summary><b>Q5: 스팸성 알림 없이 중요한 매매 신호만 스마트폰으로 받으려면 어떻게 하나요?</b></summary>
<br>
모바일 MT5 앱에서 <b>MetaQuotes ID</b>를 확인하여 PC MT5의 <code>도구 ➔ 옵션 ➔ 알림</code>에 입력하고 <b>"푸시 알림 사용"</b>을 체크합니다.
<br><br>
⚠️ <b>스팸 차단 팁:</b> <i>"로컬 터미널의 알림"</i>과 <i>"거래 서버의 알림"</i> <b>체크를 반드시 해제</b>하세요. 그러면 증권사의 단순 주문 체결 알림 없이 Rango의 핵심 퀀트 신호만 깨끗하게 수신할 수 있습니다!
</details>

---

## 💬 기술 지원 및 커뮤니티

- ✈️ **텔레그램 고객지원 및 라이선스 발급**: [@trading_world_support](https://t.me/trading_world_support)
- 📺 **YouTube 공식 채널**: 튜토리얼 영상, 파라미터 최적화 가이드 및 라이브 세션 시청.
- 🐛 **이슈 트래커**: 버그 제보나 기능 개선 아이디어는 본 GitHub 저장소의 Issue 탭에 남겨주세요.

---

## ⚠️ 투자 위험 고지

외환(Forex), 차액결제거래(CFD), 귀금속 및 암호화폐 거래는 높은 수준의 투자 위험이 수반되며 모든 투자자에게 적합하지 않을 수 있습니다. 높은 레버리지는 이익을 증대시킬 수도 있지만 손실을 가속화할 수도 있습니다. 자동매매 시스템을 도입하기 전 투자 목적, 경험 수준 및 위험 감수 능력을 면밀히 검토하시기 바랍니다.

과거의 성과가 미래의 수익을 보장하지 않습니다. 본 저장소에서 제공되는 소프트웨어와 자료는 교육 및 연구 정보 제공 목적으로 제작되었습니다. 실계좌에서의 모든 매매 판단과 최종 금융 손익에 대한 책임은 전적으로 본인에게 있습니다.

---

<div align="center">
  <sub>Built with ❤️ by World Trading Lab. All Rights Reserved.</sub>
</div>
