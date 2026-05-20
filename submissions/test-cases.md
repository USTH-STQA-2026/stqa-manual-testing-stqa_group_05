# Test Cases

> **Instructions**: Write at least **20 TCs** covering all main features (REQ-01 → REQ-08).
> See [examples/sample-test-case.md](../examples/sample-test-case.md) to understand how to write a good TC.
> Organize and group your test cases in the most logical way.

| Information | |
|---|---|
| **Group** | Group 1 |
| **Date Created** | 16/05/2026 |
| **System** | https://stqa.rbc.vn |
| **Reference** | SRS v1.0 |

---

## Step 1: Input Domain Modeling (IDM)

### IDM — Login (REQ-01)
| Characteristic | Block | Representative Value | Expected Result |
|---|---|---|---|
| Does the email exist in the DB? | Yes | `librarian@library.com` | Login successful |
| | No | `noone@email.com` | Error message |
| Is the password correct? | Correct | `admin123` | Login successful |
| | Incorrect | `wrongpass` | Error message |
| Is the input field empty? | Not empty | (any value) | Processed normally |
| | Empty | `""` | Message "Please enter..." |

### IDM — View Book List (REQ-02)
| Characteristic | Block | Representative Value | Expected Result |
|---|---|---|---|
| Role of the viewer? | Librarian | `librarian@library.com` | Sees the full book list |
| | Member | `ba.nguyen@email.com` | Sees the full book list |
| Book status after borrowing? | Book just borrowed | `BOOK001` (initially: Available) | Status updates to "Borrowed" immediately (real-time) |
| Book status after returning? | Book just returned | Recently returned borrowing slip | Book status updates back to "Available" immediately (real-time) |
| Is the information complete? | Complete | (any book) | Displays: title, author, genre, publication year, status |
| | Missing field | N/A | No field may be hidden |

### IDM — Search for Books (REQ-03)
| Characteristic | Block | Representative Value | Expected Result |
|---|---|---|---|
| Does the keyword exist in the DB? | Yes (book title) | `"Flutter"` | Displays books containing "Flutter" |
| | Yes (author name) | `"Nguyen"` | Displays books by author Nguyen |
| | No | `"XYZ123"` | Empty list |
| Case-sensitive? | Lowercase | `"flutter"` | Same result as "Flutter" |
| | UPPERCASE | `"FLUTTER"` | Same result as "Flutter" |
| Does the filter genre exist in the DB? | Yes | `"Management"` | Displays books in the Management genre: BOOK004, BOOK012, BOOK013 |
| | No | `"Does not exist"` | Empty list, displays "No books found" message |

### IDM & Decision Table — Borrow Book (REQ-04)

**Decision Table for Borrow Book Feature:**

| Conditions / Actions | R1 | R2 | R3 | R4 |
|---------------------|----|----|----|-----|
| **Conditions** | | | | |
| C1: Book status is "Available"? | False | True | True | True |
| C2: Account status is "Active"? | - | False | True | True |
| C3: Number of books borrowed < 3? | - | - | False | True |
| **Actions** | | | | |
| A1: Error — book not available | X | | | |
| A2: Error — invalid account | | X | | |
| A3: Error — borrow limit exceeded | | | X | |
| A4: Allow borrow successfully | | | | X |

**Supplementary IDM Table (for other attributes):**
| Characteristic | Block | Representative Value | Expected Result |
|---|---|---|---|
| Book status? | Available | BOOK001 | Allow borrow |
| | Borrowed | BOOK003 | Not allowed |
| | Lost | BOOK007 | Not allowed |
| Member status? | Active | MEM002 | Allow borrow |
| | Suspended | MEM004 | Reject, show error |
| | Expired | MEM005 | Reject, show error |
| Number of books borrowed? | < 3 (BVA: 0, 1, 2) | MEM003 (0 books) / MEM002 (1 book) | Allow borrow |
| | = 3 (BVA: limit) | MEM002 after borrowing 2 more books | Reject, show limit exceeded message |

### IDM — Return, Overdue Handling, Member Management, Lookup (REQ-05 to REQ-08)
| Characteristic | Block | Representative Value | Expected Result |
|---|---|---|---|
| Is the returned book overdue? (REQ-05) | Not yet / On time | Return before due date | Return successful, no warning |
| | Overdue | Return after due date | Return successful, with overdue warning |
| Is the slip in "Borrowed" status? (REQ-05) | Yes | Return BR001 | Allow return |
| | No | Return already-returned BR004 | Reject or do not show Return button |
| Click "Check Overdue" (REQ-06) | Current date >= dueDate | Scan slips BR001, BR003 | Update active borrowed slips that are overdue to "Overdue" status |
| New email format (REQ-07) | Valid | `new@gmail.com` | Added successfully |
| | Missing `@` | `newgmail.com` | Show format error |
| | Missing `.` in domain | `new@gmail` | Show format error |
| Duplicate email (REQ-07) | Yes | `librarian@library.com` | Show already-exists error |
| | No | `newuser@gmail.com` | Allow if other fields are valid |
| Permission to add member (REQ-07) | Librarian | LIB001 adds new member | Allow adding member |
| | Member | MEM002 accesses add member feature | Reject or do not show feature |
| Permission to view borrowing slips (REQ-08) | Member views own slip | MEM002 views BR001 | View successful |
| | Member views another member's slip | MEM002 looks up MEM006/BR003 | Reject or do not display data |
| | Librarian views all | LIB001 views all slips | View successful |
| Does the slip belong to the logged-in member? (REQ-05/REQ-08) | Yes | MEM002 returns BR001 | Allow return |
| | No | MEM002 returns BR003 of MEM006 | Reject return |

---

## Step 2: Test Cases

| TC ID | Test Objective | Precondition | Steps | Input Data | Expected Result | REQ | Technique |
|-------|----------------|--------------|-------|------------|-----------------|-----|-----------|
| TC-01 | Successful login as Librarian | None | 1. Navigate to the website<br>2. Enter Email<br>3. Enter Password<br>4. Click "Login" | Email: `librarian@library.com`<br>Pass: `admin123` | Redirected to home page, displays name "Nguyen Thu Thu" and role Librarian. | REQ-01 | EP |
| TC-02 | Successful login as Member | None | 1. Navigate to the website<br>2. Enter Email<br>3. Enter Password<br>4. Click "Login" | Email: `ba.nguyen@email.com`<br>Pass: `password123` | Redirected to home page, displays name "Nguyen Hoc Ba" and role Member. | REQ-01 | EP |
| TC-03 | Failed login — email does not exist | None | 1. Navigate to the website<br>2. Enter Email<br>3. Enter Password<br>4. Click Login | Email: `nobody@test.com`<br>Pass: `admin123` | Displays message: "Member not found". | REQ-01 | EP |
| TC-04 | Failed login — wrong password | None | 1. Enter valid Email<br>2. Enter wrong Password<br>3. Click Login | Email: `ba.nguyen@email.com`<br>Pass: `wrongpass` | Displays message: "Incorrect password". | REQ-01 | EP |
| TC-05 | Failed login — empty fields | None | 1. Leave Email and Password empty<br>2. Click Login | Email: `""`<br>Pass: `""` | Displays message "Please enter email and password". | REQ-01 | BVA |
| TC-06 | Book list displays correct data | Logged in | 1. Switch to "Books" tab | N/A | Book list displays title, author, genre, publication year, status. | REQ-02 | EP |
| TC-07 | Search books by title (with results) | Logged in | 1. Enter keyword in the search box | Keyword: `Flutter` | Displays book `BOOK001`. | REQ-03 | EP |
| TC-08 | Search books by author name | Logged in | 1. Enter author name in the search box | Keyword: `Tran Van Hung` | Displays book `BOOK002`. | REQ-03 | EP |
| TC-09 | Search books is case-insensitive | Logged in | 1. Enter keyword in lowercase/UPPERCASE | Keyword: `flutter` | Displays book `BOOK001` (same as TC-07). | REQ-03 | EP |
| TC-10 | Search books — no results | Logged in | 1. Enter a non-existent keyword | Keyword: `XYZ123` | Displays message "No books found". | REQ-03 | EP |
| TC-11 | Filter books by genre | Logged in | 1. Enter genre "Management" in the filter box | Genre: `Management` | List displays BOOK004, BOOK012, BOOK013. | REQ-03 | EP |
| TC-12 | Borrow book successfully (Active, Available, Borrowed < 3) | Logged in as MEM006 | 1. Select an Available book<br>2. Click Borrow | Book: `BOOK001` | Borrow successful, book status changes to "Borrowed". | REQ-04 | DT |
| TC-13 | Cannot borrow a book that is already borrowed | Logged in as Member | 1. Select a Borrowed book<br>2. Click Borrow | Book: `BOOK003` | Borrow button is hidden or rejection message shown. | REQ-04 | EP |
| TC-14 | Cannot borrow a lost book | Logged in as Member | 1. Find a Lost book<br>2. Click Borrow | Book: `BOOK007` | Borrow button is hidden or rejection message shown. | REQ-04 | EP |
| TC-15 | Suspended member cannot borrow books | Logged in as MEM004 | 1. Select an Available book<br>2. Click Borrow | Book: `BOOK001` | Borrow rejected, displays a **distinct** message for suspended status (e.g., "Account is currently suspended. Cannot borrow book."). Per SRS REQ-04: suspended message ≠ expired message. | REQ-04 | DT |
| TC-16 | Expired member cannot borrow books | Logged in as MEM005 | 1. Select an Available book<br>2. Click Borrow | Book: `BOOK001` | Borrow rejected, displays message that the account has expired. | REQ-04 | DT |
| TC-17 | Cannot borrow more than 3 books | Reset data, logged in as MEM002 | 1. Confirm MEM002 currently has 1 active borrow (`BR001` - `BOOK003`)<br>2. Borrow 2 more available books to reach a total of 3<br>3. Continue to borrow a 4th book | Additional books: `BOOK001`, `BOOK002`<br>4th book: `BOOK004` | Reject borrowing the 4th book, display message that the maximum 3-book borrow limit has been exceeded. | REQ-04 | BVA |
| TC-18 | Return book on time successfully | Reset data, logged in as MEM006 | 1. Go to Books tab<br>2. Borrow an "Available" book<br>3. Go to Borrow/Return tab<br>4. Find the newly created slip<br>5. Click Return Book | Book: `BOOK001` | Book returned successfully, status reverts to "Available". No overdue warning displayed. | REQ-05 | EP |
| TC-19 | Returning an overdue book displays warning | Reset data, logged in as MEM002 | 1. Go to Borrow/Return tab<br>2. Find slip `BR001` — active but past due date<br>3. Click Return Book | Slip: `BR001` (`BOOK003`, due date `15/09/2024`) | Book returned successfully but with an overdue warning. | REQ-05 | BVA |
| TC-20 | Librarian updates Overdue status | Reset data, logged in as Librarian | 1. Go to Borrow/Return<br>2. Click "Check Overdue" | N/A | Active borrowed slips with due dates ≤ today are updated to "Overdue", at minimum including `BR001` and `BR003`. | REQ-06 | DT |
| TC-21 | Librarian adds valid member | Logged in as Librarian | 1. Members tab<br>2. Click Add New<br>3. Enter full name, email, phone number<br>4. Click Add Member | Name: `Nguyen Test`<br>Email: `testnewuser99@gmail.com`<br>Phone: `0901234567` | New member is added to the list and a new member code is displayed. | REQ-07 | EP |
| TC-22 | Librarian fails to add member due to invalid format | Logged in as Librarian | 1. Members tab<br>2. Click Add New<br>3. Enter valid name and phone number<br>4. Enter invalid Email<br>5. Click Add Member | Name: `Test Invalid Email`<br>Email: `new@gmail` (missing `.` in domain)<br>Phone: `0901234567` | System **must reject** and display email format error. Member **must not be created**. | REQ-07 | EP |
| TC-23 | Librarian fails to add member due to duplicate Email | Logged in as Librarian | 1. Members tab<br>2. Click Add New<br>3. Enter valid name and phone number<br>4. Enter an already-existing Email<br>5. Click Add Member | Name: `Nguyen Duplicate Email`<br>Email: `librarian@library.com`<br>Phone: `0901234567` | System rejects member creation and displays duplicate email error. | REQ-07 | EP |
| TC-24 | Member can only view their own borrowing slips | Logged in as MEM002 | 1. Go to Borrow/Return | N/A | Only slips BR001, BR004 are visible. BR002 is not visible. | REQ-08 | EP |
| TC-25 | Librarian can view all borrowing slips | Logged in as Librarian | 1. Go to Borrow/Return | N/A | Displays a list of all borrowing slips from all members. | REQ-08 | EP |
| TC-26 | Book status updates real-time after borrowing | Logged in as MEM003 | 1. Go to Books tab, note BOOK002 is "Available"<br>2. Click Borrow BOOK002<br>3. Observe the book list immediately after | Book: `BOOK002` | BOOK002 must immediately change status to "Borrowed" in the list without needing to reload the page (real-time update). | REQ-02 | EP |
| TC-27 | Member cannot look up another member's borrowing slip | Reset data, logged in as MEM002 | 1. Go to Borrow/Return tab<br>2. Select Look Up Borrowing Slip<br>3. Enter another member's ID<br>4. Click Look Up | Member ID: `MEM006` | System rejects or does not display MEM006/BR003's borrowing slips. Member MEM002 can only view their own slips. | REQ-08 | EP |
| TC-28 | Member cannot return another member's book/slip | Reset data, logged in as MEM002 | 1. Go to Borrow/Return tab<br>2. Look up ID `MEM006`<br>3. If slip `BR003` appears, click Return Book | Slip: `BR003` of `MEM006` | System rejects the return action; slip BR003 remains in "Borrowed" status with no return date. | REQ-05, REQ-08 | EP |

---

## Summary

| Feature Group | # TC | # TC Fail | REQ Coverage | IDM Techniques Applied |
|---------------|------|-----------|--------------|------------------------|
| Login | 5 | 0 | REQ-01 | EP, BVA |
| View List & Search, Filter | 7 | 0 | REQ-02, REQ-03 | EP |
| Borrow, Return, Overdue | 9 | 3 | REQ-04, REQ-05, REQ-06 | EP, BVA, Decision Table (DT) |
| Member Management, Lookup | 7 | 5 | REQ-07, REQ-08 | EP |
| **Total** | **28** | **8** | **8 REQ** | **EP, BVA, Decision Table** |
