# Test Execution

| Information | |
|---|---|
| **Group** | STQA_GROUP_05 |
| **Date created** | 22/05/2026 |
| **System** | https://stqa.rbc.vn |
| **Operation** | Windows/macOS |

---

## Detailed Result

| TC ID | Expected result (summary) | Actual result | Status | Reference | Bug |
|------- |---------------------------|-----------------|---------|-----------|----|
| **REQ-01: Login** | | | | | |
| TC-01 | Redirected to home page, displays name "Nguyen Thu Thu" and role Librarian | Redirected to home page, displays name "Nguyen Thu Thu" and role Librarian | Pass | | |
| TC-02 | Redirected to home page, displays name "Nguyen Hoc Ba" and role Member | Redirected to home page, displays name "Nguyen Hoc Ba" and role Member | Pass | | |
| TC-03 | Displays message: "Member not found" | Displays message: "Member not found" | Pass | | |
| TC-04 | Displays message: "Incorrect password" | Displays message: "Incorrect password" | Pass | | |
| TC-05 | Displays message "Please enter email and password" | Displays message "Please enter email and password" | Pass | | |
| TC-06 | Displays message: "Please enter email" | Displays message "Please enter email and password" | Fail | | BUG-01 |
| TC-07 | Displays message: "Please enter password" | Displays message: "Please enter email and password" | Fail | | BUG-02 |
| **REQ-02: View Book List** | | | | | |
| TC-08 | Book list displays title, author, genre, publication year, status | Book list displays title, author, genre, publication year, status | Pass | | |
| TC-09 | `BOOK002` immediately changes status to "Borrowed" in the list without needing to reload the page | `BOOK002` immediately changes status from `Available` to `Borrowed` in the list without needing to reload the page | Pass | | |
| **REQ-03: Search & Filter Books** | | | | | |
| TC-10 | Displays book `BOOK001` | Search result displays book `BOOK001` | Pass | | |
| TC-11 | Displays book `BOOK002` | Search result displays book `BOOK002` | Pass | | |
| TC-12 | Displays book `BOOK001` | Search result displays book `BOOK001` when using lowercase keyword | Pass | | |
| TC-13 | Displays message "No books found" | System displays message "No books found" | Pass | | |
| TC-14 | List displays correct books belonging to the filtered category | Filtered list displays `BOOK007`, `BOOK014`, `BOOK015` | Pass | | |
| **REQ-04: Borrow Book** | | | | | |
| TC-15 | Borrow successfully, book status changes to "Borrowed" | Book is borrowed successfully and its status changes to "Borrowed" | Pass | | |
| TC-16 | Borrow button is hidden or rejection message shown | Borrow button is hidden, user cannot borrow the book | Pass | | |
| TC-17 | Borrow button is hidden or rejection message shown | Borrow button is hidden, user cannot borrow the book | Pass | | |
| TC-18 | Borrow rejected, displays distinct error message for **suspended account** | Borrow is rejected, but the system displays an **expired account** error message instead of a **suspended account** error message | Fail | | BUG-03 |
| TC-19 | Borrow rejected, displays distinct error message for **expired** account | Borrow rejected, displays distinct error message for **expired** account | Pass | | |
| TC-20 | Reject borrowing the 4th book, display borrow limit exceeded error message | System rejects the 4th borrow attempt and displays the borrow limit exceeded error message | Pass | | |
| **REQ-05 & REQ-06: Return Book & Overdue Handling** | | | | | |
| TC-21 | Book returned successfully, status reverts to "Available". No warning shown | Book returned successfully, status reverts to "Available" without any warning | Pass | | |
| TC-22 | Book returned successfully but with an overdue warning notification | Book returned successfully, status reverts to "Available", displays return date but **no overdue warning displayed** | Fail | | BUG-04 |
| TC-23 | System rejects the return action; borrow record remains in "Borrowed" status | System displays "Book returned successfully", book's status returned to "Available" | Fail | | BUG-05 |
| TC-24 | Active borrowed records with due dates ≤ today (`BR001`, `BR003`) update to "Overdue" | System displays notification "Updated: 2 overdue records"; borrow records with status "Borrowing" are updated to "Overdue" | Pass | | |
| **REQ-07: Member Management** | | | | | |
| TC-25 | New member added successfully, member code is generated and displayed | Add member failed and system displayed "Invalid email" | Fail | | BUG-06 |
| TC-26 | System rejects creation, displays email format error message | System displays "Member added successfully! ID: MEM007" | Fail | | BUG-07 |
| TC-27 | System rejects creation, displays duplicate email error message | Add member failed and system displayed "Invalid email" | Fail | | BUG-08 |
| TC-28 | System rejects action, displays "Full name must not be blank" | Add member failed and system displays "Full name must not be blank" | Pass | | |
| TC-29 | System rejects action, displays "Phone number must not be blank" | Add member failed and system displayed "Invalid email" | Fail | | BUG-09 |
| TC-30 | System rejects action, displays "Invalid Phone number" | Add member failed and system displayed "Invalid email" | Fail | | BUG-10 |
| **REQ-08: Borrow Record Lookup** | | | | | |
| TC-31 | Only borrow records `BR001`, `BR004` belonging to MEM002 are visible | When clicking `My borrow records`, the system displays only `BR001` and `BR004` belonging to `MEM002`. However, the member can still use `Search borrow record` to view another member’s records by entering another Member ID | Fail | | BUG-11 |
| TC-32 | Displays a comprehensive list of all borrow records from all library members | System displays borrow records from all library members | Pass | | |
| TC-33 | System rejects lookup or does not display `MEM006`'s records | After logging in as `MEM002`, searching with Member ID `MEM006` still displays `MEM006`’s borrow records | Fail | | BUG-11 |



---

## Summary

| Metric | Value |
|--------|---------|
| Total Test Cases | 33 |
| Pass | 21 |
| Fail | 12 |
| Blocked | 0 |
| Not Run | 0 |
| **Pass rate** | 63.64% |

### Result by Feature Group

| Feature Group | Total TC | Pass | Fail | Pass rate |
|------|---------|------|------|------------|
| REQ-01 | 7 | 5 | 2 | 71.43% |
| REQ-02 | 2 | 2 | 0 | 100% |
| REQ-03 | 5 | 5 | 0 | 100% |
| REQ-04 | 6 | 5 | 1 | 83.33% |
| REQ-05 & REQ-06 | 4 | 2 | 2 | 50% |
| REQ-07 | 6 | 1 | 5 | 16.67% |
| REQ-08 | 3 | 1 | 2 | 33.33% |
|------|---------|------|------|------------|
| **Total** | 33 | 21 | 12 | 63.64% |
