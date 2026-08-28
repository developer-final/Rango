# 🎯 RANGO EA — Sistema de Trading Quantitativo DCA Adaptativo ao Regime de Mercado para MT5

[![Platform](https://img.shields.io/badge/Plataforma-MetaTrader%205-blue.svg)](https://www.metatrader5.com/)
[![Language](https://img.shields.io/badge/Linguagem-MQL5-orange.svg)]()
[![Strategy](https://img.shields.io/badge/Estrat%C3%A9gia-Regime--Adaptive%20DCA%20%7C%20Mean%20Reversion-green.svg)]()
[![Backtest](https://img.shields.io/badge/Testador%20de%20Estrat%C3%A9gias-100%25%20Simula%C3%A7%C3%A3o%20Gratuita-brightgreen.svg)]()
[![Telegram Support](https://img.shields.io/badge/Suporte%20Telegram-@trading__world__support-blue.svg)](https://t.me/trading_world_support)

> **Rango EA** é um sistema avançado de trading quantitativo de nível institucional desenvolvido para o **MetaTrader 5 (MT5)**. Ele substitui o tradicional preço médio cego (DCA) e as arriscadas grades Martingale por um mecanismo inteligente **Regime-Dependent Permission Engine**, pontuação de perigo de tendência **Trend Danger Scoring (TDS)** e pontuação de exaustão estatística **Statistical Exhaustion Scoring (ES)**.

---

## 📌 Índice

- [Visão Geral](#-visão-geral)
- [Por que o Rango é Diferente (O DCA Anti-Quebra de Conta)](#-por-que-o-rango-é-diferente-o-dca-anti-quebra-de-conta)
- [Principais Funcionalidades](#-principais-funcionalidades)
- [Arquitetura do Sistema e Modelos Quantitativos](#-arquitetura-do-sistema-e-modelos-quantitativos)
- [Guia de Início Rápido e Instalação](#-guia-de-início-rápido-e-instalação)
- [Como Ativar (Backtesting Gratuito e Configuração Real)](#-como-ativar-backtesting-gratuito-e-configuração-real)
- [Configuração de Notificações Push no Celular (App MT5 e MetaQuotes ID)](#-configuração-de-notificações-push-no-celular-app-mt5-e-metaquotes-id)
- [Referência de Parâmetros de Entrada (Input Parameters)](#-referência-de-parâmetros-de-entrada-input-parameters)
- [Configuração de Negociação Recomendada](#-configuração-de-negociação-recomendada)
- [Perguntas Frequentes (FAQ)](#-perguntas-frequentes-faq)
- [Suporte e Comunidade](#-suporte-e-comunidade)
- [Aviso de Risco](#-aviso-de-risco)

---

## 💡 Visão Geral

Sistemas convencionais de grade e Martingale inevitavelmente falham durante fortes explosões de tendência unidirecionais. Eles abrem posições cegamente contra um forte momentum adverso até a conta sofrer uma chamada de margem (Margin Call).

**O Rango EA foi desenvolvido para solucionar exatamente esse problema.**

Em vez de abrir ordens a intervalos fixos de pips, o Rango utiliza um **Portal de Permissão Quantitativo Multifatorial (Multi-Factor Quantitative Permission Gate)**:
1. Monitora continuamente a persistência do regime de mercado através do **Expoente de Hurst**, **ADX** e **Rácios de Expansão ATR**.
2. Se o mercado estiver em forte tendência desfavorável, **a expansão de ordens DCA é estritamente bloqueada**.
3. Sinais e entradas só são autorizados quando métricas quantitativas confirmam que o momentum adverso se esgotou e a **probabilidade de reversão à média (Mean Reversion) está matematicamente elevada**.

```
                        ┌─────────────────────────────────┐
                        │    Preço de Mercado e Volatil.  │
                        └────────────────┬────────────────┘
                                         │
                 ┌───────────────────────┴───────────────────────┐
                 ▼                                               ▼
   ┌───────────────────────────┐                   ┌───────────────────────────┐
   │ Pontuação de Perigo (TDS) │                   │Pontuação de Exaustão (ES) │
   │  - Força de Tendência ADX │                   │  - Distância da EMA 200   │
   │  - Memória Hurst (H>0.55) │                   │  - Extremos RSI e Bandas  │
   │  - Aceleração de Volat.   │                   │  - Pinbars de desaceleração│
   └─────────────┬─────────────┘                   └─────────────┬─────────────┘
                 │                                               │
                 └───────────────────────┬───────────────────────┘
                                         ▼
                        ┌─────────────────────────────────┐
                        │Pontuação de Prontidão DCA (DRS) │
                        └────────────────┬────────────────┘
                                         │
        ┌────────────────────────────────┼────────────────────────────────┐
        ▼                                ▼                                ▼
┌───────────────┐                ┌───────────────┐                ┌───────────────┐
│  DCA BLOQUEADO│                │  DCA EM ESPERA│                │   DCA PRONTO  │
│Pausar Entradas│                │Aguardar Gatilho│               │Executar Ordem │
└───────────────┘                └───────────────┘                └───────────────┘
```

---

## ⚡ Por que o Rango é Diferente (O DCA Anti-Quebra de Conta)

| Recurso | DCA Tradicional / Martingale | **Rango EA** |
| :--- | :--- | :--- |
| **Gatilho de Médias** | Distância fixa em pips ou tempo | **Pontuação Quantitativa de Prontidão DCA (DRS)** |
| **Proteção de Tendência**| Nenhuma (acumula ordens no crash) | **Portal Anti-Expansão e Bloqueio de Perigo de Tendência** |
| **Precisão de Entrada** | Cega / Pips Fixos | **Exaustão Quantitativa e Confirmação Estatística** |
| **Isolamento Direcional**| Abre Buy/Sell desordenadamente | **Isolamento Direcional (Sem DCA contra a tendência)** |
| **Limite de Risco** | Risco ilimitado de quebra | **Gestão Estrita de Risco e Stop Dinâmico via ATR** |
| **Telemetria Visual** | Básica ou inexistente | **Painel HUD em Tempo Real e Setas Inteligentes** |
| **Testador de Estratégias** | Geralmente limitado ou pago | **100% Gratuito, Backtesting Ilimitado** |

---

## 🚀 Principais Funcionalidades

### 1. 🛡️ Motor de Permissão DCA e Regime de Mercado
- **Trend Danger Score (TDS: 0–100)**: Avalia a força da tendência, persistência de Hurst e aceleração de volatilidade. Se $TDS > 65$, novas entradas de DCA são congeladas.
- **Exhaustion Score (ES: 0–100)**: Quantifica o estiramento estatístico em relação à EMA 200, extremos de sobrecompra/sobrevenda do RSI e pinbars de rejeição de preço.
- **Portal Anti-Expansão**: Bloqueia instantaneamente o DCA caso o ATR de curto prazo supere fortemente o ATR de longo prazo ($ATR_5 / ATR_{30} > 1.35$) durante picos de notícias ou rompimentos.
- **Kill Switch de Emergência DCA**: Paralisação automática de segurança se as condições adversas excederem os limites críticos ($TDS > 85$).

### 2. 🎯 Isolamento Direcional de DCA
- Em **Tendência de Alta (Uptrend)** confirmada: O DCA de Compra é bloqueado e apenas a reversão à média de Venda é avaliada no topo de exaustão.
- Em **Tendência de Baixa (Downtrend)** confirmada: O DCA de Venda é bloqueado e apenas a reversão à média de Compra é avaliada no fundo de exaustão.
- Sinais bidirecionais são permitidos exclusivamente em mercados de **Lateralização / Range**.

### 3. 🛡️ Gestão Profissional de Risco e Dimensionamento Flexível
- **Múltiplos Modos de Dimensionamento**: Volume fixo (Lotes), valor financeiro em risco ($), ou percentual da conta (%).
- **Projeção Automática de SL/TP**: Cálculos dinâmicos com base no ATR do timeframe superior e relação Risco:Retorno ajustável.
- **Proteções de Execução**: Filtro de spread, validação de distância mínima de stops da corretora e verificação de margem livre antes de executar ordens.

### 4. 📊 Painel HUD Profissional no Gráfico
- Interface escura de alto contraste exibindo:
  - Estado atual do Regime de Mercado e Volatilidade.
  - Valores ao vivo de **TDS** (Perigo de Tendência) e **DRS** (Prontidão DCA).
  - Estatísticas de assertividade, estado dos pullbacks e métricas de lucro em tempo real.

### 5. 🏹 Setas de Sinal Inteligentes e Notificações Push
- Setas exibidas apenas em **transições de estado**, com intervalo de espera (cooldown) e limpeza automática de sinais antigos.
- **Notificações Push Diretas no MT5 Mobile**: Envio em tempo real de sinais de negociação e alertas DCA READY para seu smartphone via MetaQuotes ID.

### 6. 🧪 Testador de Estratégias MT5 Sem Restrições
- Execute backtests, testes de estresse e otimizações sobre dados históricos em qualquer corretora e timeframe **sem qualquer limitação**.

---

## 🛠️ Guia de Início Rápido e Instalação

### Requisitos Prévios
- **Plataforma**: MetaTrader 5 (Build 3800 ou superior recomendada)
- **Sistema Operacional**: Windows 10 / 11 ou Windows Server (VPS)
- **Tipo de Conta**: Conta Hedging recomendada (Standard ou ECN com spread baixo)

---

### Instalação Passo a Passo

1. **Baixar o `Rango.ex5`**:
   - Baixe a versão compilada `Rango.ex5` da aba [Releases](../../releases) ou da raiz do repositório.

2. **Copiar para a Pasta do MT5**:
   - Abra o terminal MetaTrader 5.
   - Clique em `Arquivo (File)` ➔ `Abrir Pasta de Dados (Open Data Folder)`.
   - Navegue até `MQL5` ➔ `Experts\`.
   - Cole o arquivo `Rango.ex5` nesta pasta.

3. **Habilitar Negociação Automatizada e DLLs**:
   - No MT5, vá em `Ferramentas (Tools)` ➔ `Opções (Options)` (ou `Ctrl + O`).
   - Selecione a aba **Expert Advisors**.
   - Marque ✅ **Permitir negociação algorítmica (Allow algorithmic trading)**.
   - Marque ✅ **Permitir importação de DLL (Allow DLL imports)** *(Necessário para validação de hardware e licença)*.
   - Marque ✅ **Permitir WebRequest para as URLs listadas** e adicione `https://api.telegram.org` *(caso utilize notificações do Telegram)*.

4. **Arrastar o Rango para o Gráfico**:
   - Na janela **Navegador** (`Ctrl + N`), expanda **Expert Advisors**.
   - Arraste o **Rango** para o gráfico desejado (ex.: `XAUUSD`, `EURUSD`, `GBPUSD`, `BTCUSD`).
   - Timeframe recomendado: **M5** ou **M15**.
   - Certifique-se de que **Permitir negociação ao vivo** esteja marcado, configure seus parâmetros e clique em **OK**.

---

## 🔑 Como Ativar (Backtesting Gratuito e Configuração Real)

O Rango oferece duas modalidades de operação:

### 1. 🧪 100% Gratuito no Testador de Estratégias (Simulação e Backtest)
- **Não requer chave de licença.**
- Abra o Testador de Estratégias do MT5 (`Ctrl + R`), selecione `Rango.ex5`, defina o ativo e o período e clique em **Iniciar**.
- Aproveite backtests com todos os recursos liberados!

### 2. 💻 Ativação em Gráfico Real / Demo
Ao inserir o Rango em um gráfico real ou demo pela primeira vez:
1. O Rango inicia no **Modo Demo Apenas Visualização (View-Only Demo Mode)** (o painel e os cálculos operam normalmente).
2. Ele gera automaticamente o número serial único da sua máquina no arquivo:
   ```
   MQL5\Files\Rango_Request_Serial.txt
   ```
3. Copie o seu serial (formato: `RANGO-REQ-XXXX-XXXX-XXXX-XXXX`).
4. Envie o serial para o suporte no Telegram: **[@trading_world_support](https://t.me/trading_world_support)** para receber sua **Chave de Ativação** (`RANGO-ACT-...`).
5. Cole a chave no parâmetro `InpLicenseKey` (ficará armazenada em cache).

---

## 📲 Configuração de Notificações Push no Celular (App MT5 e MetaQuotes ID)

O Rango EA suporta o envio instantâneo de alertas push para o aplicativo **MetaTrader 5 Mobile** (iOS e Android) através da função nativa do MQL5 `SendNotification`, sem necessidade de robôs externos.

### Passo 1: Obter seu MetaQuotes ID no Celular
* **Android**: Abra o app MT5 ➔ Menu (☰) ➔ **Mensagens** (ou **Configurações** ➔ **Mensagens**) ➔ Anote o seu **MetaQuotes ID** (código de 8 dígitos alfanuméricos, ex.: `1A2B3C4D`).
* **iOS (iPhone/iPad)**: Abra o app MT5 ➔ **Configurações** ➔ **Chat e Mensagens** ➔ Localize **Meu MetaQuotes ID** na parte inferior.

### Passo 2: Configurar Notificações no MT5 Desktop
1. No MT5 do computador, vá em `Ferramentas` ➔ `Opções` (`Ctrl + O`).
2. Acesse a aba **Notificações (Notifications)**.
3. Marque ✅ **Ativar notificações Push**.
4. ⚠️ **IMPORTANTE (Configuração Anti-Spam)**:
   * **DESMARQUE** ❌ **Notificações do terminal local**
   * **DESMARQUE** ❌ **Notificações do servidor de negociação**
   > 💡 **Por que desmarcar essas opções?** Se mantidas ativas, a corretora enviará notificações para cada pequena alteração ou ordem pendente. Desmarcando essas duas opções, você **receberá apenas** os sinais estratégicos de alta relevância do Rango sem spam!
5. Insira seu **MetaQuotes ID** no campo indicado (separe múltiplos IDs com vírgulas).
6. Clique no botão **Testar**. Você receberá uma notificação de teste imediatamente no celular!
7. Clique em **OK** para salvar.

### Passo 3: Configurar Notificações no Rango EA
Nas configurações de parâmetros do EA:
* `EnableNotifications = true` — Ativar notificações push.
* `InpNotifyDCASignal = true` — Receber alertas em tempo real quando o estado passar para **DCA READY**.
* `InpNotifyTrendSignal = true` — Receber alertas em caso de sinal confirmado de pullback na tendência.
* `InpMetaQuotesID = "..."` — Inserir o seu MetaQuotes ID.

---

## ⚙️ Referência de Parâmetros de Entrada (Input Parameters)

### Configurações Gerais (General Settings)
| Parâmetro | Padrão | Descrição |
| :--- | :--- | :--- |
| `InpLicenseKey` | `""` | Chave de ativação fornecida pelo suporte (deixar em branco após validação). |
| `InputMagicNumber` | `0` | Identificador único Magic Number (`0` = Atribuição automática por hash do símbolo). |
| `Language` | `LANG_ENGLISH` | Idioma do painel e alertas (`LANG_ENGLISH` ou `LANG_VIETNAMESE`). |
| `InpShowDashboard` | `true` | Exibir painel HUD em tempo real no gráfico. |
| `SinglePositionMode` | `true` | Restringir o robô a um único ciclo de estratégia por vez. |
| `ATR_Multiplier` | `1.0` | Multiplicador global de ATR para cálculos de distância dinâmica. |
| `MaxSpreadPoints` | `0` | Filtro de spread máximo permitido em pontos (`0` = Desativado). |

### Gestão de Risco (Risk Management)
| Parâmetro | Padrão | Descrição |
| :--- | :--- | :--- |
| `RiskMode` | `RISK_BY_VOLUME` | Modo de cálculo de risco (`RISK_BY_PERCENT`, `RISK_BY_AMOUNT`, `RISK_BY_VOLUME`). |
| `RiskDefaulMinimum` | `0.01` | Lote base ou porcentagem de risco. |
| `RiskRewardRatio` | `1.0` | Relação Risco:Retorno padrão para ordens individuais. |
| `MaxVolumeFixed` | `0` | Limite máximo de lote fixo por ordem (`0` = Ilimitado). |

### Configurações do Motor de Permissão DCA (DCA Permission Engine Settings)
| Parâmetro | Padrão | Descrição |
| :--- | :--- | :--- |
| `InpEnableDCAPermission` | `true` | Chave mestra do portal de permissão DCA. |
| `InpMinDRS_Ready` | `65.0` | Pontuação mínima de prontidão DRS para autorizar DCA ($0–100$). |
| `InpMaxTDS_Allowed` | `65.0` | Pontuação máxima de perigo de tendência TDS antes do bloqueio ($0–100$). |
| `InpEnableAntiExpansion` | `true` | Congelar DCA diante de picos súbitos de ATR ($ATR_5/ATR_{30} > 1.35$). |
| `InpEnableDCAKillSwitch` | `true` | Parada de emergência para novas ordens se $TDS > 85$. |
| `InpShowDCAArrows` | `true` | Exibir setas de autorização de DCA no gráfico. |
| `InpDCAArrowCooldownBars` | `3` | Espaçamento mínimo em barras entre setas consecutivas. |

### Notificações Push e Alertas
| Parâmetro | Padrão | Descrição |
| :--- | :--- | :--- |
| `EnableNotifications` | `true` | Chave geral de notificações. |
| `InpNotifyDCASignal` | `true` | Enviar alerta móvel quando o estado DCA mudar para READY. |
| `InpNotifyTrendSignal` | `false` | Enviar alerta móvel em sinais de pullback na tendência. |
| `InpMetaQuotesID` | `""` | MetaQuotes ID para notificações diretas no app MT5. |

---

## 📈 Configuração de Negociação Recomendada

| Classe de Ativos | Símbolos Recomendados | Timeframe Ideal | Saldo Mínimo (0.01 lote) | Alavancagem Recomendada |
| :--- | :--- | :--- | :--- | :--- |
| **Metais Preciosos** | `XAUUSD` (Ouro) | `M1` / `M5` | $10,000+ | 1:2000 |
| **Forex Principal** | `EURUSD`, `GBPUSD`, `USDJPY` | `M5` / `M15` | $10,000+ | 1:2000 |
| **Criptomoedas** | `BTCUSD`, `ETHUSD` | `M5` / `M15` | $10,000+ | 1:2000 |

> 💡 **Dica Profissional**: Para máxima estabilidade durante eventos de alto impacto econômico (CPI, Payroll, taxas de juros), mantenha `InpEnableAntiExpansion = true` e `EnableNewsAlert = true`.

---

## ❓ Perguntas Frequentes (FAQ)

<details>
<summary><b>Q1: Posso fazer backtest do Rango no Testador de Estratégias do MT5 gratuitamente?</b></summary>
<br>
<b>Sim, 100% gratuito!</b> O Rango conta com um bypass nativo para o testador de estratégias, permitindo testes históricos, otimização de parâmetros e simulações em modo visual sem exigir chave de ativação.
</details>

<details>
<summary><b>Q2: Por que o MT5 exibe "A importação de DLL deve estar ativada"?</b></summary>
<br>
O Rango utiliza a API padrão do Windows (`kernel32.dll`) para leitura do serial de hardware para fins de licenciamento. Marque <b>"Permitir importação de DLL"</b> em <code>Ferramentas ➔ Opções ➔ Expert Advisors</code>.
</details>

<details>
<summary><b>Q3: Onde encontro o serial da minha máquina para ativação ao vivo?</b></summary>
<br>
Ao carregar o Rango em qualquer gráfico, verifique o arquivo:
<code>MetaTrader 5 ➔ MQL5 ➔ Files ➔ Rango_Request_Serial.txt</code>.
O serial também é impresso na aba <b>Diário / Experts Log</b> do MT5.
</details>

<details>
<summary><b>Q4: O Rango funciona em servidor VPS?</b></summary>
<br>
<b>Sim!</b> O Rango foi otimizado para baixíssimo consumo de CPU e opera ininterruptamente 24/7 em qualquer VPS com Windows.
</details>

<details>
<summary><b>Q5: Como receber alertas no celular sem mensagens indesejadas?</b></summary>
<br>
Localize seu <b>MetaQuotes ID</b> no app MT5 Mobile, insira-o no MT5 desktop em <code>Ferramentas ➔ Opções ➔ Notificações</code> e marque <b>"Ativar notificações Push"</b>.
<br><br>
⚠️ <b>Dica Anti-Spam:</b> Certifique-se de <b>DESMARCAR</b> <i>"Notificações do terminal local"</i> e <i>"Notificações do servidor de negociação"</i> para receber apenas as análises quantitativas do Rango!
</details>

---

## 💬 Suporte e Comunidade

- ✈️ **Suporte Telegram e Ativação de Licenças**: [@trading_world_support](https://t.me/trading_world_support)
- 📺 **Canal no YouTube**: Vídeos tutoriais, guias de otimização de parâmetros e operações ao vivo.
- 🐛 **Rastreador de Problemas**: Para relatar bugs ou sugerir melhorias, abra uma Issue neste repositório GitHub.

---

## ⚠️ Aviso de Risco

A negociação de Forex, contratos por diferença (CFDs), metais e criptomoedas envolve alto risco e pode não ser adequada para todos os investidores. O alto nível de alavancagem pode operar tanto a seu favor quanto contra você. Antes de operar ou utilizar robôs de investimento, avalie seus objetivos, nível de experiência e tolerância a riscos.

Desempenhos passados não são garantia de rentabilidade futura. O software e os materiais fornecidos neste repositório têm caráter estritamente educacional. Você é o único responsável por suas decisões financeiras em contas reais.

---

<div align="center">
  <sub>Desenvolvido com ❤️ por World Trading Lab. Todos os direitos reservados.</sub>
</div>
