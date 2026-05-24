# Test Execution — Kết quả thực thi kiểm thử

| Thông tin | |
|---|---|
| **Nhóm** | Group 19 |
| **Ngày thực thi** | 24/05/2026 |
| **Trình duyệt** | Chrome 148.0.7778.179 (Official Build) (64-bit) |
| **Hệ điều hành** | Windows |

---

## Detailed Results

### REQ-01 — Login

| TC ID | Functional Group | Expected Result (Summary)                               | Actual Result                                                                                             | Conclusion | Evidence | Bug    |
| ----- | -------------- | --------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------- | -------- | ---------- | ------ |
| TC-01 | Login      | Go to home page, AppBar displays "Thủ thư"              | System redirects to home page. AppBar shows name and role "Thủ thư". "Thành viên" tab appears.      | **Pass** | —          | —      |
| TC-02 | Login      | Go to home page, AppBar displays "Thành viên"           | System redirects to home page. AppBar shows "Ba Nguyễn — Thành viên". "Thành viên" tab does not appear. | **Pass** | —          | —      |
| TC-03 | Login      | "Không tìm thấy thành viên"                              | System does not redirect. Displays message "Không tìm thấy thành viên".                               | **Pass** | —          | —      |
| TC-04 | Login      | "Mật khẩu không đúng"                                    | System does not redirect. Displays message "Mật khẩu không đúng".                                     | **Pass** | —          | —      |
| TC-05 | Login      | "Vui lòng nhập email và mật khẩu"                        | System does not redirect. Displays message "Vui lòng nhập email và mật khẩu".                         | **Pass** | —          | —      |
| TC-06 | Login      | "Vui lòng nhập email và mật khẩu"                        | System does not redirect. Displays message "Vui lòng nhập email và mật khẩu".                         | **Pass** | —          | —      |
| TC-07 | Login      | "Vui lòng nhập email và mật khẩu"                        | System does not redirect. Displays message "Vui lòng nhập email và mật khẩu".                         | **Pass** | —          | —      |

---

### REQ-02 — View Book List

| TC ID | Feature Group | Expected Result (Summary) | Actual Result | Conclude | Proof(evidence) | Bug |
|-------|---------------|---------------------------|-----------------|---------|-----------|----| 
| TC-08 | REQ-02 (View Book) |Displays all 20 books with 5 detailed information fields | System displays all 20 books with 5 fields (Title, Author, Genre, Publication Year, Status) | Pass |-| None |
| TC-09 | REQ-02 (View Book) |Book status changes to "Borrowed" and (+) button is hidden instantly without page refresh (F5)|Status updates to "Borrowed", and the (+) button disappears instantly without needing a page refresh|Pass|-|None |
| TC-10 | REQ-02 (View Book) |Librarian can view all 20 books with the 5 detailed fields |Librarian can view all 20 books with the exact same 5 details as a Member |Pass|-|None|

### REQ-03 — Search and Filter Books (TCs based on SRS)

| TC ID | Functional Group     | Expected Result (Summary)                                       | Actual Result                                                                                               | Conclusion | Evidence | Bug |
| ----- | ------------------ | ----------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------- | -------- | ---------- | --- |
| TC-11 | Search & Filter    | Only display BOOK001 when searching "Flutter"                           | List only displays BOOK001 "Lập trình Flutter cơ bản". Other books are hidden.                             | **Pass** | —          | —   |
| TC-12 | Search & Filter    | Display BOOK001, BOOK009 when searching "Nguyễn Minh Đức"             | List displays BOOK001 and BOOK009. Books by other authors are hidden.                                         | **Pass** | —          | —   |
| TC-13 | Search & Filter    | "Không tìm thấy sách" when searching "XYZ123abc"                       | List does not display any book. Displays message "Không tìm thấy sách".                                | **Pass** | —          | —   |
| TC-14 | Search & Filter    | Result same as TC-10 when searching "flutter" (lowercase)              | List displays BOOK001. Search is case-insensitive.                                            | **Pass** | —          | —   |
| TC-15 | Search & Filter    | Result same as TC-10 when searching "FLUTTER" (uppercase)                 | List displays BOOK001. Search is case-insensitive.                                            | **Pass** | —          | —   |
| TC-16 | Search & Filter    | Result same as TC-10 when searching "fLuTtEr" (mixed case)            | List displays BOOK001. Search is case-insensitive.                                            | **Pass** | —          | —   |
| TC-17 | Search & Filter    | Re-display 20 books when clearing search box to empty              | List displays all 20 book titles again. No longer filtered by any keyword.                                 | **Pass** | —          | —   |
| TC-18 | Search & Filter    | Only display Công nghệ books when entering "Công nghệ" into filter box      | List only displays BOOK001, 002, 003, 005, 008, 009, 010, 011. Books from other categories are hidden.                | **Pass** | —          | —   |
| TC-19 | Search & Filter    | Filter "công nghệ" (lowercase)                                   | List displays no books. Filter is case-sensitive.                                            | **Fail** | —          | BUG-01 |
| TC-20 | Search & Filter    | Filter "CÔNG NGHỆ" (uppercase)                                      | List displays no books. Filter is case-sensitive.                                            | **Fail** | —          | BUG-01 |
| TC-21 | SRS Gap — combine  | Observation: filter "Công nghệ" + search "Python" — system uses AND or OR?        | List displays **only BOOK009** "Nhập môn lập trình Python". Confirm system uses **AND logic** (both category filter AND search are applied).       | **Pass** *(SRS gap — observe)* | —      | —      |
| TC-22 | SRS Gap — combine  | Observation: filter "Kinh tế" + search "Flutter" — result when AND has no books? | List displays **BOOK007, BOOK014, BOOK015** (all Kinh tế books). The "Flutter" search is **ignored** as there's no matching result in category — behavior is **inconsistent** with TC-25 (AND logic). | **Fail** *(SRS gap)*        | —          | BUG-02 |


### REQ-04: / Borrow Book

| TC ID | Feature Group | Expected Result (Summary) | Actual Result | Conclude | Proof(evidence) | Bug |
|-------|---------------|---------------------------|-----------------|---------|-----------|----| 
| TC-23 | REQ-04 (Borrow) |Shows success message. Borrow record shows due date = borrow date + 14 days |Success message displayed. Borrow/Return tab shows the due date is exactly 14 days from the borrow date|Pass|-|None|
| TC-24 | REQ-04 (Borrow) |System rejects the 4th book attempt with a "limit reached" error message|System allows borrowing up to the 4th book successfully. It only rejects and shows an error on the 5th attempt|Fail|<img src="bug01.png" width="150">|BUG|                 
| TC-25 | REQ-04 (Borrow) |System rejects the action and shows a specific error for "Suspended" accounts|System blocks the action but displays the incorrect error message: "Member has expired"|Fail|<img src="bug02.png" width="150">|BUG|
| TC-26 | REQ-04 (Borrow) |Books with "Borrowed" status have the (+) borrow button hidden/disabled|BOOK003 does not have the (+) button; the borrow action cannot be performed|Pass|-|None|
| TC-27 | REQ-04 (Borrow) |System rejects the action and shows an error message for "Expired" accounts|System blocks the action and displays the correct error message for expired accounts|Pass|-|None |
| TC-28 | REQ-04 (Borrow) |Books with "Lost" status have the (+) borrow button hidden/disabled|BOOK007 does not have the (+) button; the borrow action cannot be performed|Pass|-|None|
| TC-29 | REQ-04 (Borrow) |System allows successful borrow (bringing the total active borrows to exactly 2)|System allows the borrow action successfully, bringing the total number of borrowed books to 2|Pass|-|None |

### REQ-05: Return Book

### REQ-06 — Overdue Handling

| TC ID | Feature Group | Expected Result (Summary) | Actual Result | Conclude | Proof(evidence) | Bug |
|-------|---------------|---------------------------|-----------------|---------|-----------|----| 
| TC-34  | REQ-06         | BR007 changes to Overdue status.                                                                      | The system changes BR007 to Overdue                                                     | Pass     |            |     |
| TC-35  | REQ-06         | BR008 DOES NOT change status (remains Borrowing).                                                     | BR008 maintains its Borrowing status.                                                   | Pass     |            |
| TC-36  | REQ-06         | BR006 changes to Overdue status.                                                                      | BR006 maintains its Borrowing status.                                                   | Fail     |            |
| TC-37  | REQ-06         | BR002 (Returned) DOES NOT change status.                                                              | BR002 isnt changed status.                                                              | Pass     |            |
| TC-38  | REQ-06         | User dam.tran sees BR005 is Overdue.                                                                  | User dam.tran sees BR005 is Overdue.                                                    | Pass     |            |
| TC-39  | REQ-06         | Librarian sees both BR005 and BR001 as Overdue.                                                       | Librarian sees both BR005 and BR001 as Overdue.                                         | Pass     |    

### REQ-07 — Member Management

| TC ID | Feature Group | Expected Result (Summary) | Actual Result | Conclude | Proof(evidence) | Bug |
|-------|---------------|---------------------------|-----------------|---------|-----------|----| 
| TC-40  | REQ-07         | Member added successfully.                                                                            | The system rejects the input and incorrectly displays an "Invalid email" error message. | Fail     |            |
| TC-41  | REQ-07         | The system display an "Invalid email" error message                                                   | The system display an "Invalid email" error message                                     | Pass     |            |
| TC-42 | REQ-07         | The system accepts the input, successfully registers the member, and displays a success notification. | The system display an "Invalid email" error message                                     | Fail     |            |
| TC-43 | REQ-07         | The system display an "Invalid email" error message                                                   | The system display an "Invalid email" error message                                     | Pass     |

## Tổng hợp kết quả

| Chỉ số | Giá trị |
|--------|---------|
| Tổng số test case | `<!-- số -->` |
| Pass | `<!-- số -->` |
| Fail | `<!-- số -->` |
| Blocked | `<!-- số -->` |
| Not Run | `<!-- số -->` |
| **Tỷ lệ Pass** | `<!-- xx% -->` |

### Kết quả theo nhóm chức năng

| Nhóm | Tổng TC | Pass | Fail | Tỷ lệ Pass |
|------|---------|------|------|------------|
| | | | | |
