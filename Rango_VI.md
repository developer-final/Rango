# 🎯 RANGO EA — Hệ Thống Giao Dịch DCA Định Lượng Thích Ứng Chế Độ Thị Trường Cho MT5

[![Platform](https://img.shields.io/badge/Nền%20tảng-MetaTrader%205-blue.svg)](https://www.metatrader5.com/)
[![Language](https://img.shields.io/badge/Ngôn%20ngữ-MQL5-orange.svg)]()
[![Strategy](https://img.shields.io/badge/Chiến%20lược-Regime--Adaptive%20DCA%20%7C%20Mean%20Reversion-green.svg)]()
[![Backtest](https://img.shields.io/badge/Strategy%20Tester-Mô%20phỏng%20100%25%20Miễn%20phí-brightgreen.svg)]()
[![Telegram Support](https://img.shields.io/badge/Hỗ%20trợ%20Telegram-@trading__world__support-blue.svg)](https://t.me/trading_world_support)

> **Rango EA** là một hệ thống giao dịch định lượng cao cấp cấp độ tổ chức dành cho **MetaTrader 5 (MT5)**. Hệ thống thay thế hoàn toàn phương pháp trung bình giá (DCA) "mù quáng" và lưới Martingale nguy hiểm bằng cơ chế cấp phép thông minh **Regime-Dependent Permission Engine**, hệ thống tính điểm nguy hiểm xu hướng **Trend Danger Scoring (TDS)**, và điểm cạn kiệt thống kê **Statistical Exhaustion Scoring (ES)**.

---

## 📌 Mục Lục

- [Tổng Quan](#-tổng-quan)
- [Vì Sao Rango Khác Biệt (DCA Chống Cháy Tài Khoản)](#-vì-sao-rango-khác-biệt-dca-chống-cháy-tài-khoản)
- [Các Tính Năng Cốt Lõi](#-các-tính-năng-cốt-lõi)
- [Kiến Trúc Hệ Thống & Mô Hình Định Lượng](#-kiến-trúc-hệ-thống--mô-hình-định-lượng)
- [Hướng Dẫn Cài Đặt Nhanh](#-hướng-dẫn-cài-đặt-nhanh)
- [Hướng Dẫn Kích Hoạt (Backtest Miễn Phí & Cài Đặt Live)](#-hướng-dẫn-kích-hoạt-backtest-miễn-phí--cài-đặt-live)
- [Thiết Lập Thông Báo Push Mobile (MT5 App & MetaQuotes ID)](#-thiết-lập-thông-báo-push-mobile-mt5-app--metaquotes-id)
- [Bảng Tham Số Đầu Vào (Input Parameters)](#-bảng-tham-số-đầu-vào-input-parameters)
- [Cấu Hình Giao Dịch Khuyến Nghị](#-cấu-hình-giao-dịch-khuyến-nghị)
- [Câu Hỏi Thường Gặp (FAQ)](#-câu-hỏi-thường-gặp-faq)
- [Hỗ Trợ & Cộng Đồng](#-hỗ-trợ--cộng-đồng)
- [Tuyên Bố Miễn Trừ Rủi Ro](#-tuyên-bố-miễn-trừ-rủi-ro)

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

## 💡 Tổng Quan

Các hệ thống lưới (grid) và Martingale truyền thống tất yếu sẽ thất bại khi gặp các đợt bùng nổ xu hướng một chiều kéo dài. Chúng nhồi lệnh một cách mù quáng vào động lượng bất lợi mạnh cho đến khi tài khoản bị Margin Call (cháy tài khoản).

**Rango EA được phát triển từ nền tảng để giải quyết triệt để vấn đề này.**

Thay vì mở lệnh DCA theo các khoảng pip cố định, Rango tích hợp **Cổng Cấp Phép Định Lượng Đa Yếu Tố (Multi-Factor Quantitative Permission Gate)**:
1. Liên tục theo dõi độ bền vững của chế độ thị trường thông qua **Chỉ số Hurst (Hurst Exponent)**, **ADX**, và **Tỷ lệ giãn nở ATR**.
2. Nếu thị trường đang có xu hướng mạnh chống lại vị thế, **lệnh DCA mới bị chặn tuyệt đối (strict block)**.
3. Tín hiệu và lệnh nhồi chỉ được phép mở khi các chỉ số định lượng xác nhận động lượng xu hướng bất lợi đã suy kiệt và **xác suất hồi quy về trung bình (mean-reversion) đạt mức toán học tối ưu**.

```
                        ┌─────────────────────────────────┐
                        │   Giá & Biến Động Thị Trường    │
                        └────────────────┬────────────────┘
                                         │
                 ┌───────────────────────┴───────────────────────┐
                 ▼                                               ▼
   ┌───────────────────────────┐                   ┌───────────────────────────┐
   │ Trend Danger Score (TDS)  │                   │   Exhaustion Score (ES)   │
   │  - Độ mạnh xu hướng ADX   │                   │  - Khoảng cách tới EMA 200│
   │  - Ký ức Hurst (H > 0.55) │                   │  - RSI cực biên / Bands   │
   │  - Gia tốc biến động ATR  │                   │  - Rút râu suy kiệt đà giá│
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
│ Khóa Mở Lệnh  │                │ Chờ Tín Hiệu  │                │ Thực Thi Lệnh │
└───────────────┘                └───────────────┘                └───────────────┘
```

---

## ⚡ Vì Sao Rango Khác Biệt (DCA Chống Cháy Tài Khoản)

| Tính Năng | DCA Truyền Thống / Martingale | **Rango EA** |
| :--- | :--- | :--- |
| **Kích Hoạt Nhồi Lệnh** | Khoảng cách Pip cố định hoặc đếm thời gian | **Điểm Sẵn Sàng DCA Định Lượng (DRS)** |
| **Bảo Vệ Khỏi Xu Hướng** | Không có (liên tục nhồi lệnh vào đà lao dốc) | **Cổng Chống Giãn Nở & Khóa Xu Hướng Nguy Hiểm** |
| **Độ Chuẩn Xác Về Thời Điểm** | Vào lệnh mù quáng / Pip cố định | **Xác Nhận Cạn Kiệt Động Lượng & Đảo Chiều Thống Kê** |
| **Cô Lập Chiều Giao Dịch** | Mở Buy/Sell đồng thời một cách hỗn loạn | **Cô Lập Định Hướng (Không nhồi lệnh ngược xu hướng)** |
| **Giới Hạn Rủi Ro** | Rủi ro vô hạn (Unbounded Risk) | **Quản Trị Rủi Ro Chặt Chẽ & Stoploss Động Theo ATR** |
| **Bảng Điều Khiển Trực Quan** | Đơn sơ hoặc không có | **Bảng HUD Thời Gian Thực & Mũi Tên Tín Hiệu Thông Minh** |
| **Kiểm Thử (Strategy Tester)**| Thường bị khóa hoặc hạn chế | **Backtest Miễn Phí 100% Không Giới Hạn** |

---

## 🚀 Các Tính Năng Cốt Lõi

### 1. 🛡️ Động Cơ Cấp Phép DCA & Nhận Diện Chế Độ Thị Trường
- **Trend Danger Score (TDS: 0–100)**: Đánh giá sức mạnh xu hướng, tính bền vững của Hurst và gia tốc biến động. Nếu $TDS > 65$, việc mở thêm lệnh DCA mới lập tức bị đóng băng.
- **Exhaustion Score (ES: 0–100)**: Định lượng độ căng dãn thống kê so với đường EMA 200, các mức quá mua/quá bán RSI, sự giảm tốc thân nến và nến pinbar từ chối giá.
- **Anti-Expansion Gate (Cổng chống bùng nổ biến động)**: Tức thì chặn mở lệnh trung bình giá nếu ATR ngắn hạn tăng đột biến vượt xa ATR dài hạn ($ATR_5 / ATR_{30} > 1.35$) trong các đợt tin tức mạnh hoặc phá vỡ giả.
- **Emergency DCA Kill Switch**: Tự động ngắt khẩn cấp việc DCA nếu điều kiện thị trường bất lợi vượt ngưỡng an toàn nghiêm ngặt ($TDS > 85$).

### 2. 🎯 Cô Lập Định Hướng DCA (Directional Isolation)
- Khi thị trường ở trong **Xu hướng Tăng (Uptrend)** rõ ràng: Chặn Buy DCA và chỉ đánh giá Sell DCA đảo chiều tại vùng cạn kiệt đỉnh.
- Khi thị trường ở trong **Xu hướng Giảm (Downtrend)** rõ ràng: Chặn Sell DCA và chỉ đánh giá Buy DCA đảo chiều tại vùng cạn kiệt đáy.
- Tín hiệu hai chiều đối ứng chỉ được phép trong thị trường **Đi ngang / Tích lũy (Sideways/Range)**.

### 3. 🛡️ Quản Trị Rủi Ro Chuyên Nghiệp & Khối Lượng Linh Hoạt
- **Nhiều Chế Độ Đi Tiền**: Hỗ trợ tính theo Khối lượng cố định (Lot), Số tiền rủi ro cố định ($), hoặc Phần trăm vốn rủi ro (%).
- **Tự Động Tính SL/TP**: Tự động phóng chiếu khoảng cách SL/TP dựa trên ATR khung lớn và tỷ lệ Risk:Reward tùy chỉnh.
- **Bộ Lọc Thực Thi An Toàn**: Bộ lọc giãn Spread, xác thực khoảng cách Stops tối thiểu của sàn môi giới và kiểm tra ký quỹ khả dụng trước khi vào lệnh.

### 4. 📊 Bảng Điều Khiển HUD Chuyên Nghiệp Trên Biểu Đồ
- Giao diện trực quan, độ tương phản cao hiển thị:
  - Trạng thái Chế độ Thị trường & Biến động hiện tại.
  - Điểm số trực tiếp **TDS** (Trend Danger Score) và **DRS** (DCA Readiness Score).
  - Thống kê Thắng/Thua, trạng thái sóng hồi (Pullback) và lợi nhuận thời gian thực.

### 5. 🏹 Mũi Tên Tín Hiệu Thông Minh & Thông Báo Push Mobile
- Mũi tên tín hiệu chỉ xuất hiện khi có **chuyển đổi trạng thái (state transitions)** kèm thời gian hồi chiêu và tự động dọn dẹp mũi tên hết hạn (không gây rác biểu đồ).
- **Thông Báo Push Trực Tiếp Tới MT5 Mobile**: Gửi tín hiệu giao dịch và cảnh báo DCA READY tức thì về điện thoại thông qua MetaQuotes ID.

### 6. 🧪 Kiểm Thử Miễn Phí 100% Trên MT5 Strategy Tester
- Backtest, stress test và tối ưu hóa Rango trên dữ liệu lịch sử của mọi sàn môi giới, mọi mã giao dịch và mọi khung thời gian **hoàn toàn không giới hạn**.

---

## 🛠️ Hướng Dẫn Cài Đặt Nhanh

### Yêu Cầu Hệ Thống
- **Nền tảng**: MetaTrader 5 (khuyến nghị Build 3800 trở lên)
- **Hệ điều hành**: Windows 10 / 11 hoặc Windows Server (VPS)
- **Loại tài khoản**: Tài khoản Hedging (tài khoản chuẩn hoặc ECN có spread thấp)

---

### Các Bước Cài Đặt Chi Tiết

1. **Tải về file `Rango.ex5`**:
   - Tải phiên bản `Rango.ex5` mới nhất từ mục [Releases](../../releases) hoặc thư mục gốc dự án.

2. **Sao chép vào thư mục MT5**:
   - Mở phần mềm MetaTrader 5.
   - Nhấp vào `File` ➔ `Open Data Folder` (Mở thư mục dữ liệu).
   - Truy cập vào đường dẫn `MQL5` ➔ `Experts\`.
   - Dán file `Rango.ex5` vào thư mục này.

3. **Bật Giao Dịch Tự Động & Cho Phép Nhập DLL**:
   - Trên MT5, vào `Tools` ➔ `Options` (hoặc phím tắt `Ctrl + O`).
   - Chọn tab **Expert Advisors**.
   - Tích chọn ✅ **Allow algorithmic trading** (Cho phép giao dịch tự động).
   - Tích chọn ✅ **Allow DLL imports** *(Bắt buộc để xác thực phần cứng & kích hoạt bản quyền)*.
   - Tích chọn ✅ **Allow WebRequest for listed URL** và thêm URL `https://api.telegram.org` *(Nếu sử dụng cảnh báo Telegram)*.

4. **Kéo Rango Vào Biểu Đồ**:
   - Trong cửa sổ **Navigator** (`Ctrl + N`), mở rộng mục **Expert Advisors**.
   - Kéo thả **Rango** vào biểu đồ mong muốn (ví dụ: `XAUUSD`, `EURUSD`, `GBPUSD`, `BTCUSD`).
   - Khung thời gian khuyến nghị: **M5** hoặc **M15**.
   - Trong bảng cài đặt EA, đảm bảo đã tích chọn **Allow live trading**, cấu hình các thông số rủi ro và nhấn **OK**.

---

## 🔑 Hướng Dẫn Kích Hoạt (Backtest Miễn Phí & Cài Đặt Live)

Rango cung cấp 2 chế độ vận hành:

### 1. 🧪 Strategy Tester (Backtest & Mô Phỏng)
- Mở cửa sổ Strategy Tester trên MT5 (`Ctrl + R`), chọn `Rango.ex5`, chọn Symbol và khoảng thời gian, sau đó nhấn **Start**.
- Tận hưởng trải nghiệm backtest đầy đủ toàn bộ tính năng!

### 2. 💻 Kích Hoạt Trên Biểu Đồ Live / Demo
Khi bạn gắn Rango vào biểu đồ Live hoặc Demo lần đầu:
1. Rango hoạt động ở chế độ **View-Only Demo Mode** (bảng HUD, thuật toán tính toán và mũi tên tín hiệu vẫn hoạt động đầy đủ).
2. EA tự động tạo mã định danh máy tính duy nhất và xuất ra tệp tin:
   ```
   MQL5\Files\Rango_Request_Serial.txt
   ```
3. Sao chép chuỗi mã Serial của bạn (định dạng: `RANGO-REQ-XXXX-XXXX-XXXX-XXXX`).
4. Gửi mã Serial tới bộ phận hỗ trợ Telegram: **[@trading_world_support](https://t.me/trading_world_support)** để nhận **Mã Kích Hoạt (Activation Key)** (`RANGO-ACT-...`).
5. Dán mã kích hoạt vào tham số đầu vào `InpLicenseKey` (sau khi kích hoạt thành công, mã sẽ được lưu vào bộ nhớ cache).

---

## 📲 Thiết Lập Thông Báo Push Mobile (MT5 App & MetaQuotes ID)

Rango EA hỗ trợ thông báo đẩy trực tiếp đến ứng dụng di động **MetaTrader 5 Mobile App** (iOS & Android) bằng cơ chế `SendNotification` tích hợp sẵn của MQL5. Đảm bảo tốc độ cảnh báo siêu nhanh, an toàn mà không cần cấu hình WebRequest phức tạp.

### Bước 1: Lấy MetaQuotes ID Trên Điện Thoại
* **Android**: Mở app MetaTrader 5 ➔ Chạm vào Menu (☰) ➔ Chọn **Tin nhắn** (hoặc **Cài đặt** ➔ **Tin nhắn**) ➔ Ghi lại mã **MetaQuotes ID** (chuỗi 8 ký tự, ví dụ: `1A2B3C4D`).
* **iOS (iPhone/iPad)**: Mở app MetaTrader 5 ➔ Vào mục **Cài đặt** (Settings) ➔ Chọn **Trò chuyện & Tin nhắn** ➔ Xem mã **MetaQuotes ID của tôi** ở dưới cùng màn hình.

### Bước 2: Cấu Hình Thông Báo Trên MT5 Máy Tính (Desktop)
1. Trên MT5 Desktop, vào `Tools` ➔ `Options` (hoặc nhấn `Ctrl + O`).
2. Chuyển sang tab **Notifications** (Thông báo).
3. Tích chọn ✅ **Enable Push Notifications** (Bật thông báo Push).
4. ⚠️ **LƯU Ý QUAN TRỌNG (Cấu hình Chống Spam)**:
   * **BỎ TÍCH** ❌ **Notifications from the local terminal** (Thông báo từ terminal nội bộ)
   * **BỎ TÍCH** ❌ **Notifications from the trade server** (Thông báo từ máy chủ giao dịch)
   > 💡 **Tại sao phải bỏ chọn 2 mục này?** Nếu bạn chọn, máy chủ sàn sẽ gửi thông báo cho từng lệnh vi mô (mỗi khi đặt lệnh chờ, sửa lệnh, đóng lệnh). Việc bỏ chọn 2 mục này đảm bảo điện thoại của bạn **chỉ nhận** các tín hiệu chiến lược chất lượng cao của Rango mà không bị quấy rầy bởi hàng trăm thông báo rác từ sàn môi giới!
5. Điền mã **MetaQuotes ID** của bạn vào ô trống (có thể nhập nhiều ID cách nhau bằng dấu phẩy).
6. Nhấn nút **Test**. Bạn sẽ lập tức nhận được tin nhắn thử nghiệm trên điện thoại!
7. Nhấn **OK** để lưu cài đặt.

### Bước 3: Cấu Hình Tham Số Thông Báo Trong Rango EA
Trong bảng cài đặt Inputs của EA:
* `EnableNotifications = true` — Bật tính năng gửi thông báo.
* `InpNotifyDCASignal = true` — Nhận cảnh báo thời gian thực khi chỉ số chuyển sang trạng thái **DCA READY**.
* `InpNotifyTrendSignal = true` — Nhận cảnh báo khi xuất hiện tín hiệu **Trend Pullback** đã xác nhận.
* `InpMetaQuotesID = "..."` — Điền MetaQuotes ID (Tùy chọn ghi chú / liên kết terminal).

---

## ⚙️ Bảng Tham Số Đầu Vào (Input Parameters)

### Cài Đặt Chung (General Settings)
| Tham Số | Mặc Định | Mô Tả Ý Nghĩa |
| :--- | :--- | :--- |
| `InpLicenseKey` | `""` | Khóa kích hoạt bản quyền từ hỗ trợ (Để trống sau khi đã lưu cache). |
| `InputMagicNumber` | `0` | Định danh Magic Number riêng của EA (`0` = Tự động cấp theo mã cặp tiền). |
| `Language` | `LANG_ENGLISH` | Ngôn ngữ giao diện bảng điều khiển & thông báo (`LANG_ENGLISH` hoặc `LANG_VIETNAMESE`). |
| `InpShowDashboard` | `true` | Bật/tắt hiển thị bảng HUD trực tiếp trên biểu đồ. |
| `SinglePositionMode` | `true` | Giới hạn EA chỉ chạy duy nhất 1 chu kỳ chiến lược tại một thời điểm. |
| `ATR_Multiplier` | `1.0` | Hệ số nhân ATR cho các phép tính khoảng cách động. |
| `MaxSpreadPoints` | `0` | Bộ lọc giãn spread tối đa theo điểm point (`0` = Tắt). |

### Quản Lý Vốn & Rủi Ro (Risk Management)
| Tham Số | Mặc Định | Mô Tả Ý Nghĩa |
| :--- | :--- | :--- |
| `RiskMode` | `RISK_BY_VOLUME` | Chế độ tính khối lượng (`RISK_BY_PERCENT`, `RISK_BY_AMOUNT`, `RISK_BY_VOLUME`). |
| `RiskDefaulMinimum` | `0.01` | Khối lượng cơ sở (Lot) hoặc tỷ lệ phần trăm rủi ro. |
| `RiskRewardRatio` | `1.0` | Tỷ lệ Risk:Reward mục tiêu cho các lệnh đơn lẻ. |
| `MaxVolumeFixed` | `0` | Khối lượng tối đa cố định cho 1 lệnh (`0` = Không giới hạn). |

### Cài Đặt Động Cơ Cấp Phép DCA (DCA Permission Engine)
| Tham Số | Mặc Định | Mô Tả Ý Nghĩa |
| :--- | :--- | :--- |
| `InpEnableDCAPermission` | `true` | Công tắc chính kích hoạt Cổng Cấp Phép DCA. |
| `InpMinDRS_Ready` | `65.0` | Điểm số Sẵn Sàng DRS tối thiểu để cho phép nhồi lệnh DCA ($0–100$). |
| `InpMaxTDS_Allowed` | `65.0` | Điểm Nguy Hiểm Xu Hướng TDS tối đa cho phép trước khi khóa DCA ($0–100$). |
| `InpEnableAntiExpansion` | `true` | Đóng băng DCA khi biến động ATR bùng nổ ($ATR_5/ATR_{30} > 1.35$). |
| `InpEnableDCAKillSwitch` | `true` | Ngắt khẩn cấp toàn bộ lệnh nhồi mới khi $TDS > 85$. |
| `InpShowDCAArrows` | `true` | Vẽ mũi tên cấp phép DCA trực quan trên biểu đồ. |
| `InpDCAArrowCooldownBars` | `3` | Khoảng cách nến tối thiểu giữa 2 mũi tên tín hiệu DCA liên tiếp. |

### Cài Đặt Cảnh Báo & Thông Báo Push
| Tham Số | Mặc Định | Mô Tả Ý Nghĩa |
| :--- | :--- | :--- |
| `EnableNotifications` | `true` | Công tắc chính bật/tắt toàn bộ thông báo. |
| `InpNotifyDCASignal` | `true` | Gửi thông báo push điện thoại khi trạng thái DCA chuyển sang READY. |
| `InpNotifyTrendSignal` | `false` | Gửi thông báo push khi có tín hiệu hồi quy theo xu hướng (Trend Pullback). |
| `InpMetaQuotesID` | `""` | MetaQuotes ID để định tuyến thông báo trực tiếp đến app MT5 Mobile. |

---

## 📈 Cấu Hình Giao Dịch Khuyến Nghị

| Nhóm Tài Sản | Cặp Tiền / Mã Khuyến Nghị | Khung Thời Gian Tối Ưu | Số Dư Tối Thiểu (0.01 lot) | Đòn Bẩy Khuyến Nghị |
| :--- | :--- | :--- | :--- | :--- |
| **Kim Loại Quý** | `XAUUSD` (Vàng) | `M1` / `M5` | $10,000+ | 1:2000 |
| **Forex Chính** | `EURUSD`, `GBPUSD`, `USDJPY` | `M5` / `M15` | $10,000+ | 1:2000 |
| **Tiền Mã Hóa** | `BTCUSD`, `ETHUSD` | `M5` / `M15` | $10,000+ | 1:2000 |

> 💡 **Mẹo chuyên gia**: Để hệ thống vận hành an toàn và mượt mà nhất qua các thời điểm tin tức kinh tế nhạy cảm (CPI, Non-Farm, Lãi suất Fed), hãy luôn bật `InpEnableAntiExpansion = true` và `EnableNewsAlert = true`.

---

## ❓ Câu Hỏi Thường Gặp (FAQ)

<details>
<summary><b>Q1: Tôi có thể kiểm thử (Backtest) Rango trong MT5 Strategy Tester miễn phí không?</b></summary>
<br>
<b>Hoàn toàn có, 100% Miễn phí!</b> Rango tích hợp sẵn cơ chế bypass cho Strategy Tester, cho phép bạn tự do backtest không giới hạn, tối ưu hóa thông số và chạy mô phỏng Visual Mode mà không cần bất kỳ khóa kích hoạt nào.
</details>

<details>
<summary><b>Q2: Tại sao MT5 báo lỗi "DLL imports must be enabled"?</b></summary>
<br>
Rango sử dụng API chuẩn của hệ điều hành Windows (`kernel32.dll`) để đọc số Serial phần cứng nhằm phục vụ cấp phép bản quyền. Bạn chỉ cần tích chọn <b>"Allow DLL imports"</b> trong <code>Tools ➔ Options ➔ Expert Advisors</code>.
</details>

<details>
<summary><b>Q3: Tôi có thể tìm mã Serial máy tính ở đâu để kích hoạt bản quyền Live?</b></summary>
<br>
Khi gắn Rango vào biểu đồ bất kỳ, bạn kiểm tra tệp tin tại:
<code>MetaTrader 5 ➔ MQL5 ➔ Files ➔ Rango_Request_Serial.txt</code>.
Ngoài ra, mã Serial cũng được in trực tiếp trên tab <b>Experts Log</b> của MT5.
</details>

<details>
<summary><b>Q4: Rango có chạy ổn định trên máy chủ ảo VPS không?</b></summary>
<br>
<b>Rất ổn định!</b> Rango được tối ưu hóa thuật toán mức CPU cực thấp, hoạt động mượt mà và bền bỉ 24/7 trên mọi máy chủ ảo Windows VPS.
</details>

<details>
<summary><b>Q5: Làm sao để nhận thông báo trên điện thoại mà không bị làm phiền bởi tin nhắn rác?</b></summary>
<br>
Lấy <b>MetaQuotes ID</b> trong app MT5 Mobile (Android hoặc iOS), điền vào MT5 Desktop tại <code>Tools ➔ Options ➔ Notifications</code> và tích chọn <b>"Enable Push Notifications"</b>.
<br><br>
⚠️ <b>Mẹo chống Spam:</b> Hãy nhớ <b>BỎ CHỌN</b> <i>"Notifications from the local terminal"</i> và <i>"Notifications from the trade server"</i> để chỉ nhận tín hiệu phân tích định lượng cốt lõi từ Rango thay vì bị gửi thông báo mỗi khi sàn mở hay đóng các lệnh phụ!
</details>

---

## 💬 Hỗ Trợ & Cộng Đồng

- ✈️ **Hỗ Trợ Telegram & Kích Hoạt Bản Quyền**: [@trading_world_support](https://t.me/trading_world_support)
- 📺 **Kênh YouTube**: Đăng ký theo dõi các video hướng dẫn, tối ưu hóa thông số và các buổi live giao dịch thực chiến.
- 🐛 **Theo Dõi Vấn Đề**: Nếu phát hiện lỗi hoặc muốn đóng góp tính năng mới, bạn có thể tạo Issue trên kho mã nguồn GitHub này.

---

## ⚠️ Tuyên Bố Miễn Trừ Rủi Ro

Giao dịch ngoại hối (Forex), hợp đồng chênh lệch (CFD), kim loại quý và tiền mã hóa tiềm ẩn mức độ rủi ro vốn rất cao và có thể không phù hợp với tất cả các nhà đầu tư. Tỷ lệ đòn bẩy tài chính cao có thể mang lại lợi nhuận lớn nhưng cũng có thể làm gia tăng thua lỗ nhanh chóng. Trước khi quyết định tham gia giao dịch hoặc áp dụng các hệ thống giao dịch tự động, bạn nên cân nhắc kỹ lưỡng mục tiêu đầu tư, kinh nghiệm cá nhân và khả năng chịu đựng rủi ro.

Hiệu quả trong quá khứ không phải là thước đo bảo đảm cho kết quả trong tương lai. Phần mềm và các tài liệu đi kèm trong kho lưu trữ này chỉ phục vụ mục đích nghiên cứu, học tập và cung cấp thông tin. Bạn hoàn toàn chịu trách nhiệm cho mọi quyết định vào lệnh và kết quả tài chính trên tài khoản thật của chính mình.

---

<div align="center">
  <sub>Được phát triển với tất cả tâm huyết ❤️ bởi World Trading Lab. Bảo lưu mọi quyền.</sub>
</div>
