# 🎯 RANGO EA — Sistema de Trading Cuantitativo DCA Adaptativo al Régimen de Mercado para MT5

[![Platform](https://img.shields.io/badge/Plataforma-MetaTrader%205-blue.svg)](https://www.metatrader5.com/)
[![Language](https://img.shields.io/badge/Lenguaje-MQL5-orange.svg)]()
[![Strategy](https://img.shields.io/badge/Estrategia-Regime--Adaptive%20DCA%20%7C%20Mean%20Reversion-green.svg)]()
[![Backtest](https://img.shields.io/badge/Probador%20de%20Estrategias-100%25%20Simulaci%C3%B3n%20Gratuita-brightgreen.svg)]()
[![Telegram Support](https://img.shields.io/badge/Soporte%20Telegram-@trading__world__support-blue.svg)](https://t.me/trading_world_support)

> **Rango EA** es un sistema de trading cuantitativo avanzado de nivel institucional para **MetaTrader 5 (MT5)**. Reemplaza el promedio a la baja (DCA) ciego tradicional y las peligrosas cuadrículas Martingala por un motor inteligente **Regime-Dependent Permission Engine**, puntuación de peligro de tendencia **Trend Danger Scoring (TDS)** y puntuación de agotamiento estadístico **Statistical Exhaustion Scoring (ES)**.

---

## 📌 Tabla de Contenidos

- [Descripción General](#-descripción-general)
- [Por qué Rango es Diferente (El DCA Anti-Quiebre de Cuenta)](#-por-qué-rango-es-diferente-el-dca-anti-quiebre-de-cuenta)
- [Características Principales](#-características-principales)
- [Arquitectura del Sistema y Modelos Cuantitativos](#-arquitectura-del-sistema-y-modelos-cuantitativos)
- [Guía de Inicio Rápido e Instalación](#-guía-de-inicio-rápido-e-instalación)
- [Cómo Activar (Backtesting Gratuito y Configuración Real)](#-cómo-activar-backtesting-gratuito-y-configuración-real)
- [Configuración de Notificaciones Push Móviles (App MT5 y MetaQuotes ID)](#-configuración-de-notificaciones-push-móviles-app-mt5-y-metaquotes-id)
- [Referencia de Parámetros de Entrada (Input Parameters)](#-referencia-de-parámetros-de-entrada-input-parameters)
- [Configuración de Trading Recomendada](#-configuración-de-trading-recomendada)
- [Preguntas Frecuentes (FAQ)](#-preguntas-frecuentes-faq)
- [Soporte y Comunidad](#-soporte-y-comunidad)
- [Descargo de Responsabilidad de Riesgo](#-descargo-de-responsabilidad-de-riesgo)

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

## 💡 Descripción General

Los sistemas de cuadrícula y Martingala convencionales fracasan inevitablemente durante explosiones de tendencia unidireccionales prolongadas. Acumulan órdenes ciegamente contra un fuerte impulso adverso hasta que la cuenta sufre una llamada de margen (Margin Call).

**Rango EA fue desarrollado desde cero para resolver exactamente este problema.**

En lugar de abrir órdenes a distancias fijas en pips, Rango incorpora una **Puerta de Autorización Cuantitativa Multifactorial (Multi-Factor Quantitative Permission Gate)**:
1. Monitorea continuamente la persistencia del régimen de mercado utilizando el **Exponente de Hurst**, **ADX** y **Ratios de Expansión ATR**.
2. Si el mercado muestra una fuerte tendencia en su contra, **la expansión de órdenes DCA queda estrictamente bloqueada**.
3. Las señales y entradas solo se permiten cuando las métricas cuantitativas confirman que el impulso tendencial adverso se ha agotado y la **probabilidad de reversión a la media (Mean Reversion) es matemáticamente elevada**.

```
                        ┌─────────────────────────────────┐
                        │    Precio de Mercado y Volat.   │
                        └────────────────┬────────────────┘
                                         │
                 ┌───────────────────────┴───────────────────────┐
                 ▼                                               ▼
   ┌───────────────────────────┐                   ┌───────────────────────────┐
   │ Puntuación Peligro (TDS)  │                   │ Puntuación Agotamiento(ES)│
   │  - Fuerza de tendencia ADX│                   │  - Distancia a la EMA 200 │
   │  - Memoria Hurst (H>0.55) │                   │  - Extremos RSI y Bandas  │
   │  - Aceleración de Volatil.│                   │  - Pinbars de agotamiento │
   └─────────────┬─────────────┘                   └─────────────┬─────────────┘
                 │                                               │
                 └───────────────────────┬───────────────────────┘
                                         ▼
                        ┌─────────────────────────────────┐
                        │ Puntuación Preparación DCA(DRS) │
                        └────────────────┬────────────────┘
                                         │
        ┌────────────────────────────────┼────────────────────────────────┐
        ▼                                ▼                                ▼
┌───────────────┐                ┌───────────────┐                ┌───────────────┐
│  DCA BLOQUEADO│                │  DCA EN ESPERA│                │  DCA LISTO    │
│Congelar Órdenes│               │ Esperar Gatillo│               │Ejecutar Señal │
└───────────────┘                └───────────────┘                └───────────────┘
```

---

## ⚡ Por qué Rango es Diferente (El DCA Anti-Quiebre de Cuenta)

| Característica | DCA Convencional / Martingala | **Rango EA** |
| :--- | :--- | :--- |
| **Gatillo de Promediación** | Pips fijos o temporizador | **Puntuación Cuantitativa de Preparación DCA (DRS)** |
| **Protección contra Tendencia** | Ninguna (sigue promediando en el crash)| **Puerta Anti-Expansión y Bloqueo de Peligro de Tendencia** |
| **Precisión Temporal** | Ciega / Pips Fijos | **Agotamiento Cuantitativo y Confirmación de Reversión** |
| **Aislamiento Direccional** | Abre Buy/Sell ciegamente | **Aislamiento Direccional (Sin contra-tendencia perjudicial)**|
| **Límite de Riesgo** | Riesgo de liquidación total | **Gestión Estricta de Riesgo y Stop Dinámico por ATR** |
| **Telemetría Visual** | Básica o inexistente | **Panel HUD en Tiempo Real y Flechas Inteligentes** |
| **Probador de Estrategias** | A menudo restringido | **100% Gratuito, Backtesting Ilimitado** |

---

## 🚀 Características Principales

### 1. 🛡️ Motor de Permisos DCA y Régimen de Mercado
- **Trend Danger Score (TDS: 0–100)**: Evalúa la fuerza de la tendencia, la persistencia de Hurst y la aceleración de volatilidad. Si $TDS > 65$, las nuevas entradas DCA se congelan.
- **Exhaustion Score (ES: 0–100)**: Cuantifica la extensión estadística respecto a la EMA 200, extremos de sobrecompra/sobreventa de RSI y desaceleración de velas.
- **Puerta Anti-Expansión**: Bloquea inmediatamente el promediado si el ATR a corto plazo supera con fuerza al ATR a largo plazo ($ATR_5 / ATR_{30} > 1.35$) en noticias o rupturas.
- **Kill Switch de Emergencia DCA**: Cierre de seguridad automático si las condiciones adversas superan los límites críticos ($TDS > 85$).

### 2. 🎯 Aislamiento Direccional de DCA
- En **Tendencia Alcista (Uptrend)** confirmada: Se bloquea el DCA de Compra y solo se evalúa la reversión a la media de Venta en el techo de agotamiento.
- En **Tendencia Bajista (Downtrend)** confirmada: Se bloquea el DCA de Venta y solo se evalúa la reversión a la media de Compra en el suelo de agotamiento.
- Las señales bidireccionales solo se permiten en mercados en **Rango / Lateral**.

### 3. 🛡️ Gestión Profesional de Riesgo y Dimensionamiento Flexible
- **Modos de Dimensionamiento Múltiples**: Por volumen fijo (Lots), importe monetario de riesgo ($), o porcentaje de la cuenta (%).
- **Proyección Automática SL/TP**: Distancias dinámicas calculadas según el ATR superior y relación Riesgo:Beneficio configurable.
- **Garantías de Ejecución**: Filtro de spread, validación de distancia mínima de stops del broker y comprobación de margen antes de operar.

### 4. 📊 Panel HUD Profesional en Gráfico
- Interfaz clara de alto contraste que muestra:
  - Estado del Régimen de Mercado y Volatilidad actual.
  - Valores en vivo de **TDS** (Peligro de Tendencia) y **DRS** (Preparación DCA).
  - Estadísticas de acierto, estado de retrocesos y métricas de beneficio.

### 5. 🏹 Flechas Inteligentes y Notificaciones Push
- Marcadores de flecha trazados únicamente en **transiciones de estado**, con enfriamiento y limpieza automática de señales antiguas.
- **Notificaciones Push Móviles Directas en MT5**: Señales de trading y alertas DCA READY en tiempo real enviadas a su teléfono mediante MetaQuotes ID.

### 6. 🧪 Probador de Estrategias MT5 sin Restricciones
- Realice backtests, pruebas de estrés y optimizaciones sobre datos históricos en cualquier broker y temporalidad **sin límites**.

---

## 🛠️ Guía de Inicio Rápido e Instalación

### Requisitos Previos
- **Plataforma**: MetaTrader 5 (Build 3800 o superior recomendado)
- **Sistema Operativo**: Windows 10 / 11 o Windows Server (VPS)
- **Tipo de Cuenta**: Cuenta Hedging recomendada (Standard o ECN con bajo spread)

---

### Instalación Paso a Paso

1. **Descargar `Rango.ex5`**:
   - Descargue la versión compilada de `Rango.ex5` desde la pestaña [Releases](../../releases) o la raíz del repositorio.

2. **Copiar a la Carpeta de MT5**:
   - Abra su terminal MetaTrader 5.
   - Haga clic en `Archivo (File)` ➔ `Abrir carpeta de datos (Open Data Folder)`.
   - Vaya a `MQL5` ➔ `Experts\`.
   - Pegue el archivo `Rango.ex5` en esta carpeta.

3. **Habilitar Trading Algorítmico e Importación de DLL**:
   - En MT5, vaya a `Herramientas (Tools)` ➔ `Opciones (Options)` (o presione `Ctrl + O`).
   - Seleccione la pestaña **Asesores Expertos (Expert Advisors)**.
   - Marque ✅ **Permitir trading algorítmico**.
   - Marque ✅ **Permitir importación de DLL** *(Requerido para la validación de hardware y licencias)*.
   - Marque ✅ **Permitir WebRequest para las URL listadas** y añada `https://api.telegram.org` *(si usa Telegram)*.

4. **Adjuntar Rango al Gráfico**:
   - En la ventana **Navegador** (`Ctrl + N`), expanda **Asesores Expertos**.
   - Arrastre **Rango** al gráfico deseado (ej. `XAUUSD`, `EURUSD`, `GBPUSD`, `BTCUSD`).
   - Temporalidad recomendada: **M5** o **M15**.
   - Asegúrese de que **Permitir trading en vivo** esté marcado, configure sus parámetros y haga clic en **Aceptar**.

---

## 🔑 Cómo Activar (Backtesting Gratuito y Configuración Real)

Rango ofrece dos modalidades:

### 1. 🧪 100% Gratuito en el Probador de Estrategias (Simulaciones)
- **No requiere clave de licencia.**
- Abra el Probador de Estrategias MT5 (`Ctrl + R`), seleccione `Rango.ex5`, elija su símbolo y fechas, y pulse **Iniciar**.
- ¡Backtesting completo con todas las funciones disponibles!

### 2. 💻 Activación en Gráfico Real / Demo
Al colocar Rango en un gráfico real o demo por primera vez:
1. Rango inicia en **Modo Demo de Solo Lectura (View-Only Demo Mode)** (el panel y los cálculos funcionan).
2. Genera automáticamente el número de serie único de su equipo en el archivo:
   ```
   MQL5\Files\Rango_Request_Serial.txt
   ```
3. Copie su número de serie (formato: `RANGO-REQ-XXXX-XXXX-XXXX-XXXX`).
4. Envíelo al soporte de Telegram: **[@trading_world_support](https://t.me/trading_world_support)** para recibir su **Clave de Activación** (`RANGO-ACT-...`).
5. Pegue la clave en el parámetro `InpLicenseKey` (se guardará en caché automáticamente).

---

## 📲 Configuración de Notificaciones Push Móviles (App MT5 y MetaQuotes ID)

Rango EA permite el envío directo de notificaciones push a la **App Móvil MetaTrader 5** (iOS y Android) usando la función nativa MQL5 `SendNotification`, asegurando alertas inmediatas sin depender de bots externos.

### Paso 1: Obtener su MetaQuotes ID en el Móvil
* **Android**: Abra MT5 ➔ Menú (☰) ➔ **Mensajes** (o **Ajustes** ➔ **Mensajes**) ➔ Anote su **MetaQuotes ID** (código de 8 caracteres alfanuméricos, ej. `1A2B3C4D`).
* **iOS (iPhone/iPad)**: Abra MT5 ➔ **Ajustes** ➔ **Chat y Mensajes** ➔ Revise **Mi MetaQuotes ID** en la parte inferior.

### Paso 2: Configurar Notificaciones en MT5 de Escritorio
1. En su MT5 de PC, abra `Herramientas` ➔ `Opciones` (`Ctrl + O`).
2. Seleccione la pestaña **Notificaciones (Notifications)**.
3. Marque ✅ **Permitir notificaciones Push**.
4. ⚠️ **IMPORTANTE (Configuración Anti-Spam)**:
   * **DESMARQUE** ❌ **Notificaciones del terminal local**
   * **DESMARQUE** ❌ **Notificaciones del servidor de trading**
   > 💡 **¿Por qué desmarcar estas casillas?** Si están activadas, el servidor del broker enviará una notificación por cada orden colocada o modificada. Al desmarcarlas, se asegura de **recibir únicamente** las señales analíticas de alta prioridad de Rango sin saturación.
5. Ingrese su **MetaQuotes ID** en el campo correspondiente (separe varios con comas).
6. Haga clic en **Probar**. ¡Recibirá una notificación de prueba en su teléfono al instante!
7. Haga clic en **Aceptar** para guardar.

### Paso 3: Configurar Parámetros de Notificación en Rango EA
En la configuración de entradas del EA:
* `EnableNotifications = true` — Activar notificaciones push.
* `InpNotifyDCASignal = true` — Recibir alertas en tiempo real cuando el estado pase a **DCA READY**.
* `InpNotifyTrendSignal = true` — Recibir alertas ante señales confirmadas de retroceso de tendencia.
* `InpMetaQuotesID = "..."` — Ingrese su MetaQuotes ID.

---

## ⚙️ Referencia de Parámetros de Entrada (Input Parameters)

### Configuración General (General Settings)
| Parámetro | Por Defecto | Descripción |
| :--- | :--- | :--- |
| `InpLicenseKey` | `""` | Clave de activación proporcionada por soporte (dejar vacío tras validación). |
| `InputMagicNumber` | `0` | Identificador Magic Number (`0` = Asignación automática por hash). |
| `Language` | `LANG_ENGLISH` | Idioma del panel y alertas (`LANG_ENGLISH` o `LANG_VIETNAMESE`). |
| `InpShowDashboard` | `true` | Mostrar el panel HUD en el gráfico. |
| `SinglePositionMode` | `true` | Restringir el EA a un solo ciclo estratégico a la vez. |
| `ATR_Multiplier` | `1.0` | Multiplicador ATR global para cálculo de distancias dinámicas. |
| `MaxSpreadPoints` | `0` | Filtro de spread máximo en puntos (`0` = Desactivado). |

### Gestión de Riesgo (Risk Management)
| Parámetro | Por Defecto | Descripción |
| :--- | :--- | :--- |
| `RiskMode` | `RISK_BY_VOLUME` | Modo de cálculo de riesgo (`RISK_BY_PERCENT`, `RISK_BY_AMOUNT`, `RISK_BY_VOLUME`). |
| `RiskDefaulMinimum` | `0.01` | Tamaño de lote base o porcentaje de riesgo. |
| `RiskRewardRatio` | `1.0` | Ratio Riesgo:Beneficio por defecto para entradas simples. |
| `MaxVolumeFixed` | `0` | Límite máximo de volumen por orden única (`0` = Ilimitado). |

### Motor de Permisos DCA (DCA Permission Engine Settings)
| Parámetro | Por Defecto | Descripción |
| :--- | :--- | :--- |
| `InpEnableDCAPermission` | `true` | Interruptor principal de la puerta de permisos DCA. |
| `InpMinDRS_Ready` | `65.0` | Puntuación DRS mínima requerida para autorizar DCA ($0–100$). |
| `InpMaxTDS_Allowed` | `65.0` | Puntuación TDS máxima permitida antes de bloquear DCA ($0–100$). |
| `InpEnableAntiExpansion` | `true` | Congelar DCA ante picos de volatilidad ($ATR_5/ATR_{30} > 1.35$). |
| `InpEnableDCAKillSwitch` | `true` | Parada de emergencia bloqueando nuevas órdenes si $TDS > 85$. |
| `InpShowDCAArrows` | `true` | Mostrar flechas visuales de autorización DCA en el gráfico. |
| `InpDCAArrowCooldownBars` | `3` | Espaciado mínimo en velas entre flechas DCA. |

### Notificaciones Push y Alertas
| Parámetro | Por Defecto | Descripción |
| :--- | :--- | :--- |
| `EnableNotifications` | `true` | Interruptor maestro de notificaciones. |
| `InpNotifyDCASignal` | `true` | Enviar alerta móvil cuando el estado DCA pase a READY. |
| `InpNotifyTrendSignal` | `false` | Enviar alerta móvil ante señales confirmadas de retroceso tendencial. |
| `InpMetaQuotesID` | `""` | MetaQuotes ID para entrega directa a la app MT5. |

---

## 📈 Configuración de Trading Recomendada

| Clase de Activo | Símbolos Recomendados | Temporalidad Óptima | Saldo Mínimo (0.01 lote) | Apalancamiento Recomendado |
| :--- | :--- | :--- | :--- | :--- |
| **Metales Preciosos** | `XAUUSD` (Oro) | `M1` / `M5` | $10,000+ | 1:2000 |
| **Forex Principal** | `EURUSD`, `GBPUSD`, `USDJPY` | `M5` / `M15` | $10,000+ | 1:2000 |
| **Criptomonedas** | `BTCUSD`, `ETHUSD` | `M5` / `M15` | $10,000+ | 1:2000 |

> 💡 **Consejo Profesional**: Para una estabilidad superior durante noticias de alto impacto (IPC, NFP, tipos de interés), asegúrese de mantener `InpEnableAntiExpansion = true` y `EnableNewsAlert = true`.

---

## ❓ Preguntas Frecuentes (FAQ)

<details>
<summary><b>Q1: ¿Puedo hacer backtest de Rango en el Probador de MT5 gratis?</b></summary>
<br>
<b>¡Sí, 100% gratis!</b> Rango incluye un bypass integrado para el probador de estrategias que permite realizar backtests, optimizaciones de parámetros y simulaciones visuales ilimitadas sin requerir clave de activación.
</details>

<details>
<summary><b>Q2: ¿Por qué MT5 muestra "Las importaciones de DLL deben estar habilitadas"?</b></summary>
<br>
Rango utiliza la API estándar de Windows (`kernel32.dll`) para leer el serial de hardware con fines de licenciamiento. Marque <b>"Permitir importación de DLL"</b> en <code>Herramientas ➔ Opciones ➔ Asesores Expertos</code>.
</details>

<details>
<summary><b>Q3: ¿Dónde encuentro el serial de mi máquina para la activación en vivo?</b></summary>
<br>
Al colocar Rango en cualquier gráfico, consulte el archivo:
<code>MetaTrader 5 ➔ MQL5 ➔ Files ➔ Rango_Request_Serial.txt</code>.
Asimismo, el serial se imprime directamente en la pestaña <b>Registro de Expertos</b> de MT5.
</details>

<details>
<summary><b>Q4: ¿Funciona Rango en un servidor VPS?</b></summary>
<br>
<b>¡Sí!</b> Rango está optimizado para un uso mínimo de CPU y opera de forma ininterrumpida 24/7 en cualquier VPS con Windows.
</details>

<details>
<summary><b>Q5: ¿Cómo recibir alertas en mi teléfono sin spam?</b></summary>
<br>
Obtenga su <b>MetaQuotes ID</b> en la app móvil MT5, introdúzcalo en MT5 de escritorio bajo <code>Herramientas ➔ Opciones ➔ Notificaciones</code> y marque <b>"Permitir notificaciones Push"</b>.
<br><br>
⚠️ <b>Consejo Anti-Spam:</b> Asegúrese de <b>DESMARCAR</b> <i>"Notificaciones del terminal local"</i> y <i>"Notificaciones del servidor de trading"</i> para recibir exclusivamente las señales de alta precisión de Rango.
</details>

---

## 💬 Soporte y Comunidad

- ✈️ **Soporte Telegram y Activación de Licencias**: [@trading_world_support](https://t.me/trading_world_support)
- 📺 **Canal de YouTube**: Vídeos tutoriales, guías de optimización y sesiones de trading en directo.
- 🐛 **Rastreador de Problemas**: Si encuentra algún error o desea solicitar una función, abra un Issue en este repositorio.

---

## ⚠️ Descargo de Responsabilidad de Riesgo

El comercio de divisas (Forex), contratos por diferencia (CFDs), metales y criptomonedas conlleva un alto nivel de riesgo y puede no ser adecuado para todos los inversores. El alto grado de apalancamiento puede operar tanto a su favor como en su contra. Antes de decidir operar o utilizar sistemas automatizados, evalúe cuidadosamente sus objetivos de inversión, experiencia y apetito por el riesgo.

El rendimiento pasado no garantiza resultados futuros. Los programas y materiales contenidos en este repositorio tienen únicamente fines educativos e informativos. Usted es el único responsable de todas sus decisiones de inversión.

---

<div align="center">
  <sub>Desarrollado con ❤️ por World Trading Lab. Todos los derechos reservados.</sub>
</div>
