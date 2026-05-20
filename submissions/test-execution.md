# Test Execution

> **Instructions**: Run the 30+ TCs written on the system <https://stqa.rbc.vn> and record the results here.

| Information | |
|---|---|
| **Group** | Group 1 (Merged STQA_GROUP_05) |
| **Execution Date** | 20/05/2026 |
| **Browser** | Chrome v120.0 |
| **OS** | Windows / macOS |

---

## Execution Details

| TC ID | Status (Pass/Fail/Block/Not Run) | Actual Result | Notes / Bug Link |
|-------|----------------------------------|---------------|------------------|
| TC-01 | Pass | Login successful as Librarian | |
| TC-02 | Pass | Login successful as Member | |
| TC-03 | Pass | Displays error "Member not found." | |
| TC-04 | Pass | Displays error "Incorrect password." | |
| TC-05 | Pass | Displays error "Please enter email and password" below the input field | |
| TC-06 | Pass | Book list displays complete information | |
| TC-07 | Pass | Correctly displays BOOK001 | |
| TC-08 | Pass | Correctly displays BOOK002 | |
| TC-09 | Pass | Searching with keyword "FLUTTER" still returns BOOK001 (case-insensitive) | |
| TC-10 | Pass | Displays "No books found" | |
| TC-11 | Pass | Only displays books in the Management genre | |
| TC-12 | Pass | Borrow successful, green notification displayed at the bottom of the screen | |
| TC-13 | Pass | Borrowed book has no Borrow button (or is disabled) | |
| TC-14 | Pass | Lost book has no Borrow button | |
| TC-15 | **Fail** | **System correctly rejects the borrow action, but displays the wrong message: "Member has expired. Cannot borrow book." instead of a message about the Suspended status.** | [BUG-002] |
| TC-16 | Pass | Borrow rejected with error message "Member has expired. Cannot borrow book." | |
| TC-17 | **Fail** | **System still allows borrowing the 4th book even after reaching the 3-book limit. Displays "Borrow successful!"** | [BUG-001] |
| TC-18 | Pass | Book returned successfully, status reverts to Available | |
| TC-19 | **Fail** | **Returned overdue slip BR001 successfully but system only displays "Book returned successfully.", no overdue warning as required by REQ-05.** | [BUG-006] |
| TC-20 | Pass | System scans and automatically updates active overdue borrowed slips, including BR001 and BR003, to Overdue status | |
| TC-21 | **Fail** | **Entered valid email `testnewuser99@gmail.com` but system shows "Invalid email." — Cannot add any new member with any email.** | [BUG-003] |
| TC-22 | **Fail** | **System accepts email `new@gmail` (missing `.` in domain) and creates member successfully with code MEM007. No error warning displayed.** | [BUG-003] |
| TC-23 | **Fail** | **Entered already-existing email `librarian@library.com` but system shows "Invalid email." instead of a duplicate email error.** | [BUG-003] |
| TC-24 | Pass | Member can only see their own borrowing slips | |
| TC-25 | Pass | Librarian can see all borrowing slips in the system | |
| TC-26 | Pass | After MEM003 borrows BOOK002, book status immediately changes to "Borrowed" in the list without needing to reload the page. | |
| TC-27 | **Fail** | **Member MEM002 looked up ID MEM006 and the system displayed slip BR003 belonging to MEM006, including book information, borrow date, due date, and the Return Book button.** | [BUG-004] |
| TC-28 | **Fail** | **Member MEM002 successfully returned slip BR003 belonging to MEM006. System shows "Book returned successfully", slip changed to "Returned" with return date 19/05/2026.** | [BUG-005] |
| TC-29 | Not Run | Newly added from Branch B - Requires execution | |
| TC-30 | Not Run | Newly added from Branch B - Requires execution | |
| TC-31 | Not Run | Newly added from Branch B - Requires execution | |

---

## Results Summary (Thống kê tổng hợp)

| Metric | Value |
|--------|---------|
| Total Test Cases | 31 |
| Pass | 20 |
| Fail | 8 |
| Blocked | 0 |
| Not Run | 3 |
| **Pass Rate (Executed)** | **71.4%** (20/28) |

### Results by Feature Group

| Group | Total TCs | Pass | Fail | Not Run | Pass Rate |
|------|---------|------|------|---------|------------|
| Login (REQ-01) | 7 | 5 | 0 | 2 | 100% |
| View, Search & Filter (REQ-02, 03) | 8 | 7 | 0 | 1 | 100% |
| Borrow, Return, Overdue (REQ-04, 05, 06) | 9 | 6 | 3 | 0 | 66.7% |
| Member Mgmt & Lookup (REQ-07, 08) | 7 | 2 | 5 | 0 | 28.6% |
