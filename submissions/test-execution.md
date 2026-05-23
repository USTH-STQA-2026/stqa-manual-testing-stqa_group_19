# Test Execution

> **Instruction**: Run each TC on the system https://stqa.rbc.vn, record actual results.
> Conclusion: **Pass** (correct result), **Fail** (incorrect result -> create bug report), **Blocked** (cannot be executed due to another blocking bug), **Not Run** (not executed yet).

| Information        |                      |
| ---------------- | -------------------- |
| **Group**         | Group 19              |
| **Execution Date**| 22/05/2026           |
| **Browser**  | Chrome 136.0.7103.93 |
| **OS** | Windows 11           |

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

### REQ-03 — Search and Filter Books (TCs based on SRS)

| TC ID | Functional Group     | Expected Result (Summary)                                       | Actual Result                                                                                               | Conclusion | Evidence | Bug |
| ----- | ------------------ | ----------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------- | -------- | ---------- | --- |
| TC-10 | Search & Filter    | Only display BOOK001 when searching "Flutter"                           | List only displays BOOK001 "Lập trình Flutter cơ bản". Other books are hidden.                             | **Pass** | —          | —   |
| TC-11 | Search & Filter    | Display BOOK001, BOOK009 when searching "Nguyễn Minh Đức"             | List displays BOOK001 and BOOK009. Books by other authors are hidden.                                         | **Pass** | —          | —   |
| TC-12 | Search & Filter    | "Không tìm thấy sách" when searching "XYZ123abc"                       | List does not display any book. Displays message "Không tìm thấy sách".                                | **Pass** | —          | —   |
| TC-13 | Search & Filter    | Result same as TC-10 when searching "flutter" (lowercase)              | List displays BOOK001. Search is case-insensitive.                                            | **Pass** | —          | —   |
| TC-14 | Search & Filter    | Result same as TC-10 when searching "FLUTTER" (uppercase)                 | List displays BOOK001. Search is case-insensitive.                                            | **Pass** | —          | —   |
| TC-15 | Search & Filter    | Result same as TC-10 when searching "fLuTtEr" (mixed case)            | List displays BOOK001. Search is case-insensitive.                                            | **Pass** | —          | —   |
| TC-16 | Search & Filter    | Re-display 20 books when clearing search box to empty              | List displays all 20 book titles again. No longer filtered by any keyword.                                 | **Pass** | —          | —   |
| TC-17 | Search & Filter    | Only display Công nghệ books when entering "Công nghệ" into filter box      | List only displays BOOK001, 002, 003, 005, 008, 009, 010, 011. Books from other categories are hidden.                | **Pass** | —          | —   |
| TC-22 | Search & Filter    | Filter "công nghệ" (lowercase)                                   | List displays no books. Filter is case-sensitive.                                            | **Fail** | —          | BUG-01 |
| TC-23 | Search & Filter    | Filter "CÔNG NGHỆ" (uppercase)                                      | List displays no books. Filter is case-sensitive.                                            | **Fail** | —          | BUG-01 |

---

### REQ-03 — Record SRS Gap (TC-25 → TC-26)

> These TCs are executed to **observe actual behavior**, not imposing an expected result from SRS because the SRS is not specified.
> - **TC-25, TC-26**: Test behavior when using search + filter simultaneously (SRS does not specify AND/OR logic).

| TC ID | Functional Group     | Observation (not expected result)                                         | Actual Result                                                                                                                                           | Conclusion                    | Evidence | Bug    |
| ----- | ------------------ | ----------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------- | ---------- | ------ |
| TC-25 | SRS Gap — combine  | Observation: filter "Công nghệ" + search "Python" — system uses AND or OR?        | List displays **only BOOK009** "Nhập môn lập trình Python". Confirm system uses **AND logic** (both category filter AND search are applied).       | **Pass** *(SRS gap — observe)* | —      | —      |
| TC-26 | SRS Gap — combine  | Observation: filter "Kinh tế" + search "Flutter" — result when AND has no books? | List displays **BOOK007, BOOK014, BOOK015** (all Kinh tế books). The "Flutter" search is **ignored** as there's no matching result in category — behavior is **inconsistent** with TC-25 (AND logic). | **Fail** *(SRS gap)*        | —          | BUG-02 |

---

## Result Summary

| Metric              | Value |
| ------------------- | ------- |
| Total test cases   | 19      |
| Pass                | 16      |
| Fail                | 3       |
| Blocked             | 0       |
| Not Run             | 0       |
| **Pass Rate**      | **84.2%** |

### Results by Functional Group

| Group                                      | Total TC | Pass | Fail | Pass Rate |
| ----------------------------------------- | ------- | ---- | ---- | ---------- |
| REQ-01 — Login                        | 7       | 7    | 0    | 100%       |
| REQ-03 — Search & Filter (with SRS)         | 10      | 8    | 2    | 80%        |
| REQ-03 — SRS Gap (TC-25 → TC-26)         | 2       | 1    | 1    | 50%        |
| **Total**                                  | **19**  | **16** | **3** | **84.2%** |

---

> ### 📝 Summary Notes
>
> **Notes on Fail test cases:**
>
> 1. **BUG-01** (TC-22, TC-23): The category filter box is **case-sensitive**. User inputs "công nghệ" -> no books found. SRS only requires case-insensitive for the *search* box, and does not mention the filter box. This lack of consistency can confuse users. Recommendation: BA needs to supplement the specification, Dev should handle case-insensitive for both the filter box and search box to standardize UX.
>
> 2. **BUG-02** (TC-26): Combined search + filter behavior is **inconsistent** — TC-25 confirmed AND logic works when there is a result, but TC-26 shows when AND has no result, the system **ignores the search box** and only displays category filter results. SRS does not specify this case — this is an SRS gap and simultaneously an inconsistent logic bug.
