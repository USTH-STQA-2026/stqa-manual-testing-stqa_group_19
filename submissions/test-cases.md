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

| Đặc tính (Characteristic)   | Phân vùng (Block)            | Giá trị đại diện (Value)        | Kết quả mong đợi                                    |
| --------------------------- | ---------------------------- | ------------------------------- | --------------------------------------------------- |
| Email có tồn tại trong DB?  | Có — vai trò Thủ thư         | `librarian@library.com`         | Đăng nhập thành công, hiển thị vai trò "Thủ thư"   |
|                             | Có — vai trò Thành viên      | `ba.nguyen@email.com`           | Đăng nhập thành công, hiển thị vai trò "Thành viên" |
|                             | Không tồn tại                | `noone@example.com`             | Hiển thị "Không tìm thấy thành viên"                |
| Mật khẩu có đúng?           | Đúng                         | `admin123`                      | Đăng nhập thành công                                |
|                             | Sai                          | `wrongpass`                     | Hiển thị "Mật khẩu không đúng"                      |
| Ô nhập có rỗng?             | Cả hai rỗng                  | email: `""`, password: `""`     | Hiển thị "Vui lòng nhập email và mật khẩu"          |
|                             | Chỉ email rỗng               | email: `""`, password: `abc`    | Hiển thị "Vui lòng nhập email và mật khẩu"          |
|                             | Chỉ mật khẩu rỗng            | email: `librarian@library.com`, password: `""` | Hiển thị "Vui lòng nhập email và mật khẩu" |
| Định dạng email             | Hợp lệ (có @ và domain)      | `librarian@library.com`         | Xử lý bình thường                                   |
|                             | Không hợp lệ (thiếu @)       | `librarylibrary.com`            | Hệ thống không xác thực được, báo lỗi               |

---

### IDM — Tìm kiếm sách (REQ-03)

| Đặc tính (Characteristic)     | Phân vùng (Block)         | Giá trị đại diện (Value) | Kết quả mong đợi                                        |
| ----------------------------- | ------------------------- | ------------------------ | ------------------------------------------------------- |
| Từ khóa có tồn tại trong DB?  | Có (theo tên sách)        | `"Flutter"`              | Hiển thị "Lập trình Flutter cơ bản" (BOOK001)           |
|                               | Có (theo tên tác giả)     | `"Nguyễn Minh Đức"`      | Hiển thị BOOK001 và BOOK009                             |
|                               | Không khớp bất kỳ sách nào | `"XYZ123abc"`           | Hiển thị thông báo "Không tìm thấy sách"                |
| Phân biệt HOA/thường?         | Toàn chữ thường           | `"flutter"`              | Kết quả giống khi tìm `"Flutter"` (case-insensitive)   |
|                               | Toàn chữ HOA              | `"FLUTTER"`              | Kết quả giống khi tìm `"Flutter"` (case-insensitive)   |
|                               | Viết hoa lẫn lộn          | `"fLuTtEr"`              | Kết quả giống khi tìm `"Flutter"` (case-insensitive)   |
| Ô tìm kiếm có rỗng?           | Rỗng (không nhập gì)      | `""`                     | Hiển thị toàn bộ danh sách sách (không lọc)             |
| Lọc theo thể loại             | Thể loại có sách           | `"Công nghệ"`            | Chỉ hiển thị sách thuộc thể loại Công nghệ             |
|                               | Thể loại có sách           | `"Quản trị"`             | Chỉ hiển thị sách thuộc thể loại Quản trị              |
|                               | Lọc + tìm kiếm kết hợp    | Lọc "Công nghệ" + tìm `"Python"` | Chỉ hiển thị sách Python thuộc Công nghệ        |

---

### IDM — Mượn sách (REQ-04, REQ-05)

| Đặc tính (Characteristic) | Phân vùng (Block)    | Giá trị đại diện (Value) | Kết quả mong đợi                   |
| ------------------------- | -------------------- | ------------------------ | ---------------------------------- |
| Trạng thái sách?          | Có sẵn               | BOOK001                  | Cho phép mượn                      |
|                           | Đang mượn            | BOOK003                  | Không cho phép                     |
|                           | Thất lạc             | BOOK007                  | Không cho phép                     |
| Trạng thái thành viên?    | Hoạt động            | MEM002                   | Cho phép mượn                      |
|                           | Tạm ngưng            | MEM004                   | Từ chối, thông báo lỗi tạm ngưng   |
|                           | Hết hạn              | MEM005                   | Từ chối, thông báo lỗi hết hạn     |
| Số sách đang mượn?        | < 3 (BVA: 0, 1, 2)   | MEM006 (0 sách)          | Cho phép mượn                      |
|                           | = 3 (BVA: giới hạn)  | MEM đã mượn 3 sách       | Từ chối, thông báo vượt giới hạn   |

---

### IDM — REQ-05 đến REQ-08 (nhóm tự bổ sung)

| Đặc tính (Characteristic) | Phân vùng (Block) | Giá trị đại diện (Value) | Kết quả mong đợi |
| ------------------------- | ----------------- | ------------------------ | ---------------- |
| `<!-- Nhóm tự điền -->`   |                   |                          |                  |

---

## Bước 2: Test Cases

### REQ-01 — Đăng nhập / Login

> **Kỹ thuật áp dụng:** EP (Equivalence Partitioning) — chia email và mật khẩu thành các lớp hợp lệ / không hợp lệ, mỗi lớp test 1 đại diện để tránh test trùng lặp.

| Mã TC  | Mục tiêu kiểm thử                              | Tiền điều kiện                                             | Bước thực hiện                                                                                                                                        | Dữ liệu đầu vào                                                        | Kết quả mong đợi                                                                                                                                  | REQ    | Kỹ thuật |
| ------ | ---------------------------------------------- | ---------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------- | ------ | -------- |
| TC-01  | Đăng nhập thành công với tài khoản Thủ thư     | Trình duyệt Chrome mở trang https://stqa.rbc.vn, chưa đăng nhập | 1. Nhập email vào ô "Email". 2. Nhập mật khẩu vào ô "Mật khẩu". 3. Nhấn nút "Đăng nhập".                                                           | Email: `librarian@library.com` · Mật khẩu: `admin123`                 | Hệ thống chuyển sang trang chủ. AppBar hiển thị tên người dùng và vai trò "Thủ thư". Tab "Thành viên" hiển thị (đặc quyền Thủ thư).              | REQ-01 | EP       |
| TC-02  | Đăng nhập thành công với tài khoản Thành viên  | Trình duyệt Chrome mở trang https://stqa.rbc.vn, chưa đăng nhập | 1. Nhập email vào ô "Email". 2. Nhập mật khẩu vào ô "Mật khẩu". 3. Nhấn nút "Đăng nhập".                                                           | Email: `ba.nguyen@email.com` · Mật khẩu: `password123`                | Hệ thống chuyển sang trang chủ. AppBar hiển thị tên "Ba Nguyễn" và vai trò "Thành viên". Tab "Thành viên" **không** hiển thị.                    | REQ-01 | EP       |
| TC-03  | Đăng nhập thất bại — email không tồn tại       | Trình duyệt Chrome mở trang https://stqa.rbc.vn, chưa đăng nhập | 1. Nhập email không có trong hệ thống vào ô "Email". 2. Nhập mật khẩu bất kỳ vào ô "Mật khẩu". 3. Nhấn nút "Đăng nhập".                           | Email: `noone@example.com` · Mật khẩu: `admin123`                     | Hệ thống **không** chuyển trang. Hiển thị thông báo lỗi: **"Không tìm thấy thành viên"**. Trang đăng nhập vẫn còn hiển thị.                     | REQ-01 | EP       |
| TC-04  | Đăng nhập thất bại — mật khẩu sai              | Trình duyệt Chrome mở trang https://stqa.rbc.vn, chưa đăng nhập | 1. Nhập email hợp lệ vào ô "Email". 2. Nhập mật khẩu **sai** vào ô "Mật khẩu". 3. Nhấn nút "Đăng nhập".                                           | Email: `librarian@library.com` · Mật khẩu: `wrongpass`                | Hệ thống **không** chuyển trang. Hiển thị thông báo lỗi: **"Mật khẩu không đúng"**. Trang đăng nhập vẫn còn hiển thị.                           | REQ-01 | EP       |
| TC-05  | Đăng nhập thất bại — bỏ trống cả hai ô         | Trình duyệt Chrome mở trang https://stqa.rbc.vn, chưa đăng nhập | 1. Để trống ô "Email" (không nhập gì). 2. Để trống ô "Mật khẩu" (không nhập gì). 3. Nhấn nút "Đăng nhập".                                          | Email: *(để trống)* · Mật khẩu: *(để trống)*                          | Hệ thống **không** chuyển trang. Hiển thị thông báo lỗi: **"Vui lòng nhập email và mật khẩu"**. Trang đăng nhập vẫn còn hiển thị.               | REQ-01 | EP       |
| TC-06  | Đăng nhập thất bại — chỉ bỏ trống email        | Trình duyệt Chrome mở trang https://stqa.rbc.vn, chưa đăng nhập | 1. Để trống ô "Email" (không nhập gì). 2. Nhập mật khẩu vào ô "Mật khẩu". 3. Nhấn nút "Đăng nhập".                                                 | Email: *(để trống)* · Mật khẩu: `admin123`                            | Hệ thống **không** chuyển trang. Hiển thị thông báo lỗi: **"Vui lòng nhập email và mật khẩu"**. Trang đăng nhập vẫn còn hiển thị.               | REQ-01 | EP       |
| TC-07  | Đăng nhập thất bại — chỉ bỏ trống mật khẩu     | Trình duyệt Chrome mở trang https://stqa.rbc.vn, chưa đăng nhập | 1. Nhập email hợp lệ vào ô "Email". 2. Để trống ô "Mật khẩu" (không nhập gì). 3. Nhấn nút "Đăng nhập".                                             | Email: `librarian@library.com` · Mật khẩu: *(để trống)*               | Hệ thống **không** chuyển trang. Hiển thị thông báo lỗi: **"Vui lòng nhập email và mật khẩu"**. Trang đăng nhập vẫn còn hiển thị.               | REQ-01 | EP       |
| TC-08  | Đăng nhập thất bại — định dạng email không hợp lệ (thiếu @) | Trình duyệt Chrome mở trang https://stqa.rbc.vn, chưa đăng nhập | 1. Nhập chuỗi không có ký tự `@` vào ô "Email". 2. Nhập mật khẩu vào ô "Mật khẩu". 3. Nhấn nút "Đăng nhập".                              | Email: `librarylibrary.com` · Mật khẩu: `admin123`                    | Hệ thống **không** chuyển trang. Hiển thị thông báo lỗi phù hợp (lỗi định dạng email hoặc "Không tìm thấy thành viên"). Không đăng nhập được.    | REQ-01 | EP       |
| TC-09  | Kiểm tra hiển thị đúng vai trò sau đăng nhập — thành viên Hoạt động | Đã đăng nhập thành công với tài khoản `ba.nguyen@email.com` | 1. Quan sát AppBar sau khi đăng nhập thành công.                                                                                              | Email: `ba.nguyen@email.com` · Mật khẩu: `password123`                | AppBar hiển thị đúng **tên người dùng** và **nhãn vai trò "Thành viên"**. Tab "Thành viên" (quản lý) **không xuất hiện** trong giao diện.        | REQ-01 | EP       |

---

### REQ-03 — Tìm kiếm và lọc sách / Search & Filter Books

> **Kỹ thuật áp dụng:** EP (Equivalence Partitioning) — chia từ khóa tìm kiếm thành lớp có kết quả / không có kết quả; BVA ngầm định ở trường hợp ô tìm kiếm rỗng (biên dưới của input). Case-insensitive là một đặc tính riêng cần test độc lập.

| Mã TC  | Mục tiêu kiểm thử                                        | Tiền điều kiện                                                         | Bước thực hiện                                                                                                                                           | Dữ liệu đầu vào                                                           | Kết quả mong đợi                                                                                                                                                       | REQ    | Kỹ thuật  |
| ------ | -------------------------------------------------------- | ---------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------ | --------- |
| TC-10  | Tìm kiếm theo tên sách — có kết quả                      | Đã đăng nhập (bất kỳ tài khoản). Đang ở tab "Sách".                   | 1. Nhập từ khóa vào ô tìm kiếm. 2. Quan sát danh sách sách được cập nhật.                                                                               | Từ khóa: `Flutter`                                                        | Danh sách chỉ hiển thị sách có từ "Flutter" trong tên: **"Lập trình Flutter cơ bản"** (BOOK001). Các sách khác bị ẩn.                                                 | REQ-03 | EP        |
| TC-11  | Tìm kiếm theo tên tác giả — có kết quả                   | Đã đăng nhập (bất kỳ tài khoản). Đang ở tab "Sách".                   | 1. Nhập tên tác giả vào ô tìm kiếm. 2. Quan sát danh sách sách được cập nhật.                                                                           | Từ khóa: `Nguyễn Minh Đức`                                                | Danh sách hiển thị tất cả sách của tác giả Nguyễn Minh Đức: **"Lập trình Flutter cơ bản"** (BOOK001) và **"Nhập môn lập trình Python"** (BOOK009). Sách khác bị ẩn. | REQ-03 | EP        |
| TC-12  | Tìm kiếm không có kết quả                                | Đã đăng nhập (bất kỳ tài khoản). Đang ở tab "Sách".                   | 1. Nhập từ khóa không khớp bất kỳ sách nào vào ô tìm kiếm. 2. Quan sát danh sách sách.                                                                  | Từ khóa: `XYZ123abc`                                                      | Danh sách **không hiển thị sách nào**. Hệ thống hiển thị thông báo: **"Không tìm thấy sách"**.                                                                       | REQ-03 | EP        |
| TC-13  | Tìm kiếm không phân biệt chữ hoa/thường — toàn chữ thường | Đã đăng nhập (bất kỳ tài khoản). Đang ở tab "Sách".                  | 1. Nhập từ khóa viết **toàn chữ thường** vào ô tìm kiếm. 2. Quan sát kết quả.                                                                           | Từ khóa: `flutter`                                                        | Danh sách hiển thị **đúng kết quả giống như TC-10** (tìm `Flutter`): "Lập trình Flutter cơ bản" (BOOK001). Tìm kiếm **không phân biệt** hoa/thường.                  | REQ-03 | EP        |
| TC-14  | Tìm kiếm không phân biệt chữ hoa/thường — toàn chữ HOA   | Đã đăng nhập (bất kỳ tài khoản). Đang ở tab "Sách".                  | 1. Nhập từ khóa viết **toàn chữ HOA** vào ô tìm kiếm. 2. Quan sát kết quả.                                                                              | Từ khóa: `FLUTTER`                                                        | Danh sách hiển thị **đúng kết quả giống như TC-10** (tìm `Flutter`): "Lập trình Flutter cơ bản" (BOOK001). Tìm kiếm **không phân biệt** hoa/thường.                  | REQ-03 | EP        |
| TC-15  | Tìm kiếm không phân biệt chữ hoa/thường — viết hoa lẫn lộn | Đã đăng nhập (bất kỳ tài khoản). Đang ở tab "Sách".                | 1. Nhập từ khóa viết **hoa lẫn thường không đồng nhất** vào ô tìm kiếm. 2. Quan sát kết quả.                                                            | Từ khóa: `fLuTtEr`                                                        | Danh sách hiển thị **đúng kết quả giống như TC-10** (tìm `Flutter`): "Lập trình Flutter cơ bản" (BOOK001). Tìm kiếm **không phân biệt** hoa/thường.                  | REQ-03 | EP        |
| TC-16  | Xóa từ khóa tìm kiếm — hiển thị lại toàn bộ danh sách   | Đã đăng nhập. Đang ở tab "Sách". Ô tìm kiếm đang có từ khóa `flutter`. | 1. Xóa toàn bộ nội dung trong ô tìm kiếm (xóa thành rỗng). 2. Quan sát danh sách sách.                                                                 | Từ khóa: *(xóa thành rỗng `""`)*                                          | Danh sách hiển thị lại **toàn bộ 20 đầu sách** (seed data). Không còn lọc theo từ khóa nào.                                                                          | REQ-03 | BVA       |
| TC-17  | Lọc theo thể loại — thể loại "Công nghệ"                  | Đã đăng nhập (bất kỳ tài khoản). Đang ở tab "Sách". Ô tìm kiếm đang rỗng. | 1. Mở dropdown lọc thể loại. 2. Chọn thể loại **"Công nghệ"**. 3. Quan sát danh sách sách.                                                      | Bộ lọc thể loại: `Công nghệ`                                              | Danh sách chỉ hiển thị sách thuộc thể loại Công nghệ: BOOK001, BOOK002, BOOK003, BOOK005, BOOK008, BOOK009, BOOK010, BOOK011. Sách thể loại khác bị ẩn.              | REQ-03 | EP        |
| TC-18  | Lọc theo thể loại — thể loại "Quản trị"                   | Đã đăng nhập (bất kỳ tài khoản). Đang ở tab "Sách". Ô tìm kiếm đang rỗng. | 1. Mở dropdown lọc thể loại. 2. Chọn thể loại **"Quản trị"**. 3. Quan sát danh sách sách.                                                       | Bộ lọc thể loại: `Quản trị`                                               | Danh sách chỉ hiển thị sách thuộc thể loại Quản trị: **"Quản trị dự án phần mềm"** (BOOK004), **"Quản trị nhân sự hiện đại"** (BOOK013), **"Quản trị chiến lược"** (BOOK012). Sách thể loại khác bị ẩn. | REQ-03 | EP        |
| TC-19  | Kết hợp tìm kiếm và lọc thể loại đồng thời               | Đã đăng nhập (bất kỳ tài khoản). Đang ở tab "Sách".                   | 1. Chọn thể loại **"Công nghệ"** trong dropdown lọc. 2. Nhập từ khóa `Python` vào ô tìm kiếm. 3. Quan sát kết quả.                                     | Bộ lọc: `Công nghệ` · Từ khóa: `Python`                                  | Danh sách chỉ hiển thị sách vừa thuộc thể loại Công nghệ **vừa** có "Python" trong tên/tác giả: **"Nhập môn lập trình Python"** (BOOK009). Sách khác bị ẩn.         | REQ-03 | EP        |
| TC-20  | Tìm kiếm theo từ khóa một phần (partial match)            | Đã đăng nhập (bất kỳ tài khoản). Đang ở tab "Sách".                   | 1. Nhập từ khóa là **một phần** của tên sách vào ô tìm kiếm. 2. Quan sát danh sách sách.                                                                | Từ khóa: `kiểm thử`                                                       | Danh sách hiển thị tất cả sách có "kiểm thử" trong tên: **"Kiểm thử phần mềm nhập môn"** (BOOK003). Nếu cả tác giả có "kiểm thử" thì cũng hiển thị. Sách khác bị ẩn. | REQ-03 | EP        |
| TC-21  | Tìm kiếm theo tên tác giả — nhiều sách cùng tác giả       | Đã đăng nhập (bất kỳ tài khoản). Đang ở tab "Sách".                   | 1. Nhập họ của tác giả có nhiều đầu sách vào ô tìm kiếm. 2. Quan sát danh sách sách.                                                                    | Từ khóa: `Lý Văn Tài`                                                     | Danh sách hiển thị tất cả sách của tác giả Lý Văn Tài: **"Mạng máy tính"** (BOOK008) và **"Hệ điều hành Linux"** (BOOK011). Sách của tác giả khác bị ẩn.           | REQ-03 | EP        |

---

## Tổng hợp

| Nhóm chức năng          | Số TC | REQ phủ | Kỹ thuật IDM áp dụng        |
| ----------------------- | ----- | ------- | ----------------------------|
| Đăng nhập               | 9     | REQ-01  | EP                          |
| Tìm kiếm & lọc sách     | 12    | REQ-03  | EP, BVA (TC-16)             |
| Mượn sách *(chờ điền)*  |       | REQ-04  |                             |
| Trả sách *(chờ điền)*   |       | REQ-05  |                             |
| Quá hạn *(chờ điền)*    |       | REQ-06  |                             |
| Quản lý thành viên *(chờ điền)* |  | REQ-07  |                             |
| Phiếu mượn *(chờ điền)* |       | REQ-08  |                             |
| **Tổng (REQ-01+03)**    | **21**|         | EP, BVA                     |