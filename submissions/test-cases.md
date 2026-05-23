# Test Cases

> **Instructions**: Write at least **20 TCs** covering all main features (REQ-01 → REQ-08).
> See [examples/sample-test-case.md](../examples/sample-test-case.md) to understand how to write a good TC.
> Organize and group your test cases in the most logical way.

| Information | |
|---|---|
| **Group** | Group 1 (Merged STQA_GROUP_05) |
| **Date Created** | 21/05/2026 |
| **System** | https://stqa.rbc.vn |
| **Reference** | SRS v1.0 |

---

## Step 1: Input Domain Modeling (IDM)

### IDM — Login (REQ-01)
| Characteristic | Block | Representative Value | Expected Result |
|---|---|---|---|
| Does the email exist in the DB? | Yes | `librarian@library.com` | Login successful |
| | No | `noone@email.com` | Error message "Member not found" |
| Is the password correct? | Correct | `admin123` | Login successful |
| | Incorrect | `wrongpass` | Error message "Incorrect password" |
| Input Space / Fields | Both fields empty | Email `""`, Pass `""` | Message "Please enter email and password" |
| | Email empty, Pass filled | Email `""`, Pass `admin123` | Message "Please enter email" |
| | Email filled, Pass empty | Email `ba.nguyen@email.com`, Pass `""` | Message "Please enter password" |

### IDM — View Book List (REQ-02)
| Characteristic | Block | Representative Value | Expected Result |
|---|---|---|---|
| Access rights (Role) | Librarian | `librarian@library.com` | Sees the full book list |
| | Member | `ba.nguyen@email.com` | Sees the full book list |
| Book status after borrowing? | Book just borrowed | `BOOK001` (Available -> Borrowed) | Status updates to "Borrowed" immediately (real-time) |
| Book status after returning? | Book just returned | `BOOK003` (Borrowed -> Available) | Status updates back to "Available" immediately (real-time) |
| Book information display | Complete | (any book) | Displays: title, author, category, publish year, code, status |
| | Missing field | N/A | No field may be hidden |

### IDM — Search for Books (REQ-03)
| Characteristic | Block | Representative Value | Expected Result |
|---|---|---|---|
| Does the keyword exist in the DB? | Yes (book title) | `"Flutter"` | Displays books containing "Flutter" |
| | Yes (author name) | `"Nguyen"` | Displays books by author Nguyen |
| | No | `"XYZ123"` | Empty list, displays "No books found" |
| Case-sensitive? | Lowercase | `"flutter"` | Same result as "Flutter" |
| | UPPERCASE | `"FLUTTER"` | Same result as "Flutter" |
| Category/Genre filter | Exist | `"Management"` / `"Công nghệ"` / `"Kinh tế"` | Displays books in selected category |
| | Non-exist | `"Psychology"` / `"Tâm lý học"` | Empty list, displays "No books found" |

### IDM & Decision Table — Borrow Book (REQ-04)

**Decision Table for Borrow Book Feature:**

| Conditions / Actions | R1 | R2 | R3 | R4 | R5 | R6 |
|---------------------|----|----|----|----|----|----|
| **Conditions** | | | | | | |
| C1: Book status is "Available"? | True | False (Borrowed) | False (Lost) | True | True | True |
| C2: Account status is "Active"? | True | True | True | False (Suspended) | False (Expired) | True |
| C3: Number of books borrowed < 3?| True | - | - | - | - | False |
| **Actions** | | | | | | |
| A1: Allow borrow successfully | X | | | | | |
| A2: Reject — book already borrowed| | X | | | | |
| A3: Reject — book lost | | | X | | | |
| A4: Reject — suspended member | | | | X | | |
| A5: Reject — expired member | | | | | X | |
| A6: Reject — limit exceeded | | | | | | X |

**Supplementary IDM Table (for other attributes):**
| Characteristic | Block | Representative Value | Expected Result |
|---|---|---|---|
| Book status | Available | `BOOK001` | Allow borrow |
| | Borrowed | `BOOK003` | Reject, show book unavailable error |
| | Lost | `BOOK007` | Reject, show book lost error |
| Member status | Active | `MEM002` | Allow borrow |
| | Suspended | `MEM004` | Reject, show suspended account error |
| | Expired | `MEM005` | Reject, show expired account error |
| Number of books borrowed (BVA) | < 3 (BVA: 0, 1, 2) | `MEM003` (0 books) / `MEM002` (1 book) | Allow borrow |
| | = 3 (BVA: limit) | `MEM002` after borrowing 2 more books | Reject, show limit exceeded message |
| | > 3 (BVA: above limit)| Attempting a 4th borrow | Reject, show limit exceeded message |

### IDM — Return, Overdue Handling, Member Management, Lookup (REQ-05 to REQ-08)

| Characteristic | Block | Representative Value | Expected Result |
|---|---|---|---|
| **Return Book (REQ-05)** | | | |
| returnDate v/s dueDate | returnDate <= dueDate | Return on or before due date | Return successful, no warning |
| | returnDate > dueDate | Return after due date | Return successful, with overdue warning |
| Ownership check | Yes | MEM002 returns own slip (BR001) | Allow return |
| | No | MEM002 returns MEM006's slip (BR003) | Reject return action |
| Slip status | Borrowed | Return active slip `BR001` | Allow return |
| | Returned | Return already-returned slip `BR004` | Reject or hide Return button |
| **Overdue Handling (REQ-06)** | | | |
| Role checking overdue | Librarian | `librarian@library.com` | Access allowed, can trigger scan |
| | Member | `ba.nguyen@email.com` | Access denied / feature hidden |
| "Check Overdue" button | Clicked | Action triggered | Scan active slips, update status |
| currentDate vs dueDate | currentDate >= dueDate | `BR001` (due `15/09/2024`) | Update status to "Overdue" |
| | currentDate < dueDate | Active slip with future due date | Keep status as "Borrowed" |
| Borrow Record Status | Borrowed | Active borrow record | Eligible for overdue scanning |
| | Returned | Already returned record | Excluded from overdue scanning |
| **Member Management (REQ-07)** | | | |
| Role adding member | Librarian | `librarian@library.com` | Access allowed |
| | Member | `ba.nguyen@email.com` | Access denied / feature hidden |
| Full Name input | Non-empty | `"Nguyen Test"` | Valid, proceed to next field |
| | Empty | `""` | Show error "Full name must not be blank" |
| Email format validation | Valid | `new@gmail.com` | Valid format |
| | Missing `@` | `newgmail.com` | Show "Invalid Email" error |
| | Missing `.` in domain | `new@gmail` | Show "Invalid Email" error |
| | Missing domain part | `new@` | Show "Invalid Email" error |
| | Missing local part | `@gmail.com` | Show "Invalid Email" error |
| | Contains space | `new member@gmail.com` | Show "Invalid Email" error |
| | Empty | `""` | Show error "Email must not be blank" |
| Email uniqueness | Duplicate | `librarian@library.com` | Show "Email already exists" error |
| | Unique | `newuser@gmail.com` | Valid uniqueness |
| Phone Number input | Non-empty digits | `0901234567` | Valid format |
| | Empty | `""` | Show error "Phone number must not be blank" |
| | Contains letters/chars | `09abcde345` | Show error "Invalid Phone number" |
| **Borrow Record Lookup (REQ-08)** | | | |
| Lookup permissions | Member looks up own | MEM002 looks up MEM002 | View successful, shows BR001, BR004 |
| | Member looks up other | MEM002 looks up MEM006 | Access denied / Hide records |
| | Librarian looks up all | LIB001 looks up any ID | View successful, shows all records |
| Record existence | Found | Existing Member ID / Slip ID | Displays detailed borrow records |
| | Not found | Non-existent ID (`MEM100`) | Show "No borrow records found" |
| Information display | Full details | Viewing `BR001` | Displays: Record ID, Book title, borrow date, due date, status |

---

## Step 2: Test Cases

| TC ID | Test Objective | Precondition | Steps | Input Data | Expected Result | REQ | Technique |
|-------|----------------|--------------|-------|------------|-----------------|-----|-----------|
| **REQ-01: Login** | | | | | | | |
| TC-01 | Successful login as Librarian | None | 1. Navigate to the website<br>2. Enter Email<br>3. Enter Password<br>4. Click "Login" | Email: `librarian@library.com`<br>Pass: `admin123` | Redirected to home page, displays name "Nguyen Thu Thu" and role Librarian. | REQ-01 | EP |
| TC-02 | Successful login as Member | None | 1. Navigate to the website<br>2. Enter Email<br>3. Enter Password<br>4. Click "Login" | Email: `ba.nguyen@email.com`<br>Pass: `password123` | Redirected to home page, displays name "Nguyen Hoc Ba" and role Member. | REQ-01 | EP |
| TC-03 | Failed login — email does not exist | None | 1. Navigate to the website<br>2. Enter Email<br>3. Enter Password<br>4. Click Login | Email: `nobody@test.com`<br>Pass: `admin123` | Displays message: "Member not found". | REQ-01 | EP |
| TC-04 | Failed login — wrong password | None | 1. Enter valid Email<br>2. Enter wrong Password<br>3. Click Login | Email: `ba.nguyen@email.com`<br>Pass: `wrongpass` | Displays message: "Incorrect password". | REQ-01 | EP |
| TC-05 | Failed login — empty fields | None | 1. Leave Email and Password empty<br>2. Click Login | Email: `""`<br>Pass: `""` | Displays message "Please enter email and password". | REQ-01 | BVA |
| TC-06 | Failed login — email empty, password filled | None | 1. Leave Email blank<br>2. Enter Password<br>3. Click Login | Email: `""`<br>Pass: `admin123` | Displays message: "Please enter email". | REQ-01 | EP |
| TC-07 | Failed login — email filled, password empty | None | 1. Enter Email<br>2. Leave Password blank<br>3. Click Login | Email: `librarian@library.com`<br>Pass: `""` | Displays message: "Please enter password". | REQ-01 | EP |
| **REQ-02: View Book List** | | | | | | | |
| TC-08 | Book list displays correct data | Logged in | 1. Switch to "Books" tab | N/A | Book list displays title, author, genre, publication year, status. | REQ-02 | EP |
| TC-09 | Book status updates real-time after borrowing | Logged in as MEM003 | 1. Go to Books tab, note BOOK002 is "Available"<br>2. Click Borrow BOOK002<br>3. Observe the book list immediately after | Book: `BOOK002` | BOOK002 must immediately change status to "Borrowed" in the list without needing to reload the page. | REQ-02 | EP |
| **REQ-03: Search & Filter Books** | | | | | | | |
| TC-10 | Search books by title (with results) | Logged in | 1. Enter keyword in the search box | Keyword: `Flutter` | Displays book `BOOK001`. | REQ-03 | EP |
| TC-11 | Search books by author name | Logged in | 1. Enter author name in the search box | Keyword: `Tran Van Hung` | Displays book `BOOK002`. | REQ-03 | EP |
| TC-12 | Search books is case-insensitive | Logged in | 1. Enter keyword in lowercase/UPPERCASE | Keyword: `flutter` | Displays book `BOOK001` (same as TC-10). | REQ-03 | EP |
| TC-13 | Search books — no results | Logged in | 1. Enter a non-existent keyword | Keyword: `XYZ123` | Displays message "No books found". | REQ-03 | EP |
| TC-14 | Filter books by genre | Logged in | 1. Enter genre in the filter box | Genre: `Management` / `Kinh tế` | List displays correct books belonging to the filtered category. | REQ-03 | EP |
| **REQ-04: Borrow Book** | | | | | | | |
| TC-15 | Borrow book successfully | Logged in as MEM006 | 1. Select an Available book<br>2. Click Borrow | Book: `BOOK001` | Borrow successful, book status changes to "Borrowed". | REQ-04 | DT (R1) |
| TC-16 | Cannot borrow a book already borrowed | Logged in as Member | 1. Select a Borrowed book<br>2. Click Borrow | Book: `BOOK003` | Borrow button is hidden or rejection message shown. | REQ-04 | EP / DT (R2) |
| TC-17 | Cannot borrow a lost book | Logged in as Member | 1. Find a Lost book<br>2. Click Borrow | Book: `BOOK007` | Borrow button is hidden or rejection message shown. | REQ-04 | EP / DT (R3) |
| TC-18 | Suspended member cannot borrow books | Logged in as MEM004 | 1. Select an Available book<br>2. Click Borrow | Book: `BOOK001` | Borrow rejected, displays distinct error message for suspended account. | REQ-04 | DT (R4) |
| TC-19 | Expired member cannot borrow books | Logged in as MEM005 | 1. Select an Available book<br>2. Click Borrow | Book: `BOOK001` | Borrow rejected, displays message that the account has expired. | REQ-04 | DT (R5) |
| TC-20 | Cannot borrow more than 3 books | Reset data, logged in as MEM002 | 1. Confirm member has 1 active borrow (`BR001`)<br>2. Borrow 2 more books to reach 3<br>3. Attempt to borrow a 4th book | Additional: `BOOK001`, `BOOK002`<br>4th book: `BOOK004` | Reject borrowing the 4th book, display borrow limit exceeded error message. | REQ-04 | BVA / DT (R6) |
| **REQ-05 & REQ-06: Return Book & Overdue Handling** | | | | | | | |
| TC-21 | Return book on time successfully | Reset data, logged in as MEM006 | 1. Borrow an Available book<br>2. Go to Borrow/Return tab<br>3. Click Return Book | Book: `BOOK001` | Book returned successfully, status reverts to "Available". No warning shown. | REQ-05 | EP |
| TC-22 | Returning an overdue book displays warning | Reset data, logged in as MEM002 | 1. Go to Borrow/Return tab<br>2. Find slip `BR001` (past due date)<br>3. Click Return Book | Slip: `BR001` (due `15/09/2024`) | Book returned successfully but with an overdue warning notification. | REQ-05 | BVA |
| TC-23 | Member cannot return another member's book | Reset data, logged in as MEM002 | 1. Go to Borrow/Return tab<br>2. Attempt to return another member's slip | Slip: `BR003` of `MEM006` | System rejects the return action; slip remains in "Borrowed" status. | REQ-05, REQ-08 | EP |
| TC-24 | Librarian updates Overdue status | Reset data, logged in as Librarian | 1. Go to Borrow/Return<br>2. Click "Check Overdue" | N/A | Active borrowed slips with due dates ≤ today (`BR001`, `BR003`) update to "Overdue". | REQ-06 | DT |
| **REQ-07: Member Management** | | | | | | | |
| TC-25 | Librarian adds valid member | Logged in as Librarian | 1. Members tab -> Add New<br>2. Enter Name, Email, Phone<br>3. Click Add Member | Name: `Nguyen Test`<br>Email: `testnewuser99@gmail.com`<br>Phone: `0901234567` | New member added successfully, member code is generated and displayed. | REQ-07 | EP |
| TC-26 | Librarian fails to add member due to invalid format | Logged in as Librarian | 1. Members tab -> Add New<br>2. Enter details with invalid email<br>3. Click Add Member | Name: `Test Invalid`<br>Email: `new@gmail` (missing `.`)<br>Phone: `0901234567` | System rejects creation, displays email format error message. | REQ-07 | EP |
| TC-27 | Librarian fails to add member due to duplicate Email | Logged in as Librarian | 1. Members tab -> Add New<br>2. Enter details with existing email<br>3. Click Add Member | Name: `Nguyen Duplicate`<br>Email: `librarian@library.com`<br>Phone: `0901234567` | System rejects creation, displays duplicate email error message. | REQ-07 | EP |
| TC-28 | Librarian fails to add member — empty Full Name | Logged in as Librarian | 1. Members tab -> Add New<br>2. Leave Name empty, fill other fields<br>3. Click Add | Name: `""`<br>Email: `valid@email.com`<br>Phone: `0901234567` | System rejects action, displays "Full name must not be blank". | REQ-07 | EP |
| TC-29 | Librarian fails to add member — empty Phone Number | Logged in as Librarian | 1. Members tab -> Add New<br>2. Leave Phone empty, fill other fields<br>3. Click Add | Name: `Nguyen Phone`<br>Email: `valid2@email.com`<br>Phone: `""` | System rejects action, displays "Phone number must not be blank". | REQ-07 | EP |
| TC-30 | Librarian fails to add member — invalid Phone format | Logged in as Librarian | 1. Members tab -> Add New<br>2. Enter Phone with characters<br>3. Click Add | Name: `Nguyen Character`<br>Email: `valid3@email.com`<br>Phone: `09abcde345` | System rejects action, displays "Invalid Phone number". | REQ-07 | EP |
| **REQ-08: Borrow Record Lookup** | | | | | | | |
| TC-31 | Member can only view their own borrowing slips | Logged in as MEM002 | 1. Go to Borrow/Return tab | N/A | Only slips `BR001`, `BR004` belonging to MEM002 are visible. | REQ-08 | EP |
| TC-32 | Librarian can view all borrowing slips | Logged in as Librarian | 1. Go to Borrow/Return tab | N/A | Displays a comprehensive list of all borrowing slips from all library members. | REQ-08 | EP |
| TC-33 | Member cannot look up another member's slip | Reset data, logged in as MEM002 | 1. Go to Borrow/Return tab<br>2. Search another Member's ID | Member ID: `MEM006` | System rejects lookup or does not display `MEM006`'s records. | REQ-08 | EP |

---

## Summary

| Feature Group | # TC | # TC Fail | REQ Coverage | IDM Techniques Applied |
|---------------|------|-----------|--------------|------------------------|
| Login | 7 | 0 | REQ-01 | EP, BVA |
| View Book List | 2 | 0 | REQ-02 | EP |
| Search & Filter Books | 5 | 0 | REQ-03 | EP |
| Borrow Book | 6 | 1 | REQ-04 | EP, BVA, Decision Table (DT) |
| Return & Overdue Handling | 4 | 2 | REQ-05, REQ-06 | EP, BVA, Decision Table (DT) |
| Member Management | 6 | 3 | REQ-07 | EP |
| Borrow Record Lookup | 3 | 2 | REQ-08 | EP |
| **Total** | **33** | **8** | **8 REQ** | **EP, BVA, Decision Table** |
