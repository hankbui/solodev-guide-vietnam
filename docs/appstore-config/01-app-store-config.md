# 01 — Cấu hình App Store Connect: Từ A đến Z (2026)

> **Mốc dữ liệu: 10/2026.** Hướng dẫn từng bước cấu hình mọi thứ trên App Store Connect — từ tạo tài khoản, khai báo thuế, tạo app record, cấu hình IAP/subscription, privacy, build, TestFlight, cho đến submit và phát hành.

---

## Mục lục

1. [Tổng quan dòng chảy cấu hình](#1-tổng-quan-dòng-chảy-cấu-hình)
2. [Đăng ký Apple Developer Program](#2-đăng-ký-apple-developer-program)
3. [App Store Connect — Agreements, Tax & Banking](#3-app-store-connect--agreements-tax--banking)
4. [Tạo App Record và cấu hình thông tin cơ bản](#4-tạo-app-record-và-cấu-hình-thông-tin-cơ-bản)
5. [Cấu hình In-App Purchases / Subscriptions](#5-cấu-hình-in-app-purchases--subscriptions)
6. [App Privacy & Nutrition Labels](#6-app-privacy--nutrition-labels)
7. [Cấu hình Build và Code Signing](#7-cấu-hình-build-và-code-signing)
8. [TestFlight — Beta Testing](#8-testflight--beta-testing)
9. [App Review — Chuẩn bị và Submit](#9-app-review--chuẩn-bị-và-submit)
10. [Phát hành & Release Management](#10-phát-hành--release-management)
11. [Pricing & Availability](#11-pricing--availability)
12. [App Analytics & Sales Reports](#12-app-analytics--sales-reports)
13. [App Store Connect API & Automation](#13-app-store-connect-api--automation)
14. [Checklist cuối cùng trước khi submit](#14-checklist-cuối-cùng-trước-khi-submit)
15. [Nguồn tham khảo](#15-nguồn-tham-khảo)

---

## 1. Tổng quan dòng chảy cấu hình

```
Apple Developer Program ($99/năm)
    │
    ▼
App Store Connect — Agreements, Tax & Banking
    │  ├── W-8BEN (thuế Mỹ)
    │  └── Bank Account (SWIFT)
    ▼
Tạo App Record
    │  ├── Bundle ID
    │  ├── App Name, SKU, Bundle ID
    │  └── Primary Language
    ▼
Cấu hình App Information
    │  ├── Description, Keywords, Screenshots
    │  ├── Privacy Policy URL
    │  ├── Support URL, Marketing URL
    │  └── App Review Contact + Demo Account
    ▼
Cấu hình IAP / Subscriptions (nếu có)
    │  ├── Product IDs
    │  ├── Pricing & Availability
    │  └── Subscription Groups
    ▼
Khai báo App Privacy
    │  ├── Data Collection Types
    │  └── Privacy Nutrition Labels
    ▼
Build & Code Signing (Xcode / Fastlane)
    │  ├── Archive & Upload (Transporter / Xcode)
    │  └── Automatically manage signing
    ▼
TestFlight
    │  ├── Internal Testers (up to 100)
    │  └── External Testers (Beta App Review)
    ▼
Submit for Review
    │  ├── Version Information
    │  ├── Review Notes
    │  └── Phased Release / Manual Release
    ▼
App Review (24–48h)
    │  ├── Approved → Release
    │  └── Rejected → Fix & Resubmit
    ▼
Phát hành & Giám sát
       ├── App Analytics
       └── Payments & Financial Reports
```

## 2. Đăng ký Apple Developer Program

### 2.1 Chuẩn bị

| Hạng mục | Yêu cầu | Ghi chú |
|---|---|---|
| Apple ID | Miễn phí, bắt buộc **2FA** | Tạo tại appleid.apple.com |
| Thẻ thanh toán | Visa/Mastercard quốc tế | Để trả $99/năm |
| Mã số thuế VN | Chưa cần ở bước này | Cần cho W-8BEN sau này |
| DUNS | **Không cần** (Individual) | Chỉ Enterprise/tổ chức mới cần |

### 2.2 Các bước đăng ký

1. Truy cập [developer.apple.com/programs](https://developer.apple.com/programs/)
2. Chọn **Individual** (cá nhân)
3. Đăng nhập bằng Apple ID (đã bật 2FA)
4. Điền thông tin cá nhân (tên, địa chỉ) — phải **khớp với thẻ thanh toán**
5. Thanh toán $99/năm
6. Xác nhận email — kích hoạt tài khoản

> **Gia hạn:** Apple tự động gia hạn hằng năm. Nếu không gia hạn, app vẫn còn trên store nhưng không thể submit bản cập nhật mới.

### 2.3 Enroll Apple Developer Program (2026)

Vào năm 2026, quy trình enroll có thể thực hiện hoàn toàn online:

- **Kiểm tra danh tính:** Có thể yêu cầu xác minh bằng CMND/CCCD/hộ chiếu
- **Thời gian active:** Ngay sau khi thanh toán (có thể mất vài giờ để đồng bộ)
- **Apple Developer app:** Có thể enroll trực tiếp từ iOS/iPadOS

### 2.4 Small Business Program (SBP)

**Luôn enroll SBP ngay khi có thể** để giảm hoa hồng từ **30% → 15%**:

- Điều kiện: tổng proceeds ≤ $1M/năm (tất cả app gộp lại)
- Enroll tại: [developer.apple.com/app-store/small-business-program](https://developer.apple.com/app-store/small-business-program/)
- Không cần chứng minh doanh thu — chỉ cần khai báo

---

## 3. App Store Connect — Agreements, Tax & Banking

> Đây là **bước quan trọng nhất** — nếu không làm, bạn **không thể nhận tiền** từ Apple.

### 3.1 Truy cập

1. Đăng nhập [appstoreconnect.apple.com](https://appstoreconnect.apple.com)
2. Vào **Agreements, Tax, and Banking**
3. Kiểm tra trạng thái từng mục

### 3.2 Paid Applications Agreement

Apple thường xuyên cập nhật hợp đồng. Bạn cần:

- Đọc và chấp nhận phiên bản mới nhất
- Kiểm tra email thông báo thay đổi hợp đồng
- Nếu hợp đồng cũ chưa chấp nhận, bạn **không thể submit app mới**

### 3.3 Khai thuế Mỹ — Mẫu W-8BEN

> **Cá nhân ngoài Mỹ bắt buộc khai.** Nếu không khai, Apple khấu trừ **30% tổng doanh thu** theo luật Chapter 3/4 IRS và **không hoàn lại**.

**Các bước khai W-8BEN:**

1. Trong **Agreements, Tax, and Banking**, chọn **Tax**
2. Chọn **Add Tax Interview** hoặc **Edit** (nếu đã có)
3. Trả lời các câu hỏi:
   - **Tax residency:** Việt Nam (hoặc quốc gia bạn cư trú)
   - **Permanent residence:** Việt Nam
   - **Treaty benefits:** Không (VN chưa có hiệp định thuế với Mỹ)
   - **US citizen/permanent resident:** No
   - **Foreign TIN:** Nhập **MST cá nhân Việt Nam** của bạn
4. Xác nhận và gửi

**Lưu ý W-8BEN:**

- Có hiệu lực **3 năm**, sau đó phải khai lại
- **Foreign TIN** = MST Việt Nam (10 số). Nếu bạn chưa có MST, hãy đăng ký tại cơ quan thuế trước
- Nếu khai W-8BEN không có Foreign TIN, Apple có thể khấu trừ ở mức cao hơn
- Apple có thể yêu cầu **W-8BEN** bản giấy nếu khai báo không hợp lệ

### 3.4 Banking — Tài khoản nhận tiền

1. Trong **Agreements, Tax, and Banking**, chọn **Banking**
2. Chọn **Add Bank Account**
3. Điền thông tin:

| Trường | Giá trị | Ví dụ (Vietcombank) |
|---|---|---|
| Bank Name | Tên ngân hàng | Joint Stock Commercial Bank for Foreign Trade of Vietnam |
| SWIFT/BIC | Mã SWIFT | BFTVVNVX |
| Account Holder Name | Tên chủ tài khoản | NGUYEN VAN A (viết HOA, khớp Apple ID) |
| Account Number | Số tài khoản | 1234567890 |
| Currency | USD (hoặc VND) | USD |
| Bank Address | Địa chỉ ngân hàng | 198 Tran Quang Khai, Hanoi, Vietnam |

**Yêu cầu:**
- Tài khoản **đứng tên cá nhân**, khớp tên với Apple Developer
- Ngân hàng hỗ trợ **nhận USD / SWIFT**
- Kiểm tra mã SWIFT chính xác — sai mã SWIFT là mất tiền

**Phí khi nhận tiền về VN:**
- Phí SWIFT: $10–25/lần (tuỳ ngân hàng)
- Phí ngân hàng trung gian: $5–20 (nếu có)
- Spread tỷ giá USD → VND: thường 1–3%

### 3.5 Kiểm tra trạng thái

Dùng bảng sau để kiểm tra trạng thái agreements của bạn:

| Mục | Trạng thái | Ý nghĩa |
|---|---|---|
| Paid Applications | Active | Đã chấp nhận, OK |
| Free Applications | Active | OK |
| iOS Paid Applications | Active | OK (nếu có) |
| Tax (W-8BEN) | Completed | Đã khai xong |
| Banking | Bank Info on File | Đã có tài khoản |

---

## 4. Tạo App Record và cấu hình thông tin cơ bản

### 4.1 Tạo App Record

> App Record là "hồ sơ" của app trên App Store Connect — bạn phải tạo record trước khi upload build.

1. Vào **My Apps** → **+** → **New App**
2. Điền thông tin:

| Trường | Mô tả | Ví dụ |
|---|---|---|
| **Platforms** | iOS, macOS, tvOS, visionOS | iOS |
| **Name** | Tên hiển thị trên store (tối đa 30 ký tự) | Business Law VN |
| **Primary Language** | Ngôn ngữ chính | Vietnamese |
| **Bundle ID** | Bundle identifier duy nhất (com.company.app) | com.hankbui.businesslaw |
| **SKU** | Mã nội bộ của bạn (tuỳ chọn, không hiển thị) | BUSINESSLAW_001 |
| **User Access** | Full access / Limited access | Full Access |

### 4.2 Bundle ID đã tồn tại?

Nếu Bundle ID đã được dùng bởi app khác, bạn phải:

- Tạo Bundle ID mới trong **Certificates, Identifiers & Profiles**
- Hoặc dùng Wildcard Bundle ID (vd: `com.hankbui.*`)

### 4.3 Điền App Information

Sau khi tạo record, vào tab **App Information**:

#### 4.3.1 General Information

| Trường | Hướng dẫn |
|---|---|
| **Bundle ID** | Chọn bundle ID bạn đã đăng ký |
| **Apple ID** | Tự động sinh (VD: 1234567890), dùng để report |
| **SKU** | Mã nội bộ |
| **Content Rights** | Khai báo nếu app có nội dung third-party |

#### 4.3.2 Localizable Information

Các trường có thể dịch sang nhiều ngôn ngữ:

- **App Name** (≤ 30 ký tự)
- **Subtitle** (≤ 30 ký tự) — optional
- **Privacy Policy URL** — **BẮT BUỘC** nếu app thu thập dữ liệu
- **Category** — Primary + Secondary (nếu có)

#### 4.3.3 Rating

Tự đánh giá rating (self-rated, sau đó Apple có thể kiểm tra):

| Rater | Lựa chọn (None / Infrequent / Frequent) |
|---|---|
| Cartoon/Fantasy Violence | None |
| Realistic Violence | None |
| Profanity/Crude Humor | None |
| Sexual Content/Nudity | None |
| Gambling | None |
| Unrestricted Web Access | No |
| Medical/Treatment Info | None/Frequent (tuỳ app) |

### 4.4 Version Information (Tab "Version" hoặc "iOS App")

#### 4.4.1 Metadata

| Trường | Yêu cầu | Mẹo |
|---|---|---|
| **Description** | ≤ 4000 ký tự | Viết rõ app làm gì, ai nên dùng |
| **What's New** | Chỉ cho bản cập nhật | Ghi rõ thay đổi, fix bug gì |
| **Keywords** | ≤ 100 ký tự | Các từ khoá cách nhau bằng dấu phẩy. VD: law, business, legal, contract, Vietnam |
| **Support URL** | Bắt buộc | URL có trang hỗ trợ thực tế |
| **Marketing URL** | Không bắt buộc | URL trang chủ của app |

#### 4.4.2 Screenshots & Previews

Yêu cầu kỹ thuật (2026):

| Device | Kích thước màn hình | Resolution |
|---|---|---|
| iPhone 6.7" | 1290 × 2796 | iPhone 16 Pro Max |
| iPhone 6.5" | 1242 × 2688 | iPhone 16 Plus |
| iPhone 5.5" | 1242 × 2208 | iPhone SE/8 Plus |
| iPad Pro 12.9" | 2048 × 2732 | iPad Pro |
| iPad Pro 11" | 1668 × 2420 | iPad Pro 11" |
| iPad 10.5" | 1668 × 2388 | iPad Air |

**Quy tắc screenshot:**
- **Tối thiểu 1 screenshot mỗi device** (khuyến nghị 3–5)
- **JPEG hoặc PNG**, 72 DPI, RGB
- **Không có status bar** (chụp từ simulator có thể bị lỗi)
- **Nội dung phải khớp với app thực tế**
- **App Preview (video):** ≤ 30 giây, .mov hoặc .m4v, H.264, 60fps tối đa

#### 4.4.3 App Review Information

| Trường | Yêu cầu |
|---|---|
| **Contact Info** | Email/SĐT của bạn (reviewer sẽ liên hệ nếu cần) |
| **Demo Account** | **Bắt buộc** nếu app yêu cầu đăng nhập |
| **Notes** | Giải thích tính năng khó hiểu, hướng dẫn reviewer |
| **Attachment** | File đính kèm (video, doc, diagram) — optional |

### 4.5 Version Release

| Tuỳ chọn | Mô tả |
|---|---|
| **Manually release this version** | Bạn phải bấm "Release" sau khi được duyệt |
| **Automatically release this version** | App tự động lên store ngay sau khi được duyệt |
| **Phased Release** | Phát hành từ từ (1% → 2% → 5% → 10% → ...) |

---

## 5. Cấu hình In-App Purchases / Subscriptions

### 5.1 Tạo Product

1. Vào **My Apps** → chọn app → **In-App Purchases**
2. Chọn **+** và chọn loại:

| Loại | Mô tả |
|---|---|
| **Consumable** | Dùng một lần (VD: 10 coins) |
| **Non-Consumable** | Mua một lần, dùng mãi mãi (VD: unlock feature) |
| **Auto-Renewable Subscription** | Tự động gia hạn (VD: monthly premium) |
| **Non-Renewing Subscription** | Gia hạn thủ công |

### 5.2 Cấu hình Product

#### 5.2.1 Reference Name
Tên nội bộ (không hiển thị với user)

#### 5.2.2 Product ID
Duy nhất, theo convention:
- Consumable: `com.hankbui.app.coins_100`
- Subscription: `com.hankbui.app.premium.monthly`

#### 5.2.3 Pricing

| Trường | Hướng dẫn |
|---|---|
| **Price Tier** | Chọn tier — Apple cập nhật giá tự động theo tỷ giá |
| **Availability** | Available / Upcoming / Cleared for Sale |
| **Subscription Duration** | 1 tháng, 2 tháng, 3 tháng, 6 tháng, 1 năm |
| **Introductory Offer** | Free trial (3/7/14/30 ngày) hoặc Pay-up-front / Pay-as-you-go |

> **2026 Update:** Apple đã bổ sung thêm nhiều price tier trung gian. Dùng **Custom Price** nếu tier mặc định không vừa.

#### 5.2.4 Subscription Groups

Nhóm các subscription lại với nhau. Mỗi group chỉ có **một subscription active** tại một thời điểm.

Ví dụ: Group "Premium" gồm:
- `com.hankbui.app.premium.monthly` — $9.99/tháng
- `com.hankbui.app.premium.yearly` — $79.99/năm

Người dùng upgrade/downgrade giữa các subscription trong cùng group mà không bị tính phí hai lần.

### 5.3 Localization cho IAP

Mỗi product cần:

| Trường | Mô tả |
|---|---|
| **Display Name** | Tên hiển thị (VD: "Premium Monthly") |
| **Description** | Mô tả ngắn gọn (VD: "Access to all premium features") |

Có thể localize cho từng ngôn ngữ.

### 5.4 Family Sharing

Non-Consumable và Auto-Renewable Subscription hỗ trợ Family Sharing:
- **Enable** nếu bạn muốn cho phép chia sẻ trong gia đình

### 5.5 Review IAP

Một số IAP (đặc biệt là subscription) cũng phải qua App Review:
- IAP được review **cùng với build** nếu là version đầu tiên
- Với bản cập nhật, IAP mới có thể được review riêng

---

## 6. App Privacy & Nutrition Labels

### 6.1 App Privacy (Bắt buộc từ 2026)

1. Vào **My Apps** → chọn app → **App Privacy**
2. Bấm **Get Started** hoặc **Edit**

### 6.2 Data Collection

Khai báo **tất cả dữ liệu** bạn thu thập:

| Loại | Ví dụ | Mô tả |
|---|---|---|
| Contact Info | Name, Email, Phone | Đăng ký tài khoản |
| Health & Fitness | Health data | App sức khoẻ |
| Financial | Payment info | Thanh toán |
| Location | GPS | Map app |
| Sensitive Info | Race, religion | Tin tưởng |
| Contacts | Address book | Invite friends |
| User Content | Photos, videos | App chỉnh sửa ảnh |
| Browsing History | Web history | Trình duyệt |
| Search History | Search queries | Search feature |
| Identifiers | Device ID, User ID | Analytics |
| Usage Data | App interactions | Crash reports |
| Diagnostics | Crash data | Bug reporting |
| Other | Any other data | — |

### 6.3 Purpose

Với mỗi loại dữ liệu, khai báo **mục đích sử dụng**:
- **App Functionality** — Chức năng chính của app
- **Analytics** — Phân tích và cải thiện app
- **Product Personalization** — Cá nhân hoá
- **Third-Party Advertising** — Quảng cáo
- **Developer's Advertising** — Quảng cáo của bạn
- **Credit & Fraud Prevention** — Chống gian lận

### 6.4 Data Linked/Not Linked to You

- **Linked to You** — Dữ liệu gắn với danh tính người dùng
- **Not Linked to You** — Dữ liệu ẩn danh

### 6.5 Tracking

> **Bắt buộc khai báo nếu dùng tracking.** Từ iOS 14.5+, dùng App Tracking Transparency (ATT) framework.

- Khai báo nếu bạn track user across apps/websites
- Nếu có tracking, bắt buộc hiển thị ATT prompt

### 6.6 Privacy Policy URL

**Bắt buộc nếu app thu thập bất kỳ dữ liệu nào.** Phải có:

1. Trang privacy policy thực tế (có thể host trên GitHub Pages)
2. Nội dung: loại dữ liệu thu thập, cách dùng, chia sẻ với bên thứ ba, cách liên hệ
3. URL đặt trong **App Information > Privacy Policy URL**

---

## 7. Cấu hình Build và Code Signing

### 7.1 Certificates, Identifiers & Profiles

Truy cập [developer.apple.com/account](https://developer.apple.com/account) → **Certificates, Identifiers & Profiles**

#### 7.1.1 Identifiers

Đăng ký **Bundle ID**:

1. Chọn **Identifiers** → **+**
2. Chọn **App IDs**
3. Điền: Description = "Business Law VN", Bundle ID = `com.hankbui.businesslaw`
4. Bật **Capabilities** cần thiết:
   - Push Notifications
   - In-App Purchase
   - Apple Sign-In (nếu có)
   - iCloud (nếu có)

#### 7.1.2 Certificates

Tạo **Certificate** cho từng mục đích:

| Loại | Dùng cho | Ghi chú |
|---|---|---|
| Apple Development | Development build | Xcode tự quản lý |
| Apple Distribution | Production build | Xcode tự quản lý |
| Apple Push Notification | Push notification | Không dùng Xcode tự động |

> **2026:** Xcode tự động quản lý signing (Automatically manage signing) — bật nó lên, Xcode sẽ lo hết certificate và profile.

### 7.2 Upload Build

#### 7.2.1 Qua Xcode

1. Chọn **Product → Archive** (Generic iOS Device)
2. Mở **Organizer** (Window → Organizer)
3. Chọn archive → **Distribute App**
4. Chọn **App Store Connect**
5. Upload — Xcode sẽ tự động signing

#### 7.2.2 Qua Transporter (ứng dụng macOS)

Dùng khi:
- Bạn build từ CI/CD
- Bạn cần upload .ipa mà không cần Xcode

1. Export .ipa từ Xcode hoặc xcodebuild
2. Mở **Transporter** app
3. Kéo thả .ipa vào
4. Kiểm tra và Deliver

#### 7.2.3 Qua Command Line (xcodebuild + altool)

```bash
# Archive
xcodebuild archive \
  -workspace App.xcworkspace \
  -scheme App \
  -configuration Release \
  -archivePath build/App.xcarchive

# Export IPA
xcodebuild -exportArchive \
  -archivePath build/App.xcarchive \
  -exportPath build/ \
  -exportOptionsPlist exportOptions.plist

# Upload (dùng xcrun)
xcrun altool --upload-app \
  -f build/App.ipa \
  -t ios \
  -u YOUR_APPLE_ID \
  -p @keychain:AC_PASSWORD
```

#### 7.2.4 Qua Fastlane

```ruby
lane :release do
  match(type: "appstore")
  gym(scheme: "App", configuration: "Release")
  pilot(
    skip_waiting_for_build_processing: false,
    distribute_only: true
  )
end
```

### 7.3 Build Processing

Sau khi upload, build vào trạng thái **Processing** (10–30 phút):

```
Uploaded → Processing → Valid → Ready to Submit
```

Nếu build bị lỗi (Invalid):
- Kiểm tra email từ Apple
- Kiểm tra mục **Activity** trên App Store Connect
- Sửa lỗi → upload lại

### 7.4 Build Number

Mỗi build phải có **build number** (CFBundleVersion) cao hơn build trước:
- VD: 1, 2, 3... hoặc 1.0.1, 1.0.2...
- Nếu dùng CI/CD, dùng timestamp làm build number

---

## 8. TestFlight — Beta Testing

### 8.1 Kích hoạt TestFlight

1. Vào **My Apps** → chọn app → **TestFlight** tab
2. Chọn build cần test

### 8.2 Internal Testing

| Tính năng | Giới hạn |
|---|---|
| **Internal Testers** | Tối đa 100 người |
| **Yêu cầu** | Phải có tài khoản Apple (thêm bằng email) |
| **Không cần Beta Review** | Phát hành ngay lập tức |
| **Thời gian hiệu lực** | Build hết hạn sau 90 ngày |

### 8.3 Internal Testers — Quản lý

1. Vào **TestFlight > App Store Connect Users**
2. Thêm user bằng email Apple ID của họ
3. Chọn role **Developer** hoặc **Admin**
4. Sau đó vào **Internal Testing** → chọn build → **Send Invite**
5. Người dùng sẽ nhận email + notification từ TestFlight app

### 8.4 External Testing

| Tính năng | Giới hạn |
|---|---|
| **External Testers** | Tối đa 10.000 người |
| **Yêu cầu** | Bất kỳ ai có email |
| **Cần Beta App Review** | **BẮT BUỘC** cho lần đầu tiên |
| **Thời gian review** | 24–48h (giống App Review) |
| **Build hết hạn** | 90 ngày |

### 8.5 Beta Review Notes

Nếu app cần login để hoạt động:

1. Trong TestFlight, điền **Beta Review Notes**
2. Cung cấp **Demo Account** + hướng dẫn
3. Mô tả tính năng cho reviewer

### 8.6 TestFlight Groups

Tạo group để phân phối build cho từng nhóm:

1. **Internal Group** — nội bộ team
2. **External Group - QA** — tester chuyên nghiệp
3. **External Group - VIP** — khách hàng VIP beta
4. **Public Link** — (2026) tạo link công khai để ai cũng có thể join beta

### 8.7 TestFlight Public Link (2026 Update)

1. Vào **TestFlight > Public Link**
2. Bật **Public Link**
3. Copy link và share với user
4. Tối đa 10.000 tester qua public link

### 8.8 Expired Build

- Build hết hạn sau **90 ngày**
- Phải upload build mới để test tiếp
- TestFlight sẽ notify user khi build sắp hết hạn

---

## 9. App Review — Chuẩn bị và Submit

### 9.1 Chuẩn bị

Trước khi submit, kiểm tra:

- [ ] App không crash trên device thật
- [ ] Tất cả link đều hoạt động
- [ ] Screenshot khớp với app thực tế
- [ ] Privacy policy URL có hiệu lực
- [ ] IAP có thể mua được (sandbox)
- [ ] Demo account hoạt động
- [ ] Review notes đầy đủ
- [ ] App không có placeholder content

### 9.2 Submit

1. Vào **My Apps** → chọn app → version hiện tại
2. Bấm **Submit for Review**
3. Trả lời các câu hỏi:
   - **Export Compliance:** App có dùng encryption không?
     - Nếu chỉ dùng HTTPS → **No** (Yes nhưng khai báo exemption)
     - Nếu dùng custom encryption → **Yes** → cần file ERN/CCATS
   - **Content Rights:** App có third-party content không?
   - **Advertising Identifier:** Có dùng IDFA không?
4. Xác nhận → **Submit**

### 9.3 Export Compliance (2026)

**Câu hỏi thường gặp nhất:**

| App có | Khai báo |
|---|---|
| Chỉ HTTPS | No (Exempt) |
| AES encryption thư viện | Yes → cần giấy phép |
| Custom encryption | Yes → ERN/CCATS |
| Không encryption | No |

> Nếu app chỉ dùng HTTPS (TLS), bạn thuộc diện **exempt**, khai báo "No" và chọn "Yes, my app uses encryption but is exempt" → điền exemption document số.

### 9.4 App Review — Các trạng thái

| Trạng thái | Ý nghĩa |
|---|---|
| **Ready for Review** | Build sẵn sàng |
| **Waiting for Review** | Trong hàng đợi |
| **In Review** | Đang được reviewer kiểm tra |
| **Pending Developer Release** | Đã approved, chờ bạn release |
| **Pending Apple Release** | Apple đang xử lý release |
| **Ready for Sale** | Đã lên store |
| **Rejected** | Bị từ chối (kèm lý do) |
| **Metadata Rejected** | Chỉ lỗi metadata, không ảnh hưởng build |
| **Invalid Binary** | Build lỗi |

### 9.5 Bị từ chối — Xử lý

1. Đọc kỹ **Resolution Center** message
2. Sửa lỗi cụ thể mà reviewer nêu
3. Trả lời trong **Resolution Center**:
   - Nếu đã sửa: nói rõ bạn đã sửa gì
   - Nếu cần giải thích: giải thích lịch sự, chi tiết
   - Nếu không đồng ý: có thể **Appeal** nhưng nên chắc chắn
4. Upload build mới → Submit lại

### 9.6 Expedited Review

Yêu cầu duyệt nhanh (thường được duyệt trong <24h):

- **Critical bug fix** — app đang live bị lỗi nặng
- **Time-sensitive event** — app gắn với sự kiện
- Yêu cầu qua form: [developer.apple.com/contact/app-store/expedited-review](https://developer.apple.com/contact/app-store/expedited-review/)

---

## 10. Phát hành & Release Management

### 10.1 Các chế độ release

| Chế độ | Mô tả |
|---|---|
| **Manual Release** | Bạn chủ động bấm release sau khi approved |
| **Automatic Release** | Tự động lên store ngay sau khi approved |
| **Phased Release** | Phát hành từ từ trong 7 ngày |
| **Scheduled Release** | Hẹn giờ phát hành |

### 10.2 Phased Release (2026)

- **Ngày 1:** 1% người dùng
- **Ngày 2:** 2% nếu không có vấn đề
- **Ngày 3–7:** Tăng dần đến 100%
- Có thể **dừng release** bất kỳ lúc nào nếu phát hiện lỗi

### 10.3 Pre-Order

- Cho phép người dùng đặt trước app
- Hỗ trợ pre-order từ **2–90 ngày** trước ngày phát hành
- Người dùng sẽ được tải tự động khi app lên store

### 10.4 Game Center / In-App Purchases

Sau khi release, kiểm tra:

- **Game Center:** leaderboard, achievement active
- **IAP:** product có thể mua được trên store version

### 10.5 Update app

1. Tạo version mới (VD: 1.0 → 1.1)
2. Điền "What's New in This Version"
3. Upload build mới (build number > previous build)
4. Submit for Review

---

## 11. Pricing & Availability

### 11.1 Giá app

1. Vào **My Apps** → chọn app → **Pricing and Availability**
2. Thiết lập:

| Trường | Mô tả |
|---|---|
| **Standard Price** | Giá gốc (phải chọn tier) |
| **Availability** | Tất cả territories / chỉ một số |
| **Start Date** | Có thể hẹn giờ bắt đầu bán |
| **End Date** | Có thể hẹn giờ kết thúc bán (custom) |
| **Education Discount** | Giảm giá cho giáo dục |

### 11.2 Price Tiers (2026)

Apple bổ sung thêm các tier mới. Một số tier phổ biến:

| Tier | Giá VN (đã bao gồm VAT) | Giá USD |
|---|---|---|
| Tier 0 | Free | Free |
| Tier 1 | 29.000đ | $0.99 |
| Tier 5 | 99.000đ | $3.99 |
| Tier 7 | 199.000đ | $6.99 |
| Tier 10 | 499.000đ | $14.99 |
| Tier 15 | 1.199.000đ | $39.99 |

> Apple cập nhật giá VND tự động theo tỷ giá. Bạn có thể **custom price** nếu muốn set giá cố định.

### 11.3 Territories

Mặc định có bán ở tất cả territories. Nếu chỉ muốn bán ở Việt Nam:

1. Vào **Availability** → **Specific Territories**
2. Bỏ chọn các territories khác
3. Chỉ giữ **Vietnam**

### 11.4 Educational Discount

Nếu app có tính năng giáo dục, bạn có thể chọn:
- **Standard** — không giảm giá
- **Educational Discount** — giảm giá cho tổ chức giáo dục

---

## 12. App Analytics & Sales Reports

### 12.1 App Analytics

Vào **My Apps** → **Analytics**:

| Metric | Mô tả |
|---|---|
| **App Store Views** | Số lượt xem trang app |
| **Product Page Views** | Số lượt xem product page |
| **Downloads** | Lượt tải |
| **Sales** | Doanh thu |
| **Proceeds** | Tiền thực nhận (sau hoa hồng Apple) |
| **Sessions** | Session trên app (nếu dùng SKAdNetwork) |
| **Crash Data** | Dữ liệu crash (nếu tích hợp) |
| **Engagement** | Thời gian dùng, retention |
| **Subscriptions** | Subscriber, renewal rate, churn |
| **App Store Engagement** | Impression → Page View → Download |

### 12.2 Payments & Financial Reports

1. Vào **App Store Connect → Payments and Financial Reports**
2. Chọn kỳ thanh toán:

| Report | Mô tả |
|---|---|
| **Earnings** | Doanh thu theo từng app |
| **Paid** | Khoản đã được thanh toán |
| **Pending** | Khoản chưa đến kỳ thanh toán |
| **Financial Reports** | Báo cáo tài chính chi tiết |

### 12.3 Sales and Trends Reports (Frog/Turtle)

Tải báo cáo qua **Sales and Trends**:

- **Turtle** — Báo cáo hằng ngày (CSV)
- **Frog** — Báo cáo tài chính (PDF)

### 12.4 App Store Connect API — Reports

```bash
# Lấy report qua API
curl -v -G "https://api.appstoreconnect.apple.com/v1/salesReports" \
  -H "Authorization: Bearer YOUR_JWT" \
  -d "filter[frequency]=DAILY" \
  -d "filter[reportType]=SALES" \
  -d "filter[reportSubType]=SUMMARY" \
  -d "filter[vendorNumber]=YOUR_VENDOR_NUMBER"
```

---

## 13. App Store Connect API & Automation

### 13.1 API Key

1. Vào **App Store Connect → Users and Access → API Keys**
2. Bấm **+ Generate API Key**
3. Chọn role:
   - **Admin** — full quyền
   - **App Manager** — quản lý app
   - **Developer** — upload build, TestFlight
   - **Finance** — xem báo cáo tài chính
4. Copy **Issuer ID**, **Key ID**
5. Download **.p8 key** (chỉ một lần duy nhất!)

### 13.2 JWT Token

Dùng .p8 key để tạo JWT token (có thư viện cho Python, Ruby, Node.js):

```python
import jwt
import time

key = open("AuthKey_XXXXX.p8").read()
claims = {
    "iss": "ISSUER_ID",
    "exp": int(time.time()) + 1200,
    "aud": "appstoreconnect-v1"
}
token = jwt.encode(claims, key, algorithm="ES256", headers={"kid": "KEY_ID"})
```

### 13.3 Các API quan trọng

| API | Endpoint | Mô tả |
|---|---|---|
| App List | `GET /v1/apps` | Danh sách app |
| Builds | `GET /v1/builds` | Danh sách build |
| TestFlight | `GET /v1/betaGroups` | Quản lý TestFlight |
| Sales | `GET /v1/salesReports` | Báo cáo doanh số |
| Subscriptions | `GET /v1/subscriptions` | Quản lý subscription |
| IAP | `GET /v1/inAppPurchases` | Quản lý IAP |

---

## 14. Checklist cuối cùng trước khi submit

### 14.1 Technical Checklist

- [ ] App build thành công (Xcode, Release mode)
- [ ] App không crash trên device thật
- [ ] App hoạt động trên iOS version tối thiểu bạn support
- [ ] All links hoạt động (Support URL, Privacy URL)
- [ ] IAP/subscription test pass trên sandbox
- [ ] Push notification hoạt động (nếu có)
- [ ] Apple Sign-In hoạt động (nếu có)
- [ ] App Tracking Transparency (ATT) prompt hiển thị (nếu có tracking)
- [ ] Export compliance khai báo đúng

### 14.2 Metadata Checklist

- [ ] App name ≤ 30 ký tự
- [ ] Subtitle ≤ 30 ký tự (nếu có)
- [ ] Description ≤ 4000 ký tự, không có HTML
- [ ] Keywords ≤ 100 ký tự
- [ ] Screenshots đúng kích thước cho từng device
- [ ] Screenshots không có placeholder/stock data
- [ ] Privacy Policy URL hoạt động
- [ ] Rating đã được khai báo
- [ ] Category chính xác

### 14.3 App Review Checklist

- [ ] Demo account hoạt động (nếu app cần login)
- [ ] Review notes chi tiết, đầy đủ
- [ ] Contact info chính xác
- [ ] No placeholder content
- [ ] No hidden features/unfinished features
- [ ] App không vi phạm guideline 2.1, 2.3, 3.x, 4.x, 5.1

### 14.4 Business Checklist

- [ ] Agreements, Tax, Banking — tất cả active
- [ ] W-8BEN còn hiệu lực
- [ ] Bank account hợp lệ
- [ ] Small Business Program enrolled
- [ ] IAP pricing chính xác
- [ ] Subscription introductory offer đã cấu hình

---

## 15. Nguồn tham khảo

| Nguồn | URL |
|---|---|
| Apple Developer Program | https://developer.apple.com/programs/ |
| App Store Connect | https://appstoreconnect.apple.com |
| Certificates, Identifiers & Profiles | https://developer.apple.com/account |
| App Review Guidelines | https://developer.apple.com/app-store/review/guidelines/ |
| App Store Small Business Program | https://developer.apple.com/app-store/small-business-program/ |
| TestFlight | https://developer.apple.com/testflight/ |
| App Store Connect API | https://developer.apple.com/documentation/appstoreconnectapi |
| Transporter | https://apps.apple.com/us/app/transporter/id1450874784 |
| Fastlane | https://fastlane.tools |
| Export Compliance (ERN) | https://developer.apple.com/export-compliance/ |
| App Privacy | https://developer.apple.com/app-store/app-privacy-details/ |
| IRS W-8BEN | https://www.irs.gov/forms-pubs/about-form-w-8-ben |

---

> **Tiếp theo:** Xem [02-revenuecat-config.md](02-revenuecat-config.md) để hướng dẫn cấu hình RevenueCat cho quản lý subscription cross-platform.