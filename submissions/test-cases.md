# Test Cases — Bảng trường hợp kiểm thử

> **Hướng dẫn**: Viết tối thiểu **20 TC** phủ đủ các chức năng chính (REQ-01 → REQ-08).
> Xem [examples/sample-test-case.md](../examples/sample-test-case.md) để hiểu cách viết TC tốt.
> Tự tổ chức và phân nhóm test case theo cách hợp lý nhất.

| Thông tin      |                        |
| -------------- | ---------------------- |
| **Nhóm**       | Nhóm 19                |
| **Ngày tạo**   | 22/05/2026             |
| **Hệ thống**   | https://stqa.rbc.vn    |
| **Tham chiếu** | SRS v1.0               |

---

## Bước 1: Mô hình hóa miền đầu vào — Input Domain Modeling (IDM)

> 📖 **Textbook:** Chương 6 — *Input Domain Modeling*, Paul Ammann & Jeff Offutt.
>
> **Trước khi viết Test Case**, nhóm **phải** phân tích miền đầu vào bằng bảng IDM bên dưới.
> Mỗi chức năng cần xác định: **Đặc tính (Characteristic)**, **Phân vùng (Block/Partition)**, và **Giá trị đại diện (Value)**.

---

### IDM — Đăng nhập (REQ-01)

> **Nguồn SRS**: REQ-01 — "email + mật khẩu đúng → chuyển trang chủ; sai → thông báo lỗi phù hợp"

| Đặc tính (Characteristic)  | Phân vùng (Block)       | Giá trị đại diện (Value)                           | Kết quả mong đợi (theo SRS)                          |
| -------------------------- | ----------------------- | -------------------------------------------------- | ---------------------------------------------------- |
| Email có tồn tại trong DB? | Có — vai trò Thủ thư    | `librarian@library.com`                            | Đăng nhập thành công, hiển thị vai trò "Thủ thư"    |
|                            | Có — vai trò Thành viên | `ba.nguyen@email.com`                              | Đăng nhập thành công, hiển thị vai trò "Thành viên" |
|                            | Không tồn tại           | `noone@example.com`                                | "Không tìm thấy thành viên"                          |
| Mật khẩu có đúng?          | Đúng                    | `admin123`                                         | Đăng nhập thành công                                 |
|                            | Sai                     | `wrongpass`                                        | "Mật khẩu không đúng"                               |
| Ô nhập có rỗng?            | Cả hai rỗng             | email: `""`, password: `""`                        | "Vui lòng nhập email và mật khẩu"                   |
|                            | Chỉ email rỗng          | email: `""`, password: `admin123`                  | "Vui lòng nhập email và mật khẩu"                   |
|                            | Chỉ mật khẩu rỗng       | email: `librarian@library.com`, password: `""`     | "Vui lòng nhập email và mật khẩu"                   |
| Định dạng email            | Có `@` và domain hợp lệ | `librarian@library.com`                            | Xử lý bình thường                                    |
|                            | Thiếu `@`               | `librarylibrary.com`                               | Không đăng nhập được, báo lỗi                        |

---

### IDM — Tìm kiếm và lọc sách (REQ-03)

> **Nguồn SRS**: REQ-03 — "Tìm kiếm theo tên sách hoặc tác giả; Lọc theo thể loại; Tìm kiếm KHÔNG phân biệt hoa/thường; Không có kết quả → 'Không tìm thấy sách'"
>
> ⚠️ **SRS gap đã xác định:**
> - SRS **không quy định** case-sensitivity cho ô lọc thể loại (chỉ quy định cho tìm kiếm)
> - SRS **không quy định** hành vi khi dùng tìm kiếm và lọc đồng thời (AND, OR, hay độc lập)
> - Các TC liên quan đến hai điểm trên sẽ được ghi rõ là "quan sát thực tế" thay vì kết luận Pass/Fail dứt khoát

| Đặc tính (Characteristic)          | Phân vùng (Block)                        | Giá trị đại diện (Value)     | Kết quả mong đợi (theo SRS)                              |
| ---------------------------------- | ---------------------------------------- | ---------------------------- | -------------------------------------------------------- |
| **Tìm kiếm** — từ khóa khớp DB?   | Có (tên sách)                            | `"Flutter"`                  | Hiển thị BOOK001                                         |
|                                    | Có (tên tác giả)                         | `"Nguyễn Minh Đức"`          | Hiển thị BOOK001, BOOK009                                |
|                                    | Không khớp                               | `"XYZ123abc"`                | "Không tìm thấy sách"                                    |
| **Tìm kiếm** — chữ hoa/thường?    | Toàn chữ thường                          | `"flutter"`                  | Kết quả giống tìm `"Flutter"` *(SRS: case-insensitive)* |
|                                    | Toàn chữ HOA                             | `"FLUTTER"`                  | Kết quả giống tìm `"Flutter"` *(SRS: case-insensitive)* |
|                                    | Hoa lẫn lộn                              | `"fLuTtEr"`                  | Kết quả giống tìm `"Flutter"` *(SRS: case-insensitive)* |
| **Tìm kiếm** — ô rỗng?            | Rỗng                                     | `""`                         | Hiển thị toàn bộ danh sách (không lọc)                   |
| **Lọc thể loại** — ô nhập tay     | Thể loại khớp chính xác                  | `"Công nghệ"`                | Chỉ hiển thị sách thuộc Công nghệ *(SRS: lọc theo thể loại)* |
|                                    | Thể loại khớp chính xác                  | `"Quản trị"`                 | Chỉ hiển thị sách thuộc Quản trị *(SRS: lọc theo thể loại)* |
|                                    | Không khớp thể loại nào                  | `"XYZ"`                      | "Không tìm thấy sách" *(SRS: không có kết quả)*         |
|                                    | Ô lọc rỗng                               | `""`                         | Hiển thị toàn bộ danh sách (không lọc)                   |
| **Lọc thể loại** — chữ hoa/thường | Toàn chữ thường *(SRS không quy định)*   | `"công nghệ"`                | **SRS gap** — quan sát và ghi nhận actual result         |
|                                    | Toàn chữ HOA *(SRS không quy định)*      | `"CÔNG NGHỆ"`                | **SRS gap** — quan sát và ghi nhận actual result         |
| **Kết hợp** tìm kiếm + lọc        | *(SRS không quy định hành vi kết hợp)*   | Lọc `"Kinh tế"` + tìm `"kinh tế"` | **SRS gap** — quan sát và ghi nhận actual result   |

---

### IDM — Mượn sách (REQ-04, REQ-05)

| Đặc tính (Characteristic) | Phân vùng (Block)   | Giá trị đại diện (Value) | Kết quả mong đợi                 |
| ------------------------- | ------------------- | ------------------------ | -------------------------------- |
| Trạng thái sách?          | Có sẵn              | BOOK001                  | Cho phép mượn                    |
|                           | Đang mượn           | BOOK003                  | Không cho phép                   |
|                           | Thất lạc            | BOOK007                  | Không cho phép                   |
| Trạng thái thành viên?    | Hoạt động           | MEM002                   | Cho phép mượn                    |
|                           | Tạm ngưng           | MEM004                   | Từ chối, thông báo lỗi tạm ngưng |
|                           | Hết hạn             | MEM005                   | Từ chối, thông báo lỗi hết hạn   |
| Số sách đang mượn?        | < 3 (BVA: 0, 1, 2)  | MEM006 (0 sách)          | Cho phép mượn                    |
|                           | = 3 (BVA: giới hạn) | MEM đã mượn 3 sách       | Từ chối, thông báo vượt giới hạn |

---

### IDM — REQ-05 đến REQ-08 (nhóm tự bổ sung)

| Đặc tính (Characteristic) | Phân vùng (Block) | Giá trị đại diện (Value) | Kết quả mong đợi |
| ------------------------- | ----------------- | ------------------------ | ---------------- |
| `<!-- Nhóm tự điền -->`   |                   |                          |                  |

---

## Bước 2: Test Cases

---

### REQ-01 — Đăng nhập / Login

> **Kỹ thuật áp dụng:** EP — chia email và mật khẩu thành các lớp hợp lệ / không hợp lệ, mỗi lớp test 1 đại diện.
> **Nguồn expected result:** SRS REQ-01 — cột "Thông báo lỗi" và "Quy tắc".

| Mã TC | Mục tiêu kiểm thử                                          | Tiền điều kiện                                                        | Bước thực hiện                                                                                                   | Dữ liệu đầu vào                                                    | Kết quả mong đợi (trích SRS REQ-01)                                                                                                         | REQ    | Kỹ thuật |
| ----- | ---------------------------------------------------------- | --------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------- | ------ | -------- |
| TC-01 | Đăng nhập thành công với tài khoản Thủ thư                 | Trình duyệt Chrome mở https://stqa.rbc.vn. Chưa đăng nhập.           | 1. Nhập email vào ô "Email". 2. Nhập mật khẩu vào ô "Mật khẩu". 3. Nhấn nút "Đăng nhập".                       | Email: `librarian@library.com` · Mật khẩu: `admin123`             | Hệ thống chuyển sang trang chủ. AppBar hiển thị tên người dùng và vai trò "Thủ thư". Tab "Thành viên" hiển thị (đặc quyền Thủ thư).        | REQ-01 | EP       |
| TC-02 | Đăng nhập thành công với tài khoản Thành viên              | Trình duyệt Chrome mở https://stqa.rbc.vn. Chưa đăng nhập.           | 1. Nhập email vào ô "Email". 2. Nhập mật khẩu vào ô "Mật khẩu". 3. Nhấn nút "Đăng nhập".                       | Email: `ba.nguyen@email.com` · Mật khẩu: `password123`            | Hệ thống chuyển sang trang chủ. AppBar hiển thị tên "Ba Nguyễn" và vai trò "Thành viên". Tab "Thành viên" (quản lý) **không** hiển thị.    | REQ-01 | EP       |
| TC-03 | Đăng nhập thất bại — email không tồn tại                   | Trình duyệt Chrome mở https://stqa.rbc.vn. Chưa đăng nhập.           | 1. Nhập email không có trong hệ thống. 2. Nhập mật khẩu bất kỳ. 3. Nhấn nút "Đăng nhập".                       | Email: `noone@example.com` · Mật khẩu: `admin123`                 | Hệ thống **không** chuyển trang. Hiển thị: **"Không tìm thấy thành viên"**. Trang đăng nhập vẫn hiển thị.                                  | REQ-01 | EP       |
| TC-04 | Đăng nhập thất bại — mật khẩu sai                          | Trình duyệt Chrome mở https://stqa.rbc.vn. Chưa đăng nhập.           | 1. Nhập email hợp lệ. 2. Nhập mật khẩu sai. 3. Nhấn nút "Đăng nhập".                                           | Email: `librarian@library.com` · Mật khẩu: `wrongpass`            | Hệ thống **không** chuyển trang. Hiển thị: **"Mật khẩu không đúng"**. Trang đăng nhập vẫn hiển thị.                                       | REQ-01 | EP       |
| TC-05 | Đăng nhập thất bại — bỏ trống cả hai ô                     | Trình duyệt Chrome mở https://stqa.rbc.vn. Chưa đăng nhập.           | 1. Để trống ô "Email". 2. Để trống ô "Mật khẩu". 3. Nhấn nút "Đăng nhập".                                       | Email: *(để trống)* · Mật khẩu: *(để trống)*                      | Hệ thống **không** chuyển trang. Hiển thị: **"Vui lòng nhập email và mật khẩu"**. Trang đăng nhập vẫn hiển thị.                           | REQ-01 | EP       |
| TC-06 | Đăng nhập thất bại — chỉ bỏ trống email                    | Trình duyệt Chrome mở https://stqa.rbc.vn. Chưa đăng nhập.           | 1. Để trống ô "Email". 2. Nhập mật khẩu. 3. Nhấn nút "Đăng nhập".                                               | Email: *(để trống)* · Mật khẩu: `admin123`                        | Hệ thống **không** chuyển trang. Hiển thị: **"Vui lòng nhập email và mật khẩu"**. Trang đăng nhập vẫn hiển thị.                           | REQ-01 | EP       |
| TC-07 | Đăng nhập thất bại — chỉ bỏ trống mật khẩu                 | Trình duyệt Chrome mở https://stqa.rbc.vn. Chưa đăng nhập.           | 1. Nhập email hợp lệ. 2. Để trống ô "Mật khẩu". 3. Nhấn nút "Đăng nhập".                                        | Email: `librarian@library.com` · Mật khẩu: *(để trống)*           | Hệ thống **không** chuyển trang. Hiển thị: **"Vui lòng nhập email và mật khẩu"**. Trang đăng nhập vẫn hiển thị.                           | REQ-01 | EP       |
| TC-08 | Đăng nhập thất bại — email sai định dạng (thiếu @)          | Trình duyệt Chrome mở https://stqa.rbc.vn. Chưa đăng nhập.           | 1. Nhập chuỗi không có ký tự `@` vào ô "Email". 2. Nhập mật khẩu. 3. Nhấn nút "Đăng nhập".                     | Email: `librarylibrary.com` · Mật khẩu: `admin123`                | Hệ thống **không** chuyển trang. Hiển thị thông báo lỗi (SRS: "Không tìm thấy thành viên" hoặc lỗi định dạng). Không đăng nhập được.      | REQ-01 | EP       |
| TC-09 | Sau đăng nhập — AppBar hiển thị đúng tên và vai trò        | Đã thực hiện TC-02 thành công (đăng nhập Thành viên).                 | 1. Quan sát thanh AppBar sau khi đăng nhập thành công.                                                           | Email: `ba.nguyen@email.com` · Mật khẩu: `password123`            | AppBar hiển thị đúng **tên "Ba Nguyễn"** và **vai trò "Thành viên"**. Tab "Thành viên" (quản lý) **không xuất hiện**.                      | REQ-01 | EP       |

---

### REQ-03 — Tìm kiếm và lọc sách / Search & Filter Books

> **Kỹ thuật áp dụng:** EP cho tìm kiếm (có kết quả / không có kết quả); BVA tại biên ô rỗng (TC-16, TC-20).
> **Nguồn expected result:** SRS REQ-03 — "Tìm kiếm theo tên sách hoặc tác giả; Lọc theo thể loại; case-insensitive; không có kết quả → 'Không tìm thấy sách'".
>
> **Lưu ý về SRS gap:** SRS không quy định case-sensitivity cho ô lọc, và không quy định hành vi khi dùng tìm kiếm + lọc đồng thời. Các TC liên quan (TC-22 → TC-26) ghi nhận actual result để báo cáo gap, **không** kết luận Pass/Fail theo expected result tự đặt ra.

| Mã TC | Mục tiêu kiểm thử                                                    | Tiền điều kiện                                                              | Bước thực hiện                                                                                                                   | Dữ liệu đầu vào                             | Kết quả mong đợi (trích SRS REQ-03)                                                                                                                                              | REQ    | Kỹ thuật |
| ----- | -------------------------------------------------------------------- | --------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------ | --------- |
| TC-10 | Tìm kiếm theo tên sách — có kết quả                                  | Đã đăng nhập (bất kỳ tài khoản). Đang ở tab "Sách". Ô lọc đang rỗng.      | 1. Nhập từ khóa vào ô tìm kiếm. 2. Quan sát danh sách sách.                                                                     | Từ khóa: `Flutter`                          | Danh sách chỉ hiển thị sách có "Flutter" trong **tên sách**: BOOK001 "Lập trình Flutter cơ bản". Sách khác bị ẩn. *(SRS: tìm kiếm theo tên sách)*                              | REQ-03 | EP        |
| TC-11 | Tìm kiếm theo tên tác giả — có kết quả                               | Đã đăng nhập (bất kỳ tài khoản). Đang ở tab "Sách". Ô lọc đang rỗng.      | 1. Nhập tên tác giả vào ô tìm kiếm. 2. Quan sát danh sách sách.                                                                 | Từ khóa: `Nguyễn Minh Đức`                  | Danh sách hiển thị tất cả sách của tác giả Nguyễn Minh Đức: BOOK001, BOOK009. Sách của tác giả khác bị ẩn. *(SRS: tìm kiếm theo tác giả)*                                      | REQ-03 | EP        |
| TC-12 | Tìm kiếm — không có kết quả                                          | Đã đăng nhập (bất kỳ tài khoản). Đang ở tab "Sách". Ô lọc đang rỗng.      | 1. Nhập từ khóa không khớp bất kỳ sách nào. 2. Quan sát danh sách sách.                                                         | Từ khóa: `XYZ123abc`                        | Danh sách **không hiển thị sách nào**. Hệ thống hiển thị thông báo **"Không tìm thấy sách"**. *(SRS: "Không có kết quả → hiển thị thông báo")*                                 | REQ-03 | EP        |
| TC-13 | Tìm kiếm case-insensitive — toàn chữ thường                          | Đã đăng nhập (bất kỳ tài khoản). Đang ở tab "Sách". Ô lọc đang rỗng.      | 1. Nhập từ khóa toàn chữ thường vào ô tìm kiếm. 2. Quan sát kết quả.                                                            | Từ khóa: `flutter`                          | Danh sách hiển thị **đúng kết quả giống TC-10**: BOOK001. *(SRS: "Tìm kiếm KHÔNG phân biệt chữ hoa/thường")*                                                                   | REQ-03 | EP        |
| TC-14 | Tìm kiếm case-insensitive — toàn chữ HOA                             | Đã đăng nhập (bất kỳ tài khoản). Đang ở tab "Sách". Ô lọc đang rỗng.      | 1. Nhập từ khóa toàn chữ HOA vào ô tìm kiếm. 2. Quan sát kết quả.                                                               | Từ khóa: `FLUTTER`                          | Danh sách hiển thị **đúng kết quả giống TC-10**: BOOK001. *(SRS: "Tìm kiếm KHÔNG phân biệt chữ hoa/thường")*                                                                   | REQ-03 | EP        |
| TC-15 | Tìm kiếm case-insensitive — hoa lẫn thường                           | Đã đăng nhập (bất kỳ tài khoản). Đang ở tab "Sách". Ô lọc đang rỗng.      | 1. Nhập từ khóa hoa lẫn thường vào ô tìm kiếm. 2. Quan sát kết quả.                                                             | Từ khóa: `fLuTtEr`                          | Danh sách hiển thị **đúng kết quả giống TC-10**: BOOK001. *(SRS: "Tìm kiếm KHÔNG phân biệt chữ hoa/thường")*                                                                   | REQ-03 | EP        |
| TC-16 | Xóa từ khóa tìm kiếm — hiển thị lại toàn bộ danh sách               | Đã đăng nhập. Ô tìm kiếm đang có từ khóa `flutter`. Đang xem kết quả lọc. | 1. Xóa toàn bộ nội dung ô tìm kiếm (để rỗng). 2. Quan sát danh sách sách.                                                       | Từ khóa: *(xóa thành rỗng)*                 | Danh sách hiển thị lại **toàn bộ 20 đầu sách** (seed data). Không còn lọc theo từ khóa nào. *(BVA: biên dưới của ô tìm kiếm)*                                                  | REQ-03 | BVA       |
| TC-17 | Lọc theo thể loại — "Công nghệ" (đúng chuẩn)                         | Đã đăng nhập (bất kỳ tài khoản). Đang ở tab "Sách". Ô tìm kiếm đang rỗng. | 1. Nhập `Công nghệ` vào **ô nhập lọc thể loại**. 2. Quan sát danh sách sách.                                                    | Bộ lọc: `Công nghệ`                         | Danh sách chỉ hiển thị sách thuộc thể loại Công nghệ: BOOK001, BOOK002, BOOK003, BOOK005, BOOK008, BOOK009, BOOK010, BOOK011. Sách thể loại khác bị ẩn. *(SRS: lọc theo thể loại)* | REQ-03 | EP        |
| TC-18 | Lọc theo thể loại — "Quản trị" (đúng chuẩn)                          | Đã đăng nhập (bất kỳ tài khoản). Đang ở tab "Sách". Ô tìm kiếm đang rỗng. | 1. Nhập `Quản trị` vào **ô nhập lọc thể loại**. 2. Quan sát danh sách sách.                                                     | Bộ lọc: `Quản trị`                          | Danh sách chỉ hiển thị sách thuộc thể loại Quản trị: BOOK004, BOOK012, BOOK013. Sách thể loại khác bị ẩn. *(SRS: lọc theo thể loại)*                                           | REQ-03 | EP        |
| TC-19 | Lọc theo thể loại — không khớp thể loại nào                          | Đã đăng nhập (bất kỳ tài khoản). Đang ở tab "Sách". Ô tìm kiếm đang rỗng. | 1. Nhập chuỗi không khớp bất kỳ thể loại nào vào ô lọc thể loại. 2. Quan sát danh sách sách.                                    | Bộ lọc: `XYZ`                               | Danh sách **không hiển thị sách nào**. Hệ thống hiển thị thông báo **"Không tìm thấy sách"**. *(SRS: không có kết quả → thông báo)*                                            | REQ-03 | EP        |
| TC-20 | Xóa ô lọc thể loại — hiển thị lại toàn bộ danh sách                  | Đã đăng nhập. Ô lọc đang chứa `Công nghệ`. Đang xem kết quả lọc TC-17.    | 1. Xóa toàn bộ nội dung ô lọc thể loại (để rỗng). 2. Quan sát danh sách sách.                                                   | Bộ lọc: *(xóa thành rỗng)*                  | Danh sách hiển thị lại **toàn bộ 20 đầu sách** (seed data). Bộ lọc thể loại không còn áp dụng. *(BVA: biên dưới của ô lọc)*                                                    | REQ-03 | BVA       |
| TC-21 | Tìm kiếm theo tác giả có nhiều đầu sách                              | Đã đăng nhập (bất kỳ tài khoản). Đang ở tab "Sách". Ô lọc đang rỗng.      | 1. Nhập tên tác giả có nhiều sách vào ô tìm kiếm. 2. Quan sát danh sách sách.                                                   | Từ khóa: `Lý Văn Tài`                       | Danh sách hiển thị **tất cả** sách của tác giả Lý Văn Tài: BOOK008, BOOK011. Sách của tác giả khác bị ẩn. *(SRS: tìm kiếm theo tác giả)*                                       | REQ-03 | EP        |

---

> ### ⚠️ TC-22 → TC-26 — Ghi nhận SRS Gap (không kết luận Pass/Fail)
>
> SRS REQ-03 **không quy định** case-sensitivity cho ô lọc thể loại và **không quy định** hành vi khi dùng tìm kiếm + lọc đồng thời.
> Các TC dưới đây được thực thi để **quan sát và ghi nhận thực tế**, không áp đặt expected result.
> Kết quả thực tế sẽ được báo cáo trong `summary.md` như một **SRS gap cần làm rõ**.

| Mã TC | Mục tiêu kiểm thử                                                        | Tiền điều kiện                                                              | Bước thực hiện                                                                                                                           | Dữ liệu đầu vào                                  | Kết quả mong đợi                                                                                                                                          | REQ    | Kỹ thuật |
| ----- | ------------------------------------------------------------------------ | --------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------- | ------ | --------- |
| TC-22 | **[SRS gap]** Lọc thể loại — "công nghệ" toàn chữ thường                | Đã đăng nhập (bất kỳ tài khoản). Đang ở tab "Sách". Ô tìm kiếm đang rỗng. | 1. Nhập `công nghệ` (toàn chữ thường) vào ô lọc thể loại. 2. Quan sát danh sách sách.                                                   | Bộ lọc: `công nghệ` *(toàn chữ thường)*          | **SRS không quy định.** Ghi lại actual result: (a) Nếu hiển thị sách Công nghệ → lọc case-insensitive; (b) Nếu không có kết quả → lọc case-sensitive.   | REQ-03 | EP        |
| TC-23 | **[SRS gap]** Lọc thể loại — "CÔNG NGHỆ" toàn chữ HOA                   | Đã đăng nhập (bất kỳ tài khoản). Đang ở tab "Sách". Ô tìm kiếm đang rỗng. | 1. Nhập `CÔNG NGHỆ` (toàn chữ HOA) vào ô lọc thể loại. 2. Quan sát danh sách sách.                                                     | Bộ lọc: `CÔNG NGHỆ` *(toàn chữ HOA)*            | **SRS không quy định.** Ghi lại actual result: (a) Nếu hiển thị sách Công nghệ → lọc case-insensitive; (b) Nếu không có kết quả → lọc case-sensitive.   | REQ-03 | EP        |
| TC-24 | **[SRS gap]** Lọc thể loại — "quản trị" toàn chữ thường                 | Đã đăng nhập (bất kỳ tài khoản). Đang ở tab "Sách". Ô tìm kiếm đang rỗng. | 1. Nhập `quản trị` (toàn chữ thường) vào ô lọc thể loại. 2. Quan sát danh sách sách.                                                   | Bộ lọc: `quản trị` *(toàn chữ thường)*           | **SRS không quy định.** Ghi lại actual result: (a) Nếu hiển thị BOOK004, BOOK012, BOOK013 → lọc case-insensitive; (b) Nếu không có kết quả → lọc case-sensitive. | REQ-03 | EP        |
| TC-25 | **[SRS gap]** Kết hợp tìm kiếm + lọc — cả hai đều có kết quả riêng lẻ  | Đã đăng nhập (bất kỳ tài khoản). Đang ở tab "Sách".                        | 1. Nhập `Công nghệ` vào ô lọc thể loại. 2. Nhập `Python` vào ô tìm kiếm. 3. Quan sát kết quả.                                          | Bộ lọc: `Công nghệ` · Từ khóa: `Python`          | **SRS không quy định hành vi kết hợp.** Ghi lại actual result: (a) Chỉ hiển thị BOOK009 → AND logic; (b) Hiển thị tất cả sách Công nghệ + sách Python → OR logic; (c) Kết quả khác. | REQ-03 | EP        |
| TC-26 | **[SRS gap]** Kết hợp tìm kiếm + lọc — tìm kiếm không có kết quả trong thể loại đã lọc | Đã đăng nhập (bất kỳ tài khoản). Đang ở tab "Sách".           | 1. Nhập `Kinh tế` vào ô lọc thể loại. 2. Nhập `Flutter` vào ô tìm kiếm. 3. Quan sát kết quả.                                           | Bộ lọc: `Kinh tế` · Từ khóa: `Flutter`           | **SRS không quy định hành vi kết hợp.** Ghi lại actual result: (a) Không có sách nào → AND logic, hiển thị "Không tìm thấy sách"; (b) Hiển thị sách Flutter (bỏ qua lọc) → OR logic; (c) Kết quả khác. | REQ-03 | EP        |

---

## Tổng hợp

| Nhóm chức năng                       | Số TC | REQ phủ | Kỹ thuật IDM áp dụng |
| ------------------------------------ | ----- | ------- | -------------------- |
| Đăng nhập                            | 9     | REQ-01  | EP                   |
| Tìm kiếm & lọc sách (có SRS rõ ràng)| 12    | REQ-03  | EP, BVA (TC-16, TC-20) |
| Ghi nhận SRS gap                     | 5     | REQ-03  | EP (quan sát)        |
| Mượn sách *(chờ điền)*               |       | REQ-04  |                      |
| Trả sách *(chờ điền)*                |       | REQ-05  |                      |
| Quá hạn *(chờ điền)*                 |       | REQ-06  |                      |
| Quản lý thành viên *(chờ điền)*      |       | REQ-07  |                      |
| Phiếu mượn *(chờ điền)*              |       | REQ-08  |                      |
| **Tổng (REQ-01+03)**                 | **26**|         | EP, BVA              |
