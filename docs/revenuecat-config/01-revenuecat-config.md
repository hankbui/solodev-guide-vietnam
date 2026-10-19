# 01 — Cấu hình RevenueCat: Từ A đến Z (2026)

> **Mốc dữ liệu: 10/2026.** Hướng dẫn từng bước cấu hình RevenueCat để quản lý in-app purchases và subscriptions cross-platform — từ tạo tài khoản, cấu hình project, products, entitlements, offerings, tích hợp SDK, cho đến testing, analytics và go-live.

---

## Mục lục

1. [Tổng quan — RevenueCat là gì?](#1-tổng-quan--revenuecat-là-gì)
2. [Tạo tài khoản & Project](#2-tạo-tài-khoản--project)
3. [Cấu hình Entitlements](#3-cấu-hình-entitlements)
4. [Tạo Products trên App Store Connect & Google Play](#4-tạo-products-trên-app-store-connect--google-play)
5. [Kết nối App Store & Google Play với RevenueCat](#5-kết-nối-app-store--google-play-với-revenuecat)
6. [Tạo Offerings & Packages](#6-tạo-offerings--packages)
7. [Tích hợp SDK RevenueCat](#7-tích-hợp-sdk-revenuecat)
8. [Cấu hình Webhooks & Integrations](#8-cấu-hình-webhooks--integrations)
9. [Testing với Sandbox & StoreKit](#9-testing-với-sandbox--storekit)
10. [Analytics & Charts](#10-analytics--charts)
11. [Customer Management](#11-customer-management)
12. [Subscription Lifecycle & Events](#12-subscription-lifecycle--events)
13. [Migration — Chuyển từ IAP hiện tại sang RevenueCat](#13-migration--chuyển-từ-iap-hiện-tại-sang-revenuecat)
14. [Best Practices & Troubleshooting (2026)](#14-best-practices--troubleshooting-2026)
15. [Checklist Go-Live](#15-checklist-go-live)
16. [Nguồn tham khảo](#16-nguồn-tham-khảo)

---

## 1. Tổng quan — RevenueCat là gì?

### 1.1 Vấn đề

Khi bạn có app trên cả iOS và Android, quản lý in-app purchases/subscriptions gặp nhiều vấn đề:

- App Store và Google Play có **cấu trúc product khác nhau**
- **Receipt validation** phức tạp
- Theo dõi **subscription lifecycle** (renewal, lapsed, churn) khó khăn
- **Cross-platform** user không đồng bộ

### 1.2 RevenueCat giải quyết

```
Ứng dụng của bạn
    │
    ▼
┌─────────────────────┐
│   RevenueCat SDK    │  ← Tích hợp một lần
└─────────┬───────────┘
          │
          ▼
┌─────────────────────┐
│  RevenueCat Server  │  ← Receipt validation, lifecycle tracking
└─────────┬───────────┘
          │
    ┌─────┴─────┐
    ▼           ▼
App Store    Google Play
(Apple IAP)  (Google IAB)
```

**RevenueCat cung cấp:**
- **SDK** cho iOS, Android, Flutter, React Native, Unity, Kotlin Multiplatform
- **Dashboard** quản lý product, customer, subscription
- **Webhooks & Integrations** — kết nối với backend, Slack, AppsFlyer, Adjust, etc.
- **Analytics** — real-time subscription metrics, charts
- **Experiments** — A/B test offerings & pricing

### 1.3 Pricing (2026)

| Plan | Giá | Phù hợp |
|---|---|---|
| **Free** | $0/tháng, up to $10k/month tracked | MVP, app nhỏ |
| **Pro** | $83/tháng (bill annually) | App vừa, cần webhook |
| **Enterprise** | Custom | Scale-up, multi-app |

---

## 2. Tạo tài khoản & Project

### 2.1 Đăng ký RevenueCat

1. Truy cập [app.revenuecat.com](https://app.revenuecat.com)
2. Đăng ký bằng email hoặc GitHub/Google
3. Xác nhận email → vào Dashboard

### 2.2 Tạo Project

1. Trong Dashboard → **Projects → New Project**
2. Điền thông tin:

| Trường | Mô tả | Ví dụ |
|---|---|---|
| **Project Name** | Tên project | Business Law VN |
| **Platform** | Chọn app platform | iOS + Android (React Native) |
| **App Name** | Tên app (có thể khác project) | BusinessLawVN |
| **Bundle ID (iOS)** | Bundle ID trên App Store | com.hankbui.businesslaw |
| **Package Name (Android)** | Package name trên Play Store | com.hankbui.businesslaw |

### 2.3 Project Settings

Sau khi tạo, vào **Project Settings > General**:

| Thiết lập | Mô tả |
|---|---|
| **Display Name** | Tên hiển thị |
| **Store-specific Shared Secret** | Dùng để verify receipt (tự động có) |
| **API Keys** | Gồm: Secret Key, Public Key, (các key cho SDK) |

> **⚠️ Lưu ý:** Secret Key là **sensitive** — không bao giờ nhúng trong client code. Chỉ dùng ở backend.

---

## 3. Cấu hình Entitlements

### 3.1 Entitlement là gì?

Entitlement là **quyền truy cập** mà người dùng có được khi mua một product. Ví dụ:
- `premium` — mở khoá tất cả tính năng premium
- `pro` — mở khoá tính năng pro
- `remove_ads` — xoá quảng cáo

**Mối quan hệ:**

```
Product (IAP) ─── cấp ───▶ Entitlement
(App Store: com.hankbui.premium.monthly)
(Google: premium_monthly)
```

**Một product có thể cấp nhiều entitlements.**
**Một entitlement có thể được cấp bởi nhiều products.**

### 3.2 Tạo Entitlements

1. **RevenueCat Dashboard → Entitlements → + New**
2. Điền:

| Trường | Ví dụ |
|---|---|
| **Identifier** | `premium` (không dấu, lowercase, underscore) |
| **Display Name** | Premium |

### 3.3 Các Entitlements phổ biến

| Identifier | Display Name | Mô tả |
|---|---|---|
| `premium` | Premium | Toàn bộ tính năng premium |
| `pro` | Pro | Tính năng pro nâng cao |
| `remove_ads` | Remove Ads | Xoá quảng cáo |
| `unlimited_access` | Unlimited Access | Truy cập không giới hạn |

---

## 4. Tạo Products trên App Store Connect & Google Play

### 4.1 App Store — Tạo IAP

> Đã có hướng dẫn chi tiết trong [App Store Config — Bước 4](/docs/appstore-config/01-app-store-config.md#4-cấu-hình-in-app-purchases--subscriptions).

Tóm tắt nhanh:

1. **App Store Connect → My Apps → chọn app → In-App Purchases → +**
2. Tạo từng product:

| Product | Type | Identifier | Price |
|---|---|---|---|
| Premium Monthly | Auto-Renewable | `com.hankbui.businesslaw.premium.monthly` | $9.99 |
| Premium Yearly | Auto-Renewable | `com.hankbui.businesslaw.premium.yearly` | $79.99 |
| Lifetime | Non-Consumable | `com.hankbui.businesslaw.lifetime` | $149.99 |

3. **Subscription Group:** Đặt monthly + yearly vào cùng group "Premium"
4. **Localization:** Điền Display Name + Description cho từng ngôn ngữ
5. **Introductory Offer:** Cấu hình free trial (VD: 7 ngày free → pay)

### 4.2 Google Play — Tạo Products

1. **Google Play Console → Monetize → Products → Subscriptions / In-app Products**
2. Tạo product tương ứng:

**Subscriptions:**

| Product ID | Title | Price | Billing Period |
|---|---|---|---|
| `premium_monthly` | Premium Monthly | $9.99 | Monthly |
| `premium_yearly` | Premium Yearly | $79.99 | Yearly |

**In-app Products (Non-consumable):**

| Product ID | Title | Price |
|---|---|---|
| `lifetime` | Lifetime Premium | $149.99 |

3. **Base plan:** Cấu hình base plan cho subscription
   - Monthly period / Yearly period
   - Pricing: set giá

### 4.3 Lưu ý khi tạo Product

> ⚠️ **Product ID trên App Store và Google Play KHÔNG cần giống nhau.** RevenueCat map chúng qua **Store-Specific IDs** (xem Bước 5).

| Nên dùng convention | App Store | Google Play |
|---|---|---|
| Monthly | `com.hankbui.app.premium.monthly` | `premium_monthly` |
| Yearly | `com.hankbui.app.premium.yearly` | `premium_yearly` |

---

## 5. Kết nối App Store & Google Play với RevenueCat

### 5.1 Kết nối App Store

1. **RevenueCat Dashboard → Project → App Store Connect → Configure**
2. Cần cung cấp:

| Thông tin | Cách lấy |
|---|---|
| **App-Specific Shared Secret** | App Store Connect → My Apps → chọn app → App Information → App-Specific Shared Secret (copy) |
| **Bundle ID** | Tự động có từ bước tạo project |

3. Bấm **Save**

### 5.2 Kết nối Google Play

1. **RevenueCat Dashboard → Project → Google Play → Configure**
2. Cần cung cấp:

| Thông tin | Cách lấy |
|---|---|
| **Service Account JSON Key** | Google Play Console → Setup → API Access → Service Account → Create → Download JSON |
| **Package Name** | Tự động có |

3. **Cách lấy Service Account JSON Key:**
   - Google Play Console → **Setup → API Access**
   - Bấm **Create Service Account** → làm theo hướng dẫn
   - Trong Google Cloud Console → **APIs & Services → Credentials**
   - Tạo key mới → **JSON format → Download**
   - Upload file JSON lên RevenueCat
4. Bấm **Save**

### 5.3 Map Products (Store-Specific IDs)

Sau khi kết nối store, RevenueCat sẽ tự động fetch products từ App Store và Google Play. Bạn cần map:

1. **RevenueCat Dashboard → Products → + Add Product**
2. Chọn từng store product → map sang RevenueCat product:

| RevenueCat Product ID | Apple ID | Google Play Product ID | Entitlement |
|---|---|---|---|
| `rc_premium_monthly` | `com.hankbui.businesslaw.premium.monthly` | `premium_monthly` | `premium` |
| `rc_premium_yearly` | `com.hankbui.businesslaw.premium.yearly` | `premium_yearly` | `premium` |
| `rc_lifetime` | `com.hankbui.businesslaw.lifetime` | `lifetime` | `premium` |

> **Identifier convention:** `rc_premium_monthly`, `rc_premium_yearly` — dùng prefix `rc_` để phân biệt với store IDs.

---

## 6. Tạo Offerings & Packages

### 6.1 Offering là gì?

**Offering** = một nhóm **Packages** mà bạn offer cho người dùng. Bạn có thể có nhiều offerings cho các mục đích khác nhau.

```
Offering "default"
  ├── Package "monthly"    →  $9.99/month
  ├── Package "yearly"     →  $79.99/year (best value)
  └── Package "lifetime"   →  $149.99 (one-time)
```

### 6.2 Tạo Packages

1. **RevenueCat Dashboard → Offerings → Packages → + New**
2. Điền:

| Trường | Ví dụ |
|---|---|
| **Identifier** | `monthly` |
| **Description** | Premium Monthly |
| **Platform** | iOS + Android |
| **iOS Product** | `rc_premium_monthly` |
| **Android Product** | `rc_premium_monthly` |

### 6.3 Tạo Offerings

1. **RevenueCat Dashboard → Offerings → + New**
2. Điền:

| Trường | Ví dụ |
|---|---|
| **Identifier** | `default` |
| **Description** | Default Offering |

3. Thêm các packages vào offering:
   - `monthly` (đánh dấu là **default** package)
   - `yearly`
   - `lifetime`

### 6.4 Multiple Offerings (2026)

RevenueCat 2026 hỗ trợ **multi-offering** — bạn có thể có:

| Offering | Dùng cho | Packages |
|---|---|---|
| `default` | User thường | monthly, yearly, lifetime |
| `sale_black_friday` | Black Friday | yearly_discounted, lifetime_discounted |
| `trial_extended` | User từ chối pay | monthly (trial 14 ngày) |

### 6.5 A/B Testing với Offerings (Experiments)

RevenueCat có **Experiments** — A/B test offerings:

1. **Dashboard → Experiments → Create Experiment**
2. Chọn variant:
   - **Baseline:** offering `default`
   - **Variant A:** offering `trial_extended`
3. Chọn % user cho mỗi variant
4. Xem kết quả: conversion rate, revenue

---

## 7. Tích hợp SDK RevenueCat

### 7.1 Cài đặt SDK

**React Native / Expo:**

```bash
npm install react-native-purchases
npx pod-install               # iOS
```

**iOS (Swift):**
```swift
// Podfile
pod 'Purchases', '~> 7.0'
```

**Android (Kotlin):**
```kotlin
// build.gradle
implementation "com.revenuecat.purchases:purchases:7.0.0"
```

**Flutter:**
```bash
flutter pub add purchases_flutter
```

### 7.2 Cấu hình SDK

**React Native:**

```typescript
import Purchases from 'react-native-purchases';

// App startup
await Purchases.configure({
  apiKey: 'public_ios_key',      // Public key từ RevenueCat Dashboard
  appUserID: userID,             // User ID từ hệ thống của bạn (optional)
});
```

**iOS:**
```swift
import Purchases

Purchases.configure(withAPIKey: "public_ios_key", appUserID: userID)
```

**Android:**
```kotlin
import com.revenuecat.purchases.Purchases

Purchases.configure(PurchasesConfiguration.Builder(context, "public_android_key").build())
```

### 7.3 API Keys — Phân biệt

| Key | Dùng ở đâu | Ghi chú |
|---|---|---|
| **Public SDK Key** | Client app (iOS/Android) | Prefix `public_` |
| **Secret Key** | Backend server | Prefix `sk_` — **tuyệt đối không ở client** |

RevenueCat 2026: Mỗi platform có key riêng:
- `public_ios_key` — cho iOS SDK
- `public_android_key` — cho Android SDK

> Lấy keys tại: **RevenueCat Dashboard → Project Settings → API Keys**.

### 7.4 Lấy Offerings & Purchase

```typescript
// Lấy offerings
const offerings = await Purchases.getOfferings();
if (offerings.current !== null) {
  const monthly = offerings.current.monthly;  // Package
  const yearly = offerings.current.yearly;
}

// Purchase
const { customerInfo } = await Purchases.purchasePackage(monthly);

// Restore purchases
const restore = await Purchases.restorePurchases();
```

### 7.5 Theo dõi Customer Info

```typescript
// Lắng nghe thay đổi
Purchases.addCustomerInfoUpdateListener((customerInfo) => {
  const isPremium = customerInfo.entitlements.active['premium'] !== undefined;
  // Update UI
});
```

### 7.6 Xác thực quyền trong app

```typescript
function checkPremiumAccess(): boolean {
  const customerInfo = await Purchases.getCustomerInfo();
  const premium = customerInfo.entitlements.active['premium'];
  return premium !== undefined;
}
```

---

## 8. Cấu hình Webhooks & Integrations

### 8.1 Webhooks

RevenueCat gửi webhook khi có sự kiện subscription thay đổi. Backend của bạn cần xử lý.

**Cấu hình:**

1. **RevenueCat Dashboard → Integrations → Webhooks → + New**
2. Điền:

| Trường | Ví dụ |
|---|---|
| **Endpoint URL** | `https://api.yourbackend.com/revenuecat/webhook` |
| **Authorization Header** | `Bearer your_secret_token` |
| **Events** | Chọn tất cả |

### 8.2 Webhook Events

RevenueCat 2026 gửi các event sau:

| Event | Mô tả | Khi nào |
|---|---|---|
| `INITIAL_PURCHASE` | Mua lần đầu | User mua subscription |
| `RENEWAL` | Gia hạn tự động | Apple/Google renew |
| `CANCELLATION` | Huỷ (không renew) | User huỷ subscription |
| `UNCANCELLATION` | Huỷ huỷ | User bật lại auto-renew |
| `NON_SUBSCRIPTION_PURCHASE` | Mua non-consumable | VD: mua lifetime |
| `SUBSCRIBER_ALIAS` | Merge user | AppUserID thay đổi |
| `TRANSFER` | Chuyển subscription | Giữa các platform |
| `PRODUCT_CHANGE` | Upgrade/downgrade | Thay đổi plan |
| `EXPIRATION` | Hết hạn | Subscription expired |

### 8.3 Xử lý Webhook (Node.js Example)

```typescript
// POST /revenuecat/webhook
app.post('/revenuecat/webhook', async (req, res) => {
  const event = req.body.event;
  
  switch (event.type) {
    case 'INITIAL_PURCHASE':
    case 'RENEWAL':
      // Cấp quyền premium cho user
      await grantPremiumAccess(event.app_user_id);
      break;
      
    case 'CANCELLATION':
      // Log — user sẽ mất quyền khi hết hạn
      await logCancellation(event.app_user_id);
      break;
      
    case 'EXPIRATION':
      // Thu hồi quyền premium
      await revokePremiumAccess(event.app_user_id);
      break;
  }
  
  res.status(200).send('OK');
});
```

### 8.4 Integrations có sẵn

RevenueCat tích hợp sẵn với:

| Integration | Mục đích | Cấu hình |
|---|---|---|
| **Slack** | Notification khi có sự kiện quan trọng | Webhook URL từ Slack |
| **AppsFlyer** | Attribution + analytics | API key |
| **Adjust** | Attribution | App token |
| **Mixpanel** | Analytics | Project token |
| **Amplitude** | Analytics | API key |
| **Firebase** | Analytics + Cloud Functions | Service account |
| **Airship** | Push notification | App key |
| **HubSpot** | CRM | API key |
| **Stripe** | Kết hợp IAP + Stripe | Webhook secret |

---

## 9. Testing với Sandbox & StoreKit

### 9.1 Sandbox Testing — Apple

1. **App Store Connect → Users and Access → Sandbox Testers → +**
2. Tạo sandbox account:

| Trường | Ví dụ |
|---|---|
| **Name** | Test User |
| **Email** | test@example.com |
| **Password** | Test123! |
| **Territory** | Vietnam |

3. Trên device: **Settings → App Store → Sandbox Account → Sign in với test account**
4. Mua IAP — sẽ qua sandbox, không tính phí thật

### 9.2 Sandbox Testing — Google Play

1. **Google Play Console → Setup → License Testing**
2. Thêm **test accounts** (email)
3. Trên device: add test account vào device → mua subscription → sandbox

### 9.3 StoreKit Configuration (Xcode, 2026)

Dùng **StoreKit Configuration** file để test local, không cần network:

1. **Xcode → File → New → File → StoreKit Configuration**
2. Đặt tên: `StoreKitConfig.storekit`
3. Thêm product matching RevenueCat product IDs
4. **Scheme → Edit Scheme → Options → StoreKit Configuration → chọn file**
5. Chạy app → IAP hoạt động local

### 9.4 RevenueCat Testing Checklist

- [ ] Sandbox purchase thành công trên iOS
- [ ] Sandbox purchase thành công trên Android
- [ ] Subscription renewal trong sandbox (Apple: 3–5 phút = 1 ngày)
- [ ] Subscription cancellation → expiry
- [ ] Restore purchases hoạt động
- [ ] Offerings hiển thị đúng (có/không có premium)
- [ ] Entitlements active sau khi purchase
- [ ] Webhook nhận được events

### 9.5 Environment trong RevenueCat

RevenueCat 2026 có hai environment riêng biệt:

| Environment | Dùng cho | Store |
|---|---|---|
| **Production** | App live trên store | Store thật |
| **Sandbox/Test** | Testing, debug | Sandbox store |

> SDK tự động detect environment — không cần cấu hình thêm.

---

## 10. Analytics & Charts

### 10.1 RevenueCat Dashboard — Tổng quan

**Dashboard** hiển thị real-time metrics:

| Metric | Mô tả |
|---|---|
| **MRR** | Monthly Recurring Revenue |
| **Active Subscribers** | Số subscriber đang active |
| **Trial Conversions** | % user chuyển từ trial → paid |
| **Churn Rate** | % user huỷ subscription |
| **LTV** | Lifetime Value (trung bình) |
| **Revenue** | Tổng doanh thu (30 ngày) |

### 10.2 Charts chi tiết

Vào **RevenueCat Dashboard → Charts**:

**Subscription Chart:**
```
Active Subscribers (30 days)
  ├── iOS: 124
  ├── Android: 89
  └── Total: 213

MRR: $4,532
  ├── iOS: $2,865
  ├── Android: $1,667
  └── Growth: +12% vs last month
```

**Event Timeline:**
- Purchases hằng ngày
- Trials started
- Conversions
- Cancellations
- Expirations

### 10.3 Export Data

RevenueCat cho phép export:

- **CSV Export:** Transactions, subscribers, events
- **API Export:** Pull data qua REST API
- **Integrations:** Push data lên Amplitude, Mixpanel, etc.

### 10.4 Cohorts

Xem theo cohort (tuần/tháng):

| Cohort | Subscribers | Trial → Paid | Churn (30d) | LTV |
|---|---|---|---|---|
| Oct 2026 W1 | 45 | 28% | 8% | $24.50 |
| Sep 2026 W4 | 52 | 32% | 6% | $22.10 |
| Sep 2026 W3 | 38 | 25% | 10% | $19.80 |

---

## 11. Customer Management

### 11.1 Tìm kiếm Customer

**RevenueCat Dashboard → Customers → Search:**

- Search bằng: App User ID, Email, Original App User ID
- Kết quả hiển thị: subscription status, history, events

### 11.2 Customer Detail

Khi click vào customer:

**Subscription Info:**
| Field | Value |
|---|---|
| Active Entitlements | `premium` |
| Active Subscription | Premium Monthly |
| Status | Active / Paused / Expired |
| Next Renewal Date | 2026-11-15 |
| First Purchase | 2026-03-20 |
| Total Lifetime Revenue | $89.91 |

**Transaction History:**
- All purchases, renewals, cancellations
- Store (iOS/Android)
- Receipt data

**Customer Events:**
- Timeline of all events (purchase, renew, cancel, etc.)

### 11.3 Customer Actions

Từ customer detail, bạn có thể:

| Action | Mô tả |
|---|---|
| **Grant Entitlement** | Cấp quyền thủ công (VD: cho beta tester) |
| **Revoke Entitlement** | Thu hồi quyền |
| **Transfer Subscription** | Chuyển từ device này sang device khác |
| **Delete Customer** | Xoá dữ liệu (GDPR compliance) |
| **View Receipt** | Xem receipt gốc từ store |

> **Grant Entitlement** hữu ích khi bạn muốn cấp premium cho user thủ công (VD: test, compensation).

### 11.4 Anonymous Users

RevenueCat hỗ trợ **anonymous user** — tạo user mà không cần appUserID:

```typescript
// Anonymous
await Purchases.configure({ apiKey: 'public_key' });
// RevenueCat tự sinh appUserID
```

Khi user đăng ký tài khoản, bạn có thể **alias** anonymous → identified:

```typescript
await Purchases.logIn('user_id_from_your_system');
```

> **⚠️ Lưu ý:** Khi login, subscription được giữ lại. RevenueCat merge dữ liệu từ anonymous sang identified user.

---

## 12. Subscription Lifecycle & Events

### 12.1 Lifecycle Flow

```
Trial (7 ngày free)
  │
  ├── Hết trial → Paid (nếu convert)
  │       │
  │       ├── Renew (Auto-renewal)
  │       │     │
  │       │     ├── Renew thành công → Active tiếp
  │       │     └── Renew thất bại (lỗi thanh toán)
  │       │           │
  │       │           └── Grace Period → Retry → Expire
  │       │
  │       ├── Cancel (user huỷ)
  │       │     │
  │       │     └── Active đến cuối kỳ → Expire
  │       │
  │       └── Upgrade/Downgrade
  │             └── Chuyển sang product khác
  │
  ├── Hết trial → Expire (không convert)
  │
  └── Cancel trong trial → Expire
```

### 12.2 RevenueCat 2026 — New Events

| Event | Mô tả |
|---|---|
| `PRODUCT_CHANGE` | User upgrade/downgrade subscription |
| `TRANSFER` | Subscription chuyển giữa các platform |
| `PAUSED` | User tạm dừng subscription (Google Play) |
| `GRACE_PERIOD_ENTER` | User vào grace period (lỗi thanh toán) |
| `BILLING_ISSUE` | Sự cố thanh toán |

### 12.3 Grace Period & Billing Retry

| Store | Grace Period | Retry |
|---|---|---|
| **App Store** | 16 days (auto) | Apple retry tự động |
| **Google Play** | Configurable (0–30 ngày) | Google retry tự động |

Trong grace period, user vẫn được dùng premium — RevenueCat vẫn report entitlement là active.

---

## 13. Migration — Chuyển từ IAP hiện tại sang RevenueCat

### 13.1 Khi nào cần migration

- Bạn đang quản lý IAP thủ công (validation tự viết)
- Bạn đang dùng IAP của từng store riêng lẻ
- Bạn muốn thêm cross-platform subscription

### 13.2 Quy trình migration

1. **Tạo project + products trên RevenueCat** — tương tự Bước 1–6
2. **Tích hợp SDK** — nhưng chưa cấu hình purchase
3. **Migrate user data:**
   - Gửi danh sách receipt cũ lên RevenueCat qua **Receipt API**
   - RevenueCat sẽ validate và tạo customer record
4. **Kiểm tra:**
   - So sánh số lượng active subscriber cũ vs mới
   - Restore purchase hoạt động
5. **Switch SDK:** Bật purchase flow mới qua RevenueCat
6. **Tắt code cũ:** Xoá code IAP thủ công

### 13.3 Receipt API — Migrate User

```bash
curl -X POST "https://api.revenuecat.com/v1/receipts" \
  -H "Authorization: Bearer SECRET_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "app_user_id": "user_123",
    "fetch_token": "base64_encoded_receipt",
    "attributes": {
      "$email": { "value": "user@example.com" }
    }
  }'
```

---

## 14. Best Practices & Troubleshooting (2026)

### 14.1 Best Practices

| # | Practice | Mô tả |
|---|---|---|
| 1 | **Giữ identifiers nhất quán** | RevenueCat Product ID, Entitlement ID, store IDs — có naming convention rõ ràng |
| 2 | **Dùng Offerings, không hardcode** | Luôn fetch offerings từ RC, không hardcode product ID trong app |
| 3 | **Listen CustomerInfo updates** | Dùng `addCustomerInfoUpdateListener` để UI luôn đúng |
| 4 | **Xử lý offline** | SDK cache CustomerInfo locally — app hoạt động offline |
| 5 | **Webhook bảo mật** | Verify webhook signature trước khi xử lý |
| 6 | **Test đầy đủ** | Sandbox, StoreKit config, TestFlight |
| 7 | **Monitor Dashboard** | Kiểm tra MRR, churn, LTV hằng tuần |
| 8 | **Backup Secret Key** | Lưu secret key ở nơi an toàn (1Password, vault) |

### 14.2 Common Issues

| Issue | Cause | Fix |
|---|---|---|
| **User purchase nhưng không có entitlement** | Receipt chưa được validate | Kiểm tra network, gọi `restorePurchases()` |
| **Subscription không auto-renew** | User huỷ trong store | Đây là behavior đúng — check status |
| **Product không hiển thị** | Offering chưa cấu hình đúng | Kiểm tra offering → package → product mapping |
| **Webhook không nhận event** | URL sai hoặc authentication | Kiểm tra endpoint, resend từ dashboard |
| **Sandbox purchase lỗi** | StoreKit config sai | Kiểm lại configuration file |
| **App rejected — IAP không dùng App Store** | RevenueCat dùng StoreKit của Apple | Đã đúng — Apple OK với RC |

### 14.3 RevenueCat 2026 — New Features

| Feature | Mô tả |
|---|---|
| **Experiments 2.0** | A/B test offerings with statistical significance |
| **Promotional Offers** | Tạo offer tạm thời cho từng user |
| **Payouts Dashboard** | Xem payout từ store trực tiếp trên RC |
| **AI Insights** | ML dự đoán churn, suggest retention offers |
| **Cross-Platform Transfer** | User chuyển subscription iOS → Android tự động |

---

## 15. Checklist Go-Live

### 15.1 Configuration Checklist

- [ ] RevenueCat Project created + configured
- [ ] App Store Connect IAP products created
- [ ] Google Play products/subscriptions created
- [ ] App Store **App-Specific Shared Secret** connected
- [ ] Google Play **Service Account JSON** connected
- [ ] Products mapped in RevenueCat (Store-Specific IDs)
- [ ] Entitlements created (`premium`, `pro`, etc.)
- [ ] Offerings created (`default` with packages)
- [ ] Packages linked to store products

### 15.2 SDK Checklist

- [ ] SDK installed + configured with public key
- [ ] `getOfferings()` returns correct offerings
- [ ] `purchasePackage()` works
- [ ] `restorePurchases()` works
- [ ] `getCustomerInfo()` returns correct entitlements
- [ ] `addCustomerInfoUpdateListener` integrated
- [ ] UI reflects entitlement changes in real-time
- [ ] Anonymous → Identified user migration (`logIn`)

### 15.3 Backend Checklist

- [ ] Webhook endpoint created
- [ ] Webhook events processed: INITIAL_PURCHASE, RENEWAL, CANCELLATION, EXPIRATION
- [ ] Grant/revoke premium access on webhook events
- [ ] Webhook signature verified
- [ ] Secret key stored securely (not in code)
- [ ] API rate limiting considered

### 15.4 Testing Checklist

- [ ] Sandbox purchase on iOS
- [ ] Sandbox purchase on Android
- [ ] Subscription renewal (sandbox)
- [ ] Subscription cancellation
- [ ] Upgrade/Downgrade subscription
- [ ] Restore purchases
- [ ] Trial → Paid conversion
- [ ] Grace period + billing retry
- [ ] Offline mode (cache CustomerInfo)
- [ ] Webhook received all events

### 15.5 Go-Live

1. [ ] Switch RevenueCat SDK to **Production** environment (auto)
2. [ ] Remove old IAP code (nếu migration)
3. [ ] Submit app for App Review
4. [ ] Monitor Dashboard trong 48h đầu
5. [ ] Check webhook events after first real purchase

---

## 16. Nguồn tham khảo

| Nguồn | URL |
|---|---|
| RevenueCat Dashboard | https://app.revenuecat.com |
| RevenueCat Docs | https://www.revenuecat.com/docs |
| RevenueCat API Reference | https://www.revenuecat.com/reference |
| RevenueCat GitHub | https://github.com/RevenueCat |
| RevenueCat SDK (React Native) | https://github.com/RevenueCat/react-native-purchases |
| RevenueCat SDK (iOS) | https://github.com/RevenueCat/purchases-ios |
| RevenueCat SDK (Android) | https://github.com/RevenueCat/purchases-android |
| RevenueCat SDK (Flutter) | https://github.com/RevenueCat/purchases-flutter |
| RevenueCat Pricing | https://www.revenuecat.com/pricing |
| RevenueCat Migration Guide | https://www.revenuecat.com/docs/migrating |
| RevenueCat Webhooks | https://www.revenuecat.com/docs/webhooks |
| RevenueCat Experiments | https://www.revenuecat.com/docs/experiments |

---

> **Xem thêm:** [App Store Config Guide — Bước 4](/docs/appstore-config/01-app-store-config.md#4-cấu-hình-in-app-purchases--subscriptions) để hướng dẫn tạo IAP trên App Store Connect chi tiết hơn.