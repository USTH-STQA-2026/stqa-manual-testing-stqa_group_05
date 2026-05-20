# Test Cases

> **Instructions**: Write at least **20 TCs** covering all main features (REQ-01 → REQ-08).
> See [examples/sample-test-case.md](../examples/sample-test-case.md) to understand how to write a good TC.
> Organize and group your test cases in the most logical way.

| Information | |
|---|---|
| **Group** | Group 1 (Merged STQA_GROUP_05) |
| **Date Created** | 16/05/2026 |
| **System** | <https://stqa.rbc.vn> |
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
| Category/Genre filter | Exist | `"Management"` / `"Công nghệ"`| Displays books in selected category |
| | Non-exist | `"Psychology"` | Empty list, displays "No books found" |

### IDM & Decision Table — Borrow Book (REQ-04)

**Decision Table for Borrow Book Feature:**

| Conditions / Actions | R1 | R2 | R3 | R4 | R5 |
|---------------------|----|----|----|-----|-----|
| **Conditions** | | | | | |
| C1: Book status is "Available"? | False | False | True | True | True |
| C2: Account status is "Active"? | - | - | False (Suspended) | False (Expired) | True |
| C3: Number of books borrowed < 3? | - | - | - | - | False |
| **Actions** | | | | | |
| A1: Reject — book already borrowed | X | | | | |
| A2: Reject — book lost | | X | | | |
| A3: Reject — suspended member | | | X | | |
| A4: Reject — expired member | | | | X | |
| A5: Reject — limit exceeded | | | | | X |
| A6: Allow borrow successfully | | | | | | (R6: All True) |

### IDM — Return, Overdue Handling, Member Management, Lookup (REQ-05 to REQ-08)

| Characteristic | Block | Representative Value | Expected Result |
|---|---|---|---|
| returnDate v/s dueDate (REQ-05) | returnDate <= dueDate | Return before/on due date | Return successful, no warning |
| | returnDate > dueDate | Return after due date | Return successful, with overdue warning |
| Book belongs to member? | Yes | MEM002 returns own slip | Allow return |
| | No | MEM002 returns MEM006's slip| Reject returning |
| Click "Check Overdue" (REQ-06) | currentDate >= dueDate | Scan slips BR001, BR003 | Update active borrowed slips to "Overdue" status |
| Email format validation (REQ-07)| Valid | `new@gmail.com` | Added successfully |
| | Invalid (miss `@`) | `newgmail.com` | Show "Invalid Email" |
| | Invalid (miss `.` domain)| `new@gmail` | Show "Invalid Email" |
| | Invalid (include space) | `new member@gmail.com` | Show "Invalid Email" |
| Duplicate email (REQ-07) | Yes | `librarian@library.com` | Show already-exists error |
| | No | `newuser@gmail.com` | Allow if other fields valid |
| Permission to add member | Librarian | LIB001 | Allow adding member |
| | Member | MEM002 | Reject or do not show feature |
| Permission to view slips (REQ-08) | Member views own | MEM002 views BR001 | View successful |
| | Member views other's | MEM002 views MEM006's slip | Reject / Cannot view other's |
| | Librarian views all | LIB001 views all slips | View successful |

---

## Step 2: Test Cases

| TC ID | Test Objective | Precondition | Steps | Input Data | Expected Result | REQ | Technique |
|-------|----------------|--------------|-------|------------|-----------------|-----|-----------|
| TC-01 | Successful login as Librarian | None | 1. Navigate to the website<br>2. Enter Email<br>3. Enter Password<br>4. Click "Login" | Email: `librarian@library.com`<br>Pass: `admin123` | Redirected to home page, displays name "Nguyen Thu Thu" and role Librarian. | REQ-01 | EP |
| TC-02 | Successful login as Member | None | 1. Navigate to the website<br>2. Enter Email<br>3. Enter Password<br>4. Click "Login" | Email: `ba.nguyen@email.com`<br>Pass: `password123` | Redirected to home page, displays name "Nguyen Hoc Ba" and role Member. | REQ-01 | EP |
| TC-03 | Failed login — email does not exist | None | 1. Navigate to the website<br>2. Enter Email<br>3. Enter Password<br>4. Click Login | Email: `nobody@test.com`<br>Pass: `admin123` | Displays message: "Member not found". | REQ-01 | EP |
| TC-04 | Failed login — wrong password | None | 1. Enter valid Email<br>2. Enter wrong Password<br>3. Click Login | Email: `ba.nguyen@email.com`<br>Pass: `wrongpass` | Displays message: "Incorrect password". | REQ-01 | EP |
| TC-05 | Failed login — both fields empty | None | 1. Leave Email and Password empty<br>2. Click Login | Email: `""`<br>Pass: `""` | Displays message "Please enter email and password". | REQ-01 | BVA |
| TC-06 | Book list displays correct data | Logged in | 1. Switch to "Books" tab | N/A | Book list displays title, author, genre, publication year, status. | REQ-02 | EP |
| TC-07 | Search books by title (with results) | Logged in | 1. Enter keyword in the search box | Keyword: `Flutter` | Displays book `BOOK001`. | REQ-03 | EP |
| TC-08 | Search books by author name | Logged in | 1. Enter author name in the search box | Keyword: `Tran Van Hung` | Displays book `BOOK002`. | REQ-03 | EP |
| TC-09 | Search books is case-insensitive | Logged in | 1. Enter keyword in lowercase/UPPERCASE | Keyword: `flutter` | Displays book `BOOK001` (same as TC-07). | REQ-03 | EP |
| TC-10 | Search books — no results | Logged in | 1. Enter a non-existent keyword | Keyword: `XYZ123` | Displays message "No books found". | REQ-03 | EP |
| TC-11 | Filter books by genre | Logged in | 1. Enter genre "Management" in the filter box | Genre: `Management` | List displays BOOK004, BOOK012, BOOK013. | REQ-03 | EP |
| TC-12 | Borrow book successfully (Active, Available, Borrowed < 3) | Logged in as MEM006 | 1. Select an Available book<br>2. Click Borrow | Book: `BOOK001` | Borrow successful, book status changes to "Borrowed". | REQ-04 | DT |
| TC-13 | Cannot borrow a book that is already borrowed | Logged in as Member | 1. Select a Borrowed book<br>2. Click Borrow | Book: `BOOK003` | Borrow button is hidden or rejection message shown. | REQ-04 | EP |
| TC-14 | Cannot borrow a lost book | Logged in as Member | 1. Find a Lost book<br>2. Click Borrow | Book: `BOOK007` | Borrow button is hidden or rejection message shown. | REQ-04 | EP |
| TC-15 | Suspended member cannot borrow books | Logged in as MEM004 | 1. Select an Available book<br>2. Click Borrow | Book: `BOOK001` | Borrow rejected, displays a **distinct** message for suspended status. | REQ-04 | DT |
| TC-16 | Expired member cannot borrow books | Logged in as MEM005 | 1. Select an Available book<br>2. Click Borrow | Book: `BOOK001` | Borrow rejected, displays message that the account has expired. | REQ-04 | DT |
| TC-17 | Cannot borrow more than 3 books | Reset data, logged in as MEM002 | 1. Confirm MEM002 has 1 active borrow<br>2. Borrow 2 more books<br>3. Borrow a 4th book | Additional books: `BOOK001`, `BOOK002`<br>4th book: `BOOK004` | Reject borrowing the 4th book, display maximum 3-book borrow limit exceeded. | REQ-04 | BVA |
| TC-18 | Return book on time successfully | Reset data, logged in as MEM006 | 1. Borrow an Available book<br>2. Go to Borrow/Return tab<br>3. Find slip<br>4. Click Return Book | Book: `BOOK001` | Book returned successfully, status reverts to "Available". No overdue warning. | REQ-05 | EP |
| TC-19 | Returning an overdue book displays warning | Reset data, logged in as MEM002 | 1. Go to Borrow/Return tab<br>2. Find slip `BR001` (past due date)<br>3. Click Return Book | Slip: `BR001` (`BOOK003`, due date `15/09/2024`) | Book returned successfully but with an overdue warning. | REQ-05 | BVA |
| TC-20 | Librarian updates Overdue status | Reset data, logged in as Librarian | 1. Go to Borrow/Return<br>2. Click "Check Overdue" | N/A | Active borrowed slips with due dates <= today updated to "Overdue" (e.g., BR001). | REQ-06 | DT |
| TC-21 | Librarian adds valid member | Logged in as Librarian | 1. Members tab -> Add New<br>2. Enter details<br>3. Click Add | Name: `Nguyen Test`<br>Email: `testnewuser99@gmail.com`<br>Phone: `0901234567` | New member added, member code displayed. | REQ-07 | EP |
| TC-22 | Librarian fails to add member due to invalid format | Logged in as Librarian | 1. Enter details with invalid Email<br>2. Click Add Member | Email: `new@gmail` (missing `.` in domain) | System **must reject** and display email format error. | REQ-07 | EP |
| TC-23 | Librarian fails to add member due to duplicate Email | Logged in as Librarian | 1. Enter details with existing Email<br>2. Click Add Member | Email: `librarian@library.com` | System rejects member creation and displays duplicate email error. | REQ-07 | EP |
| TC-24 | Member can only view their own borrowing slips | Logged in as MEM002 | 1. Go to Borrow/Return | N/A | Only slips BR001, BR004 are visible. BR002 is not visible. | REQ-08 | EP |
| TC-25 | Librarian can view all borrowing slips | Logged in as Librarian | 1. Go to Borrow/Return | N/A | Displays a list of all borrowing slips from all members. | REQ-08 | EP |
| TC-26 | Book status updates real-time after borrowing | Logged in as MEM003 | 1. Borrow `BOOK002`<br>2. Observe the book list | Book: `BOOK002` | Status immediately changes to "Borrowed" without needing to reload the page. | REQ-02 | EP |
| TC-27 | Member cannot look up another member's slip | Reset data, logged in as MEM002 | 1. Go to Borrow/Return tab<br>2. Enter another member's ID<br>3. Click Look Up | Member ID: `MEM006` | System rejects or does not display MEM006/BR003's borrowing slips. | REQ-08 | EP |
| TC-28 | Member cannot return another member's book | Reset data, logged in as MEM002 | 1. Look up ID `MEM006`<br>2. If slip `BR003` appears, click Return | Slip: `BR003` of `MEM006` | System rejects the return action; slip BR003 remains in "Borrowed" status. | REQ-05, 08 | EP |
| TC-29 | Failed login — password filled, email empty | None | 1. Leave Email blank<br>2. Enter Password<br>3. Click Login | Email: `""`<br>Pass: `admin123` | Displays message: "Please enter email". | REQ-01 | EP |
| TC-30 | Failed login — email filled, password empty | None | 1. Enter Email<br>2. Leave Password blank<br>3. Click Login | Email: `librarian@library.com`<br>Pass: `""` | Displays message: "Please enter password". | REQ-01 | EP |
| TC-31 | Validate full book information display | Logged in | 1. Navigate to Books tab<br>2. Observe details of BOOK001 | Book ID: `BOOK001` | Displays exact info: title `Lập trình Flutter cơ bản`, author `Nguyễn Minh Đức`, category `Công nghệ`, publish year `2023`, status `Available` | REQ-02 | EP |

---

## Summary

| Feature Group | # TC | REQ Coverage | IDM Techniques Applied |
|---------------|------|--------------|------------------------|
| Login | 7 | REQ-01 | EP, BVA |
| View List & Search, Filter | 8 | REQ-02, REQ-03 | EP |
| Borrow, Return, Overdue | 9 | REQ-04, REQ-05, REQ-06 | EP, BVA, Decision Table (DT) |
| Member Management, Lookup | 7 | REQ-07, REQ-08 | EP |
| **Total** | **31** | **8 REQ** | **EP, BVA, Decision Table** |
