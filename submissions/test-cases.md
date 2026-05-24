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

### IDM — REQ-05

| Đặc tính (Characteristic) | Phân vùng (Block) | Giá trị đại diện (Value) | Kết quả mong đợi |
|---|---|---|---|
| Does the book belong to this member's borrow record? | Yes  | BR001 — MEM002 borrowed BOOK003 | Return is allowed |
| | No  | BR003 — BOOK013 belongs to MEM006, accessed by MEM002 | Return button not shown / rejected |
| When is the book returned relative to due date? | On time (≤ dueDate) | Borrowed today, returned same day (dueDate = today + 14) | Return succeeds, no overdue warning |
| | Overdue (> dueDate) | BR001 — dueDate 15/09/2024, returned in 2026 | Return succeeds, overdue warning displayed |
| Book status after return? | Changes to "Available" | BOOK003 after BR001 is returned | BOOK003 = "Available" |
| | Unchanged if not yet returned | BOOK003 while BR001 still active | BOOK003 = "Borrowed" |
| Is status updated in real-time? | Yes (immediately) | Go to Books tab right after returning | Status updates without page refresh |
| | No (delayed) | — | Bug — does not meet REQ-02 |

### IDM — REQ-08
| Đặc tính (Characteristic) | Phân vùng (Block) | Giá trị đại diện (Value) | Kết quả mong đợi |
|---|---|---|---|
| User role? | Librarian | `librarian@library.com` | Can view all borrow records of every member |
| | Member | `ba.nguyen@email.com` (MEM002) | Can only view own records |
| Whose record is being viewed? | Belongs to the logged-in member | BR001, BR004 — owned by MEM002 | Displayed normally |
| | Belongs to another member | BR003 — owned by MEM006, accessed by MEM002 | Not displayed / access denied |
| Does the member have any borrow records? | Has records | MEM002 has BR001, BR004 | Record list is displayed |
| | No records | New member who has never borrowed | Empty list displayed |
| Record status displayed? | Active | BR001 — not yet returned, not yet overdue | Displays "Borrowed" |
| | Returned | BR002 — returned on 20/08/2024 | Displays "Returned" |
| | Overdue | BR001 — after librarian runs overdue check | Displays "Overdue" |

| `<!-- Nhóm tự điền -->` | | | |

> 💡 **Gợi ý kỹ thuật**: Sử dụng **Phân lớp tương đương (EP)** cho các phân vùng rời rạc, **Phân tích giá trị biên (BVA)** cho các phân vùng số (ví dụ: giới hạn 3 sách). Xem textbook §6.1–6.3.

---

## Bước 2: Test Cases

<!-- Tự tổ chức bảng test case: có thể chia nhóm theo chức năng, theo REQ, hoặc theo luồng nghiệp vụ — tùy nhóm quyết định. -->
<!-- Mỗi TC phải ánh xạ ngược về ít nhất 1 dòng trong bảng IDM ở Bước 1. -->

| Mã TC | Mục tiêu kiểm thử | Tiền điều kiện | Bước thực hiện | Dữ liệu đầu vào | Kết quả mong đợi | REQ | Kỹ thuật |
|-------|-------------------|---------------|---------------|-----------------|------------------|-----|---------|
|TC-01|Return a borrowed book on time, no overdue warning| Logged in ba.nguyen@email.com. Just borrowed BOOK001.|1.Go to tab "Borrow/Return". 2.Find BOOK001 in "My borrow records". 3. Click "Return book". Comfirm if prompted |BOOK001, borrow date = today, due date = today + 14 days|1.Success message show. 2.Record status "Returned" 3.BOOK001 status -> "Available". 4.No overdue warning|REQ -05|EP, Decision Table|
|TC-02 |Return an overdue book- allowed but shows overdue warning | Logged in ba.nguyen@email.com. Record BR001 exists: MEM002 borrowed BOOK003, due 15/09/2024|1.Go to "Borrow/Return" tab. 2. Find "BOOK003" in "My Borrow records". 3. Click "Return book" |Record: BR001, BOOK003, due date: 15/09/2024, return date: today  |1. Return is accepted. 2. Overdue warning is diplayed 3. BR001 status -> "Returned" . 4. BOOK003 status -> "Available" |REQ -05 |EP, BVA, Decision Table |
|TC-03 |Cannot return a book borrowed by another member |Logged in ba.nguyen@email.com. BOOK013 is borrowed by another member, not MEM002 |1. Go to "Borrow/Return" tab. 2. Find BOOK013 in "My borrow records" |Account of MEM002 but BOOK013 is borrowed by MEM006 |1. Can not find BOOK013 in MEM002's list. 2. No "Return" button available for BOOK013. 3. System does not allow returning another member's book |REQ -05 |EP, Decision Table |
|TC-04| Book status updates immediately after return| Logged in as ba.nguyen@email.com.  BOOK003 currently shows "Borrowed" in Books tab|1.Go to "Borrow/Return". 2. Find BR001 in "My borrow records". 3. Click "Return book" . 4. Immediately go back to "Books" tab. 5. Find BOOK003 |Record:BR001, BOOK003 |1.1. Before return: BOOK003 = "Borrowed". 2. After return: BOOK003 = "Available" — updates immediately |REQ -05 |EP |
|TC-05 |Librarian call view all borrow records from all members | Logged in as librarian@library.com. |1. Go to "Borrow/ Return tab. 2. View the full value records list. 3. Check if records from multiple members are visible | Account: librarian@library.com|1. All 5 seed records (BR001–BR005) are visible. 2. Records belong to different members (MEM002, MEM003, MEM006).3. Each record shows: record ID, book name, borrow date, due date, status. |REQ -08 |EP |
|TC-06 |Member can only see their own borrow records |Logged in as ba.nguyen@email.com(MEM002). MEM002 has BR001 and BR004. |1. Go to "Borrow/Return" tab. 2. View "My borrow records" list. Note all records shown |Account: ba.nguyen@email.com |1. Only BR001 (BOOK003) and BR004 (BOOK005) are visible. 2. Records from MEM003 (BR002, BR005) and MEM006 (BR003) are not visible. 3.Each record shows:book, record ID,member, borrow date, due date, status|REQ -08 |EP, Decision Table |
|TC-07 |Member can not access another member's borrow records |Logged in as ba.nguyen@email.com (MEM002). BR003 belongs to MEM006. |1. Go to "Borrow/Return" tab. 2.Look for any way to view records of other members in "Search borrow records"(search by member ID,..). 3.Attempt to access BR003(MEM006's record) by any means |Account: ba.nguyen@email.com. Target record: BR003 (owned by MEM006) |1. No option to search or browse other members' records. 2. BR003 is not visible anywhere in MEM002's view. 3. System does not expose other members' borrow data.  |REQ- 08 |EP, Decision Table |
|TC-08|Returned-on-time record displays correct "Returned" status and all fields |Logged in as dam.tran@email.com (MEM003).BR002: BOOK001, returned 20/08/2024 — within due date. |1.Go to "Borrow/Return" tab. 2. Find record BR002. 3.Check status field. 4. Check all other fields |Account: dam.tran@email.com. Record: BR002 (BOOK001, returned on time) |1.BR002 visible in MEM003's list,. 2.Status "Returned". 3.Other fields correct: ID = BR002, book = "Lập trình Flutter cơ bản", borrow date = 10/08/2024, due date = 24/08/2024, return date. |REQ-08 |EP |
|TC-09 |Record status shows "Overdue" after overdue check | Logged in as librarian@library.com. BR001 (MEM002, BOOK003) is past due date 15/09/2024.|1. As Librarian,Go to "Borrow/Return" then click "Check overdue books" button. 2. Log out, log back in as ba.nguyen@email.com. 3. Go to "Borrow/Return" tab. 4. Find BR001. |Librarian triggers overdue check. Account for viewing: ba.nguyen@email.com. Record: BR001 |1. BR001 status updates to "Overdue". 2. MEM002 can see BR001 with status "Overdue" in their own records. 3. Librarian also sees BR001 as "Overdue" in the full list. |REQ -08|EP |
|TC-10 |Overdue record correctly transitions to "Returned" after late return |Logged in as librarian@library.com. BR001 not yet marked overdue. |1. As Librarian,go to "Borrow/Return" tab then click "Check overdue books" → BR001 becomes "Overdue". 2. Log out. 3. Log in as ba.nguyen@email.com. 4. Go to "Borrow/Return" tab, find BR001( status: "Overdue")5. Click "Return" on BR001. 6. Check BR001 status after returning | Record: BR001. Returning account: ba.nguyen@gmail.com|1.After step 1: BR001 status: "Overdue". 2.After step 5 BR001 status : "Returned" and success message show. 3.Record does not stuck at "Overdue" 4.BOOK003 changes to "Available" |REQ- 08 |EP |
| | | | | | | | |
---

## Tổng hợp

| Nhóm chức năng | Số TC | REQ phủ | Kỹ thuật IDM áp dụng |
|----------------|-------|---------|----------------------|
| | | | |
| **Tổng** | **<!-- ≥ 20 -->** | | |
