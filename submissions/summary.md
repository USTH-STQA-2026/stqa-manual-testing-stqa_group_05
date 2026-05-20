# Test Summary Report

> **Instructions**: Fill in the tables and answer the questions below to provide an overall quality assessment of the system.

---

## 1. Statistics

| Item | Count | Percentage (%) |
|------|-------|----------------|
| **Total Test Cases Written** | 28 | 100% |
| **TC Pass** | 20 | 71.4% |
| **TC Fail** | 8 | 28.6% |
| **Bugs Reported** | 6 | N/A |

## 2. Analysis by Feature

| Feature Group | # TC | # Bugs | Stability | Notes |
|---------------|------|--------|-----------|-------|
| Login | 5 | 0 | 🟢 Good | Handles all valid/invalid cases properly. |
| View Book List | 2 | 0 | 🟢 Good | Displays complete information, real-time updates work as designed. |
| Search & Filter | 5 | 0 | 🟢 Good | Works as designed, results match expected output. |
| Borrow, Return, Overdue | 9 | 3 | 🔴 Poor | BUG-001: Exceeding 3-book limit is not blocked. BUG-002: Wrong error message for Suspended accounts. BUG-006: No overdue warning when returning overdue books. |
| Member Management, Lookup | 7 | 3 | 🔴 Poor | BUG-003: Email validation logic is broken. BUG-004/BUG-005: Members can view and return other members' borrowing slips. |

## 3. Bug Summary

| Bug ID | Short Description | Related REQ | Severity | Status |
|--------|-------------------|-------------|----------|--------|
| BUG-001 | Member can borrow more than 3 books | REQ-04 | High | Open |
| BUG-002 | Wrong error message when a Suspended account tries to borrow | REQ-04 | Medium | Open |
| BUG-003 | Email validation logic is broken, including duplicate email being reported with wrong error | REQ-07 | Critical | Open |
| BUG-004 | Member can look up another member's borrowing slip | REQ-08 | High | Open |
| BUG-005 | Member can return another member's borrowing slip | REQ-05, REQ-08 | Critical | Open |
| BUG-006 | No overdue warning displayed when returning an overdue book | REQ-05 | Medium | Open |

## 4. Quality Assessment

**Q1. Overall system stability:**
The system is stable in basic read-only features (view books, search, login), covering 12/28 TCs (42.9%). However, 6 bugs were found in critical business flows, including Critical-severity bugs in the add member feature and return slip authorization logic.

**Q2. Which bug is the most critical? Why?**
**BUG-003** (Critical) — The librarian cannot add new members. This is the most severe bug because it **completely disables** a core librarian feature. Every valid email is rejected by the system as "invalid", making it impossible for the library to expand its member list for as long as the bug persists.

**BUG-001** (High) — Members can borrow more than 3 books at the same time. This is the second most severe bug because it directly violates the business rule, potentially causing a shortage of books for other members.

**BUG-005** (Critical) — Members can return another member's borrowing slip. This is a critical authorization bug because it allows a user to modify borrow/return data that does not belong to them.

**Q3. Recommendations and Bug Fix Priority:**
- **[CRITICAL PRIORITY]** Fix BUG-003: Review and fix the email regex/validation logic in the "Add Member" form. This bug blocks the entire feature and must be fixed immediately.
- **[CRITICAL PRIORITY]** Fix BUG-005: Prevent members from returning other members' slips at both the UI and business logic layers.
- **[HIGH PRIORITY]** Fix BUG-001: Add a `currentBorrowedBooksCount >= 3` check before allowing a borrow action. The borrow button should also be disabled when the limit has been reached.
- **[HIGH PRIORITY]** Fix BUG-004: Prevent members from looking up other members' borrowing slips; only the librarian should have access to all slips.
- **[MEDIUM PRIORITY]** Fix BUG-002: Separate error message logic for "Suspended" and "Expired" statuses so that the correct rejection reason is displayed for each case.
- **[MEDIUM PRIORITY]** Fix BUG-006: When returning an overdue book, the result notification must include a clear overdue warning as required by REQ-05.
- **[LOW PRIORITY]** UI/UX improvement: Consider disabling (graying out) the Borrow button when an account is locked, expired, or has already reached the 3-book limit, to improve user experience.

## 5. AI Usage Declaration

- The team used AI (ChatGPT) to suggest some basic test case scenarios and to assist with formatting Markdown tables.
- All test execution on the browser, result analysis, and bug report writing were performed manually by the team.
