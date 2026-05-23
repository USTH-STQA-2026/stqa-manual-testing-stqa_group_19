bash

# Test Execution — Kết quả thực thi kiểm thử

> **Hướng dẫn**: Chạy từng TC trên hệ thống https://stqa.rbc.vn, ghi lại kết quả thực tế.
> Kết luận: **Pass** (kết quả đúng), **Fail** (kết quả sai → tạo bug report), **Blocked** (không thực hiện được vì lỗi khác chặn), **Not Run** (chưa chạy).

| Thông tin        |                      |
| ---------------- | -------------------- |
| **Nhóm**         | Nhóm 19              |
| **Ngày thực thi**| 22/05/2026           |
| **Trình duyệt**  | Chrome 136.0.7103.93 |
| **Hệ điều hành** | Windows 11           |

---

## Kết quả chi tiết

### REQ-01 — Đăng nhập

| Mã TC | Nhóm chức năng | Kết quả mong đợi (tóm tắt)                               | Kết quả thực tế                                                                                             | Kết luận | Minh chứng | Bug    |
| ----- | -------------- | --------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------- | -------- | ---------- | ------ |
| TC-01 | Đăng nhập      | Chuyển trang chủ, AppBar hiển thị "Thủ thư"              | Hệ thống chuyển sang trang chủ. AppBar hiển thị tên và vai trò "Thủ thư". Tab "Thành viên" xuất hiện.      | **Pass** | —          | —      |
| TC-02 | Đăng nhập      | Chuyển trang chủ, AppBar hiển thị "Thành viên"           | Hệ thống chuyển sang trang chủ. AppBar hiển thị "Ba Nguyễn — Thành viên". Tab "Thành viên" không hiển thị. | **Pass** | —          | —      |
| TC-03 | Đăng nhập      | "Không tìm thấy thành viên"                              | Hệ thống không chuyển trang. Hiển thị thông báo "Không tìm thấy thành viên".                               | **Pass** | —          | —      |
| TC-04 | Đăng nhập      | "Mật khẩu không đúng"                                    | Hệ thống không chuyển trang. Hiển thị thông báo "Mật khẩu không đúng".                                     | **Pass** | —          | —      |
| TC-05 | Đăng nhập      | "Vui lòng nhập email và mật khẩu"                        | Hệ thống không chuyển trang. Hiển thị thông báo "Vui lòng nhập email và mật khẩu".                         | **Pass** | —          | —      |
| TC-06 | Đăng nhập      | "Vui lòng nhập email và mật khẩu"                        | Hệ thống không chuyển trang. Hiển thị thông báo "Vui lòng nhập email và mật khẩu".                         | **Pass** | —          | —      |
| TC-07 | Đăng nhập      | "Vui lòng nhập email và mật khẩu"                        | Hệ thống không chuyển trang. Hiển thị thông báo "Vui lòng nhập email và mật khẩu".                         | **Pass** | —          | —      |

---

### REQ-03 — Tìm kiếm và lọc sách (TC có căn cứ SRS)

| Mã TC | Nhóm chức năng     | Kết quả mong đợi (tóm tắt)                                       | Kết quả thực tế                                                                                               | Kết luận | Minh chứng | Bug |
| ----- | ------------------ | ----------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------- | -------- | ---------- | --- |
| TC-10 | Tìm kiếm & lọc    | Chỉ hiển thị BOOK001 khi tìm "Flutter"                           | Danh sách chỉ hiển thị BOOK001 "Lập trình Flutter cơ bản". Các sách khác bị ẩn.                             | **Pass** | —          | —   |
| TC-11 | Tìm kiếm & lọc    | Hiển thị BOOK001, BOOK009 khi tìm "Nguyễn Minh Đức"             | Danh sách hiển thị BOOK001 và BOOK009. Sách của tác giả khác bị ẩn.                                         | **Pass** | —          | —   |
| TC-12 | Tìm kiếm & lọc    | "Không tìm thấy sách" khi tìm "XYZ123abc"                       | Danh sách không hiển thị sách nào. Hiển thị thông báo "Không tìm thấy sách".                                | **Pass** | —          | —   |
| TC-13 | Tìm kiếm & lọc    | Kết quả giống TC-10 khi tìm "flutter" (chữ thường)              | Danh sách hiển thị BOOK001. Tìm kiếm không phân biệt hoa/thường.                                            | **Pass** | —          | —   |
| TC-14 | Tìm kiếm & lọc    | Kết quả giống TC-10 khi tìm "FLUTTER" (chữ HOA)                 | Danh sách hiển thị BOOK001. Tìm kiếm không phân biệt hoa/thường.                                            | **Pass** | —          | —   |
| TC-15 | Tìm kiếm & lọc    | Kết quả giống TC-10 khi tìm "fLuTtEr" (hoa lẫn lộn)            | Danh sách hiển thị BOOK001. Tìm kiếm không phân biệt hoa/thường.                                            | **Pass** | —          | —   |
| TC-16 | Tìm kiếm & lọc    | Hiển thị lại 20 sách khi xóa ô tìm kiếm thành rỗng              | Danh sách hiển thị lại toàn bộ 20 đầu sách. Không còn lọc theo từ khóa nào.                                 | **Pass** | —          | —   |
| TC-17 | Tìm kiếm & lọc    | Chỉ hiển thị sách Công nghệ khi nhập "Công nghệ" vào ô lọc      | Danh sách chỉ hiển thị BOOK001, 002, 003, 005, 008, 009, 010, 011. Sách thể loại khác bị ẩn.                | **Pass** | —          | —   |
| TC-22 | Tìm kiếm & lọc    | Lọc "công nghệ" (chữ thường)                                   | Danh sách không hiển thị sách nào. Ô lọc phân biệt hoa/thường.                                            | **Fail** | —          | BUG-01 |
| TC-23 | Tìm kiếm & lọc    | Lọc "CÔNG NGHỆ" (chữ HOA)                                      | Danh sách không hiển thị sách nào. Ô lọc phân biệt hoa/thường.                                            | **Fail** | —          | BUG-01 |

---

### REQ-03 — Ghi nhận SRS Gap (TC-25 → TC-26)

> Các TC này được thực thi để **quan sát hành vi thực tế**, không áp đặt expected result từ SRS vì SRS không đặc tả.
> - **TC-25, TC-26**: Kiểm tra hành vi khi dùng tìm kiếm + lọc đồng thời (SRS không quy định AND/OR logic).

| Mã TC | Nhóm chức năng     | Quan sát (không phải expected result)                                         | Kết quả thực tế                                                                                                                                           | Kết luận                    | Minh chứng | Bug    |
| ----- | ------------------ | ----------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------- | ---------- | ------ |
| TC-25 | SRS Gap — kết hợp  | Quan sát: lọc "Công nghệ" + tìm "Python" — hệ thống dùng AND hay OR?        | Danh sách hiển thị **chỉ BOOK009** "Nhập môn lập trình Python". Xác nhận hệ thống dùng **AND logic** (lọc thể loại VÀ tìm kiếm đều được áp dụng).       | **Pass** *(SRS gap — quan sát)* | —      | —      |
| TC-26 | SRS Gap — kết hợp  | Quan sát: lọc "Kinh tế" + tìm "Flutter" — kết quả khi AND không có sách nào? | Danh sách hiển thị **BOOK007, BOOK014, BOOK015** (toàn bộ sách Kinh tế). Ô tìm kiếm "Flutter" bị **bỏ qua** khi không có kết quả khớp trong thể loại — hành vi **không nhất quán** với TC-25 (AND logic). | **Fail** *(SRS gap)*        | —          | BUG-02 |

---

## Tổng hợp kết quả

| Chỉ số              | Giá trị |
| ------------------- | ------- |
| Tổng số test case   | 19      |
| Pass                | 16      |
| Fail                | 3       |
| Blocked             | 0       |
| Not Run             | 0       |
| **Tỷ lệ Pass**      | **84.2%** |

### Kết quả theo nhóm chức năng

| Nhóm                                      | Tổng TC | Pass | Fail | Tỷ lệ Pass |
| ----------------------------------------- | ------- | ---- | ---- | ---------- |
| REQ-01 — Đăng nhập                        | 7       | 7    | 0    | 100%       |
| REQ-03 — Tìm kiếm & lọc (có SRS)         | 10      | 8    | 2    | 80%        |
| REQ-03 — SRS Gap (TC-25 → TC-26)         | 2       | 1    | 1    | 50%        |
| **Tổng**                                  | **19**  | **16** | **3** | **84.2%** |

---

> ### 📝 Ghi chú tổng hợp
>
> **Lưu ý về các test case Fail:**
>
> 1. **BUG-01** (TC-22, TC-23): Ô lọc thể loại **phân biệt hoa/thường**. Người dùng nhập "công nghệ" → không tìm thấy sách. SRS chỉ yêu cầu case-insensitive cho ô *tìm kiếm*, không đề cập ô lọc. Việc thiếu đồng nhất này có thể gây hiểu lầm cho người dùng. Khuyến nghị: BA cần bổ sung đặc tả, Dev nên xử lý case-insensitive cho cả ô lọc để đồng nhất UX.
>
> 2. **BUG-02** (TC-26): Hành vi kết hợp tìm kiếm + lọc **không nhất quán** — TC-25 xác nhận AND logic hoạt động khi có kết quả, nhưng TC-26 cho thấy khi AND không có kết quả, hệ thống **bỏ qua ô tìm kiếm** và chỉ hiển thị kết quả lọc thể loại. SRS không đặc tả trường hợp này — đây là SRS gap và đồng thời là lỗi logic không nhất quán.
