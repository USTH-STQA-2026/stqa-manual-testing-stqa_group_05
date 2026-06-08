# Test Execution

| Information | |
|---|---|
| **Group** | STQA_GROUP_05 |
| **Date created** | 07/06/2026 |
| **System** | https://stqa.rbc.vn |
| **Operation** | Windows/macOS |

---

## Detailed Result

### REQ-01: Login
| TC ID | Expected result (summary) | Actual result | Status | Reference | Bug |
|------- |---------------------------|-----------------|---------|-----------|----|
| TC-01 | Redirected to home page, displays name "Nguyen Thu Thu" and role Librarian | Redirected to home page, displays name "Nguyen Thu Thu" and role Librarian | Pass | | |
| TC-02 | Redirected to home page, displays name "Nguyen Hoc Ba" and role Member | Redirected to home page, displays name "Nguyen Hoc Ba" and role Member | Pass | | |
| TC-03 | Displays message: "Member not found" | Displays message: "Member not found" | Pass | | |
| TC-04 | Displays message: "Incorrect password" | Displays message: "Incorrect password" | Pass | | |
| TC-05 | Displays message "Please enter email and password" | Displays message "Please enter email and password" | Pass | | |
| TC-06 | Displays message: "Please enter email" | Displays message "Please enter email and password" | Fail | [BUG-01](bug-evidence/bug01.png) | BUG-01 |
| TC-07 | Displays message: "Please enter password" | Displays message: "Please enter email and password" | Fail | [BUG-02](bug-evidence/bug02.png) | BUG-02 |

### REQ-02: View Book List
| TC ID | Expected result (summary) | Actual result | Status | Reference | Bug |
|------- |---------------------------|-----------------|---------|-----------|----|
| TC-08 | List displays 20 books, each includes title, author, genre, publication year, status | Book list displays title, author, genre, publication year, status | Pass | | |
| TC-09 | `BOOK002` immediately changes status to `Borrowed` in the list | `BOOK002` immediately changes status from `Available` to `Borrowed` in the list without needing to reload the page | Pass | | |
| TC-10 | `BOOK002` status changes `Available` immediately | `BOOK002` immediately changes status from `Borrowed` to `Available` in the list without needing to reload the page | Pass | | |
| TC-11 | Display a full info of the book: title, author, category, publish year, code, status | Display `BOOK001` info: <br>title `Lập trình Flutter cơ bản`<br>author `Nguyễn Minh Đức`<br>category `Công nghệ`<br>publish year `2023`<br>code `BOOK001`<br>status `Available` | Pass | | |

### REQ-03: Search & Filter Books
| TC ID | Expected result (summary) | Actual result | Status | Reference | Bug |
|------- |---------------------------|-----------------|---------|-----------|----|
| TC-12 | Displays book(s) containing `Flutter` in title | Search result displays book `BOOK001` | Pass | | |
| TC-13 | Displays book(s) belonging to `Nguyễn Minh Đức` | Search result displays book `BOOK001`, `BOOK009` | Pass | | |
| TC-14 | Empty list, displays "No books found" | System displays message "No books found" | Pass | | |
| TC-15 | Displays book(s) containing `flutter` in title | Search result displays book `BOOK001` | Pass | | |
| TC-16 | List displays books belonging `Kinh tế` category | Filtered list displays `BOOK007`, `BOOK014`, `BOOK015` | Pass | | |
| TC-17 | Empty list, displays "No books found" | System displays message "No books found" | Pass | | |

### REQ-04: Borrow Book
| TC ID | Expected result (summary) | Actual result | Status | Reference | Bug |
|-------|---------------------------|-----------------|---------|-----------|----|
| TC-18 | Borrow successfully, book status changes to "Borrowed" | Book is borrowed successfully and its status changes to "Borrowed" | Pass | | |
| TC-19 | Borrow button is hidden or rejection message shown | Borrow button is hidden, user cannot borrow the book | Pass | | |
| TC-20 | Borrow button is hidden or rejection message shown | Borrow button is hidden, user cannot borrow the book | Pass | | |
| TC-21 | Borrow rejected, displays distinct error message for **suspended account** | Borrow rejected<br>Display warning `Expired member. Cannot borrow books!` | Fail | [BUG-04](bug-evidence/bug04.png) | BUG-04 |
| TC-22 | Borrow rejected, displays distinct error message for **expired** account | Borrow rejected<br>Display warning `Expired member. Cannot borrow books!` | Pass | | |
| TC-23 | Reject borrowing the 4th book, display borrow limit exceeded error message | System accept the 4th borrow, borrow successfully, member’s borrow records display 4 books | Fail | [BUG-03](bug-evidence/bug03.png) | BUG-03 |

### REQ-05: Return Book
| TC ID | Expected result (summary) | Actual result | Status | Reference | Bug |
|------- |---------------------------|-----------------|---------|-----------|----|
| TC-24 | Book returned successfully, status reverts to "Available". No warning shown | Book returned successfully, status reverts to "Available" without any warning | Pass | | |
| TC-25*| Book returned successfully but with an overdue warning notification | Book returned successfully, status reverts to "Available", displays return date but **no overdue warning displayed** | Fail | [BUG-05](bug-evidence/bug05.png) | BUG-05 |
| TC-26*| Book returned successfully but with an overdue warning notification | Book returned successfully, status reverts to "Available", displays return date but **no overdue warning displayed** | Fail | [BUG-05](bug-evidence/bug05.png) | BUG-05 |
| TC-27 | System rejects the return action; borrow record remains in "Borrowed" status | System displays "Book returned successfully", book's status returned to "Available" | Fail | [BUG-06](bug-evidence/bug06.png) | BUG-06 |

_* TC-25 and TC-26 check boundary conditions on `dueDate` and overdue. According to [REQ-06](../docs/SRS-library-system.md#req-06-x%E1%BB%AD-l%C3%BD-s%C3%A1ch-qu%C3%A1-h%E1%BA%A1n--overdue-handling), these two TCs can produce the same result, which is one of the expected results._

### REQ-06: Overdue Handling
| TC ID | Expected result (summary) | Actual result | Status | Reference | Bug |
|------- |---------------------------|-----------------|---------|-----------|----|
| TC-28 | Active borrowed records with due dates ≤ today update to "Overdue" | System displays notification "Updated: 2 overdue records"; borrow records with status "Borrowing" are updated to "Overdue" (`BR001`, `BR003`) | Pass | | |
| TC-29 | Display `BR001` with status **Overdue** | Display `BR001` with status **Overdue** | Pass | | |

### REQ-07: Member Management
| TC ID | Expected result (summary) | Actual result | Status | Reference | Bug |
|------- |---------------------------|-----------------|---------|-----------|----|
| TC-30 | Display each member’s info correctly and their status | Each member’s info includes name, ID, email, phone number, nb of books borrowed and status<br>Member status displays correctly(eg: `MEM002` = `Active`, `MEM004` = `Suspended`, `MEM005` = `Expired`) | Pass | | |
| TC-31 | New member added successfully, member code is generated and displayed | Add member failed and system displayed "Invalid email" | Fail | [BUG-07](bug-evidence/bug07.png) | BUG-07 |
| TC-32 | System rejects creation, displays email format error message | System displays "Member added successfully! ID: MEM007" | Fail | [BUG-08](bug-evidence/bug08-1.png)<br>[BUG-08](bug-evidence/bug08-2.png)<br>[BUG-08](bug-evidence/bug08-3.png) | BUG-08 |
| TC-33 | System rejects creation, displays duplicate email error message | Add member failed and system displayed "Invalid email" | Fail | [BUG-09](bug-evidence/bug09.png) | BUG-09 |
| TC-34 | System rejects action, displays "Full name must not be blank" | Add member failed and system displays "Full name must not be blank" | Pass | | |
| TC-35 | System rejects action, displays "Phone number must not be blank" | Add member failed and system displayed "Invalid email" | Fail | [BUG-10](bug-evidence/bug10.png) | BUG-10 |
| TC-36 | System rejects action, displays "Invalid Phone number" | Add member failed and system displayed "Invalid email" | Fail | [BUG-11](bug-evidence/bug11.png) | BUG-11 |

### REQ-08: Borrow Record Lookup
| TC ID | Expected result (summary) | Actual result | Status | Reference | Bug |
|------- |---------------------------|-----------------|---------|-----------|----|
| TC-37 | Only borrow records `BR001`, `BR004` belonging to MEM002 are visible | When clicking `My borrow records`, the system displays only `BR001` and `BR004` belonging to `MEM002`. However, the member can still use `Search borrow record` to view another member’s records by entering another Member ID | Fail | [BUG-12](bug-evidence/bug12-1.png)<br>[BUG-12](bug-evidence/bug12-2.png) | BUG-12 |
| TC-38 | Displays list of all borrow records from all library members | System displays borrow records from all library members | Pass | | |
| TC-39 | System rejects lookup or does not display `MEM006`'s records | After logging in as `MEM002`, searching with Member ID `MEM006` still displays MEM006’s borrow records | Fail | [BUG-12](bug-evidence/bug12-1.png)<br>[BUG-12](bug-evidence/bug12-2.png) | BUG-12 |
| TC-40 | Display Record ID, book title, borrow date, due date, and status | `BR001` record displays: <br> - title: `Kiểm thử phần mềm nhập môn`,<br> - record ID: `BR001`, <br> - borrower: `Nguyễn Học Bá`,<br> - borrow date: `01/09/2024`, <br> - due date: `15/09/2024`, <br> - status: `Đang mượn` / `Borrowing` | Pass | | |


---

## Summary

| Metric | Value |
|--------|---------|
| Total Test Cases | 40 |
| Pass | 26 |
| Fail | 14 |
| Blocked | 0 |
| Not Run | 0 |
| **Pass rate** | 65% |

### Result by Feature Group

| Feature Group | Total TC | Pass | Fail | Pass rate |
|------|---------|------|------|------------|
| REQ-01 | 7 | 5 | 2 | 71.43% |
| REQ-02 | 4 | 4 | 0 | 100% |
| REQ-03 | 6 | 6 | 0 | 100% |
| REQ-04 | 6 | 4 | 2 | 66.67% |
| REQ-05 | 4 | 1 | 3 | 25% |
| REQ-06 | 2 | 2 | 0 | 100% |
| REQ-07 | 7 | 2 | 5 | 28.57% |
| REQ-08 | 4 | 2 | 2 | 50% |
|------|---------|------|------|------------|
| **Total** | 40 | 26 | 14 | 65% |
