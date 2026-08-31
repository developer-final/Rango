# 🎯 RANGO EA — MT5 İçin Piyasa Rejimi Uyumlu Kantitatif DCA Alım-Satım Sistemi

[![Platform](https://img.shields.io/badge/Platform-MetaTrader%205-blue.svg)](https://www.metatrader5.com/)
[![Language](https://img.shields.io/badge/Dil-MQL5-orange.svg)]()
[![Strategy](https://img.shields.io/badge/Strateji-Regime--Adaptive%20DCA%20%7C%20Mean%20Reversion-green.svg)]()
[![Backtest](https://img.shields.io/badge/Strateji%20Test%20Edici-%25100%20%C3%9Ccretsiz%20Sim%C3%BClasyon-brightgreen.svg)]()
[![Telegram Support](https://img.shields.io/badge/Telegram%20Destek-@trading__world__support-blue.svg)](https://t.me/trading_world_support)

> **Rango EA**, **MetaTrader 5 (MT5)** platformu için geliştirilmiş kurumsal düzeyde gelişmiş bir kantitatif algoritmik ticaret sistemidir. Geleneksel "kör" maliyet ortalaması (DCA) ve tehlikeli Martingale ızgaralarını akıllı **Piyasa Rejimine Dayalı İzin Motoru (Regime-Dependent Permission Engine)**, **Trend Tehlike Puanlaması (TDS)** ve **İstatistiksel Tükenme Puanlaması (ES)** ile değiştirir.

---

## 📌 İçindekiler

- [Genel Bakış](#-genel-bakış)
- [Rango Neden Farklıdır (Hesap Patlatmayan DCA)](#-rango-neden-farklıdır-hesap-patlatmayan-dca)
- [Temel Özellikler](#-temel-özellikler)
- [Sistem Mimarisi ve Kantitatif Modeller](#-sistem-mimarisi-ve-kantitatif-modeller)
- [Hızlı Başlangıç ve Kurulum Kılavuzu](#-hızlı-başlangıç-ve-kurulum-kılavuzu)
- [Etkinleştirme (Ücretsiz Test ve Canlı Kurulum)](#-etkinleştirme-ücretsiz-test-ve-canlı-kurulum)
- [Mobil Push Bildirim Kurulumu (MT5 App ve MetaQuotes ID)](#-mobil-push-bildirim-kurulumu-mt5-app-ve-metaquotes-id)
- [Girdi Parametreleri Referansı (Input Parameters)](#-girdi-parametreleri-referansı-input-parameters)
- [Önerilen Ticaret Ayarları](#-önerilen-ticaret-ayarları)
- [Sıkça Sorulan Sorular (FAQ)](#-sıkça-sorulan-sorular-faq)
- [Destek ve Topluluk](#-destek-ve-topluluk)
- [Risk Uyarısı](#-risk-uyarısı)

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

## 💡 Genel Bakış

Geleneksel ızgara (grid) ve Martingale sistemleri, uzun süreli ve güçlü tek yönlü trend patlamalarında kaçınılmaz olarak başarısız olur. Hesap teminat tamamlama çağrısı (Margin Call) alana kadar ters yöndeki güçlü momentuma körü körüne ek emirler yığarlar.

**Rango EA, tam olarak bu sorunu kökünden çözmek için sıfırdan inşa edildi.**

Rango, sabit pip aralıklarında rastgele emir açmak yerine **Çok Faktörlü Kantitatif İzin Geçidi (Multi-Factor Quantitative Permission Gate)** kullanır:
1. **Hurst Üssü (Hurst Exponent)**, **ADX** ve **ATR Genişleme Oranları** ile piyasa rejiminin kalıcılığını sürekli takip eder.
2. Piyasa aleyhinize güçlü bir trend içindeyse, **yeni DCA emirlerinin açılması kesin olarak engellenir**.
3. Sinyaller ve eklemeler, yalnızca kantitatif ölçütler ters trend momentumunun tükendiğini ve **ortalamaya dönüş (Mean Reversion) olasılığının matematiksel olarak zirveye ulaştığını** onayladığında verilir.

```
                        ┌─────────────────────────────────┐
                        │     Piyasa Fiyatı ve Volatilite │
                        └────────────────┬────────────────┘
                                         │
                 ┌───────────────────────┴───────────────────────┐
                 ▼                                               ▼
   ┌───────────────────────────┐                   ┌───────────────────────────┐
   │  Trend Tehlike Skoru (TDS)│                   │   Tükenme Skoru (ES)      │
   │  - ADX Trend Gücü         │                   │  - 200 EMA'dan Uzaklık    │
   │  - Hurst Hafızası (H>0.55)│                   │  - RSI Ekstrem Değerleri  │
   │  - Volatilite İvmesi      │                   │  - Momentum Yavaşlama Pini│
   └─────────────┬─────────────┘                   └─────────────┬─────────────┘
                 │                                               │
                 └───────────────────────┬───────────────────────┘
                                         ▼
                        ┌─────────────────────────────────┐
                        │    DCA Hazırlık Skoru (DRS)     │
                        └────────────────┬────────────────┘
                                         │
        ┌────────────────────────────────┼────────────────────────────────┐
        ▼                                ▼                                ▼
┌───────────────┐                ┌───────────────┐                ┌───────────────┐
│ DCA ENGELLENDİ│                │  DCA İZLEMEDE │                │   DCA HAZIR   │
│Emirleri Durdur│                │ Tetikleyiciyi │                │ Sinyali İcra  │
│               │                │     Bekle     │                │      Et       │
└───────────────┘                └───────────────┘                └───────────────┘
```

---

## ⚡ Rango Neden Farklıdır (Hesap Patlatmayan DCA)

| Özellik | Geleneksel DCA / Martingale | **Rango EA** |
| :--- | :--- | :--- |
| **Ortalama Tetikleyici**| Sabit pip aralığı veya zamanlayıcı | **Kantitatif DCA Hazırlık Skoru (DRS)** |
| **Trend Koruması** | Yok (çöküş boyunca emir ekler) | **Genişleme Önleme Geçidi & Trend Tehlike Engeli** |
| **Zamanlama Hassasiyeti**| Kör / Sabit Pip | **Kantitatif Tükenme ve Dönüş Onayı** |
| **Yönsel İzolasyon** | Çift yönlü alım/satımı rastgele açar | **Yönsel İzolasyon (Ters trend kirliliği yok)** |
| **Risk Sınırı** | Sınırsız sıfırlanma riski | **Sıkı Risk Yönetimi ve Dinamik ATR Stop Koruması** |
| **Görsel Telemetri** | Yok veya çok basit | **Gerçek Zamanlı HUD Gösterge Paneli ve Akıllı Oklar** |
| **Strateji Test Edici** | Genellikle kısıtlı veya ücretli | **%100 Ücretsiz ve Sınırsız Backtest** |

---

## 🚀 Temel Özellikler

### 1. 🛡️ DCA İzin ve Piyasa Rejimi Motoru
- **Trend Danger Score (TDS: 0–100)**: Trend gücünü, Hurst kalıcılığını ve volatilite ivmesini değerlendirir. $TDS > 65$ ise yeni DCA girişleri anında dondurulur.
- **Exhaustion Score (ES: 0–100)**: 200 EMA'dan istatistiksel sapmayı, RSI aşırı alım/satım seviyelerini ve mum gövdesi yavaşlamalarını ölçer.
- **Anti-Expansion Gate (Genişleme Önleme)**: Haber patlamalarında kısa vadeli ATR uzun vadeli ATR'yi aştığında ($ATR_5 / ATR_{30} > 1.35$) maliyet ortalamasını anında durdurur.
- **Acil Durum DCA Kill Switch**: Olumsuz piyasa koşulları kritik güvenlik limitlerini aştığında ($TDS > 85$) yeni emirleri otomatik olarak durdurur.

### 2. 🎯 Yönsel DCA İzolasyonu (Directional Isolation)
- Onaylanmış **Yükseliş Trendinde (Uptrend)**: Alış DCA engellenir ve yalnızca tepe tükenmesinde Satış DCA ortalamaya dönüş değerlendirilir.
- Onaylanmış **Düşüş Trendinde (Downtrend)**: Satış DCA engellenir ve yalnızca dip tükenmesinde Alış DCA ortalamaya dönüş değerlendirilir.
- Çift yönlü sinyallere yalnızca **Yatay / Bant (Range-bound)** piyasalarda izin verilir.

### 3. 🛡️ Profesyonel Risk Yönetimi ve Esnek Lot Boyutlandırma
- **Çoklu Lot Modları**: Sabit Lot (Volume), Sabit Para Miktarı ($), veya Hesap Riski Yüzdesi (%).
- **Otomatik SL/TP Hesaplama**: Üst zaman dilimi ATR'si ve özelleştirilebilir Risk:Ödül oranına dayalı dinamik hedef mesafeleri.
- **İcra Güvenlik Filtreleri**: Spread filtresi, aracı kurum minimum stop mesafesi doğrulaması ve emir öncesi teminat kontrolü.

### 4. 📊 Grafik Üzerinde Profesyonel HUD Paneli
- Net, yüksek kontrastlı ekran:
  - Mevcut Piyasa Rejimi ve Volatilite durumu.
  - Canlı **TDS** (Trend Tehlikesi) ve **DRS** (DCA Hazırlığı).
  - Kazanma/Kaybetme istatistikleri, trend düzeltme durumu ve anlık kâr/zarar.

### 5. 🏹 Akıllı Sinyal Okları ve Push Bildirimleri
- Oklar yalnızca **durum geçişlerinde** çizilir, bekleme süresi (cooldown) ve otomatik temizleme özelliğine sahiptir.
- **Doğrudan MT5 Mobil Push Bildirimleri**: MetaQuotes ID aracılığıyla alım-satım sinyallerini ve DCA READY uyarılarını telefonunuza iletir.

### 6. 🧪 Sınırsız MT5 Strateji Test Edici
- Rango'yu herhangi bir aracı kurumda, enstrümanda ve zaman diliminde **hiçbir kısıtlama olmaksızın** test edin ve optimize edin.

---

## 🛠️ Hızlı Başlangıç ve Kurulum Kılavuzu

### Gereksinimler
- **Platform**: MetaTrader 5 (Build 3800 veya üzeri önerilir)
- **İşletim Sistemi**: Windows 10 / 11 veya Windows Server (VPS)
- **Hesap Türü**: Hedging hesabı önerilir (Düşük spreadli Standart veya ECN)

---

### Adım Adım Kurulum

1. **`Rango.ex5` Dosyasını İndirin**:
   - En güncel derlenmiş `Rango.ex5` dosyasını [Releases](../../releases) sekmesinden veya ana dizinden indirin.

2. **MT5 Dizinine Kopyalayın**:
   - MetaTrader 5 terminalinizi açın.
   - `Dosya (File)` ➔ `Veri Klasörünü Aç (Open Data Folder)` seçeneğine tıklayın.
   - `MQL5` ➔ `Experts\` klasörüne gidin.
   - `Rango.ex5` dosyasını bu klasöre yapıştırın.

3. **Algoritmik Ticareti ve DLL İçe Aktarımını Etkinleştirin**:
   - MT5'te `Araçlar (Tools)` ➔ `Seçenekler (Options)` (veya `Ctrl + O`) menüsüne gidin.
   - **Uzman Danışmanlar (Expert Advisors)** sekmesini seçin.
   - ✅ **Algoritmik ticarete izin ver (Allow algorithmic trading)** seçeneğini işaretleyin.
   - ✅ **DLL içe aktarımına izin ver (Allow DLL imports)** seçeneğini işaretleyin *(Donanım lisans doğrulaması için gereklidir)*.
   - ✅ **Listelenen URL için WebRequest'e izin ver** seçeneğine `https://api.telegram.org` ekleyin *(Telegram bildirimleri için)*.

4. **Rango'yu Grafiğe Ekleyin**:
   - **Kılavuz (Navigator)** penceresinde (`Ctrl + N`), **Uzman Danışmanlar** başlığını genişletin.
   - **Rango**'yu istediğiniz grafiğe sürükleyip bırakın (örn. `XAUUSD`, `EURUSD`, `GBPUSD`, `BTCUSD`).
   - Önerilen Zaman Dilimi: **M5** veya **M15**.
   - Açılan pencerede **Canlı ticarete izin ver** seçeneğinin işaretli olduğundan emin olun, risk parametrelerinizi ayarlayın ve **Tamam**'a tıklayın.

---

## 🔑 Etkinleştirme (Ücretsiz Test ve Canlı Kurulum)

Rango iki farklı çalışma modu sunar:

### 1. 🧪 %100 Ücretsiz Strateji Test Edici (Simülasyon ve Test)
- **Lisans anahtarı gerekmez.**
- MT5 Strateji Test Ediciyi açın (`Ctrl + R`), `Rango.ex5`'i seçin, sembol ve tarih aralığını belirleyin ve **Başlat**'a tıklayın.
- Tüm özelliklere sınırsız erişimle geçmişi test edin!

### 2. 💻 Canlı / Demo Grafik Etkinleştirme
Rango'yu ilk kez canlı veya demo bir grafiğe yüklediğinizde:
1. Rango **Yalnızca Görüntüleme Demo Modunda (View-Only Demo Mode)** başlar (gösterge paneli ve hesaplamalar aktiftir).
2. Bilgisayarınıza özel donanım seri numarasını otomatik olarak oluşturur ve dosyaya yazar:
   ```
   MQL5\Files\Rango_Request_Serial.txt
   ```
3. Seri numaranızı kopyalayın (format: `RANGO-REQ-XXXX-XXXX-XXXX-XXXX`).
4. **Aktivasyon Anahtarınızı** (`RANGO-ACT-...`) almak için bu numarayı Telegram Destek hattına iletin: **[@trading_world_support](https://t.me/trading_world_support)**.
5. Anahtarı `InpLicenseKey` girdi parametresine yapıştırın (doğrulandıktan sonra otomatik olarak önbelleğe alınır).

---

## 📲 Mobil Push Bildirim Kurulumu (MT5 App ve MetaQuotes ID)

Rango EA, harici botlara ihtiyaç duymadan MQL5'in yerel `SendNotification` altyapısını kullanarak **MetaTrader 5 Mobil Uygulamasına** (iOS ve Android) anında bildirim gönderir.

### 1. Adım: Mobilde MetaQuotes ID'nizi Bulun
* **Android**: MT5 uygulamasını açın ➔ Menü (☰) ➔ **Mesajlar** (veya **Ayarlar** ➔ **Mesajlar**) ➔ 8 haneli **MetaQuotes ID**'nizi not edin (örn. `1A2B3C4D`).
* **iOS (iPhone/iPad)**: MT5 uygulamasını açın ➔ **Ayarlar** ➔ **Sohbet ve Mesajlar** ➔ Ekranın altındaki **MetaQuotes ID'm** alanına bakın.

### 2. Adım: Masaüstü MT5'te Bildirimleri Yapılandırın
1. Bilgisayarınızdaki MT5 terminalinde `Araçlar` ➔ `Seçenekler` (`Ctrl + O`) menüsünü açın.
2. **Bildirimler (Notifications)** sekmesine gelin.
3. ✅ **Push Bildirimlerini Etkinleştir (Enable Push Notifications)** kutusunu işaretleyin.
4. ⚠️ **ÖNEMLİ (Spam Önleme Yapılandırması)**:
   * **İŞARETİ KALDIRIN** ❌ **Yerel terminalden gelen bildirimler**
   * **İŞARETİ KALDIRIN** ❌ **Ticaret sunucusundan gelen bildirimler**
   > 💡 **Neden bu işaretleri kaldırmalısınız?** İşaretli kalırlarsa, aracı kurum sunucusu her bekleyen emirde, değiştirmede ve küçük işlemde bildirim gönderir. Bunları kaldırmak, telefonunuza **yalnızca** Rango'nun yüksek öncelikli strateji sinyallerinin gelmesini sağlar!
5. **MetaQuotes ID** alanına mobil ID'nizi girin (birden fazla ID virgülle ayrılabilir).
6. **Test** düğmesine tıklayın. Telefonunuza anında bir test bildirimi gelecektir!
7. Ayarları kaydetmek için **Tamam**'a tıklayın.

### 3. Adım: Rango EA Bildirim Ayarlarını Yapın
EA girdi ayarlarında:
* `EnableNotifications = true` — Push bildirimlerini etkinleştirir.
* `InpNotifyDCASignal = true` — Göstergeler **DCA READY** durumuna geçtiğinde bildirim gönderir.
* `InpNotifyTrendSignal = true` — Onaylanmış trend düzeltme sinyallerinde bildirim gönderir.
* `InpMetaQuotesID = "..."` — MetaQuotes ID'nizi girin.

---

## ⚙️ Girdi Parametreleri Referansı (Input Parameters)

### Genel Ayarlar (General Settings)
| Parametre | Varsayılan | Açıklama |
| :--- | :--- | :--- |
| `InpLicenseKey` | `""` | Destek ekibinden alınan aktivasyon anahtarı (kaydedildikten sonra boş bırakılabilir). |
| `InputMagicNumber` | `0` | Benzersiz EA sihirli numarası (`0` = Sembol karmasına göre otomatik atanır). |
| `Language` | `LANG_ENGLISH` | Panel ve bildirim dili (`LANG_ENGLISH` veya `LANG_VIETNAMESE`). |
| `InpShowDashboard` | `true` | Grafik üzerinde gerçek zamanlı HUD panelini gösterir. |
| `SinglePositionMode` | `true` | EA'yı aynı anda tek bir strateji döngüsü ile sınırlar. |
| `ATR_Multiplier` | `1.0` | Dinamik mesafe hesaplamaları için genel ATR çarpanı. |
| `MaxSpreadPoints` | `0` | İzin verilen maksimum spread filtresi (`0` = Devre Dışı). |

### Risk Yönetimi (Risk Management)
| Parametre | Varsayılan | Açıklama |
| :--- | :--- | :--- |
| `RiskMode` | `RISK_BY_VOLUME` | Risk hesaplama modu (`RISK_BY_PERCENT`, `RISK_BY_AMOUNT`, `RISK_BY_VOLUME`). |
| `RiskDefaulMinimum` | `0.01` | Temel lot boyutu veya risk yüzdesi. |
| `RiskRewardRatio` | `1.0` | Tekli girişler için varsayılan Risk:Ödül oranı. |
| `MaxVolumeFixed` | `0` | Tek emir başına maksimum sabit lot sınırı (`0` = Sınırsız). |

### DCA İzin Motoru Ayarları (DCA Permission Engine Settings)
| Parametre | Varsayılan | Açıklama |
| :--- | :--- | :--- |
| `InpEnableDCAPermission` | `true` | DCA İzin Geçidi ana anahtarı. |
| `InpMinDRS_Ready` | `65.0` | DCA eklemesine izin vermek için gereken minimum DRS Hazırlık Skoru ($0–100$). |
| `InpMaxTDS_Allowed` | `65.0` | DCA engellenmeden önce izin verilen maksimum TDS Tehlike Skoru ($0–100$). |
| `InpEnableAntiExpansion` | `true` | Ani ATR volatilite artışlarında DCA'yı dondurur ($ATR_5/ATR_{30} > 1.35$). |
| `InpEnableDCAKillSwitch` | `true` | $TDS > 85$ olduğunda yeni emirleri durduran acil kapatma anahtarı. |
| `InpShowDCAArrows` | `true` | Grafik üzerinde görsel DCA izin oklarını gösterir. |
| `InpDCAArrowCooldownBars` | `3` | Ardışık DCA okları arasındaki minimum mum aralığı. |

### Bildirimler ve Uyarılar
| Parametre | Varsayılan | Açıklama |
| :--- | :--- | :--- |
| `EnableNotifications` | `true` | Bildirimler için ana açma/kapama anahtarı. |
| `InpNotifyDCASignal` | `true` | DCA durumu READY olduğunda mobil push bildirimi gönderir. |
| `InpNotifyTrendSignal` | `false` | Trend düzeltme sinyallerinde mobil push bildirimi gönderir. |
| `InpMetaQuotesID` | `""` | MT5 mobil uygulamasına doğrudan iletim için MetaQuotes ID. |

---

## 📈 Önerilen Ticaret Ayarları

| Varlık Sınıfı | Önerilen Semboller | En Uygun Zaman Dilimi | Minimum Bakiye (0.01 lot) | Önerilen Kaldıraç |
| :--- | :--- | :--- | :--- | :--- |
| **Değerli Metaller**| `XAUUSD` (Altın) | `M1` / `M5` | $10,000+ | 1:2000 |
| **Majör Forex** | `EURUSD`, `GBPUSD`, `USDJPY` | `M5` / `M15` | $10,000+ | 1:2000 |
| **Kripto Para** | `BTCUSD`, `ETHUSD` | `M5` / `M15` | $10,000+ | 1:2000 |

> 💡 **Uzman İpucu**: Önemli ekonomik haberler (TÜFE/CPI, Tarım Dışı İstihdam, Faiz Kararları) sırasında maksimum güvenlik için `InpEnableAntiExpansion = true` ve `EnableNewsAlert = true` ayarlarını açık tutun.

---

## ❓ Sıkça Sorulan Sorular (FAQ)

<details>
<summary><b>Q1: Rango'yu MT5 Strateji Test Edicide ücretsiz test edebilir miyim?</b></summary>
<br>
<b>Evet, %100 ücretsiz!</b> Rango, lisans anahtarı gerektirmeden sınırsız geriye dönük test, parametre optimizasyonu ve görsel mod simülasyonu sağlayan yerleşik bir test atlama mekanizmasına sahiptir.
</details>

<details>
<summary><b>Q2: MT5 neden "DLL içe aktarımına izin verilmelidir" uyarısı veriyor?</b></summary>
<br>
Rango, donanım kimliği ile lisans doğrulaması yapmak için standart Windows API'sini (`kernel32.dll`) kullanır. Lütfen <code>Araçlar ➔ Seçenekler ➔ Uzman Danışmanlar</code> menüsünden <b>"DLL içe aktarımına izin ver"</b> kutusunu işaretleyin.
</details>

<details>
<summary><b>Q3: Canlı aktivasyon için bilgisayar seri numaramı nerede bulabilirim?</b></summary>
<br>
Rango'yu herhangi bir grafiğe yükledikten sonra şu dosyayı kontrol edin:
<code>MetaTrader 5 ➔ MQL5 ➔ Files ➔ Rango_Request_Serial.txt</code>.
Ayrıca seri numarası doğrudan MT5 <b>Uzmanlar (Experts Log)</b> sekmesinde de yazdırılır.
</details>

<details>
<summary><b>Q4: Rango bir VPS sunucusunda çalışır mı?</b></summary>
<br>
<b>Evet!</b> Rango, son derece düşük CPU kullanımı için optimize edilmiştir ve tüm Windows VPS sunucularında 7/24 kesintisiz çalışır.
</details>

<details>
<summary><b>Q5: Spam bildirimler almadan telefonuma yalnızca önemli sinyalleri nasıl alırım?</b></summary>
<br>
Mobildeki <b>MetaQuotes ID</b>'nizi masaüstü MT5'te <code>Araçlar ➔ Seçenekler ➔ Bildirimler</code> altına girin ve <b>"Push Bildirimlerini Etkinleştir"</b> kutusunu işaretleyin.
<br><br>
⚠️ <b>Spam Önleme İpucu:</b> <i>"Yerel terminalden gelen bildirimler"</i> ve <i>"Ticaret sunucusundan gelen bildirimler"</i> seçeneklerinin <b>İŞARETİNİ MUTLAKA KALDIRIN</b>, böylece sadece Rango'nun yüksek kaliteli analiz sinyallerini alırsınız!
</details>

---

## 💬 Destek ve Topluluk

- ✈️ **Telegram Destek ve Lisans Aktivasyonu**: [@trading_world_support](https://t.me/trading_world_support)
- 📺 **YouTube Kanalı**: Eğitim videoları, optimizasyon kılavuzları ve canlı işlem yayınları.
- 🐛 **Hata Bildirimi**: Bir hata bulursanız veya özellik önermek isterseniz GitHub depomuzda Issue açabilirsiniz.

---

## ⚠️ Risk Uyarısı

Kaldıraçlı döviz (Forex), fark sözleşmeleri (CFD), emtia ve kripto para ticareti yüksek düzeyde risk içerir ve tüm yatırımcılar için uygun olmayabilir. Yüksek kaldıraç lehinize olabileceği gibi aleyhinize de çalışabilir. Otomatik sistemleri kullanmaya karar vermeden önce yatırım hedeflerinizi, tecrübenizi ve risk toleransınızı dikkatlice değerlendirmelisiniz.

Geçmiş performans gelecekteki sonuçların garantisi değildir. Bu depoda sağlanan yazılım ve materyaller yalnızca eğitim ve bilgilendirme amaçlıdır. Canlı hesaplarınızdaki tüm ticaret kararları ve finansal sonuçlardan yalnızca siz sorumlusunuz.

---

<div align="center">
  <sub>World Trading Lab tarafından ❤️ ile geliştirilmiştir. Tüm hakları saklıdır.</sub>
</div>
