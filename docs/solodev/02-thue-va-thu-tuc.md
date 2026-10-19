# 02 — Thuế Việt Nam, thủ tục và checklist cho solo dev

> ⚖️ **Miễn trừ:** Thông tin tham khảo, không phải tư vấn thuế/pháp lý. Mốc dữ liệu **19/10/2026** — quy định 2025–2026 có nhiều thay đổi lớn so với các bài viết trước đó. Kiểm tra lại với **Chi cục Thuế nơi cư trú / đại lý thuế** trước khi kê khai.

## 1. Thu nhập từ bán app là loại thu nhập gì?

**Khoản 1 Điều 3 Luật Thuế TNCN** — bán app qua App Store/Google Play là **"thu nhập từ kinh doanh"** của cá nhân (kinh doanh dịch vụ/phần mềm), **không** phải loại thu nhập riêng "từ nước ngoài".

- Thông tư 40/2021/TT-BTC (Điều 2.1đ): bao gồm hoạt động **thương mại điện tử, sản phẩm/dịch vụ nội dung thông tin số**.
- Cơ quan thuế (thực tế trả lời tỉnh Hưng Yên, Báo Chính phủ 8/2025) xác nhận cá nhân/sở hữu hộ cho thuê phần mềm trực tuyến phải đăng ký, kê khai theo TT 40/2021.
- **Doanh thu tính thuế = toàn bộ giá trị người dùng trả (gross)**, trước khi trừ hoa hồng Apple/Google (Công văn 1063/CT-CS).
- **Không có căn cứ miễn trừ** vì nhận tiền từ nước ngoài — ngành thuế đã và đang rà soát dev nhận tiền từ App Store/Google Play.

## 2. Ngưỡng miễn thuế mới năm 2026 — người bạn lớn nhất của indie dev

→**01 tỷ đồng/năm.**

Chuỗi văn bản: Luật GTGT 48/2024 nâng ngưỡng hộ kinh doanh từ 100 triệu → 200 triệu (hiệu lực 1/1/2026) → Luật 149/2025 sửa đổi → NĐ 68/2026 (ban đầu 500 triệu) → **NĐ 141/2026/NĐ-CP (29/4/2026, hồi tố 1/1/2026): ngưỡng 1 tỷ đồng/năm**.

| Doanh thu năm | Nghĩa vụ |
|---|---|
| **≤ 1 tỷ đồng** | **Miễn thuế GTGT + TNCN**; nhưng **PHẢI thông báo doanh thu thực tế** cho cơ quan thuế. Hộ vượt ngưỡng giữa năm → chuyển kê khai từ quý vượt. |
| **> 1 tỷ đồng** | Kê khai GTGT + TNCN theo quý (mẫu 01/CNKD trên eTax), quyết toán năm; bắt buộc dùng **hóa đơn điện tử**; thuế tính trên **toàn bộ** doanh thu (cửa, không phải bậc). |

> Đã **bãi bỏ lệ phí môn bài** từ 1/1/2026 (Nghị quyết 198/2025/QH15, NĐ 362/2025) — hộ KD không phải nộp/khai môn bài nữa.

## 3. Tỷ lệ thuế khi doanh thu > 1 tỷ

| Loại hoạt động | GTGT | TNCN | Ghi chú |
|---|---|---|---|
| **Cung cấp/cho thuê phần mềm, app tính phí** | **0–5%** (0% nếu đạt chuẩn sản phẩm phần mềm theo Luật CNTT — **không chịu GTGT**; còn không thì 5%) | **2%** | Trả lời Thuế Hưng Yên + TT 40/2021 Phụ lục I |
| **Nội dung số / game / nhạc, phim, ảnh số, quảng cáo số** | **5%** | **5%** | Luật TNCN 109/2025, Điều 7.3 |

Bảng tỷ lệ chung theo ngành (Luật TNCN 109/2025 Điều 7): phân phối hàng hóa 0,5% · dịch vụ 2% · sản xuất/vận tải 1,5% · nội dung số 5% · cho thuê tài sản 5% · khác 1%.

⚠️ **Điểm "vùng xám" quan trọng:** phân loại **app tính phí = "dịch vụ phần mềm" (2%)** vs **"nội dung số" (5%)** vs **thu nhập bản quyền (5%)** — cơ quan thuế **chưa có hướng dẫn thống nhất** (câu hỏi gửi Bộ Tài chính 4/2026 vẫn chưa có trả lời chính thức đến 10/2026). **Chọn một cách, kê khai nhất quán, và xin xác nhận từ Cục Thuế nơi bạn ở.**

## 4. Nghĩa vụ của Apple/Google — đừng trông chờ

- Apple/Google nộp thuế riêng theo cơ chế **nhà cung cấp nước ngoài** (GTGT 10% + TNDN 5% trên doanh thu phát sinh tại VN) cho **phần hoa hồng của họ** — qua Cổng thông tin nhà cung cấp nước ngoài của Tổng cục Thuế (Luật GTGT 48/2024, Luật TNDN 67/2025, hiệu lực 7/2025–10/2025).
- **Phần trả cho bạn: bạn tự kê khai, tự nộp.** NĐ 117/2025/NĐ-CP có cơ chế nền tảng TMĐT "khấu trừ, khai thay, nộp thay" thuế GTGT+TNCN cho hộ/cá nhân — nhưng chỉ cho nền tảng **có chức năng đặt hàng + thanh toán** (Shopee/Amazon...); **việc Apple/Google có bị xếp vào nhóm này cho dev VN hay không đang là vùng xám** — hãy tự kê khai và theo dõi hướng dẫn mới.

## 5. Nhận tiền, ngân hàng, ngoại tệ

- **Được phép nhận** chuyển tiền quốc tế (SWIFT) từ Apple về tài khoản cá nhân — nguồn thu vãng lai từ **xuất khẩu dịch vụ phần mềm** (NĐ 70/2014/NĐ-CP Điều 4, 5.1).
- Không bắt buộc tài khoản ngoại tệ, nhưng nên nhận vào tài khoản ngoại tệ/VND và **bán USD cho ngân hàng** khi cần (đổi ngoài hệ thống bị phạt).
- **Không cần tự khai báo NHNN** khi nhận tiền. Ngân hàng **báo cáo giao dịch lớn ≥ ~400 triệu VND/lần** (Luật Phòng chống rửa tiền 2022, TT 27/2025/TT-NHNN) → tiền lớn sẽ bị hỏi nguồn gốc. "50.000 USD/lần" không phải hạn mức nhận, mà là **trần chuyển tiền ra nước ngoài** (TT 20/2022/TT-NHNN).
- **Tỷ giá quy đổi khi kê khai:** doanh thu ngoại tệ phải quy đổi VND — nguyên tắc phổ biến là **tỷ giá mua vào của NHTM nơi bạn mở tài khoản tại thời điểm phát sinh**; có sự khác biệt giữa các hướng dẫn → **lưu phiếu báo có kèm tỷ giá** và xác nhận với cơ quan thuế.
- **Chứng từ cần giữ mỗi lần nhận tiền:** báo cáo Payments/Financial Reports từ App Store Connect + sao kê/phiếu báo có + hợp đồng Apple (Apple Developer Program License Agreement).

## 6. Đăng ký: cá nhân → hộ kinh doanh → công ty

| Tiêu chí | Cá nhân không đăng ký | Hộ kinh doanh | TNHH 1 thành viên / DNTN |
|---|---|---|---|
| Thích hợp khi | Thu nhập nhỏ, app phụ | Doanh thu ổn định vượt ngưỡng, cần MST giải trình | Doanh thu vài tỷ, có chi phí/nhân sự, cần khấu trừ chi phí |
| Thuế khi ≤ 1 tỷ/năm | Miễn GTGT+TNCN (phải thông báo doanh thu) | Như trái | **Miễn TNDN** doanh thu ≤ 1 tỷ (NĐ 141/2026) |
| Thuế khi > 1 tỷ/năm | TNCN 2–5% + GTGT 0–5% trên doanh thu | Như trái (hoặc tính trên thu nhập thực: 15–20%) | TNDN **20% trên lợi nhuận** (+ TNCN khi rút lãi) |
| Khấu trừ chi phí (phí Apple 15–30%, công cụ...) | Không | Hạn chế | **Có** (nếu có chứng từ) |
| Hóa đơn điện tử | Không bắt buộc (≤ 1 tỷ) | Bắt buộc khi > 1 tỷ | Bắt buộc |
| Trách nhiệm tài sản | Vô hạn | Vô hạn | Hữu hạn |
| Chi phí tuân thủ | Thấp nhất | Thấp | Cao (kế toán, BCTC, quyết toán) |

**Khuyến nghị thực dụng cho indie dev:**
- Doanh thu < 1 tỷ: **không cần công ty**; đăng ký **hộ kinh doanh** nếu doanh thu ổn định để có MST, dễ giải trình ngân hàng, và thông báo doanh thu đúng hạn.
- Vượt 1 tỷ nhưng chưa có chi phí/nhân sự: cân nhắc tiếp tục **hộ kinh doanh** (thuế 0–5% + 2% trên doanh thu đơn giản hơn hạch toán công ty).
- Vài tỷ + muốn khấu trừ chi phí/trách nhiệm hữu hạn/gọi vốn/đấu thầu: chuyển **TNHH MTV** — so sánh bằng mô phỏng cụ thể trước.
- Ngành "lập trình máy tính (6201)" **không thuộc ngành nghề kinh doanh có điều kiện** — không cần giấy phép con để đăng ký hộ/công ty làm app tiêu dùng.

## 7. Checklist định kỳ

### Hàng tháng
- [ ] Xuất báo cáo **Payments & Financial Reports** từ App Store Connect; đối chiếu với **sao kê ngân hàng** (kèm phí, tỷ giá).
- [ ] Lưu phiếu báo có + tỷ giá quy đổi từng khoản nhận.
- [ ] Ghi doanh thu vào sổ/bảng tính (cơ sở cho thông báo cuối năm).
- [ ] Hộ nộp thuế khoán (nếu thuộc diện khoán theo thông báo CCT): nộp đúng hạn trong tháng/quý theo ấn định.

### Hàng quý
- [ ] Hộ thuộc diện kê khai (> 1 tỷ): lập **tờ khai 01/CNKD** trên eTax, nộp thuế GTGT + TNCN — **hạn 30 của tháng đầu quý sau** (30/4, 30/7, 30/10, 30/1).
- [ ] Đối soát doanh thu nền tảng ↔ tiền về tài khoản.

### Hàng năm
- [ ] **Chậm nhất 31/01** năm sau: **thông báo doanh thu thực tế** cho cơ quan thuế (hộ ≤ 1 tỷ).
- [ ] Quyết toán thuế TNCN: cá nhân kinh doanh tự quyết toán hạn **30/4** năm sau.
- [ ] Rà soát ngưỡng 1 tỷ để biết sang năm thuộc nhóm nào, có phải dùng hóa đơn điện tử/đổi phương pháp không.
- [ ] Lưu trữ hồ sơ (tờ khai, sao kê, báo cáo nền tảng) **tối thiểu 10 năm**.

## 8. Sai lầm thường gặp (và hậu quả)

1. **Không kê khai / không đăng ký MST** khi có thu nhập → truy thu nhiều năm + phạt + tiền chậm nộp **0,03%/ngày** (Luật Quản lý thuế Điều 59), tổng có thể gấp nhiều lần thuế gốc.
2. **Doanh thu lẫn trong tài khoản cá nhân**, không tách bạch → khó giải trình, nguy cơ bị cơ quan thuế **ấn định doanh thu**.
3. **Nhận tiền nước ngoài định kỳ không có hồ sơ** → ngân hàng hãm/yêu cầu chứng từ; bị chậm thanh toán với Apple.
4. **Dùng ngoại tệ thanh toán/định giá trong nước hoặc đổi chợ đen** → vi phạm ngoại hối, phạt theo NĐ 88/2019 (nay thay bằng NĐ 340/2025) từ cảnh cáo tới 10–50 triệu đồng.
5. **Kê khai sai nhóm ngành** (khai "hàng hóa" 0,5% thay vì "dịch vụ phần mềm" 2%) → chênh thuế, truy thu, phạt thiếu chính xác.
6. **Chạy theo sách cũ**: ngưỡng "100 triệu/năm" đã thành **1 tỷ** và **lệ phí môn bài đã bãi bỏ** (2026) — khai/thông báo sai thiệt thòi hoặc dính phạt.
7. **Kê khai doanh thu bằng số ngoại tệ gốc / quy đổi sai tỷ giá** → sai cơ sở tính thuế, khó đối chiếu khi thanh tra.
8. **"Né thuế"** bằng ví nước ngoài, chia nhỏ giao dịch, mượn tài khoản người khác → bị coi là **giao dịch đáng ngờ**, tài khoản người thân bị liên lụy, thuế vẫn truy theo thực nhận + phạt.

## Nguồn chính

1. Luật Thuế TNCN 109/2025/QH15 (hiệu lực 1/7/2026) — trích Điều 3, Điều 7
2. Nghị định 68/2026/NĐ-CP và Nghị định 141/2026/NĐ-CP (ngưỡng 1 tỷ, bỏ thuế khoán) — baochinhphu.vn, congbao.chinhphu.vn
3. Nghị định 117/2025/NĐ-CP (khấu trừ/thay nộp qua nền tảng TMĐT) — misa.vn, baochinhphu.vn
4. Nghị định 70/2014/NĐ-CP + Luật Phòng chống rửa tiền 2022 + Thông tư 27/2025/TT-NHNN — luatvietnam.vn, tapchinganhang.gov.vn
5. Thông tư 40/2021/TT-BTC, Thông tư 18/2026/TT-BTC, Nghị định 126/2020/NĐ-CP, Nghị định 125/2020/NĐ-CP
6. Trả lời Thuế tỉnh Hưng Yên trên baochinhphu.vn (8/2025); Công văn 1063/CT-CS (2/2026); hỏi đáp CSTC Bộ Tài chính tpcp.mof.gov.vn

*Các chi tiết vận hành (mẫu tờ khai mới nhất, tỷ giá quy đổi, tỷ lệ chính xác cho từng loại app, khung phạt cụ thể) — kiểm tra lại với cơ quan thuế quản lý trước khi nộp.*