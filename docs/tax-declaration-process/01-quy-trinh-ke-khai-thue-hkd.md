# 01 — Quy trình kê khai thuế HKD từ A đến Z

> ⚖️ Miễn trừ: thông tin tham khảo, không phải tư vấn thuế. Thủ tục trên eTax có thể thay đổi theo phiên bản. Đối chiếu hướng dẫn trên `thuedientu.gdt.gov.vn` trước khi thực hiện.

---

## I. Chuẩn bị (làm 1 lần)

### 1. Mã số thuế (MST)
- Nếu chưa có → đăng ký hộ kinh doanh (xem `docs/Thue-HKD-VN-2026/01-quy-trinh-mo-hkd.html`). HKD nhận MST tự động qua liên thông.
- Nếu có MST cá nhân rồi: MST cá nhân có thể dùng chung cho HKD.

### 2. Tài khoản eTax (bắt buộc)

| Cách | Mô tả |
|---|---|
| **eTax Mobile** (nhanh nhất) | Tải app eTax Mobile → Đăng ký → Nhập MST + SĐT → Xác thực qua **ngân hàng** hoặc **VNeID** (định danh mức 2) |
| **Cổng DVC Quốc gia** | dichvucong.gov.vn → Đăng ký → Chọn phương thức xác thực → Kích hoạt |

Nếu không xác thực ngân hàng/VNeID được → phải mang CCCD đến cơ quan thuế để xác minh.

### 3. Chữ ký số / OTP

| HKD > 1 tỷ | HKD ≤ 1 tỷ |
|---|---|
| **Bắt buộc chữ ký số** (USB token hoặc Remote Signing) | **Không bắt buộc** — có thể dùng OTP gửi về SĐT để gửi thông báo DT |

- Giá chữ ký số: ~300k–800k/năm (VNPT CA, Viettel CA, BKAV, MISA eSign, FPT CA).
- Nếu ký số qua Remote Signing: không cần USB token, ký trên điện thoại/web.

### 4. Phần mềm hỗ trợ

| Công cụ | Mục đích |
|---|---|
| **eTax Mobile** | Kê khai + nộp thuế trên ĐT (miễn phí) |
| **Cổng Thuế điện tử** (`thuedientu.gdt.gov.vn`) | Kê khai qua web |
| **MISA eShop** | Miễn phí trọn đời cho HKD < 1 tỷ; tích hợp bán hàng → tự động lập tờ khai |
| **iTaxViewer** | Đọc file XML tờ khai |

---

## II. Sơ đồ quyết định — bạn thuộc luồng nào?

```
HKD có doanh thu
       │
       ▼
  Doanh thu > 1 tỷ?
  ╱              ╲
KHÔNG            CÓ
  │               │
  ▼               ▼
Thông báo DT    Kê khai quý
01/TKN-CNKD     01/CNKD
2 lần/năm       Hạn: 30/4,31/7
(31/7 & 31/1)     31/10,31/1
  │               │
  ▼               ▼
Miễn thuế       Nộp thuế GTGT+TNCN
  │               │
  ▼               ▼
Không QT        Quyết toán 30/04
cuối năm
```

---

## III. HKD > 1 tỷ — Kê khai thuế quý (mẫu 01/CNKD)

### Bước 1 — Đăng nhập eTax

- Web: `thuedientu.gdt.gov.vn` → Đăng nhập (Cá nhân) → **Kê khai thuế**
- App: eTax Mobile → **Khai thuế** → **Kê khai HKD**

### Bước 2 — Chọn mẫu & kỳ

- Chọn **Mẫu 01/CNKD** — "Tờ khai thuế GTGT, TNCN đối với hộ kinh doanh, cá nhân kinh doanh"
- Chọn **Kỳ: Quý** (hoặc Tháng nếu doanh thu rất lớn)
- Chọn quý cần kê khai (Q1: T1–3, Q2: T4–6, Q3: T7–9, Q4: T10–12)

### Bước 3 — Điền các chỉ tiêu

| Chỉ tiêu | Cách nhập |
|---|---|
| **[21] Doanh thu tính thuế GTGT** | Tổng doanh thu phát sinh trong quý |
| **[22] Doanh thu tính thuế TNCN** | Thường = [21] |
| **[23] Tỷ lệ % GTGT** | Chọn theo ngành (bảng bên dưới) |
| **[24] Tỷ lệ % TNCN** | Theo ngành |
| **[25] = [21] × [23]** | Hệ thống tự tính |
| **[26] = [22] × [24]** | Hệ thống tự tính |

**Tỷ lệ % theo ngành** (Thông tư 89/2026/TT-BTC + TT 40/2021):

| Ngành | GTGT | TNCN |
|---|---|---|
| Phân phối hàng hóa | 1% | 0,5% |
| **Dịch vụ (CNTT, phần mềm)** | **5%** (0% nếu là sản phẩm phần mềm) | **2%** |
| Sản xuất, gia công, vận tải | 3% | 1,5% |
| Nội dung số, game, quảng cáo số | 5% | 5% |
| Hoạt động khác | 5% | 2% |

> **Ví dụ:** HKD cung cấp dịch vụ CNTT, Q1 doanh thu 300 triệu. Thuế GTGT = 300tr × 2% = 6tr. Thuế TNCN = 300tr × 0,5% = 1,5tr. **Tổng nộp quý = 7,5 triệu.**

### Bước 4 — Ký & gửi

- Kiểm tra lại các chỉ tiêu → nhấn **"Ký điện tử"**
- Nhập mã OTP gửi về SĐT và mã captcha → **"Nộp tờ khai"**
- Màn hình báo **"Nộp tờ khai thành công"**

### Bước 5 — Nộp tiền thuế

| Phương thức | Thao tác |
|---|---|
| **Trên eTax Mobile** | Nộp thuế → Chọn khoản thu → Chọn ngân hàng liên kết → Xác nhận |
| **Cổng Thuế điện tử** | thuedientu.gdt.gov.vn → Nộp thuế → Chọn ngân hàng |
| **Ngân hàng thương mại** | Điền giấy nộp NSNN, nộp tại quầy |
| **Internet Banking** | Một số NH hỗ trợ nộp thuế trực tuyến |

> **Hạn nộp tiền = hạn nộp tờ khai** (30 tháng đầu quý sau). Nếu chậm: **0,03%/ngày** trên số chậm nộp.

### Hóa đơn điện tử (HĐĐT)

- Bắt buộc với HKD > 1 tỷ.
- Đăng ký HĐĐT trên eTax (mẫu 01/ĐKTĐ-HĐĐT), CQT xử lý 2 ngày.
- Nhà cung cấp: MISA meInvoice (miễn phí cho <1 tỷ), BKAV, Viettel V-invoice, FPT.
- Với doanh thu từ nước ngoài (App Store, Google Play): HKD vẫn phải kê khai vào 01/CNKD; nền tảng đã nộp thuế nhà thầu riêng — dev tự kê phần thu nhập nhận được.

---

## IV. HKD ≤ 1 tỷ — Thông báo doanh thu

### Khi nào?

- 2 lần/năm: **31/07** (6 tháng đầu) & **31/01** năm sau (cả năm).
- Hoặc gộp 1 lần, nộp trước 31/01 năm sau.

### Các bước

1. Đăng nhập eTax → **"Thông báo doanh thu"** → Mẫu **01/TKN-CNKD**
2. Điền tổng doanh thu thực tế phát sinh trong kỳ (không cần chi tiết tháng).
3. Nếu doanh thu ≤ 1 tỷ → hệ thống tự động xác định = **0 đồng thuế**.
4. Ký (hoặc OTP) → **Nộp**.

> Dù không phải nộp thuế, **vẫn phải nộp tờ khai** — không nộp có thể bị phạt.

---

## V. Quyết toán thuế cuối năm

| HKD > 1 tỷ | HKD ≤ 1 tỷ |
|---|---|
| **Phải quyết toán** TNCN — mẫu **02/CNKD-TNCN-QTT** | **Không cần** quyết toán |
| Hạn: **30/04** năm sau | Chỉ cần thông báo DT là đủ |
| Các chỉ tiêu: doanh thu cả năm, thuế đã nộp các quý, điều chỉnh nếu có | |

---

## VI. Sai lầm & cách xử lý

| Sai lầm | Hậu quả | Cách khắc phục |
|---|---|---|
| **Sai chỉ tiêu** (sai doanh thu, sai tỷ lệ) | Chênh thuế, có thể bị truy thu | Nộp **tờ khai bổ sung** trên eTax (chọn "Khai bổ sung") |
| **Sai kỳ** | Nhầm quý | Huỷ tờ khai sai (nếu CQT chưa duyệt), nộp lại đúng kỳ |
| **Quên ký số** | Tờ khai không hợp lệ | Kiểm tra USB token/Remote Signing còn hạn → ký lại |
| **Nộp thiếu tiền** | Bị tính chậm nộp 0,03%/ngày | Nộp bổ sung; không cần tờ khai mới |
| **Chậm nộp tờ khai 1-30 ngày** | Phạt cảnh cáo đến 5 triệu | Nộp ngay khi phát hiện + nộp tiền phạt |
| **Chậm nộp > 90 ngày** | Phạt 15–30 triệu | Kê khai bổ sung + nộp thuế + nộp phạt |
| **Không nộp tờ khai** | Phạt 20–40 triệu, truy thu, hình sự nếu trốn thuế | |

**Liên hệ hỗ trợ:**
- Cơ quan thuế quản lý nơi đăng ký HKD
- Tổng đài eTax: **1900 599 999**
- Trên eTax Mobile: Chatbot AI (Cục Thuế triển khai 10/2026)

---

## VII. Lịch kê khai năm 2026

| Kỳ | Hạn |
|---|---|
| **Q1** (T1-3) — nộp thuế Q4 năm trước + thông báo DT 6T cuối năm trước | **30/01 & 31/01** |
| **Q1** (T1-3) — nộp tờ khai Q1 | **30/04** |
| **Quyết toán thuế TNCN năm trước** | **30/04** |
| **Q2** (T4-6) — nộp tờ khai Q2 | **31/07** |
| **Q3** (T7-9) — nộp tờ khai Q3 | **31/10** |
| **Thông báo DT 6 tháng đầu** | **31/07** |
| **Q4** (T10-12) — nộp tờ khai Q4 + thông báo DT cả năm | **31/01** năm sau |

## Nguồn chính

- Cổng Thuế điện tử: [thuedientu.gdt.gov.vn](https://thuedientu.gdt.gov.vn) · [dichvucong.gdt.gov.vn](https://dichvucong.gdt.gov.vn)
- MISA — Hướng dẫn kê khai thuế HKD: [misaeshop.vn](https://www.misaeshop.vn/29457/ke-khai-thue-ho-kinh-doanh/)
- MISA — Cách đăng ký eTax Mobile: [misaeshop.vn](https://www.misaeshop.vn/45785/cach-dang-ky-etax-mobile-cho-ho-kinh-doanh/)
- Thông tư 18/2026/TT-BTC, Thông tư 89/2026/TT-BTC, Nghị định 254/2026/NĐ-CP
- Nghị định 141/2026/NĐ-CP (ngưỡng 1 tỷ), Nghị định 68/2026/NĐ-CP (quản lý HKD)