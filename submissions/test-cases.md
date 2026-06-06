# Test Cases — Bảng trường hợp kiểm thử

> **Hướng dẫn**: Viết tối thiểu **20 TC** phủ đủ các chức năng chính (REQ-01 → REQ-08).
> Xem [examples/sample-test-case.md](../examples/sample-test-case.md) để hiểu cách viết TC tốt.
> Tự tổ chức và phân nhóm test case theo cách hợp lý nhất.

| Thông tin      |                       |
| -------------- | --------------------- |
| **Nhóm**       | `<!-- Tên nhóm -->`   |
| **Ngày tạo**   | `<!-- DD/MM/YYYY -->` |
| **Hệ thống**   | https://stqa.rbc.vn   |
| **Tham chiếu** | SRS v1.0              |

---

## Bước 1: Mô hình hóa miền đầu vào — Input Domain Modeling (IDM)

> 📖 **Textbook:** Chương 6 — _Input Domain Modeling_, Paul Ammann & Jeff Offutt.
>
> **Trước khi viết Test Case**, nhóm **phải** phân tích miền đầu vào bằng bảng IDM bên dưới.
> Mỗi chức năng cần xác định: **Đặc tính (Characteristic)**, **Phân vùng (Block/Partition)**, và **Giá trị đại diện (Value)**.

### IDM — Đăng nhập (REQ-01)

| Đặc tính (Characteristic)  | Phân vùng (Block) | Giá trị đại diện (Value) | Kết quả mong đợi             |
| -------------------------- | ----------------- | ------------------------ | ---------------------------- |
| Email có tồn tại trong DB? | Có                | `librarian@library.com`  | Đăng nhập thành công         |
|                            | Không             | `noone@email.com`        | Thông báo lỗi                |
| Mật khẩu có đúng?          | Đúng              | `admin123`               | Đăng nhập thành công         |
|                            | Sai               | `wrongpass`              | Thông báo lỗi                |
| Ô nhập có rỗng?            | Không rỗng        | (giá trị bất kỳ)         | Xử lý bình thường            |
|                            | Rỗng              | `""`                     | Thông báo "Vui lòng nhập..." |

### IDM — Tìm kiếm sách (REQ-03)

| Đặc tính (Characteristic)    | Phân vùng (Block) | Giá trị đại diện (Value) | Kết quả mong đợi                 |
| ---------------------------- | ----------------- | ------------------------ | -------------------------------- |
| Từ khóa có tồn tại trong DB? | Có (tên sách)     | `"Flutter"`              | Hiển thị sách chứa "Flutter"     |
|                              | Có (tên tác giả)  | `"Nguyễn"`               | Hiển thị sách của tác giả Nguyễn |
|                              | Không             | `"XYZ123"`               | Danh sách rỗng                   |
| Phân biệt HOA/thường?        | Chữ thường        | `"flutter"`              | Kết quả giống "Flutter"          |
|                              | Chữ HOA           | `"FLUTTER"`              | Kết quả giống "Flutter"          |

### IDM — Mượn sách (REQ-04, REQ-05)

| Đặc tính (Characteristic) | Phân vùng (Block)   | Giá trị đại diện (Value) | Kết quả mong đợi                 |
| ------------------------- | ------------------- | ------------------------ | -------------------------------- |
| Trạng thái sách?          | Có sẵn              | BOOK001                  | Cho phép mượn                    |
|                           | Đang mượn           | BOOK003                  | Không cho phép                   |
|                           | Thất lạc            | BOOK007                  | Không cho phép                   |
| Trạng thái thành viên?    | Hoạt động           | MEM002                   | Cho phép mượn                    |
|                           | Tạm ngưng           | MEM004                   | Từ chối, thông báo lỗi           |
|                           | Hết hạn             | MEM005                   | Từ chối, thông báo lỗi           |
| Số sách đang mượn?        | < 3 (BVA: 0, 1, 2)  | MEM006 (0 sách)          | Cho phép mượn                    |
|                           | = 3 (BVA: giới hạn) | MEM đã mượn 3 sách       | Từ chối, thông báo vượt giới hạn |

### IDM — Overdue Handling (REQ-06)

| Đặc tính (Characteristic) | Phân vùng (Block)       | Giá trị đại diện (Value) | Kết quả mong đợi                                                                   |
| ------------------------- | ----------------------- | ------------------------ | ---------------------------------------------------------------------------------- |
| dueDate                   | dueDate < current date  | BR001                    | Overdue                                                                            |
|                           | dueDate == current date | BR006                    | Overdue                                                                            |
|                           | dueDate > current date  | BR002                    | prompt                                                                             |
| login account             | Librarian               | LIB001                   | Librarian can view all the overdue record                                          |
|                           | Member                  | MEM003                   | MEM003 can just see their own overdue record and cannot see others overdue records |

### IDM — Member Management (REQ-07)

| Đặc tính (Characteristic) | Phân vùng (Block)                   | Giá trị đại diện (Value) | Kết quả mong đợi                    |
| ------------------------- | ----------------------------------- | ------------------------ | ----------------------------------- |
| Full name                 | empty                               | `""`                     | The full name cannot be left blank. |
|                           | not empty                           | (any value)              | allowed                             |
| Email                     | empty                               | `""`                     | The email cannot be left blank.     |
|                           | Missing @                           | `nooneemail.com`         | invalid email                       |
|                           | no .                                | `test@email`             | invalid email                       |
|                           | duplicate email                     | `ba.nguyen@email.com`    | invalid email                       |
|                           | no domain                           | `noone@emailcom`         | invalid email                       |
|                           | have @ and . and domain             | `test@gmail.com`         | allowed                             |
|                           | email already exists                | `dam.tran@email.com`     | invalid                             |
| Phone number              | start with the number 0             | `0154486524`             | allowed                             |
|                           | It doesn't start with the number 0. | `1234567891`             | invalid                             |
|                           | empty                               | ``                       | invalid                             |

> 💡 **Gợi ý kỹ thuật**: Sử dụng **Phân lớp tương đương (EP)** cho các phân vùng rời rạc, **Phân tích giá trị biên (BVA)** cho các phân vùng số (ví dụ: giới hạn 3 sách). Xem textbook §6.1–6.3.

---

## Bước 2: Test Cases

<!-- Tự tổ chức bảng test case: có thể chia nhóm theo chức năng, theo REQ, hoặc theo luồng nghiệp vụ — tùy nhóm quyết định. -->
<!-- Mỗi TC phải ánh xạ ngược về ít nhất 1 dòng trong bảng IDM ở Bước 1. -->

| Mã TC | Mục tiêu kiểm thử                                                                               | Tiền điều kiện                                                                       | Bước thực hiện                                                                                                                  | Dữ liệu đầu vào                                                      | Kết quả mong đợi                                   | REQ   | Kỹ thuật |
| ----- | ----------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------- | -------------------------------------------------- | ----- | -------- |
| TC-01 | Check the record with a due date == today's date - 1                                            | The BR007 record exists, with status "Borrowing"                                     | 1. Log in using your Librarian account. 2. Click the "Check Expired" button.                                                    | System date: 5/23/2026. (dueDate == 5/22/2026)                       | BR007 changed to Overdue.                          | REQ-6 | EP, BVA  |
| TC-02 | Check the record with a due date == today's date + 1                                            | The BR008 record exists, with status "Borrowing"                                     | 1. Log in using your Librarian account. 2. Click the "Check Expired" button.                                                    | System date: 5/23/2026. (dueDate == 5/24/2026)                       | BR008 doesn't change                               | REQ-6 | EP, BVA  |
| TC-03 | Check the record with a due date == today's date                                                | The BR006 record exists, with status "Borrowing"                                     | 1. Log in using your Librarian account. 2. Click the "Check Expired" button.                                                    | System date: 5/23/2026. (dueDate == 5/23/2026)                       | BR006 change to overdue                            | REQ-6 | EP, BVA  |
| TC-04 | Check The system will ignore overdue records that have been returned.                           | The BR002 record exists, with status "Returned"                                      | 1. Log in using your Librarian account. 2. Click the "Check Expired" button.                                                    | System date: 5/23/2026. (dueDate == 24/08/2024) Status = Returned    | BR002 doesn't change                               | REQ-6 | EP, BVA  |
| TC-05 | Members can view their OWN overdue records.                                                     | Dam.tran's BR003 record is in Overdue status.                                        | 1. Log in using biet.hoang account. 2. Click the "Borrowed/Returned" tab                                                        | Log in using the account dam.tran                                    | BR003 is displaying a "Overdue" status.            | REQ-6 | ACC      |
| TC-06 | Librarian can view all the overdue records.                                                     | biet.hoang's BR003 record and ba.nguyen's BR001 are in Overdue status.               | 1. Log in using Librarian account. 2. Click the "Borrowed/Returned" tab 3. Click the "Check Expired" button.                    | Log in using the Librarian account.                                  | BR003 and BR001 are displaying a "Overdue" status. | REQ-6 | ACC      |
| TC-08 | Verify that the "Add Member Successfully" (Happy Path) flow has been completed.                 | 1. Log in using Librarian account. 2. The email doesn't exist                        | 1. Log in using Librarian account. 2. Click the "Add member" button. 3. Enter the information. 4. Click the "Add member" button | Full Name: ABC. Email: test@gmail.com. Phone number: 0123456789      | Successfully                                       | REQ-7 | EP, ACC  |
| TC-09 | Catching email formatting errors: Missing @ character.                                          | 1. Log in using Librarian account.                                                   | 1. Log in using Librarian account. 2. Click the "Add member" button. 3. Enter the information. 4. Click the "Add member" button | Full Name: ABC. Email: userdomain.com. Phone number: 0123456789      | Invalid email                                      | REQ-7 | EP, ACC  |
| TC-10 | Catching email formatting errors: There is an @ symbol but NO period (.) in the domain section. | 1. Log in using Librarian account.                                                   | 1. Log in using Librarian account. 2. Click the "Add member" button. 3. Enter the information. 4. Click the "Add member" button | Full Name: ABC. Email: test@email. Phone number: 0123456789          | Invalid email                                      | REQ-7 | EP, ACC  |
| TC-11 | Ensure the system blocks the creation of DUPLICATE emails.                                      | 1. Log in using Librarian account. 2. The email "ba.nguyen@email.com" already exists | 1. Log in using Librarian account. 2. Click the "Add member" button. 3. Enter the information. 4. Click the "Add member" button | Full Name: ABC. Email: ba.nguyen@email.com. Phone number: 0123456789 | Email already exists                               | REQ-7 | EP, ACC  |

---

## Tổng hợp

| Nhóm chức năng | Số TC             | REQ phủ | Kỹ thuật IDM áp dụng |
| -------------- | ----------------- | ------- | -------------------- |
|                |                   |         |                      |
| **Tổng**       | **<!-- ≥ 20 -->** |         |                      |
