# Bug Reports

> **Instruction**: Create 1 bug item for each TC with a **Fail** result.
> See [examples/sample-bug-report.md](../examples/sample-bug-report.md) to understand how to write a good bug report.
> Each bug needs: descriptive title for error behavior, steps to reproduce, expected vs actual, severity + explanation.

| Information | |
|---|---|
| **Group** | Group 19 |
| **Report Date** | 22/05/2026 |

---

## BUG-01

| Attribute | Details |
|-----------|---------|
| **Bug ID** | BUG-01 |
| **Related TC** | TC-22, TC-23 |
| **Related REQ** | REQ-03 |
| **Severity** | Low |
| **Reporter** | Nguyen Tung Duong |
| **Date Found** | 22/05/2026 |
| **Status** | Open |

**Title:**
Category filter is case-sensitive, causing "Book not found" error

**Environment:**
- Browser: Chrome 136.0.7103.93
- OS: Windows 11
- UI Language: Vietnamese

**Prerequisites:**
Successfully logged in. Currently on "Books" tab. Search box is empty.

**Steps to Reproduce:**
1. Enter the keyword `công nghệ` (all lowercase) or `CÔNG NGHỆ` (all uppercase) into the category filter input box.
2. Observe the resulting list of books displayed.

**Expected Result:**
The filter system should be case-insensitive, just like the search box, returning books under the "Công nghệ" (Technology) category.

**Actual Result:**
The book list is empty, displaying the message "Không tìm thấy sách" (Book not found). The filter box is currently case-sensitive.

**Impact:**
Causes confusion for users when typing lower/uppercase, resulting in an inconsistent experience with the search box (which is case-insensitive).

**Evidence:**
—

**Proposed Solution:**
Add a lowercase normalization function (e.g., `toLowerCase()`) for both the filter keyword and the book category data before comparing.

---

## BUG-02

| Attribute | Details |
|-----------|---------|
| **Bug ID** | BUG-02 |
| **Related TC** | TC-26 |
| **Related REQ** | REQ-03 |
| **Severity** | Medium |
| **Reporter** | Ha Dang Huy |
| **Date Found** | 22/05/2026 |
| **Status** | Open |

**Title:**
Combined search and filter behave inconsistently when the search box has no results

**Environment:**
- Browser: Chrome 136.0.7103.93
- OS: Windows 11
- UI Language: Vietnamese

**Prerequisites:**
Successfully logged in. Currently on "Books" tab.

**Steps to Reproduce:**
1. Enter `Kinh tế` into the category filter box (there are books in the Economy category).
2. Enter `Flutter` into the search box.
3. Observe the list of books.

**Expected Result:**
The system consistently applies the "AND" logic (as seen in TC-25). Since there are no books named "Flutter" in the "Kinh tế" category, the system should return an empty list and display "Không tìm thấy sách" (Book not found).

**Actual Result:**
The system seems to ignore the search condition and continues to display all books in the "Kinh tế" category (BOOK007, BOOK014, BOOK015).

**Impact:**
Combined search logic is flawed, returning incorrect results when one of the two conditions does not match, which misleads user expectations.

**Evidence:**
—

**Proposed Solution:**
Update the book list retrieval logic: Both filters must be applied simultaneously (AND). If either condition is not met, the final result list must be empty.

---
