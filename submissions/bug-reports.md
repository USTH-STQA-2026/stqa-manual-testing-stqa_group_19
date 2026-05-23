# Bug Reports — Báo cáo lỗi

> **Hướng dẫn**: Tạo 1 mục bug cho mỗi TC có kết quả **Fail**.
> Xem [examples/sample-bug-report.md](../examples/sample-bug-report.md) để hiểu cách viết bug report tốt.
> Mỗi bug cần: tiêu đề mô tả hành vi lỗi, bước tái hiện, expected vs actual, severity + giải thích.

| Thông tin | |
|---|---|
| **Nhóm** | Nhóm 19 |
| **Ngày báo cáo** | 22/05/2026 |

---

## BUG-01

| Thuộc tính | Chi tiết |
|-----------|---------|
| **Mã lỗi** | BUG-01 |
| **TC liên quan** | TC-22, TC-23 |
| **REQ liên quan** | REQ-03 |
| **Mức độ** | Low |
| **Người phát hiện** | Nguyễn Tùng Dương |
| **Ngày phát hiện** | 22/05/2026 |
| **Trạng thái** | Open |

**Tiêu đề:**
Lọc thể loại phân biệt chữ hoa/thường gây lỗi "Không tìm thấy sách"

**Môi trường:**
- Trình duyệt: Chrome 136.0.7103.93
- Hệ điều hành: Windows 11
- Ngôn ngữ giao diện: Tiếng Việt

**Điều kiện tiên quyết:**
Đã đăng nhập thành công. Đang ở tab "Sách". Ô tìm kiếm đang rỗng.

**Bước tái hiện:**
1. Nhập từ khóa `công nghệ` (toàn chữ thường) hoặc `CÔNG NGHỆ` (toàn chữ HOA) vào ô nhập lọc thể loại.
2. Quan sát kết quả danh sách sách được hiển thị.

**Kết quả mong đợi:**
Hệ thống lọc không phân biệt hoa/thường (case-insensitive) giống như ô tìm kiếm, trả về các sách thuộc thể loại "Công nghệ".

**Kết quả thực tế:**
Danh sách sách trống, hiển thị thông báo "Không tìm thấy sách". Ô lọc hiện tại đang phân biệt chữ hoa chữ thường.

**Tác động:**
Gây nhầm lẫn cho người dùng khi gõ chữ thường/hoa, mang lại trải nghiệm không đồng nhất với ô tìm kiếm (vốn không phân biệt hoa thường).

**Minh chứng:**
—

**Đề xuất xử lý:**
Bổ sung hàm chuẩn hóa chữ thường (VD: `toLowerCase()`) cho cả từ khóa lọc và dữ liệu thể loại sách trước khi so sánh.

---

## BUG-02

| Thuộc tính | Chi tiết |
|-----------|---------|
| **Mã lỗi** | BUG-02 |
| **TC liên quan** | TC-26 |
| **REQ liên quan** | REQ-03 |
| **Mức độ** | Medium |
| **Người phát hiện** | Hà Đăng Huy |
| **Ngày phát hiện** | 22/05/2026 |
| **Trạng thái** | Open |

**Tiêu đề:**
Kết hợp tìm kiếm và lọc hoạt động không nhất quán khi ô tìm kiếm không có kết quả

**Môi trường:**
- Trình duyệt: Chrome 136.0.7103.93
- Hệ điều hành: Windows 11
- Ngôn ngữ giao diện: Tiếng Việt

**Điều kiện tiên quyết:**
Đã đăng nhập thành công. Đang ở tab "Sách".

**Bước tái hiện:**
1. Nhập `Kinh tế` vào ô lọc thể loại (có sách thuộc thể loại Kinh tế).
2. Nhập `Flutter` vào ô tìm kiếm.
3. Quan sát kết quả danh sách sách.

**Kết quả mong đợi:**
Hệ thống áp dụng logic "AND" một cách nhất quán (như đã thấy ở TC-25). Do không có sách nào tên "Flutter" trong thể loại "Kinh tế", hệ thống phải trả về danh sách rỗng và hiển thị "Không tìm thấy sách".

**Kết quả thực tế:**
Hệ thống dường như bỏ qua điều kiện tìm kiếm, tiếp tục hiển thị toàn bộ sách thuộc thể loại "Kinh tế" (BOOK007, BOOK014, BOOK015).

**Tác động:**
Logic tìm kiếm kết hợp lọc bị lỗi, trả về sai kết quả khi một trong hai điều kiện không khớp, làm sai lệch kỳ vọng của người dùng.

**Minh chứng:**
—

**Đề xuất xử lý:**
Cập nhật logic lấy danh sách sách: Phải áp dụng đồng thời (AND) cả 2 filter. Nếu một trong 2 không thỏa mãn, list kết quả cuối cùng phải là rỗng.

---
