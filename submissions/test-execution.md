# Test Execution — Test Execution Result

> **Instructions**: Execute each test case on the system https://stqa.rbc.vn and record the actual result.  
> Conclusion: **Pass** (expected behavior), **Fail** (incorrect behavior → create bug report), **Blocked** (cannot execute due to another blocking issue), **Not Run** (not executed yet).

| Information | |
|---|---|
| **Group** | STQA_GROUP_05 |
| **Execution Date** | 22/05/2026 |
| **Browser** | Chrome |
| **Operating System** | Windows / macOS |

---

## Detailed Result

| TC ID | Feature Group | Expected Result (Summary) | Actual Result | Status | Evidence | Bug |
|-------|---------------|---------------------------|-----------------|---------|-----------|----|
| TC-01 | REQ-01 Login | Login successfully as Librarian | Redirected to homepage and displays Librarian information | Pass | | |
| TC-02 | REQ-01 Login | Login successfully as Member | Redirected to homepage and displays Member information | Pass | | |
| TC-03 | REQ-01 Login | Display "Member not found" | Displays "Member not found" | Pass | | |
| TC-04 | REQ-01 Login | Display "Incorrect password" | Displays "Incorrect password" | Pass | | |
| TC-05 | REQ-01 Login | Display "Please enter email and password" | Displays "Please enter email and password" | Pass | | |
| TC-06 | REQ-01 Login | Display "Please enter email" | System displays "Please enter email and password" | Fail | [BUG-01](bug-evidence/bug01.png) | BUG-01 |
| TC-07 | REQ-01 Login | Display "Please enter password" | System displays "Please enter email and password" | Fail | [BUG-02](bug-evidence/bug02.png) | BUG-02 |
| TC-08 | REQ-02 View Book List | Display complete book information | Displays title, author, genre, publication year, and status correctly | Pass | | |
| TC-09 | REQ-02 View Book List | BOOK002 status updates immediately after borrowing | BOOK002 changes from Available to Borrowed immediately | Pass | | |
| TC-10 | REQ-03 Search & Filter | Display BOOK001 when searching | Search result displays BOOK001 | Pass | | |
| TC-11 | REQ-03 Search & Filter | Display BOOK002 when searching by author | Search result displays BOOK002 | Pass | | |
| TC-12 | REQ-03 Search & Filter | Search is case-insensitive | Keyword "flutter"/"FLUTTER" still returns BOOK001 | Pass | | |
| TC-13 | REQ-03 Search & Filter | Display "No books found" | Displays "No books found" | Pass | | |
| TC-14 | REQ-03 Search & Filter | Display correct books based on selected genre | Displays correct books belonging to the selected genre | Pass | | |
| TC-15 | REQ-04 Borrow Book | Borrow book successfully | Borrow successful and status changes to Borrowed | Pass | | |
| TC-16 | REQ-04 Borrow Book | Cannot borrow already borrowed book | Borrow button is hidden / disabled | Pass | | |
| TC-17 | REQ-04 Borrow Book | Cannot borrow lost book | Borrow button is hidden | Pass | | |
| TC-18 | REQ-04 Borrow Book | Suspended member receives suspended-account message | System rejects borrowing but displays expired-account message instead of suspended-account message | Fail | [BUG-03](bug-evidence/bug03.png) | BUG-03 |
| TC-19 | REQ-04 Borrow Book | Expired member cannot borrow book | Displays "Member has expired. Cannot borrow book." | Pass | | |
| TC-20 | REQ-04 Borrow Book | Cannot borrow more than 3 books | System still allows borrowing the 4th book and displays "Borrow successful!" | Fail | | BUG-04 |
| TC-21 | REQ-05 Return Book | Return book successfully on time | Return successful and status changes back to Available | Pass | | |
| TC-22 | REQ-05 Return Book | Returning overdue book displays warning | Return successful but no overdue warning is displayed | Fail | [BUG-05](bug-evidence/bug04.png) | BUG-05 |
| TC-23 | REQ-05 Return Book | Cannot return another member’s book | MEM002 successfully returned slip BR003 belonging to MEM006 | Fail | [BUG-06](bug-evidence/bug05.png) | BUG-06 |
| TC-24 | REQ-06 Overdue Handling | Librarian updates overdue records successfully | Overdue borrow records are updated correctly | Pass | | |
| TC-25 | REQ-07 Member Management | Add valid member successfully | System displays "Invalid email" even though email is valid | Fail | [BUG-07](bug-evidence/bug06.png) | BUG-07 |
| TC-26 | REQ-07 Member Management | Reject invalid email format | System still creates member using email `new@gmail` | Fail | [BUG-08](bug-evidence/bug07-1.png) | BUG-08 |
| TC-27 | REQ-07 Member Management | Reject duplicate email | System displays "Invalid email" instead of duplicate-email error | Fail | [BUG-09](bug-evidence/bug08.png) | BUG-09 |
| TC-28 | REQ-07 Member Management | Reject empty full name | Displays "Full name must not be blank" | Pass | | |
| TC-29 | REQ-07 Member Management | Reject empty phone number | System displays "Invalid email" | Fail | [BUG-10](bug-evidence/bug09.png) | BUG-10 |
| TC-30 | REQ-07 Member Management | Reject invalid phone number format | System displays "Invalid email" | Fail | [BUG-11](bug-evidence/bug10.png) | BUG-11 |
| TC-31 | REQ-08 Borrow Record Lookup | Member can only view their own borrow records | Member can still search another Member ID and view records | Fail | [BUG-12](bug-evidence/bug11-1.png) | BUG-12 |
| TC-32 | REQ-08 Borrow Record Lookup | Librarian can view all borrow records | Displays all borrow records correctly | Pass | | |
| TC-33 | REQ-08 Borrow Record Lookup | Cannot lookup another member’s records | MEM002 searches MEM006 and can still view BR003 details | Fail | [BUG-12](bug-evidence/bug11-2.png) | BUG-12 |

---

## Summary

| Metric | Value |
|--------|---------|
| Total Test Cases | 33 |
| Pass | 21 |
| Fail | 12 |
| Blocked | 0 |
| Not Run | 0 |
| **Pass Rate** | 63.64% |

### Result by Feature Group

| Feature Group | Total TC | Pass | Fail | Pass Rate |
|------|---------|------|------|------------|
| REQ-01 Login | 7 | 5 | 2 | 71.43% |
| REQ-02 View Book List | 2 | 2 | 0 | 100% |
| REQ-03 Search & Filter | 5 | 5 | 0 | 100% |
| REQ-04 Borrow Book | 6 | 4 | 2 | 66.67% |
| REQ-05 & REQ-06 Return & Overdue | 4 | 2 | 2 | 50% |
| REQ-07 Member Management | 6 | 1 | 5 | 16.67% |
| REQ-08 Borrow Record Lookup | 3 | 1 | 2 | 33.33% |
| **Total** | 33 | 21 | 12 | 63.64% |