# 01 — Quy trình xuất bản app lên App Store và nhận tiền

> ⚖️ **Miễn trừ:** Thông tin tham khảo, không phải tư vấn pháp lý/thuế. Mốc dữ liệu 19/10/2026. Các tỷ lệ/chu kỳ thanh toán của Apple có thể thay đổi — kiểm tra trong hợp đồng (Apple Developer Program License Agreement) và phần Payments & Financial Reports trên App Store Connect.

## Dòng chảy tổng quan

```
Đăng ký Apple Developer → Khai thuế Mỹ + ngân hàng → Build → App Review → Phát hành → Thanh toán hằng tháng
        ($99/năm)              (W-8BEN + SWIFT)      (Xcode)   (24–48h)    (release)   (30–45 ngày sau cuối tháng)
```

## Bước 1 — Đăng ký Apple Developer Program

- **Tài khoản Individual**: phí **$99/năm**, gia hạn tự động.
- Chuẩn bị: Apple ID (miễn phí), email, **thẻ thanh toán quốc tế** (Visa/Mastercard) để trả $99.
- **Không cần mã DUNS** — DUNS chỉ dành cho tổ chức/doanh nghiệp đăng ký Enterprise.
- Số MST cá nhân VN **không bắt buộc ở bước này** (Apple không hỏi), nhưng bạn **cần MST VN** để khai báo thuế Việt Nam và giúp khai W-8BEN.
- Đăng ký tại: https://developer.apple.com/programs/
- Bắt buộc bật **2FA** cho Apple ID.

## Bước 2 — App Store Connect: khai thuế Mỹ + ngân hàng

Mục **"Agreements, Tax, and Banking"** — quan trọng nhất, làm trước khi nhận tiền:

| Hạng mục | Việc phải làm | Hệ quả nếu bỏ |
|---|---|---|
| **Tax (W-8BEN)** | Cá nhân ngoài Mỹ **bắt buộc khai** Mẫu W-8BEN của IRS, mục "tax info" trong App Store Connect | Apple có thể khấu trừ **~30% tổng doanh thu** theo luật thuế Mỹ (Chapter 3/4). Đây là khoản **không được hoàn** với dev VN (VN không có hiệp định thuế song phương với Mỹ). Khai W-8BEN với mã số thuế nước ngoài hợp lệ giúp giảm thiểu/ minh bạch khoản này. |
| **Banking** | Thêm **tài khoản ngân hàng** nhận tiền (SWIFT/BIC). Tài khoản **đứng tên cá nhân**, cùng tên với tài khoản Apple Developer | Không có tài khoản → không nhận được tiền |
| **Currency** | Apple thanh toán bằng **USD**; ngân hàng VN quy đổi theo tỷ giá | Phí SWIFT/ngân hàng trung gian thường $10–25/lần + spread tỷ giá. Tính vào chi phí thực nhận. |
| **Minimum payout** | Một số nguồn ghi mức tối thiểu để được thanh toán khoảng **$10–25** (thay đổi qua các năm) | Doanh thu nhỏ có thể bị cộng dồn nhiều tháng mới đủ ngưỡng |

> Lưu ý dòng tiền: một khoản lớn sẽ "chảy mất" trước khi về tay bạn — xem **sơ đồ dòng tiền** bên dưới.

## Bước 3 — Chuẩn bị build

- **Xcode** (miễn phí), build iOS/iPadOS/macOS; code signing + provisioning tự động xử lý trong App Store Connect.
- **TestFlight**: beta test nội bộ, **không** dùng phân phối thương mại.
- **Privacy Nutrition Labels**: bắt buộc — khai báo data collection, data sharing, security practices trong App Privacy; hiển thị trên product page.
- **App Review Notes**: cung cấp tài khoản demo + hướng dẫn reviewer (nếu app cần login).
- Có thể dùng **App Store Connect API** để tự động hoá submit/check status.

## Bước 4 — App Review

- Thời gian: **~50% bài duyệt dưới 24h, 90% dưới 48h** (2026). App mới thường lâu hơn bản cập nhật.
- **Expedited review** nếu có lý do chính đáng (critical bug fix, gắn với sự kiện).
- Lý do hay bị từ chối nhất:
  1. **2.1 App Completeness** — crash, placeholder, link hỏng, thiếu demo account, metadata chưa đủ.
  2. **3.x Business** — IAP không dùng Apple IAP / sai business model.
  3. **4.1/4.2/4.3** — copycat, quá ít chức năng, spam.
  4. **2.3 Metadata** — screenshot mô tả sai.
  5. **5.1 Privacy** — thiếu privacy policy, khai data access không rõ.

## Bước 5 — Phát hành

- Sau khi approve, chọn **Release This Version** (hiện trên store trong ~24h) hoặc **Phased Release** (tăng dần % người dùng), **Scheduled Release** (hẹn ngày giờ), **Pre-order**.
- Theo dõi doanh số bằng **App Analytics** (sales, subscriptions, installations, external mếtric).

## Bước 6 — Nhận tiền & phí

**Chu kỳ:** Apple thanh toán **mỗi tháng một lần**, ~**30–45 ngày sau khi kết thúc tháng phát sinh doanh thu** (liệt kê trong Payments & Financial Reports).

**Hoa hồng Apple (lấy trước khi bạn thấy tiền):**

| Program | Hoa hồng | Điều kiện |
|---|---|---|
| Standard | **30%** | Mặc định |
| **Small Business Program** | **15%** | Tổng proceeds ≤ $1M/năm cho **tất cả app gộp**; dev mới enroll được luôn |

### ️💰 Sơ đồ dòng tiền — ví dụ app bán $10 × 1.000 bản = $10.000

```
Doanh thu người mua trả          $10.000
   │  ▼ Apple hoa hồng 30% (SBP 15%)
Apple giữ                        $3.000  ($1.500 nếu SBP)
Thuế Mỹ khấu trừ (nếu W-8BEN chưa khai tốt) ~$3.000
   ▼
Proceeds Apple trả bạn            ≈ $4.000 ($5.500 nếu SBP)
   ▼  phí SWIFT + spread tỷ giá
Tiền thực nhận về VND             = $4.000 − phí ngân hàng
   ▼  ▲ và CHƯA kể thuế VN                                 
Bạn kê khai thuế TNCN/GTGT VN     (xem tài liệu 02)
```

**Chốt lại:** trước khi bắt đầu, ước tính "trong tay thực nhận ≈ **45–60% doanh thu**" sau hoa hồng Apple + thuế Mỹ + phí ngân hàng (chưa kể thuế VN). Enroll **Small Business Program** ngay khi đủ điều kiện để lấy 15% thay vì 30%.

## Nguồn

1. Apple Developer Program — https://developer.apple.com/programs/
2. App Store Small Business Program — https://developer.apple.com/app-store/small-business-program/
3. App Review Guidelines — https://developer.apple.com/app-store/review/guidelines/
4. App Review — quy trình & lý do thường gặp — https://developer.apple.com/distribute/app-review/
5. IRS Form W-8 BEN — https://www.irs.gov/forms-pubs/about-form-w-8-ben
6. App Store Get Started — https://developer.apple.com/app-store/get-started/

*Chu kỳ thanh toán và mức payout tối thiểu thay đổi theo thời gian — xác nhận lại trên App Store Connect (mục Payments and Financial Reports) và hợp đồng hiện hành.*