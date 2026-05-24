# Bug Reports — Báo cáo lỗi

> **Hướng dẫn**: Tạo 1 mục bug cho mỗi TC có kết quả **Fail**.
> Xem [examples/sample-bug-report.md](../examples/sample-bug-report.md) để hiểu cách viết bug report tốt.
> Mỗi bug cần: tiêu đề mô tả hành vi lỗi, bước tái hiện, expected vs actual, severity + giải thích.

| Thông tin        |                       |
| ---------------- | --------------------- |
| **Nhóm**         | `<!-- Tên nhóm -->`   |
| **Ngày báo cáo** | `<!-- DD/MM/YYYY -->` |

---

## BUG-01

| Thuộc tính          | Chi tiết      |
| ------------------- | ------------- |
| **Mã lỗi**          | BUG-01        |
| **TC liên quan**    | `TC-03`       |
| **REQ liên quan**   | `REQ-06 `     |
| **Mức độ**          | `Medium `     |
| **Người phát hiện** | `Ha Dang Huy` |
| **Ngày phát hiện**  | `23/5/2026`   |
| **Trạng thái**      | ` Open`       |

**Tiêu đề:**
`Due date = today will not be marked as overdue`

**Môi trường:**

- Trình duyệt: Chrome ` 136.0.7103.93`
- Hệ điều hành: `Windows 11`
- Ngôn ngữ giao diện: Tiếng Việt / English

**Điều kiện tiên quyết:**
`The BR006 record exists, with status "Borrowing"`

**Bước tái hiện:**

1. `1. Log in using your Librarian account. `
2. `2. Click the "Check Expired" button.`

**Kết quả mong đợi:**
`The BR006 record will be marked as	Overdue`

**Kết quả thực tế:**
`The BR006 record will be marked as Overdue or Returned depending on the time.`

**Tác động:**
`Displays the incorrect status for the borrowing record, violating core business rules (failing to properly update the status to "Overdue" on the due date itself). This can prevent librarians from handling overdue books in a timely manner and may result in incorrect fine calculations if applicable`

**Minh chứng:**
![Bug 01 Evidence](bug01.png)
![Bug 01 Evidence](bug01.1.png.png)

**Đề xuất xử lý:**
`<!-- Gợi ý cách sửa lỗi nếu có -->`

---

## BUG-02

| Thuộc tính          | Chi tiết      |
| ------------------- | ------------- |
| **Mã lỗi**          | BUG-02        |
| **TC liên quan**    | `TC-8`        |
| **REQ liên quan**   | `REQ-07`      |
| **Mức độ**          | `High `       |
| **Người phát hiện** | `Ha Dang Huy` |
| **Ngày phát hiện**  | `23/5/2026`   |
| **Trạng thái**      | `Open`        |

**Tiêu đề:**
`Email validation is too loose — it accepts 'test@email`

**Bước tái hiện:**

1. `Log in using Librarian account.  `
2. `Click the "Add member" button.`
3. `Enter the information with email: 'test@email` .`
4. `Click the "Add member" button.`

**Kết quả mong đợi:**
`An error message should appear indicating "Invalid email", and the registration should be blocked`

**Kết quả thực tế:**
`The system accepts the input and displays a "Successfully" notification`

**Tác động:**
`Saves malformed emails into the database, breaking data integrity. This prevents the system from sending crucial notifications (e.g., registration or overdue reminders) and may cause downstream email delivery errors`

**Minh chứng:**
![Bug 01 Evidence](bug02.png)

**Đề xuất xử lý:**
`<!-- -->`

---

<!-- Copy template BUG trên để thêm BUG-03, BUG-04, ... cho mỗi TC Fail -->

## BUG-03

| Thuộc tính          | Chi tiết      |
| ------------------- | ------------- |
| **Mã lỗi**          | BUG-03        |
| **TC liên quan**    | `TC-10`       |
| **REQ liên quan**   | `REQ-07`      |
| **Mức độ**          | `Medium `     |
| **Người phát hiện** | `Ha Dang Huy` |
| **Ngày phát hiện**  | `23/5/2026`   |
| **Trạng thái**      | `Open`        |

**Tiêu đề:**
`Valid email syntax is not accepted`

**Bước tái hiện:**

1. `Log in using Librarian account.  `
2. `Click the "Add member" button.`
3. `Enter the information with email: 'test@email.com` .`
4. `Click the "Add member" button.`

**Kết quả mong đợi:**
`The system accepts the input, successfully registers the member, and displays a success notification. `

**Kết quả thực tế:**
`The system rejects the input and incorrectly displays an "Invalid email" error message.`

**Tác động:**
`Prevents valid users from registering or updating their profiles with legitimate email addresses. This completely blocks the account creation workflow for valid members and disrupts library operations.`

**Minh chứng:**
![Bug 01 Evidence](bug03.png)

**Đề xuất xử lý:**
`<!-- -->`

---
