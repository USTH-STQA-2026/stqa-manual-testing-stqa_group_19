# Test Cases — Bảng trường hợp kiểm thử

> **Hướng dẫn**: Viết tối thiểu **20 TC** phủ đủ các chức năng chính (REQ-01 → REQ-08).
> Xem [examples/sample-test-case.md](../examples/sample-test-case.md) để hiểu cách viết TC tốt.
> Tự tổ chức và phân nhóm test case theo cách hợp lý nhất.

| Thông tin | |
|---|---|
| **Nhóm** | `<!-- Tên nhóm -->` |
| **Ngày tạo** | `<!-- DD/MM/YYYY -->` |
| **Hệ thống** | https://stqa.rbc.vn |
| **Tham chiếu** | SRS v1.0 |

---

## Bước 1: Mô hình hóa miền đầu vào — Input Domain Modeling (IDM)

> 📖 **Textbook:** Chương 6 — *Input Domain Modeling*, Paul Ammann & Jeff Offutt.
>
> **Trước khi viết Test Case**, nhóm **phải** phân tích miền đầu vào bằng bảng IDM bên dưới.
> Mỗi chức năng cần xác định: **Đặc tính (Characteristic)**, **Phân vùng (Block/Partition)**, và **Giá trị đại diện (Value)**.

### IDM — Đăng nhập (REQ-01)

| Đặc tính (Characteristic) | Phân vùng (Block) | Giá trị đại diện (Value) | Kết quả mong đợi |
|---|---|---|---|
| Email có tồn tại trong DB? | Có | `librarian@library.com` | Đăng nhập thành công |
| | Không | `noone@email.com` | Thông báo lỗi |
| Mật khẩu có đúng? | Đúng | `admin123` | Đăng nhập thành công |
| | Sai | `wrongpass` | Thông báo lỗi |
| Ô nhập có rỗng? | Không rỗng | (giá trị bất kỳ) | Xử lý bình thường |
| | Rỗng | `""` | Thông báo "Vui lòng nhập..." |

### IDM — Tìm kiếm sách (REQ-03)

| Đặc tính (Characteristic) | Phân vùng (Block) | Giá trị đại diện (Value) | Kết quả mong đợi |
|---|---|---|---|
| Từ khóa có tồn tại trong DB? | Có (tên sách) | `"Flutter"` | Hiển thị sách chứa "Flutter" |
| | Có (tên tác giả) | `"Nguyễn"` | Hiển thị sách của tác giả Nguyễn |
| | Không | `"XYZ123"` | Danh sách rỗng |
| Phân biệt HOA/thường? | Chữ thường | `"flutter"` | Kết quả giống "Flutter" |
| | Chữ HOA | `"FLUTTER"` | Kết quả giống "Flutter" |

### IDM — Mượn sách (REQ-04, REQ-05)

| Đặc tính (Characteristic) | Phân vùng (Block) | Giá trị đại diện (Value) | Kết quả mong đợi |
|---|---|---|---|
| Trạng thái sách? | Có sẵn | BOOK001 | Cho phép mượn |
| | Đang mượn | BOOK003 | Không cho phép |
| | Thất lạc | BOOK007 | Không cho phép |
| Trạng thái thành viên? | Hoạt động | MEM002 | Cho phép mượn |
| | Tạm ngưng | MEM004 | Từ chối, thông báo lỗi |
| | Hết hạn | MEM005 | Từ chối, thông báo lỗi |
| Số sách đang mượn? | < 3 (BVA: 0, 1, 2) | MEM006 (0 sách) | Cho phép mượn |
| | = 3 (BVA: giới hạn) | MEM đã mượn 3 sách | Từ chối, thông báo vượt giới hạn |

### IDM — View Book List (REQ-02)
| Đặc tính (Characteristic) | Phân vùng (Block) | Giá trị đại diện (Value) | Kết quả mong đợi |
|---|---|---|---|
|User Role Access|Librarian|`"librarian@library.com"`|Display the complete list of books with full detail(Title, Author, Genre, Publication Year, Status)|
||Member|`"ba.nguyen@email.com"`|Display the complete list of books with full detail(Title, Author, Genre, Publication Year, Status)|
|Real-time Data Update|Status Changed|`"BOOK001"` gets borrowed|The book status immediately changes to "Borrowed" on screen without needing a page refresh|
||No Change|Leave screen as is|All book statuses remain unchanged.|

| `<!-- Nhóm tự điền -->` | | | |


### IDM — `<!-- Nhóm tự bổ sung cho REQ-05 đến REQ-08 -->`

| Đặc tính (Characteristic) | Phân vùng (Block) | Giá trị đại diện (Value) | Kết quả mong đợi |
|---|---|---|---|
| `<!-- Nhóm tự điền -->` | | | |

> 💡 **Gợi ý kỹ thuật**: Sử dụng **Phân lớp tương đương (EP)** cho các phân vùng rời rạc, **Phân tích giá trị biên (BVA)** cho các phân vùng số (ví dụ: giới hạn 3 sách). Xem textbook §6.1–6.3.

---

## Bước 2: Test Cases

<!-- Tự tổ chức bảng test case: có thể chia nhóm theo chức năng, theo REQ, hoặc theo luồng nghiệp vụ — tùy nhóm quyết định. -->
<!-- Mỗi TC phải ánh xạ ngược về ít nhất 1 dòng trong bảng IDM ở Bước 1. -->

| Mã TC | Mục tiêu kiểm thử | Tiền điều kiện | Bước thực hiện | Dữ liệu đầu vào | Kết quả mong đợi | REQ | Kỹ thuật |
|-------|-------------------|---------------|---------------|-----------------|------------------|-----|---------|
|TC-01|Verify book list display for Member role|Member`"ba.nguyen@email.com"`is logged in|1. Navigate to the "Books" tab.<br>2. Observe the displayed list|None|Displays all 20 books. Each book includes: Title, Author, Genre, Publication Year, and Status|REQ-02|EP(IDM: Member)|
|TC-02|Verify real-time book status update on UI|Member`"ba.nguyen@email.com"`is logged in|1. Go to "Books" tab.<br>2. Click the "+" button for an "Available" book (`"BOOK001"`) then confirm "Borrow".<br>3. Observe the status of `"BOOK001"` on the book list|Book: `"BOOK001"`|The status of `"BOOK001"`on the Member's screen instantly changes from "Available" to "Borrowed" without page refresh|REQ-02|Happy Path(IDM: Status Changed)|
|TC-03|Verify book list display for Librarian role|Librarian `"librarian@library.com"` is logged in|1. Navigate to "Books" tab|None|Displays all 20 books with full details|REQ-02|EP (IDM: Librarian)|
|TC-04|Verify successful book borrowing under limit (< 3)|Logged in as `"biet.hoang@email.com"` (currently has 1 active borrow)| 1. Go to "Books" tab.<br>2. Select an "Available" book (`"BOOK001"`).<br>3. Click the"+" button then click "Borrow"|`"BOOK001"`|Displays a success message. A borrow record is created with a due date set to exactly +14 days from today|REQ-04|EP(IDM: Available, Active, < 3)|
|TC-05|Verify borrow rejection at the boundary limit (= 3)|Logged in as `"ba.nguyen@email.com"` (currently has 1 active borrow)| 1. Borrow `"BOOK002"` (Total = 2).<br>2. Borrow `"BOOK004"` (Total = 3).<br>3. Attempt to borrow `"BOOK005"`|`"BOOK002"`, `"BOOK004"`, `"BOOK005"`|System allows the first 2 borrows. Rejects the 3rd attempt (`"BOOK005"`) with an error message stating the limit of 3 books is reached|REQ-04|BVA(IDM: = 3)|
|TC-06|Verify borrow rejection for "Suspended" member|Logged in as `"cu.le@email.com"` (Member status: Suspended)|1. Go to "Books" tab.<br>2. Select an "Available" book (`"BOOK001"`).<br>3. Click the"+" button then "Borrow"|`"BOOK001"`|The system rejects the request and displays a specific error message for the "Suspended" account status|REQ-04|DT/EP(IDM: Suspended)|
|TC-07|Verify borrow rejection for an already "Borrowed" book|Logged in as `"dam.tran@email.com"`|1. Go to "Books" tab.<br>2. Search or locate `"BOOK003"` (Status: Borrowed).<br>3. Observe the book card|`"BOOK003"`|The "+" borrow button is completely hidden/disabled for this book|REQ-04|EP(IDM: Borrowed)|
|TC-08|Verify borrow rejection for "Expired" member|Logged in as `"binh.pham@email.com"` (Member status: Expired)|1. Go to "Books" tab.<br>2. Select an "Available" book (`"BOOK001"`).<br>3. Click the"+" button then "Borrow"|`"BOOK001"`|The system rejects the request and displays a specific error message for the "Expired" account status|REQ-04|EP(IDM: Expired)|
|TC-09|Verify borrow rejection for "Lost" book|Member `"ba.nguyen@email.com"` is logged in|1. Locate `"BOOK007"` (Status: Lost).<br>2. Observe the book card|Book: `"BOOK007"`|The "+" borrow button is completely hidden/disabled|REQ-04|EP (IDM: Lost)|
|TC-10|Verify successful borrow at boundary limit (= 2)|Member `"ba.nguyen@email.com"` is logged in(has 1 active borrow)|1. Borrow BOOK002.<br>2. Observe system response|Book: `"BOOK002"`|System allows borrowing successfully (Total active borrows becomes exactly 2)|REQ-04|BVA (IDM: = 2)|
---

## Tổng hợp

| Nhóm chức năng | Số TC | REQ phủ | Kỹ thuật IDM áp dụng |
|----------------|-------|---------|----------------------|
| | | | |
| **Tổng** | **<!-- ≥ 20 -->** | | |
