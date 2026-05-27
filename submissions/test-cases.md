# Test Cases — Bảng trường hợp kiểm thử

| Thông tin | |
|---|---|
| **Nhóm** | group 19 |
| **Ngày tạo** | 24/05/2026 |
| **Hệ thống** | https://stqa.rbc.vn |
| **Tham chiếu** | SRS v1.0 |

---

## Bước 1: Mô hình hóa miền đầu vào — Input Domain Modeling (IDM)

### IDM — Login (REQ-01)

| Đặc tính (Characteristic) | Phân vùng (Block) | Giá trị đại diện (Value) | Kết quả mong đợi |
|---|---|---|---|
| Email có tồn tại trong DB? | Có | `librarian@library.com` | Đăng nhập thành công |
| | Không | `noone@email.com` | Thông báo lỗi |
| Mật khẩu có đúng? | Đúng | `admin123` | Đăng nhập thành công |
| | Sai | `wrongpass` | Thông báo lỗi |
| Ô nhập có rỗng? | Không rỗng | (giá trị bất kỳ) | Xử lý bình thường |
| | Rỗng | `""` | Thông báo "Vui lòng nhập..." |

### IDM — View book list (REQ-02)

| Đặc tính (Characteristic) | Phân vùng (Block) | Giá trị đại diện (Value) | Kết quả mong đợi |
|---|---|---|---|
| Role of the logged-in user? | Librarian | `librarian@library.com` | Views all 20 books with complete details |
| | Member | `ba.nguyen@email.com` | Views all 20 books with complete details |
| Display status of the book? | Available | BOOK001, BOOK002 | Displays status as "Available" |
| | Borrowed | BOOK003 (currently borrowed by MEM002) | Displays status as "Borrowed" |
| | Lost | BOOK007, BOOK020 | Displays status as "Lost" |
| Is the displayed information complete? | Complete with 5 fields: title, author, genre, year, status | BOOK001 | Displays: "Lập trình Flutter cơ bản", "Nguyễn Minh Đức", "Công nghệ", "2023", "Available" |
| Real-time update after an action? | After borrowing a book | Borrow BOOK001 | Status of BOOK001 immediately changes to "Borrowed" without needing a refresh |
| | After returning a book | Return BOOK003 | Status of BOOK003 immediately changes to "Available" |

---

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

### IDM — REQ-05: Return Book (More)

| Characteristic | Block / Partition | Representative Value | Expected Result |
|---|---|---|---|
| Does the borrow receipt belong to the logged-in member? | Yes — correct person | MEM002 returns BOOK003 (BR001) | Allows return |
| | No — someone else | MEM002 attempts to return BOOK013 (belonging to MEM006) | Return option is hidden / rejected |
| Status of the borrow receipt upon return? | Borrowed, within due date | BR003 (MEM006, due 15/10/2024) | Successful return, no overdue warning |
| | Borrowed, overdue | BR001 (MEM002, due 15/09/2024) | Successful return **and** system displays an **overdue warning** |
| | Already returned | BR002 (status "Returned") | Return option is hidden (receipt is closed) |
| Book status after return? | Successfully returned | BOOK003 after MEM002 returns it | Book status immediately changes to **"Available"** |
| Borrow receipt status after return? | Successfully returned | BR001 after return | Receipt status changes to **"Returned"** |

---

### IDM — REQ-06: Process Overdue Books

| Characteristic | Block / Partition | Representative Value | Expected Result |
|---|---|---|---|
| Who triggers the "Check Overdue" function? | Librarian | `librarian@library.com` clicks button | System scans and updates all overdue receipts |
| | Member | Any member | "Check Overdue" button is not visible (permissions hidden) |
| Current date compared to `dueDate`? (BVA) | **Before due date** (today < dueDate) | Receipt not yet due | Status remains "Borrowed", **not** marked as overdue |
| | **On due date** (today = dueDate) | Receipt due today | Marked as **"Overdue"** (dueDate ≤ today per SRS) |
| | **After due date** (today > dueDate) | BR001 (dueDate = 15/09/2024) | Marked as **"Overdue"** |
| Can members see their own overdue receipts? | Has overdue receipts | MEM002 logs in, BR001 is overdue | Sees BR001 with "Overdue" status in their receipt list |
| | Has no overdue receipts | MEM006 logs in, BR003 is within due date | Does not see any receipts marked as overdue |
| Can the Librarian see all overdue receipts? | After clicking check | Librarian views list | Sees **all** overdue receipts from all members |

---

### IDM — REQ-07: Member Management (Add New)

| Characteristic | Block / Partition | Representative Value | Expected Result |
|---|---|---|---|
| Who performs the member addition? | Librarian | `librarian@library.com` | Form to add member is visible, action can be completed |
| | Member | `ba.nguyen@email.com` | "Members" tab is not visible (permissions hidden) |
| Is the entered email valid? | Valid — contains `@` and `.` in domain | `newmember@email.com` | Accepted |
| | **Missing `.` in domain** | `newmember@domain` | Rejected, displays invalid email message |
| | Missing `@` | `newmemberdomain.com` | Rejected, displays invalid email message |
| | Empty | `""` | Rejected, displays required field message |
| Does the email already exist in the system? | Does not exist yet | `brandnew@email.com` | New member created successfully |
| | Already exists | `ba.nguyen@email.com` (MEM002) | Rejected, displays message "Email already exists" |
| Is the full name entered? | Entered | `"Nguyễn Văn A"` | Accepted |
| | Empty | `""` | Rejected, displays required field message |
| Is the phone number entered? | Entered | `"0912345678"` | Accepted |
| | Empty | `""` | Handled according to system logic (required or optional?) |

---

### IDM — REQ-08: Borrow Receipt Lookup

| Characteristic | Block / Partition | Representative Value | Expected Result |
|---|---|---|---|
| Role of the viewing user? | Librarian | `librarian@library.com` | Sees **all** receipts for all members (BR001 → BR005) |
| | Member | `ba.nguyen@email.com` (MEM002) | Only sees receipts belonging to MEM002 (BR001, BR004) — **cannot** see BR002, BR003, BR005 |
| Can a member view someone else's receipt? | Intentional access to another's receipt | MEM002 views receipt of MEM006 | **Not allowed** — hidden or displays an error |
| Is the displayed receipt information complete? | Borrowed receipt | BR001 | Displays: receipt ID, book title, borrow date, due date, status "Borrowed" |
| | Returned receipt | BR002 | Displays status "Returned" |
| | Overdue receipt | BR001 after Librarian checks | Displays status "Overdue" |
| Are all 3 receipt statuses fully displayed? | Borrowed | BR001, BR003 | Displays "Borrowed" |
| | Returned | BR002, BR004, BR005 | Displays "Returned" |
| | Overdue | BR001 after overdue check | Displays "Overdue" |

## Bước 2: Test Cases

### REQ-01 — Login

> **Applied technique:** EP — divide email and password into valid / invalid classes, each class tests 1 representative.
> **Expected result source:** SRS REQ-01 — "Error message" and "Rule" columns.

| TC ID | Test Objective                                          | Prerequisites                                                        | Execution Steps                                                                                                   | Input Data                                                    | Expected Result (excerpt SRS REQ-01)                                                                                                         | REQ    | Technique |
| ----- | ---------------------------------------------------------- | --------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------- | ------ | -------- |
| TC-101 | Successful login with Librarian account                 | Chrome browser opened https://stqa.rbc.vn. Not logged in.           | 1. Enter email into "Email" box. 2. Enter password into "Password" box. 3. Click "Login" button.                       | Email: `librarian@library.com` · Password: `admin123`             | System redirects to home page. AppBar shows username and role "Thủ thư". "Thành viên" tab is displayed (Librarian privilege).        | REQ-01 | EP       |
| TC-102 | Successful login with Member account              | Chrome browser opened https://stqa.rbc.vn. Not logged in.           | 1. Enter email into "Email" box. 2. Enter password into "Password" box. 3. Click "Login" button.                       | Email: `ba.nguyen@email.com` · Password: `password123`            | System redirects to home page. AppBar shows name "Ba Nguyễn" and role "Thành viên". "Thành viên" (management) tab is **not** displayed.    | REQ-01 | EP       |
| TC-103 | Login failed — email does not exist                   | Chrome browser opened https://stqa.rbc.vn. Not logged in.           | 1. Enter email not in system. 2. Enter any password. 3. Click "Login" button.                       | Email: `noone@example.com` · Password: `admin123`                 | System does **not** redirect. Displays: **"Không tìm thấy thành viên"**. Login page is still displayed.                                  | REQ-01 | EP       |
| TC-104 | Login failed — incorrect password                          | Chrome browser opened https://stqa.rbc.vn. Not logged in.           | 1. Enter valid email. 2. Enter wrong password. 3. Click "Login" button.                                           | Email: `librarian@library.com` · Password: `wrongpass`            | System does **not** redirect. Displays: **"Mật khẩu không đúng"**. Login page is still displayed.                                       | REQ-01 | EP       |
| TC-105 | Login failed — leave both boxes empty                     | Chrome browser opened https://stqa.rbc.vn. Not logged in.           | 1. Leave "Email" empty. 2. Leave "Password" empty. 3. Click "Login" button.                                       | Email: *(empty)* · Password: *(empty)*                      | System does **not** redirect. Displays: **"Vui lòng nhập email và mật khẩu"**. Login page is still displayed.                           | REQ-01 | EP       |
| TC-106 | Login failed — only leave email empty                    | Chrome browser opened https://stqa.rbc.vn. Not logged in.           | 1. Leave "Email" empty. 2. Enter password. 3. Click "Login" button.                                               | Email: *(empty)* · Password: `admin123`                        | System does **not** redirect. Displays: **"Vui lòng nhập email và mật khẩu"**. Login page is still displayed.                           | REQ-01 | EP       |
| TC-107 | Login failed — only leave password empty                 | Chrome browser opened https://stqa.rbc.vn. Not logged in.           | 1. Enter valid email. 2. Leave "Password" empty. 3. Click "Login" button.                                        | Email: `librarian@library.com` · Password: *(empty)*           | System does **not** redirect. Displays: **"Vui lòng nhập email và mật khẩu"**. Login page is still displayed.                           | REQ-01 | EP       |

---

### REQ-2 — View Book List
| TC ID | Test Objective                                          | Prerequisites                                                        | Execution Steps                                                                                                   | Input Data                                                    | Expected Result (excerpt SRS REQ-01)                                                                                                         | REQ    | Technique |
| ----- | ---------------------------------------------------------- | --------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------- | ------ | -------- |
|TC-08|Verify book list display for Member role|Member`"ba.nguyen@email.com"`is logged in|1. Navigate to the "Books" tab.<br>2. Observe the displayed list|None|Displays all 20 books. Each book includes: Title, Author, Genre, Publication Year, and Status|REQ-02|EP(IDM: Member)|
|TC-09|Verify real-time book status update on UI|Member`"ba.nguyen@email.com"`is logged in|1. Go to "Books" tab.<br>2. Click the "+" button for an "Available" book (`"BOOK001"`) then confirm "Borrow".<br>3. Observe the status of `"BOOK001"` on the book list|Book: `"BOOK001"`|The status of `"BOOK001"`on the Member's screen instantly changes from "Available" to "Borrowed" without page refresh|REQ-02|Happy Path(IDM: Status Changed)|
|TC-10|Verify book list display for Librarian role|Librarian `"librarian@library.com"` is logged in|1. Navigate to "Books" tab|None|Displays all 20 books with full details|REQ-02|EP (IDM: Librarian)|

---

### REQ-03 — Search and Filter Books

> **Expected result source:** SRS REQ-03 — "Search by book title or author; Filter by category; case-insensitive; no result -> 'Book not found'".
>
> **Note on SRS gap:** SRS does not specify case-sensitivity for filter box, and does not specify behavior when using search + filter simultaneously. Related TCs (TC-18 → TC-22) record actual result to report gap, **do not** conclude Pass/Fail based on self-determined expected result.
> ### ⚠️ TC-311 — Record SRS Gap (do not conclude Pass/Fail)
>
> SRS REQ-03 **does not specify**
| TC ID | Test Objective                                          | Prerequisites          ase-sensitivity for the category filter box and **does not specify** behavior when combining search + filter.
> The TCs below are executed to **observe and record actual reality**, not imposing expected result.
> Actual results will be reported in `summary.md` as an **SRS gap needing clarification**.

| TC ID | Test Objective                                                    | Prerequisites                                                              | Execution Steps                                                                                                                   | Input Data                             | Expected Result (excerpt SRS REQ-03)                                                                                                                                              | REQ    | Technique |
| ----- | -------------------------------------------------------------------- | --------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------ | --------- |
| TC-301 | Search by book title — has result                                  | Logged in (any account). On "Books" tab. Filter box is empty.      | 1. Enter keyword into search box. 2. Observe book list.                                                                     | Keyword: `Flutter`                          | List only displays books with "Flutter" in **book title**: BOOK001 "Lập trình Flutter cơ bản". Other books are hidden. *(SRS: search by book title)*                              | REQ-03 | EP        |
| TC-302 | Search by author name — has result                               | Logged in (any account). On "Books" tab. Filter box is empty.      | 1. Enter author name into search box. 2. Observe book list.                                                                 | Keyword: `Nguyễn Minh Đức`                  | List displays all books by author Nguyễn Minh Đức: BOOK001, BOOK009. Books by other authors are hidden. *(SRS: search by author)*                                      | REQ-03 | EP        |
| TC-303 | Search — no result                                          | Logged in (any account). On "Books" tab. Filter box is empty.      | 1. Enter keyword not matching any book. 2. Observe book list.                                                         | Keyword: `XYZ123abc`                        | List **does not display any book**. System displays message **"Không tìm thấy sách"**. *(SRS: "No result -> display message")*                                 | REQ-03 | EP        |
| TC-304 | Case-insensitive search — all lowercase                          | Logged in (any account). On "Books" tab. Filter box is empty.      | 1. Enter all lowercase keyword into search box. 2. Observe result.                                                            | Keyword: `flutter`                          | List displays **exact result as TC-10**: BOOK001. *(SRS: "Search is NOT case-sensitive")*                                                                   | REQ-03 | EP        |
| TC-305 | Case-insensitive search — all uppercase                             | Logged in (any account). On "Books" tab. Filter box is empty.      | 1. Enter all uppercase keyword into search box. 2. Observe result.                                                               | Keyword: `FLUTTER`                          | List displays **exact result as TC-10**: BOOK001. *(SRS: "Search is NOT case-sensitive")*                                                                   | REQ-03 | EP        |
| TC-306 | Case-insensitive search — mixed case                           | Logged in (any account). On "Books" tab. Filter box is empty.      | 1. Enter mixed case keyword into search box. 2. Observe result.                                                             | Keyword: `fLuTtEr`                          | List displays **exact result as TC-10**: BOOK001. *(SRS: "Search is NOT case-sensitive")*                                                                   | REQ-03 | EP        |
| TC-307 | Clear search keyword — display full list again               | Logged in. Search box has keyword `flutter`. Viewing filter results. | 1. Clear entire content of search box (leave empty). 2. Observe book list.                                                       | Keyword: *(clear to empty)*                 | List re-displays **all 20 book titles** (seed data). No longer filtered by any keyword. *(BVA: lower boundary of search box)*                                                  | REQ-03 | BVA       |
| TC-308 | Filter by category — "Công nghệ" (correct format)                         | Logged in (any account). On "Books" tab. Search box is empty. | 1. Enter `Công nghệ` into **category filter box**. 2. Observe book list.                                                    | Filter: `Công nghệ`                         | List only displays books in Technology category: BOOK001, BOOK002, BOOK003, BOOK005, BOOK008, BOOK009, BOOK010, BOOK011. Other category books are hidden. *(SRS: filter by category)* | REQ-03 | EP        |
| TC-309 | Case-insensitive category filter — all lowercase                          | Logged in (any account). On "Books" tab. Search box is empty. | 1. Enter `công nghệ` (all lowercase) into category filter box. 2. Observe book list.                                                   | Filter: `công nghệ` *(all lowercase)*          | List only displays books in Technology category    | REQ-03 | EP        |
| TC-310 | Case-insensitive category filter — all uppercase                             | Logged in (any account). On "Books" tab. Search box is empty. | 1. Enter `CÔNG NGHỆ` (all uppercase) into category filter box. 2. Observe book list.                                                     | Filter: `CÔNG NGHỆ` *(all uppercase)*            | List only displays books in Technology category    | REQ-03 | EP        |
| TC-311 | **[SRS gap]** Combine search + filter — search has no result in filtered category | Logged in (any account). On "Books" tab.           | 1. Enter `Kinh tế` into category filter box. 2. Enter `Flutter` into search box. 3. Observe result.                                           | Filter: `Kinh tế` · Keyword: `Flutter`           | **SRS does not specify combination behavior.** Record actual result: (a) No books -> AND logic, displays "Book not found"; (b) Display Flutter books (ignore filter) -> OR logic; (c) Other result. | REQ-03 | EP        |

---

### REQ-04 — Borrow Book

| TC ID | Test Objective                                                    | Prerequisites                                                              | Execution Steps                                                                                                                   | Input Data                             | Expected Result (excerpt SRS REQ-03)                                                                                                                                              | REQ    | Technique |
| ----- | -------------------------------------------------------------------- | --------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------ | --------- |
|TC-23|Verify successful book borrowing under limit (< 3)|Logged in as `"biet.hoang@email.com"` (currently has 1 active borrow)| 1. Go to "Books" tab.<br>2. Click the "+" button for an "Available" book (`"BOOK001"`) then "Borrow".<br>3. Navigate to the "Borrow / Return" tab.<br>4. Observe the newly created borrow record |`"BOOK001"`|Displays a success message. A borrow record is created with a due date set to exactly +14 days from today|REQ-04|EP(IDM: Available, Active, < 3)|
|TC-24|Verify borrow rejection at the boundary limit (= 3)|Logged in as `"ba.nguyen@email.com"` (currently has 1 active borrow)| 1. Borrow `"BOOK001"` (Total = 2).<br>2. Borrow `"BOOK002"` (Total = 3).<br>3. Attempt to borrow `"BOOK005"`|`"BOOK001"`, `"BOOK002"`, `"BOOK005"`|System allows the first 2 borrows. Rejects the 3rd attempt (`"BOOK005"`) with an error message stating the limit of 3 books is reached|REQ-04|BVA(IDM: = 3)|
|TC-25|Verify borrow rejection for "Suspended" member|Logged in as `"cu.le@email.com"` (Member status: Suspended)|1. Go to "Books" tab.<br>2. Select an "Available" book (`"BOOK001"`).<br>3. Click the"+" button then "Borrow"|`"BOOK001"`|The system rejects the request and displays a specific error message for the "Suspended" account status|REQ-04|DT/EP(IDM: Suspended)|
|TC-26|Verify borrow rejection for an already "Borrowed" book|Logged in as `"dam.tran@email.com"`|1. Go to "Books" tab.<br>2. Search or locate `"BOOK003"` (Status: Borrowed).<br>3. Observe the book card|`"BOOK003"`|The "+" borrow button is completely hidden/disabled for this book and status is "Borrowed"|REQ-04|EP(IDM: Borrowed)|
|TC-27|Verify borrow rejection for "Expired" member|Logged in as `"binh.pham@email.com"` (Member status: Expired)|1. Go to "Books" tab.<br>2. Select an "Available" book (`"BOOK001"`).<br>3. Click the"+" button then "Borrow"|`"BOOK001"`|The system rejects the request and displays a specific error message for the "Expired" account status|REQ-04|EP(IDM: Expired)|
|TC-28|Verify borrow rejection for "Lost" book|Member `"ba.nguyen@email.com"` is logged in|1. Locate `"BOOK007"` (Status: Lost).<br>2. Observe the book card|Book: `"BOOK007"`|The "+" borrow button is completely hidden/disabled and status is "Lost"|REQ-04|EP (IDM: Lost)|
|TC-29|Verify successful borrow at boundary limit (= 2)|Member `"ba.nguyen@email.com"` is logged in(has 1 active borrow)|1. Borrow BOOK002.<br>2. Observe system response|Book: `"BOOK002"`|System allows borrowing successfully (Total active borrows becomes exactly 2)|REQ-04|BVA (IDM: = 2)|

---

### REQ-05 — Return Book

| TC ID | Test Objective                                                    | Prerequisites                                                              | Execution Steps                                                                                                                   | Input Data                             | Expected Result (excerpt SRS REQ-03)                                                                                                                                              | REQ    | Technique |
| ----- | -------------------------------------------------------------------- | --------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------ | --------- |
|TC-30|Return a borrowed book on time, no overdue warning| Logged in ba.nguyen@email.com. Just borrowed BOOK001.|1.Go to tab "Borrow/Return". 2.Find BOOK001 in "My borrow records". 3. Click "Return book". Comfirm if prompted |BOOK001, borrow date = today, due date = today + 14 days|1.Success message show. 2.Record status "Returned" 3.BOOK001 status -> "Available". 4.No overdue warning|REQ -05|EP|
|TC-31|Return an overdue book- allowed but shows overdue warning | Logged in ba.nguyen@email.com. Record BR001 exists: MEM002 borrowed BOOK003, due 15/09/2024|1.Go to "Borrow/Return" tab. 2. Find "BOOK003" in "My Borrow records". 3. Click "Return book" |Record: BR001, BOOK003, due date: 15/09/2024, return date: today  |1. Return is accepted. 2. Overdue warning is diplayed 3. BR001 status -> "Returned" . 4. BOOK003 status -> "Available" |REQ -05 |EP, BVA |
|TC-32 |Cannot return a book borrowed by another member |Logged in ba.nguyen@email.com. BOOK013 is borrowed by another member, not MEM002 |1. Go to "Borrow/Return" tab. 2. Find BOOK013 in "My borrow records" |Account of MEM002 but BOOK013 is borrowed by MEM006 |1. Can not find BOOK013 in MEM002's list. 2. No "Return" button available for BOOK013. 3. System does not allow returning another member's book |REQ -05 |EP |
|TC-33| Book status updates immediately after return| Logged in as ba.nguyen@email.com.  BOOK003 currently shows "Borrowed" in Books tab|1.Go to "Borrow/Retur". 2. Find BR001 in "My borrow records". 3. Click "Return book" . 4. Immediately go back to "Books" tab. 5. Find BOOK003 |Record:BR001, BOOK003 |1.1. Before return: BOOK003 = "Borrowed". 2. After return: BOOK003 = "Available" — updates immediately |REQ -05 |EP |

---

### REQ-06 — Overdue Handling

| TC ID | Test Objective                                                    | Prerequisites                                                              | Execution Steps                                                                                                                   | Input Data                             | Expected Result (excerpt SRS REQ-03)                                                                                                                                              | REQ    | Technique |
| ----- | -------------------------------------------------------------------- | --------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------ | --------- |
| TC-34 | Check the record with a due date == today's date - 1                                            | The BR007 record exists, with status "Borrowing"                                     | 1. Log in using your Librarian account. 2. Click the "Check Expired" button.                                                    | System date: 5/23/2026. (dueDate == 5/22/2026)                       | BR007 changed to Overdue.                          | REQ-6 | EP, BVA  |
| TC-35 | Check the record with a due date == today's date + 1                                            | The BR008 record exists, with status "Borrowing"                                     | 1. Log in using your Librarian account. 2. Click the "Check Expired" button.                                                    | System date: 5/23/2026. (dueDate == 5/22/2026)                       | BR008 doesn't change                               | REQ-6 | EP, BVA  |
| TC-36 | Check the record with a due date == today's date                                                | The BR006 record exists, with status "Borrowing"                                     | 1. Log in using your Librarian account. 2. Click the "Check Expired" button.                                                    | System date: 5/23/2026. (dueDate == 5/23/2026)                       | BR006 change to overdue                            | REQ-6 | EP, BVA  |
| TC-37 | Check The system will ignore overdue records that have been returned.                           | The BR002 record exists, with status "Returned"                                      | 1. Log in using your Librarian account. 2. Click the "Check Expired" button.                                                    | System date: 5/23/2026. (dueDate == 24/08/2024) Status = Returned    | BR002 doesn't change                               | REQ-6 | EP, BVA  |
| TC-38 | Members can view their OWN overdue records.                                                     | Dam.tran's BR005 record is in Overdue status.                                        | 1. Log in using dam.tran account. 2. Click the "Borrowed/Returned" tab                                                          | Log in using the account dam.tran                                    | BR005 is displaying a "Overdue" status.            | REQ-6 | ACC      |
| TC-39 | Librarian can view all the overdue records.                                                     | Dam.tran's BR005 record and ba.nguyen's BR001 are in Overdue status.                 | 1. Log in using Librarian account. 2. Click the "Borrowed/Returned" tab 3. Click the "Check Expired" button.                    | Log in using the Librarian account.                                  | BR005 and BR001 are displaying a "Overdue" status. | REQ-6 | ACC      |

---

### REQ-07 — Member Management

| TC ID | Test Objective                                                    | Prerequisites                                                              | Execution Steps                                                                                                                   | Input Data                             | Expected Result (excerpt SRS REQ-03)                                                                                                                                              | REQ    | Technique |
| ----- | -------------------------------------------------------------------- | --------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------ | --------- |
| TC-40 | Verify that the "Add Member Successfully" (Happy Path) flow has been completed.                 | 1. Log in using Librarian account. 2. The email doesn't exist                        | 1. Log in using Librarian account. 2. Click the "Add member" button. 3. Enter the information. 4. Click the "Add member" button | Full Name: ABC. Email: test@gmail.com. Phone number: 0123456789      | Successfully                                       | REQ-7 | EP, ACC  |
| TC-41 | Catching email formatting errors: Missing @ character.                                          | 1. Log in using Librarian account.                                                   | 1. Log in using Librarian account. 2. Click the "Add member" button. 3. Enter the information. 4. Click the "Add member" button | Full Name: ABC. Email: userdomain.com. Phone number: 0123456789      | Invalid email                                      | REQ-7 | EP, ACC  |
| TC-42 | Catching email formatting errors: There is an @ symbol but NO period (.) in the domain section. | 1. Log in using Librarian account.                                                   | 1. Log in using Librarian account. 2. Click the "Add member" button. 3. Enter the information. 4. Click the "Add member" button | Full Name: ABC. Email: test@email. Phone number: 0123456789          | Invalid email                                      | REQ-7 | EP, ACC  |
| TC-43 | Ensure the system blocks the creation of DUPLICATE emails.                                      | 1. Log in using Librarian account. 2. The email "ba.nguyen@email.com" already exists | 1. Log in using Librarian account. 2. Click the "Add member" button. 3. Enter the information. 4. Click the "Add member" button | Full Name: ABC. Email: ba.nguyen@email.com. Phone number: 0123456789 | Email already exists                               | REQ-7 | EP, ACC  |

---

### REQ-08 — Borrow Record Lookup

| TC ID | Test Objective                                                    | Prerequisites                                                              | Execution Steps                                                                                                                   | Input Data                             | Expected Result (excerpt SRS REQ-03)                                                                                                                                              | REQ    | Technique |
| ----- | -------------------------------------------------------------------- | --------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------ | --------- |
|TC-44 |Librarian call view all borrow records from all members | Logged in as librarian@library.com. |1. Go to "Borrow/ Return tab. 2. View the full value records list. 3. Check if records from multiple members are visible | Account: librarian@library.com|1. All 5 seed records (BR001–BR005) are visible. 2. Records belong to different members (MEM002, MEM003, MEM006).3. Each record shows: record ID, book name, borrow date, due date, status. |REQ -08 |EP |
|TC-45 |Member can only see their own borrow records |Logged in as ba.nguyen@email.com(MEM002). MEM002 has BR001 and BR004. |1. Go to "Borrow/Return" tab. 2. View "My borrow records" list. Note all records shown |Account: ba.nguyen@email.com |1. Only BR001 (BOOK003) and BR004 (BOOK005) are visible.1. Only BR001 (BOOK003) and BR004 (BOOK005) are visible. 2. Records from MEM003 (BR002, BR005) and MEM006 (BR003) are NOT visible. 3.Each record shows: record ID, book, borrow date, due date, status|REQ -08 |EP |
|TC-46 |Member can not access another member's borrow records |Logged in as ba.nguyen@email.com (MEM002). BR003 belongs to MEM006. |1. Go to "Borrow/Return" tab. 2.Look for any way to view records of other members in "Search borrow records"(search by member ID,..). 3.Attempt to access BR003(MEM006's record) by any means |Account: ba.nguyen@email.com. Target record: BR003 (owned by MEM006) |1. No option to search or browse other members' records. 2. BR003 is not visible anywhere in MEM002's view. 3. System does not expose other members' borrow data.  |REQ- 08 |EP |
|TC-47|Returned-on-time record displays correct "Returned" status and all fields |Logged in as dam.tran@email.com (MEM003).BR002: BOOK001, returned 20/08/2024 — within due date. |1.Go to "Borrow/Return" tab. 2. Find record BR002. 3.Check status field. 4. Check all other fields |Account: dam.tran@email.com. Record: BR002 (BOOK001, returned on time) |1.BR002 visible in MEM003's list,. 2.Status "Returned". 3.Other fields correct: ID = BR002, book = "Lập trình Flutter cơ bản", borrow date = 10/08/2024, due date = 24/08/2024, return date. |REQ-08 |EP |
|TC-48 |Record status shows "Overdue" after overdue check | Logged in as librarian@library.com. BR001 (MEM002, BOOK003) is past due date 15/09/2024.|1. As Librarian,Go to "Borrow/Return" then click "Check overdue books" button. 2. Log out, log back in as ba.nguyen@email.com. 3. Go to "Borrow/Return" tab. 4. Find BR001. |Librarian triggers overdue check. Account for viewing: ba.nguyen@email.com. Record: BR001 |1. BR001 status updates to "Overdue". 2. MEM002 can see BR001 with status "Overdue" in their own records. 3. Librarian also sees BR001 as "Overdue" in the full list. |REQ -08|EP |
|TC-49 |Overdue record correctly transitions to "Returned" after late return |Logged in as librarian@library.com. BR001 not yet marked overdue. |1. As Librarian, click "Check overdue books" → BR001 becomes "Overdue". 2. Log out. 3. Log in as ba.nguyen@email.com. 4. Go to "Borrow/Return" tab, find BR001( status: "Overdue")5. Click "Return" on BR001. 6. Check BR001 status after returning | Record: BR001. Returning account: ba.nguyen@gmail.com|1.After step 1: BR001 status: "Overdue". 2.After step 5 BR001 status : "Returned" and success message show. 3.Record does not stuck at "Overdue" 4.BOOK003 changes to "Available" |REQ- 08 |EP |

---


## Tổng hợp

| Feature Group | # TCs | REQ Coverage | IDM Techniques Applied |
|---|---|---|---|
| Login | 7 | REQ-01 | EP |
| View Book List | 3 | REQ-02 | EP |
| Search & Filter Books | 11 | REQ-03 | EP, BVA |
| Borrow Book | 7 | REQ-04 | EP, BVA |
| Return Book | 4 | REQ-05 | EP, BVA |
| Overdue Handling | 6 | REQ-06 | EP, BVA |
| Member Management | 4 | REQ-07 | EP |
| Borrow Record Lookup | 6 | REQ-08 | EP |
| **Total** | **48** | **REQ-01 → REQ-08** | **EP, BVA** |
