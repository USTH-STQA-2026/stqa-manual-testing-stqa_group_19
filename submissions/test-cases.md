# Test Cases

> **Instruction**: Write a minimum of **20 TCs** covering all main functions (REQ-01 → REQ-08).
> See [examples/sample-test-case.md](../examples/sample-test-case.md) to understand how to write good TCs.
> Organize and group test cases in the most logical way.

| Information      |                        |
| -------------- | ---------------------- |
| **Group**       | Group 19                |
| **Creation Date**   | 22/05/2026             |
| **System**   | https://stqa.rbc.vn    |
| **Reference** | SRS v1.0               |

---

## Step 1: Input Domain Modeling (IDM)

> 📖 **Textbook:** Chapter 6 — *Input Domain Modeling*, Paul Ammann & Jeff Offutt.
>
> **Before writing Test Cases**, the group **must** analyze the input domain using the IDM table below.
> Each function needs to determine: **Characteristic**, **Block/Partition**, and **Value**.

---

### IDM — Login (REQ-01)

> **SRS Source**: REQ-01 — "correct email + password -> redirect to home page; incorrect -> appropriate error message"

| Characteristic  | Block       | Value                           | Expected Result (from SRS)                          |
| -------------------------- | ----------------------- | -------------------------------------------------- | ---------------------------------------------------- |
| Does email exist in DB? | Yes — Librarian role    | `librarian@library.com`                            | Login successfully, display "Librarian" role    |
|                            | Yes — Member role | `ba.nguyen@email.com`                              | Login successfully, display "Member" role |
|                            | Does not exist           | `noone@example.com`                                | "Member not found"                          |
| Is password correct?          | Correct                    | `admin123`                                         | Login successfully                                 |
|                            | Incorrect                     | `wrongpass`                                        | "Password incorrect"                               |
| Is input box empty?            | Both are empty             | email: `""`, password: `""`                        | "Please enter email and password"                   |
|                            | Only email empty          | email: `""`, password: `admin123`                  | "Please enter email and password"                   |
|                            | Only password empty       | email: `librarian@library.com`, password: `""`     | "Please enter email and password"                   |

---

### IDM — Search and Filter Books (REQ-03)

> **SRS Source**: REQ-03 — "Search by book title or author; Filter by category; Search is case-insensitive; No results -> 'Book not found'"
>
> ⚠️ **Identified SRS gap:**
> - SRS **does not specify** case-sensitivity for the category filter box (only specifies for search)
> - SRS **does not specify** behavior when using search and filter simultaneously (AND, OR, or independent)
> - TCs related to these two points will be clearly stated as "observe actual" instead of concluding Pass/Fail definitively

| Characteristic          | Block                        | Value     | Expected Result (from SRS)                              |
| ---------------------------------- | ---------------------------------------- | ---------------------------- | -------------------------------------------------------- |
| **Search** — keyword matches DB?   | Yes (book title)                            | `"Flutter"`                  | Displays BOOK001                                         |
|                                    | Yes (author name)                         | `"Nguyễn Minh Đức"`          | Displays BOOK001, BOOK009                                |
|                                    | No match                               | `"XYZ123abc"`                | "Book not found"                                    |
| **Search** — case sensitivity?    | All lowercase                          | `"flutter"`                  | Result same as searching `"Flutter"` *(SRS: case-insensitive)* |
|                                    | All uppercase                             | `"FLUTTER"`                  | Result same as searching `"Flutter"` *(SRS: case-insensitive)* |
|                                    | Mixed case                              | `"fLuTtEr"`                  | Result same as searching `"Flutter"` *(SRS: case-insensitive)* |
| **Search** — empty box?            | Empty                                     | `""`                         | Displays full list (no filter)                   |
| **Category Filter** — manual input     | Exact category match                  | `"Công nghệ"`                | Only displays Technology books *(SRS: filter by category)* |
| **Category Filter** — case sensitivity | All lowercase                          | `"công nghệ"`                | Displays Technology books (same as search, case-insensitive) |
|                                    | All uppercase                             | `"CÔNG NGHỆ"`                | Displays Technology books (same as search, case-insensitive) |
| **Combine** search + filter        | Both have individual results           | Filter `"Công nghệ"` + search `"Python"` | **SRS gap** — observe and record actual result   |
|                                    | One condition not matched in category  | Filter `"Kinh tế"` + search `"Flutter"`  | **SRS gap** — observe and record actual result   |

---

### IDM — Borrow Books (REQ-04, REQ-05)

| Characteristic | Block   | Value | Expected Result                 |
| ------------------------- | ------------------- | ------------------------ | -------------------------------- |
| Book status?          | Available              | BOOK001                  | Allowed to borrow                    |
|                           | Borrowed           | BOOK003                  | Not allowed                   |
|                           | Lost            | BOOK007                  | Not allowed                   |
| Member status?    | Active           | MEM002                   | Allowed to borrow                    |
|                           | Suspended           | MEM004                   | Denied, suspended error message |
|                           | Expired             | MEM005                   | Denied, expired error message   |
| Number of borrowed books?        | < 3 (BVA: 0, 1, 2)  | MEM006 (0 books)          | Allowed to borrow                    |
|                           | = 3 (BVA: limit) | MEM borrowed 3 books       | Denied, exceed limit error message |

---

### IDM — REQ-05 to REQ-08 (added by group)

| Characteristic | Block | Value | Expected Result |
| ------------------------- | ----------------- | ------------------------ | ---------------- |
| `<!-- Group fill in -->`   |                   |                          |                  |

---

## Step 2: Test Cases

---

### REQ-01 — Login

> **Applied technique:** EP — divide email and password into valid / invalid classes, each class tests 1 representative.
> **Expected result source:** SRS REQ-01 — "Error message" and "Rule" columns.

| TC ID | Test Objective                                          | Prerequisites                                                        | Execution Steps                                                                                                   | Input Data                                                    | Expected Result (excerpt SRS REQ-01)                                                                                                         | REQ    | Technique |
| ----- | ---------------------------------------------------------- | --------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------- | ------ | -------- |
| TC-01 | Successful login with Librarian account                 | Chrome browser opened https://stqa.rbc.vn. Not logged in.           | 1. Enter email into "Email" box. 2. Enter password into "Password" box. 3. Click "Login" button.                       | Email: `librarian@library.com` · Password: `admin123`             | System redirects to home page. AppBar shows username and role "Thủ thư". "Thành viên" tab is displayed (Librarian privilege).        | REQ-01 | EP       |
| TC-02 | Successful login with Member account              | Chrome browser opened https://stqa.rbc.vn. Not logged in.           | 1. Enter email into "Email" box. 2. Enter password into "Password" box. 3. Click "Login" button.                       | Email: `ba.nguyen@email.com` · Password: `password123`            | System redirects to home page. AppBar shows name "Ba Nguyễn" and role "Thành viên". "Thành viên" (management) tab is **not** displayed.    | REQ-01 | EP       |
| TC-03 | Login failed — email does not exist                   | Chrome browser opened https://stqa.rbc.vn. Not logged in.           | 1. Enter email not in system. 2. Enter any password. 3. Click "Login" button.                       | Email: `noone@example.com` · Password: `admin123`                 | System does **not** redirect. Displays: **"Không tìm thấy thành viên"**. Login page is still displayed.                                  | REQ-01 | EP       |
| TC-04 | Login failed — incorrect password                          | Chrome browser opened https://stqa.rbc.vn. Not logged in.           | 1. Enter valid email. 2. Enter wrong password. 3. Click "Login" button.                                           | Email: `librarian@library.com` · Password: `wrongpass`            | System does **not** redirect. Displays: **"Mật khẩu không đúng"**. Login page is still displayed.                                       | REQ-01 | EP       |
| TC-05 | Login failed — leave both boxes empty                     | Chrome browser opened https://stqa.rbc.vn. Not logged in.           | 1. Leave "Email" empty. 2. Leave "Password" empty. 3. Click "Login" button.                                       | Email: *(empty)* · Password: *(empty)*                      | System does **not** redirect. Displays: **"Vui lòng nhập email và mật khẩu"**. Login page is still displayed.                           | REQ-01 | EP       |
| TC-06 | Login failed — only leave email empty                    | Chrome browser opened https://stqa.rbc.vn. Not logged in.           | 1. Leave "Email" empty. 2. Enter password. 3. Click "Login" button.                                               | Email: *(empty)* · Password: `admin123`                        | System does **not** redirect. Displays: **"Vui lòng nhập email và mật khẩu"**. Login page is still displayed.                           | REQ-01 | EP       |
| TC-07 | Login failed — only leave password empty                 | Chrome browser opened https://stqa.rbc.vn. Not logged in.           | 1. Enter valid email. 2. Leave "Password" empty. 3. Click "Login" button.                                        | Email: `librarian@library.com` · Password: *(empty)*           | System does **not** redirect. Displays: **"Vui lòng nhập email và mật khẩu"**. Login page is still displayed.                           | REQ-01 | EP       |

---

### REQ-03 — Search and Filter Books

> **Applied technique:** EP for search (has result / no result); BVA at empty box boundary (TC-16, TC-20).
> **Expected result source:** SRS REQ-03 — "Search by book title or author; Filter by category; case-insensitive; no result -> 'Book not found'".
>
> **Note on SRS gap:** SRS does not specify case-sensitivity for filter box, and does not specify behavior when using search + filter simultaneously. Related TCs (TC-22 → TC-26) record actual result to report gap, **do not** conclude Pass/Fail based on self-determined expected result.

| TC ID | Test Objective                                                    | Prerequisites                                                              | Execution Steps                                                                                                                   | Input Data                             | Expected Result (excerpt SRS REQ-03)                                                                                                                                              | REQ    | Technique |
| ----- | -------------------------------------------------------------------- | --------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------ | --------- |
| TC-10 | Search by book title — has result                                  | Logged in (any account). On "Books" tab. Filter box is empty.      | 1. Enter keyword into search box. 2. Observe book list.                                                                     | Keyword: `Flutter`                          | List only displays books with "Flutter" in **book title**: BOOK001 "Lập trình Flutter cơ bản". Other books are hidden. *(SRS: search by book title)*                              | REQ-03 | EP        |
| TC-11 | Search by author name — has result                               | Logged in (any account). On "Books" tab. Filter box is empty.      | 1. Enter author name into search box. 2. Observe book list.                                                                 | Keyword: `Nguyễn Minh Đức`                  | List displays all books by author Nguyễn Minh Đức: BOOK001, BOOK009. Books by other authors are hidden. *(SRS: search by author)*                                      | REQ-03 | EP        |
| TC-12 | Search — no result                                          | Logged in (any account). On "Books" tab. Filter box is empty.      | 1. Enter keyword not matching any book. 2. Observe book list.                                                         | Keyword: `XYZ123abc`                        | List **does not display any book**. System displays message **"Không tìm thấy sách"**. *(SRS: "No result -> display message")*                                 | REQ-03 | EP        |
| TC-13 | Case-insensitive search — all lowercase                          | Logged in (any account). On "Books" tab. Filter box is empty.      | 1. Enter all lowercase keyword into search box. 2. Observe result.                                                            | Keyword: `flutter`                          | List displays **exact result as TC-10**: BOOK001. *(SRS: "Search is NOT case-sensitive")*                                                                   | REQ-03 | EP        |
| TC-14 | Case-insensitive search — all uppercase                             | Logged in (any account). On "Books" tab. Filter box is empty.      | 1. Enter all uppercase keyword into search box. 2. Observe result.                                                               | Keyword: `FLUTTER`                          | List displays **exact result as TC-10**: BOOK001. *(SRS: "Search is NOT case-sensitive")*                                                                   | REQ-03 | EP        |
| TC-15 | Case-insensitive search — mixed case                           | Logged in (any account). On "Books" tab. Filter box is empty.      | 1. Enter mixed case keyword into search box. 2. Observe result.                                                             | Keyword: `fLuTtEr`                          | List displays **exact result as TC-10**: BOOK001. *(SRS: "Search is NOT case-sensitive")*                                                                   | REQ-03 | EP        |
| TC-16 | Clear search keyword — display full list again               | Logged in. Search box has keyword `flutter`. Viewing filter results. | 1. Clear entire content of search box (leave empty). 2. Observe book list.                                                       | Keyword: *(clear to empty)*                 | List re-displays **all 20 book titles** (seed data). No longer filtered by any keyword. *(BVA: lower boundary of search box)*                                                  | REQ-03 | BVA       |
| TC-17 | Filter by category — "Công nghệ" (correct format)                         | Logged in (any account). On "Books" tab. Search box is empty. | 1. Enter `Công nghệ` into **category filter box**. 2. Observe book list.                                                    | Filter: `Công nghệ`                         | List only displays books in Technology category: BOOK001, BOOK002, BOOK003, BOOK005, BOOK008, BOOK009, BOOK010, BOOK011. Other category books are hidden. *(SRS: filter by category)* | REQ-03 | EP        |
| TC-22 | Case-insensitive category filter — all lowercase                          | Logged in (any account). On "Books" tab. Search box is empty. | 1. Enter `công nghệ` (all lowercase) into category filter box. 2. Observe book list.                                                   | Filter: `công nghệ` *(all lowercase)*          | List only displays books in Technology category    | REQ-03 | EP        |
| TC-23 | Case-insensitive category filter — all uppercase                             | Logged in (any account). On "Books" tab. Search box is empty. | 1. Enter `CÔNG NGHỆ` (all uppercase) into category filter box. 2. Observe book list.                                                     | Filter: `CÔNG NGHỆ` *(all uppercase)*            | List only displays books in Technology category    | REQ-03 | EP        |

---

> ### ⚠️ TC-25 → TC-26 — Record SRS Gap (do not conclude Pass/Fail)
>
> SRS REQ-03 **does not specify** case-sensitivity for the category filter box and **does not specify** behavior when combining search + filter.
> The TCs below are executed to **observe and record actual reality**, not imposing expected result.
> Actual results will be reported in `summary.md` as an **SRS gap needing clarification**.

| TC ID | Test Objective                                                        | Prerequisites                                                              | Execution Steps                                                                                                                           | Input Data                                  | Expected Result                                                                                                                                          | REQ    | Technique |
| ----- | ------------------------------------------------------------------------ | --------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------- | ------ | --------- |
| TC-25 | **[SRS gap]** Combine search + filter — both have individual results  | Logged in (any account). On "Books" tab.                        | 1. Enter `Công nghệ` into category filter box. 2. Enter `Python` into search box. 3. Observe result.                                          | Filter: `Công nghệ` · Keyword: `Python`          | **SRS does not specify combination behavior.** Record actual result: (a) Only display BOOK009 -> AND logic; (b) Display all Technology books + Python books -> OR logic; (c) Other result. | REQ-03 | EP        |
| TC-26 | **[SRS gap]** Combine search + filter — search has no result in filtered category | Logged in (any account). On "Books" tab.           | 1. Enter `Kinh tế` into category filter box. 2. Enter `Flutter` into search box. 3. Observe result.                                           | Filter: `Kinh tế` · Keyword: `Flutter`           | **SRS does not specify combination behavior.** Record actual result: (a) No books -> AND logic, displays "Book not found"; (b) Display Flutter books (ignore filter) -> OR logic; (c) Other result. | REQ-03 | EP        |

---

## Summary

| Functional Group                       | TC Count | REQ Covered | IDM Technique Applied |
| ------------------------------------ | ----- | ------- | -------------------- |
| Login                            | 7     | REQ-01  | EP                   |
| Search & filter books (with clear SRS)| 10    | REQ-03  | EP, BVA (TC-16)      |
| Record SRS gap                     | 2     | REQ-03  | EP (observe)        |
| Borrow books *(waiting to fill)*               |       | REQ-04  |                      |
| Return books *(waiting to fill)*                |       | REQ-05  |                      |
| Overdue *(waiting to fill)*                 |       | REQ-06  |                      |
| Member management *(waiting to fill)*      |       | REQ-07  |                      |
| Borrow receipt *(waiting to fill)*              |       | REQ-08  |                      |
| **Total (REQ-01+03)**                 | **19**|         | EP, BVA              |
