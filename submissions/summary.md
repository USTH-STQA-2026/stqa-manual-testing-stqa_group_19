# Test Summary Report
 
---
 
## 1. Team Information
 
| Item | Details |
|------|---------|
| **Group** | Group 19 |
| **Class** | Class 2 |
| **Report Date** | 24/05/2026 |
| **System Under Test** | https://stqa.rbc.vn — v1.0 |
 
---
 
## 2. Overall Results
 
| Metric | Value |
|--------|-------|
| Total test cases | 49 |
| Pass | 39 |
| Fail | 10 |
| Blocked | 0 |
| Not Run | 0 |
| **Pass Rate** | **79.6% (39/49)** |
| **Bugs found** | **9** |
 
### Distribution by Feature Group
 
| Feature Group | TC | Pass | Fail | Bug | Assessment |
|---|---|---|---|---|---|
| Login (REQ-01) | 7 | 7 | 0 | 0 | ✅ Fully stable |
| View Book List (REQ-02) | 3 | 3 | 0 | 0 | ✅ Fully stable |
| Search & Filter (REQ-03) | 12 | 9 | 3 | 2 | ⚠️ Filter logic flawed |
| Borrow Book (REQ-04) | 7 | 5 | 2 | 2 | 🔴 Core business rules violated |
| Return Book (REQ-05) | 4 | 3 | 1 | 1 | ⚠️ Missing overdue warning |
| Overdue Handling (REQ-06) | 6 | 5 | 1 | 1 | ⚠️ Off-by-one on due date boundary |
| Member Management (REQ-07) | 4 | 2 | 2 | 2 | 🔴 Email validation broken in both directions |
| Borrow Record Lookup (REQ-08) | 6 | 5 | 1 | 1 | 🔴 Critical access control breach |
 
### Bug Distribution by Severity
 
| Severity | Count | Bug IDs |
|----------|-------|---------|
| Critical | 1 | BUG-09 |
| High | 4 | BUG-03, BUG-04, BUG-06, BUG-08 |
| Medium | 3 | BUG-02, BUG-05, BUG-07 |
| Low | 1 | BUG-01 |
 
---
 
## 3. Design Techniques Applied
 
| Technique | Applied to REQ | # TCs Using It | How It Was Applied |
|---|---|---|---|
| **EP** (Equivalence Partitioning) | REQ-01 → REQ-08 (all) | 43 | Primary technique across all features. Inputs were partitioned into valid/invalid classes; one representative value chosen per partition. For example: email exists / does not exist (REQ-01); book status: Available / Borrowed / Lost (REQ-04); member status: Active / Suspended / Expired (REQ-04). |
| **BVA** (Boundary Value Analysis) | REQ-03, REQ-04, REQ-05, REQ-06 | 6 | Applied at numeric and date boundaries. Key boundaries: borrow limit = 3 books (TC-24, TC-29 test values 2 and 3); overdue check on due date = today (TC-34, TC-35, TC-36 test values today−1, today, today+1); clearing search box to empty (TC-17). BVA directly exposed BUG-03 and BUG-05. |
 
---
 
## 4. Software Quality Analysis
 
### 4.1. Strengths
 
- **Authentication (REQ-01):** All 7 test cases passed. Login flows, role-based redirection, and all three distinct error messages ("Member not found", "Wrong password", "Please enter email and password") work exactly as specified in the SRS.
- **Book display (REQ-02):** Real-time status updates after borrow/return actions work correctly without requiring a page refresh, as required.
- **Keyword search (REQ-03):** Case-insensitive search is fully functional for both book title and author name. The "No books found" message also appears correctly.
- **Core borrow/return flow (REQ-04, REQ-05):** The happy path for borrowing and returning works. Due date is calculated correctly (+14 days). Returned books immediately change status to "Available".
- **Role-based record display (REQ-08):** The Librarian sees all 5 seed records; members only see their own records by default. Overdue status transitions (Borrowed → Overdue → Returned) work correctly.
### 4.2. Weaknesses
 
- **BUG-09 — Critical (REQ-08):** Any member can search for another member's borrow records by entering their member ID, and can even click "Return" on another member's book. This is a complete access control failure. Violates REQ-08 directly and constitutes a serious data privacy breach.
- **BUG-03 — High (REQ-04):** Off-by-one error in the borrow limit check allows a member to borrow 4 books instead of the maximum of 3. The validation likely uses `> 3` or `<= 3` instead of the correct `>= 3`. Core business rule violated.
- **BUG-04 — High (REQ-04):** When a Suspended member attempts to borrow, the system displays the error "Thành viên đã hết hạn" (Member has expired) — the message for an Expired account. SRS explicitly requires distinct messages for each rejection reason.
- **BUG-08 — High (REQ-05):** When a member returns an overdue book, no overdue warning is displayed. The return completes silently as if on time. REQ-05 explicitly requires an overdue warning in this case.
- **BUG-06 — High (REQ-07):** Email validation is too loose — the system accepts `test@email` (missing `.` in domain) as valid and creates the member. Malformed emails are stored in the system.
- **BUG-07 — Medium (REQ-07):** Email validation is also too strict in the opposite direction — the system rejects legitimate emails like `test@gmail.com` as invalid. The happy path for member creation is completely blocked.
- **BUG-05 — Medium (REQ-06):** A borrow record whose due date is exactly today is not marked as overdue when the librarian runs the check. SRS specifies the condition as `dueDate ≤ today` (inclusive), suggesting the code uses `<` instead of `<=`.
- **BUG-02 — Medium (REQ-03):** Combined search + filter applies AND logic inconsistently. When one condition matches nothing, the system ignores that condition instead of returning an empty list.
- **BUG-01 — Low (REQ-03):** The category filter box is case-sensitive, while the keyword search box is case-insensitive. The inconsistency confuses users and SRS implies uniform behavior.
---
 
## 5. Recommended Fix Priority
 
> Priority is determined by combining **severity** (technical impact) and **business priority** (impact on core library operations and data security).
 
| Order | Bug | Severity | Reason for Priority |
|-------|-----|----------|---------------------|
| 1 | BUG-09 | Critical | Privacy and access control breach — any member can read and act on another member's data. Must be fixed before any deployment. |
| 2 | BUG-03 | High | Core business rule violation — the 3-book borrow limit is not enforced. Affects inventory tracking for all members. |
| 3 | BUG-07 | Medium | Blocks member creation happy path entirely — valid emails are rejected, making the Add Member feature unusable for normal inputs. Pair with BUG-06 fix. |
| 4 | BUG-06 | High | Invalid emails are silently accepted and stored — causes downstream data integrity issues. Fix together with BUG-07 in the same validation rewrite. |
| 5 | BUG-08 | High | Overdue return warning is completely missing — library staff lose visibility into late returns. |
| 6 | BUG-04 | High | Wrong error message for Suspended member — administratively misleading and confusing to library staff. |
| 7 | BUG-05 | Medium | Off-by-one on overdue date boundary — books due today are not flagged. Causes late detection of overdue items. |
| 8 | BUG-02 | Medium | Combined search + filter inconsistency — incorrect results mislead users about available books. |
| 9 | BUG-01 | Low | Case-sensitive category filter — low impact but creates inconsistent UX. Straightforward fix (`toLowerCase()`). |
 
---
 
## 6. Conclusion
 
**The system is NOT ready for release.**
 
Of the 49 test cases executed, 10 failed (20.4%), revealing 9 bugs across 6 of the 8 tested requirements. Two features are entirely blocked in their core functionality: member creation (REQ-07) fails on a valid email, and the borrow limit (REQ-04) can be exceeded. Most critically, a **Critical-severity access control breach** (BUG-09) allows any authenticated member to view the borrow history of all other members and perform return actions on their behalf — a direct violation of REQ-08 that would constitute a serious data security incident in a production environment.
 
The system demonstrates solid implementation of the authentication module and the book display/search core. However, the 5 High+ severity bugs must be resolved and re-tested before release can be considered. A re-test focusing on REQ-04, REQ-07, and REQ-08 is strongly recommended after the next development cycle.
 
---
 
## 7. Lessons Learned
 
- **Boundary testing pays off immediately:** Both BUG-03 (borrow limit) and BUG-05 (overdue date) were found directly by BVA test cases at the exact boundary values. Without explicitly designing tests for `n = limit` and `today = dueDate`, these off-by-one bugs would likely have gone to production.
- **Access control must be tested from the user's perspective, not just the UI:** BUG-09 was only found because TC-46 actively tried to search another member's data. Role-based restrictions that look correct in the UI can still be bypassed at the data layer.
- **Distinct error messages need distinct test cases:** REQ-04 specifies that "Suspended" and "Expired" messages must be different. TC-25 and TC-27 tested each status independently, which is what revealed BUG-04. Testing only one negative path would have missed the mismap entirely.
- **SRS gaps should be documented, not assumed:** TC-21 and TC-22 correctly noted that combined search + filter behavior was unspecified in the SRS before concluding Pass/Fail. This practice prevents false failures and provides the development team with concrete feedback for the next SRS revision.
---
 
## 8. AI Usage Declaration
 
| AI Tool | Used for | How we reviewed/edited |
|---------|----------|------------------------|
| Claude (Anthropic) | IDM design guidance, summary drafting | All IDM tables were reviewed and corrected against SRS data by team members before use. Summary analysis and bug priority reasoning were written by the team lead based on actual execution results. |