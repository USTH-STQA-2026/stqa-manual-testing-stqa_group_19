# Bug Reports — Báo cáo lỗi

> **Hướng dẫn**: Tạo 1 mục bug cho mỗi TC có kết quả **Fail**.
> Xem [examples/sample-bug-report.md](../examples/sample-bug-report.md) để hiểu cách viết bug report tốt.
> Mỗi bug cần: tiêu đề mô tả hành vi lỗi, bước tái hiện, expected vs actual, severity + giải thích.

| Thông tin | |
|---|---|
| **Nhóm** | `<!-- Tên nhóm -->` |
| **Ngày báo cáo** | `<!-- DD/MM/YYYY -->` |

---

## BUG-01

| Thuộc tính | Chi tiết |
|-----------|---------|
| **Mã lỗi** | BUG-01 |
| **TC liên quan** | TC-502 |
| **REQ liên quan** | REQ-05 |
| **Mức độ** | High |
| **Người phát hiện** | Tạ Quang Huy |
| **Ngày phát hiện** | 24/05/2026|
| **Trạng thái** | Open |

**Tiêu đề:**
System does not display overdue warning when a member returns an overdue book


**Môi trường:**
- Trình duyệt: Chrome 148.0.7778.179
- Hệ điều hành: Window 11
- Ngôn ngữ giao diện: Tiếng Việt/ English

**Điều kiện tiên quyết:**
Logged in as ba.nguyen@email.com (MEM002)
Fresh seed data (page refreshed or "Khôi phục dữ liệu" clicked)
Borrow record BR001 exists: MEM002 borrowed BOOK003, due date 15/09/2024 (overdue as of 2026)



**Bước tái hiện:**
1. Log in as `ba.nguyen@email.com`
2. Go to tab "Borrow/Return"
3. Find borrow record BR001 (BOOK003 — "Kiểm thử phần mềm nhập môn", due 15/09/2024)
4. Click "Return" on BR001
5. Confirm the return action if prompted
6. Observe the result

**Kết quả mong đợi:**
According to REQ-05:
- Return is accepted
- An overdue warning message is displayed (e.g. "Sách trả quá hạn", banner, or modal alert)
- BR001 status changes to "Returned"
- BOOK003 status changes to "Available"

**Kết quả thực tế:**
- Return was accepted and BR001 status changed to "Returned"
- BOOK003 status changed to "Available"
- No overdue warning was displayed at any point — the return completed silently as if the book was returned on time

**Tác động:**
- Violates REQ-05 business rule: overdue returns must trigger a warning
- Librarian and member have no visibility into whether a return was late
- Late return history is lost — cannot be used for future penalty or policy enforcement
- Members are not informed of overdue behavior, undermining accountability`

**Minh chứng:**

Before Return
![BUG-01 Screenshot](bug01-tc502.png) 
After Return
![BUG-01 Screenshot](bug01-tc502(2).png)
**Đề xuất xử lý:**
After the return action, compare the actual return date with `dueDate` on the borrow record. If `returnDate > dueDate`, display an overdue warning message before or after confirming the return

---

## BUG-02

| Thuộc tính | Chi tiết |
|-----------|---------|
| **Mã lỗi** | BUG-02 |
| **TC liên quan** | TC-803 |
| **REQ liên quan** | REQ-08 |
| **Mức độ** | High |
| **Người phát hiện** | Tạ Quang Huy |
| **Ngày phát hiện** | 25/05/2026 |
| **Trạng thái** | Open |

**Tiêu đề:**
Member can search and view borrow records of other members, via "Search borrow records" = unauthorized read access

**Môi trường:**
- Trình duyệt: Chrome 148.0.7778.179
- Hệ điều hành: Window 11
- Ngôn ngữ giao diện: Tiếng Việt/ English

**Điều kiện tiên quyết:**
- Logged in as `ba.nguyen@email.com` (MEM002 — Nguyễn Học Bá, role: Member)
- Fresh seed data (page refreshed or "Khôi phục dữ liệu" clicked)
- BR003 exists: borrowed by MEM006 (Hoàng Cá Biệt), book "Quản trị nhân sự hiện đại", status "Borrowing"

**Bước tái hiện:**
1. Log in as `ba.nguyen@email.com` (MEM002)
2. Go to tab "Borrow/Return"
3. Click "Search borrow records" tab
4. In the search field, type `MEM006` and click "Search"
5. Observe the search results


**Kết quả mong đợi:**
According to REQ-08: 
- Search by another member's ID returns **no results** for a Member-role user
- MEM002 cannot see BR003 (owned by MEM006) under any circumstance
- BR003 is not visible anywhere in MEM002's view under any circumstance
- System enforces read and write isolation between member accounts

**Kết quả thực tế:**
- Searching "MEM006" in the "Search borrow records" field returned BR003 (book: "Quản trị nhân sự hiện đại", member: Hoàng Cá Biệt, borrow date: 01/10/2024, due: 15/10/2024)
- The record was fully visible including member name and borrow details
**Tác động:**
- Directly violates REQ-08: a member can read the full borrow history of any other member
- Privacy breach — any member can find out what books other members are currently borrowing
- The search field provides no access control filtering for Member-role users

**Minh chứng:**
![BUG-02 Screenshot](bug02-tc803.png)

**Đề xuất xử lý:**
- The "Search borrow records" feature must enforce role-based filtering on the server/data side:
  - If role = "Member": search scope must be restricted to records where `memberId == currentUserId` only — regardless of search input
  - If role = "Librarian": full search access is permitted
- Search input from a Member-role user must never override the access control scope

---

# BUG-03

| Thuộc tính | Chi tiết |
|-----------|---------|
| **Mã lỗi** | BUG-03 |
| **TC liên quan** | TC-804 |
| **REQ liên quan** | REQ-05, REQ-08 |
| **Mức độ** | Critical |
| **Người phát hiện** |Tạ Quang Huy |
| **Ngày phát hiện** | 25/05/2026 |
| **Trạng thái** | Open |

**Tiêu đề:**
Member can return a book on behalf of another member — unauthorized write action

**Môi trường:**
- Trình duyệt: Chrome 148.0.7778.179
- Hệ điều hành: Window 11
- Ngôn ngữ giao diện: Tiếng Việt/ English

**Điều kiện tiên quyết:**
- Logged in as `ba.nguyen@email.com` (MEM002 — Nguyễn Học Bá, role: Thành viên)
- Fresh seed data (page refreshed or "Khôi phục dữ liệu" clicked)
- BR003 exists: borrowed by MEM006 (Hoàng Cá Biệt), BOOK013 "Quản trị nhân sự hiện đại", status "Đang mượn"
- **Note:** This test case depends on BUG-02 being present — BR003 must be visible via search to proceed

**Bước tái hiện:**
1. Log in as `ba.nguyen@email.com` (MEM002)
2. Go to tab "Borrow/Return" tab
3. Click "Search borrow records" tab
4. Type `MEM006` in the search field and click "Search"
5. Locate BR003 ("Quản trị nhân sự hiện đại", owned by MEM006) in results
6. Click "Return" button on BR003
7. Observe whether the return action is accepted or rejected

**Kết quả mong đợi:**
- "Return" button is either not displayed or disabled for records not owned by MEM002
- If button is somehow accessible and clicked, system rejects the action with an authorization error
- BR003 status remains "Borrowing"
- BOOK013 status remains "Borrowed"

**Kết quả thực tế:**
- After searching MEM006, BR003 was visible with an active **Trả sách** button
- Clicking "Return" was accepted by the system 
- BR003 status changed to "Returned"
- BOOK013 status changed to "Available"
- No authorization error or warning was shown

**Tác động:**
- **Critical data integrity violation**: any member can modify borrow records belonging to another member
- A malicious member could return all books currently borrowed by other members, disrupting their borrow history
- Violates both REQ-05 (return condition) and REQ-08 (member data isolation)
**Minh chứng:**
After Return
![BUG-01 Screenshot](bug03-tc804.png) 
BOOK013 status
![BUG-01 Screenshot](bug03-tc804(2).png)

**Đề xuất xử lý:**
- The "Return" button must validate `record.memberId == currentUserId` before allowing the action, independent of how the record was retrieved
- This validation must exist as a separate check from the search filter (BUG-02 fix) — even if search is fixed, the return action itself must also enforce ownership
- Suggested check: before processing return, verify on the data layer that the borrow record belongs to the currently logged-in member

---

<!-- Copy template BUG trên để thêm BUG-04, BUG-05, ... cho mỗi TC Fail -->
