# Test Cases

> **Instructions**: Write at least **20 TCs** covering all main features (REQ-01 → REQ-08).
> See [examples/sample-test-case.md](../examples/sample-test-case.md) to understand how to write a good TC.
> Organize and group your test cases in the most logical way.

| Information |  |
| --- | --- |
| **Group** | STQA_GROUP_05 |
| **Date Created** | 07/06/2026 |
| **System** | https://stqa.rbc.vn |
| **Reference** | SRS v1.0 |

---

## Step 1: Input Domain Modeling (IDM)

### IDM - Login (REQ-01)
| Characteristic | Block | Representative Value | Expected Result |
| --- | --- | --- | --- |
| Does the email exist in the DB? | Yes | `librarian@library.com` | Login successful |
|  | No | `noone@email.com` | Error message "Member not found" |
| Is the password correct? | Correct | `admin123` | Login successful |
|  | Incorrect | `wrongpass` | Error message "Incorrect password" |
| Input Space / Fields | Both fields empty | Email `""`, Pass `""` | Message "Please enter email and password" |
|  | Email empty, Pass filled | Email `""`, Pass `admin123` | Message "Please enter email" |
|  | Email filled, Pass empty | Email `ba.nguyen@email.com`, Pass `""` | Message "Please enter password" |

### IDM - View Book List (REQ-02)
| Characteristic | Block | Representative Value | Expected Result |
| --- | --- | --- | --- |
| Access rights (Role) | Librarian | `librarian@library.com` | Sees the full book list |
|  | Member | `ba.nguyen@email.com` | Sees the full book list |
| Book status after borrowing? | Book just borrowed | `BOOK001` (Available → Borrowed) | Status updates to "Borrowed" immediately (real-time) |
| Book status after returning? | Book just returned | `BOOK003` (Borrowed → Available) | Status updates back to "Available" immediately (real-time) |
| Book information display | Complete | (any book) | Displays: title, author, category, publish year, code, status |

### IDM - Search for Books (REQ-03)
| Characteristic | Block | Representative Value | Expected Result |
| --- | --- | --- | --- |
| Does the keyword exist in the DB? | Yes (book title) | `Flutter` | Displays books containing `Flutter` |
|  | Yes (author name) | `Nguyễn` | Displays books by author `Nguyễn` |
|  | No | `XYZ123` | Empty list, displays "No books found" |
| Case-sensitive? | Lowercase | `"flutter"` | Same result as `Flutter` |
|  | UPPERCASE | `"FLUTTER"` | Same result as `Flutter` |
| Category/Genre filter | Exist | `Công nghệ` | Displays books in selected category |
|  | Non-exist | `Tâm lý học` | Empty list, displays "No books found" |

### IDM & Decision Table - Borrow Book (REQ-04)

**Decision Table for Borrow Book Feature:**

| Conditions / Actions | R1 | R2 | R3 | R4 | R5 | R6 |
| --- | --- | --- | --- | --- | --- | --- |
| **Conditions** |  |  |  |  |  |  |
| C1: Book status is "Available"? | True | False (Borrowed) | False (Lost) | True | True | True |
| C2: Account status is "Active"? | True | True | True | False (Suspended) | False (Expired) | True |
| C3: Number of books borrowed < 3? | True | - | - | - | - | False |
| **Actions** |  |  |  |  |  |  |
| A1: Allow borrow successfully | X |  |  |  |  |  |
| A2: Reject — book already borrowed |  | X |  |  |  |  |
| A3: Reject — book lost |  |  | X |  |  |  |
| A4: Reject — suspended member |  |  |  | X |  |  |
| A5: Reject — expired member |  |  |  |  | X |  |
| A6: Reject — limit exceeded |  |  |  |  |  | X |

**Supplementary IDM Table (for other attributes):**
| Characteristic | Block | Representative Value | Expected Result |
| --- | --- | --- | --- |
| Book status | Available | `BOOK001` | Allow borrow |
|  | Borrowed | `BOOK003` | Reject, show book unavailable error |
|  | Lost | `BOOK007` | Reject, show book lost error |
| Member status | Active | `MEM002` | Allow borrow |
|  | Suspended | `MEM004` | Reject, show suspended account error |
|  | Expired | `MEM005` | Reject, show expired account error |
| Number of books borrowed (BVA) | < 3 (BVA: 0, 1, 2) | `MEM003` (0 books) / `MEM002` (1 book) | Allow borrow |
|  | = 3 (BVA: limit) | `MEM002` after borrowing 2 more books | Reject, show limit exceeded message |
|  | > 3 (BVA: above limit) | Attempting a 4th borrow | Reject, show limit exceeded message |

### IDM & Decision Table - Return book (REQ-05)

| Conditions / Actions | R1 | R2 | R3 | R4 |
|---|---|---|---|---|
| **Conditions** | | | | |
| C1: Book Belonging is Borrower? | T | T | T | F (non-borrower) |
| C2: returnDate < dueDate? | T | F (returnDate = dueDate) | F (returnDate > dueDate) | - |
| **Actions** | | | | |
| A1: Accept (no warning) | X | | | |
| A2: Accept (warning) | | X<br>(return late) | X<br>(return late) | |
| A3: Reject (reason) | | | | X<br>(not borrow by this member) | 

### IDM - Overdue Handling (REQ-06)
| Characteristic | Block | Representative Value | Expected Result |
| --- | --- | --- | --- |
| Check and View Overdue Borrow Records | Librarian | `librarian@library.com` | Can check and view all |
| | Member | `ba.nguyen@email.com` | Cannot check<br>Can view own, cannot view other's |
| currentDate vs dueDate | currentDate > dueDate | `06/06 > 01/06` | Update status to "Overdue" |
| | currentDate = dueDate | `06/06 = 06/06` | Update status to "Overdue" |
|  | currentDate < dueDate | `06/06 < 10/06` | Keep status as "Borrowed", not mark "Overdue" |
| Borrow Record Status | Borrowed | Active borrow record | Eligible for overdue scanning |
|  | Returned | Already returned record | Excluded from overdue scanning |

### IDM - Member Management (REQ-07)
| Characteristic | Block | Representative Value | Expected Result |
| --- | --- | --- | --- |
| Role adding member | Librarian | `librarian@library.com` | Access allowed |
|  | Member | `ba.nguyen@email.com` | Access denied / feature hidden |
| Full Name input | Non-empty | `"Nguyen Test"` | Valid, proceed to next field |
|  | Empty | `""` | Show error "Full name must not be blank" |
| Email format validation | Valid | `new@gmail.com` | Valid format |
|  | Missing `@` | `newgmail.com` | Show "Invalid Email" error |
|  | Missing `.` in domain | `new@gmail` | Show "Invalid Email" error |
|  | Missing domain part | `new@` | Show "Invalid Email" error |
|  | Missing local part | `@gmail.com` | Show "Invalid Email" error |
|  | Contains space | `new member@gmail.com` | Show "Invalid Email" error |
|  | Empty | `""` | Show error "Email must not be blank" |
| Email uniqueness | Duplicate | `librarian@library.com` | Show "Email already exists" error |
|  | Unique | `newuser@gmail.com` | Valid uniqueness |
| Phone Number input | Non-empty digits | `0901234567` | Valid format |
|  | Empty | `""` | Show error "Phone number must not be blank" |
|  | Contains letters/chars | `09abcde345` | Show error "Invalid Phone number" |

### IDM - Borrow Record Lookup (REQ-08)
| Characteristic | Block | Representative Value | Expected Result |
| --- | --- | --- | --- |
| Lookup permissions | Member looks up own | MEM002 looks up MEM002 | View successful, shows BR001, BR004 |
|  | Member looks up other | MEM002 looks up MEM006 | Access denied / Hide records |
|  | Librarian looks up all | LIB001 looks up any ID | View successful, shows all records |
| Record existence | Found | Existing Member ID / Slip ID | Displays detailed borrow records |
|  | Not found | Non-existent ID (`MEM100`) | Show "No borrow records found" |
| Information display | Full details | Viewing `BR001` | Displays: Record ID, Book title, borrow date, due date, status |

---

## Step 2: Test Cases

### REQ-01: Login

| TC ID | Test Objective | Precondition | Steps | Input Data | Expected Result | REQ | Technique |
| --- | --- | --- | --- | --- | --- | --- | --- |
| TC-01 | Successful login as Librarian | Library App is open<br>Librarian account exists in the system | 1. Navigate to the website<br>2. Enter Email<br>3. Enter Password<br>4. Click "Login" | Email: `librarian@library.com`<br>Pass: `admin123` | Redirected to home page, displays name "Nguyen Thu Thu" and role Librarian. | REQ-01 | EP |
| TC-02 | Successful login as Member | Library App is open<br> Member account exists in the system | 1. Navigate to the website<br>2. Enter Email<br>3. Enter Password<br>4. Click "Login" | Email: `ba.nguyen@email.com`<br>Pass: `password123` | Redirected to home page, displays name "Nguyen Hoc Ba" and role Member. | REQ-01 | EP |
| TC-03 | Failed login - email does not exist | Library App open<br>Email address not in the list | 1. Navigate to the website<br>2. Enter Email<br>3. Enter Password<br>4. Click Login | Email: `nobody@test.com`<br>Pass: `admin123` | Displays message: "Member not found". | REQ-01 | EP |
| TC-04 | Failed login - wrong password | Library App open<br>Account exists in the system | 1. Enter valid Email<br>2. Enter wrong Password<br>3. Click Login | Email: `ba.nguyen@email.com`<br>Pass: `wrongpass` | Displays message: "Incorrect password". | REQ-01 | EP |
| TC-05 | Failed login - empty fields | Library App open<br>Leave both email and password empty when logging in | 1. Leave Email and Password empty<br>2. Click Login | Email: `""`<br>Pass: `""` | Displays message "Please enter email and password". | REQ-01 | EP |
| TC-06 | Failed login - email empty, password filled | Library App open<br>Leave email empty when logging in | 1. Leave Email blank<br>2. Enter Password<br>3. Click Login | Email: `""`<br>Pass: `admin123` | Displays message: "Please enter email". | REQ-01 | EP |
| TC-07 | Failed login - email filled, password empty | Library App open<br>Leave password empty when logging in | 1. Enter Email<br>2. Leave Password blank<br>3. Click Login | Email: `librarian@library.com`<br>Pass: `""` | Displays message: "Please enter password". | REQ-01 | EP |

### REQ-02: View Book List

| TC ID | Test Objective | Precondition | Steps | Input Data | Expected Result | REQ | Technique |
| --- | --- | --- | --- | --- | --- | --- | --- |
| TC-08 | View all books display by list | User logged in as either  `Librarian` or `Member` and currently on `Books` tab | 1. Login as `Librarian` or `Member`<br>2. Go to `Books` tab<br>3. View book list | Librarian account: `librarian@library.com` / `admin123` <br> OR <br> Member account: `ba.nguyen@email.com` / `password123` | List displays 20 books, each includes title, author, genre, publication year, status. | REQ-02 | EP |
| TC-09 | Book status updates real-time after borrowing | User logged in as `MEM003`<br>`BOOK002` is `Available` | 1. Login as `MEM003`<br>Go to `Books` tab, <br>2. Click Borrow `BOOK002`<br>3. Observe `BOOK002` immediately after confirm borrowing | `MEM003` account: `dam.tran@email.com` / `password123`<br>Book: `BOOK002` | BOOK002 must immediately change status to "Borrowed" in the list without needing to reload the page. | REQ-02 | EP |
| TC-10 | Book status updates real-time after returning | User logged in as `MEM003`<br>`BOOK002` is `Borrowed` | 1. Login as `MEM003`<br>2. Go to `Borrow / Return` tab<br>3. Return `BOOK002`<br>4. Go to `Books` tab<br>5. Observe `BOOK002` status | `MEM003` account: `dam.tran@email.com` / `password123`<br>Book: `BOOK002` | `BOOK002` status changes from `Borrowed` to `Available` immediately | REQ-02 | EP |
| TC-11 | Book displays full info | User account login successfully | 1. Login user account<br>2. Open `Books` tab<br>3. Observe `BOOK001` | Email: `ba.nguyen@email.com`<br>Password: `password123`<br>Book ID: `BOOK001` | Display  `BOOK001` with title `Lập trình Flutter cơ bản`, author `Nguyễn Minh Đức`, category `Công nghệ`, publish year `2023`, code `BOOK001`, status `Available` | REQ-02 | EP |

### REQ-03: Search & Filter Books

| TC ID | Test Objective | Precondition | Steps | Input Data | Expected Result | REQ | Technique |
| --- | --- | --- | --- | --- | --- | --- | --- |
| TC-12 | Search books by title (with results) | User account login successfully<br>Keyword exists | 1. Login user account<br>2. Go to `Books` tab<br>3. Enter `Flutter` in the search box | Account: `ba.nguyen@email.com` / `password123`<br>Keyword: `Flutter` | Displays book(s) containing keyword in its title. | REQ-03 | EP |
| TC-13 | Search books by author name | User account login successfully<br>Author exists | 1. Login user account<br>2. Go to `Books` tab<br>3. Enter `Nguyễn Minh Đức` in the search box | Account: `ba.nguyen@email.com` / `password123`<br>Keyword: `Nguyễn Minh Đức` | Displays book(s) belonging to the searched author's keyword. | REQ-03 | EP |
| TC-14 | Search books by non-existent keyword | User account login successfully<br>Keyword does not exist | 1. Login<br>2. Go to `Books` tab<br>3. Enter `XYZ123` in the search box | Account: `ba.nguyen@email.com` / `password123` <br>Keyword: `XYZ123` | Empty list, displays "No books found". | REQ-03 | EP |
| TC-15 | Search books is case-insensitive | Logged in | 1. Enter keyword in lowercase/UPPERCASE | Keyword: `flutter` OR `FLUTTER` | Displays book(s) containing keyword in its title, either in lowercase or UPPERCASE (same as TC-10). | REQ-03 | EP |
| TC-16 | Filter books by genre (with results) | User account login successfully <br>Genre: `Kinh tế` | 1. Login user account<br>2. Go to `Books` tab<br>3. Enter `Kinh tế` in the filter box | Account: `ba.nguyen@email.com` / `password123`<br> Genre: `Kinh tế` | List displays books belonging to the filtered category. | REQ-03 | EP |
| TC-17 | Filter books by non-existent genre | User account login successfully<br>Genre: `Tâm lý học` does not exist | 1. Login user account<br>2. Go to `Books` tab<br>3. Enter `Tâm lý học` in the filter box | Account: `ba.nguyen@email.com` / `password123`<br>Genre: `Tâm lý học` | Empty list, displays "No books found". | REQ-03 | EP |


### REQ-04: Borrow Book

| TC ID | Test Objective | Precondition | Steps | Input Data | Expected Result | REQ | Technique |
| --- | --- | --- | --- | --- | --- | --- | --- |
| TC-18 | Borrow book successfully | Logged in as `Active` member<br>`Available` book status<br>borrowCount < 3 | 1. Login as `Active` member<br>2. Select an `Available` book<br>3. Click borrow | `MEM006` account: `biet.hoang@email.com` / `password123`<br>`Available` book: `BOOK001` | Borrow successful, book status changes to `Borrowed`<br>A new borrow record is created with due date = borrow date + 14 days in `My borrow records` from `Borrow / Return` tab. | REQ-04 | DT (R1) |
| TC-19 | Cannot borrow a book already borrowed | Logged in as `Active` member <br>`Borrowed` book status<br>borrow Count < 3 | 1. Login as `Active` member <br> 2. Select a `Borrowed` book<br>3. Click borrow | `MEM006` account: `biet.hoang@email.com` / `password123`<br>`Borrowed` book: `BOOK003` | Borrow button is hidden or rejection message shown. | REQ-04 | EP / DT (R2) |
| TC-20 | Cannot borrow a `Lost` book | Logged in as `Active` member<br>`Lost` book status<br>borrowCount < 3 | 1. Login as `Active` member<br>2. Select a `Lost` book<br>3. Click Borrow | `MEM006` account: `biet.hoang@email.com` / `password123`<br>`Lost` book: `BOOK007` | Borrow button is hidden or rejection message shown. | REQ-04 | EP / DT (R3) |
| TC-21 | Suspended member cannot borrow books | Logged in as `Suspended` member<br>`Available` book status | 1. Login as `MEM004`<br>2. Select an `Available` book<br>3. Click Borrow | `MEM004` account: `cu.le@email.com` / `password123`<br>`Available` book: `BOOK001` | Borrow rejected<br>Display warning `Suspended member. Cannot borrow books!` | REQ-04 | DT (R4) |
| TC-22 | Expired member cannot borrow books | Logged in as `Expired` member<br>`Available` book status | 1. Login as `MEM005`<br>2. Select an `Available` book<br>3. Click Borrow | Book: `BOOK001` | Borrow rejected<br>Display warning `Expired member. Cannot borrow books!` | REQ-04 | DT (R5) |
| TC-23 | Cannot borrow more than 3 books | Reset data, logged in as `MEM002`<br>`MEM002` has borrowed `BOOK003` already<br>Other `Available` books | 1. Login as `MEM002`<br>2. Borrow 2 more books to reach 3<br>3. Attempt to borrow a 4th book | Additional: `BOOK001`, `BOOK002`<br>4th book: `BOOK005` | Reject borrowing the 4th book, display borrow limit exceeded error message. | REQ-04 | BVA / DT (R6) |

### REQ-05: Return Book

| TC ID | Test Objective | Precondition | Steps | Input Data | Expected Result | REQ | Technique |
| --- | --- | --- | --- | --- | --- | --- | --- |
| TC-24 | Return book on time successfully | Reset data, logged in as `Active` member | 1. Login as `Active` member<br>2. Borrow an `Available` book in `Books` tab<br>2. Go to `Borrow/Return` tab<br>3. Click Return Book | `Active` account: `biet.hoang@email.com` / `password123`<br>Book: `BOOK001` | Book returned successfully, status reverts to `Available`. No warning shown. | REQ-05 | BVA / DT (R1) |
| TC-25 | Returning an on dueDate book | Reset data, logged in as `MEM002` | 1. Login as `MEM002`<br>2. Go to `Borrow/Return` tab<br>3. Find borrow record `BR001` (past due date)<br>4. Click Return Book | Borrow record: `BR001`<br>`currentDate = dueDate` (eg: `15/09/2024 = 15/09/2024`) | Book returned successfully but with an overdue warning notification<br>Book status changes to `Available`<br>Borrow record status changes to `Returned`. | REQ-05 | BVA / DT (R2) |
| TC-26 | Returning an overdue book | Reset data, logged in as `MEM002` | 1. Login as `MEM002`<br>2. Go to `Borrow/Return` tab<br>3. Find borrow record `BR001` (past due date)<br>4. Click Return Book | Borrow record: `BR001` <br> `currentDate > dueDate` (eg: `06/06/2026 > 15/09/2024`) | Book returned successfully but with an overdue warning notification<br>Book status changes to `Available`<br>Borrow record status changes to `Returned`. | REQ-05 | BVA / DT (R3) |
| TC-27 | Member cannot return another member's book | Reset data, logged in as `MEM002` | 1. Login as `MEM002`<br>2. Go to `Borrow/Return` tab<br>3. Attempt to return another member's borrow record | Borrow record: `BR003` of `MEM006` | System rejects the return action; record remains in "Borrowed" status. | REQ-05, REQ-08 | EP / DT (R4) |

### REQ-06: Overdue Handling

| TC ID | Test Objective | Precondition | Steps | Input Data | Expected Result | REQ | Technique |
| --- | --- | --- | --- | --- | --- | --- | --- |
| TC-28 | Librarian check and view Overdue books | Reset data, logged in as Librarian | 1. Login as `Librarian`<br>2. Go to `Borrow/Return` tab<br>3. Click "Check Overdue Books" | `Librarian` account: `librarian@library.com` / `admin123` | Active borrowed records with due dates ≤ today (`BR001`, `BR003`) update to **Overdue**. | REQ-06 | EP / BVA |
| TC-29 | Member view their own Overdue books | After Librarian check Overdue books, logging out<br>Then logging in as `MEM002` | 1. Login as Librarian and check Overdue books<br>2. Log out<br>3. Login as `MEM002`<br>4. Go to `Borrow / Return` tab<br>5. Observe Overdue book status | `Librarian` account: `librarian@library.com` / `admin123`<br>`MEM002` account: `ba.nguyen@email.com` / `password123` | Display `BR001` with status **Overdue** | REQ-06 | EP |


### REQ-07: Member Management

| TC ID | Test Objective | Precondition | Steps | Input Data | Expected Result | REQ | Technique |
| --- | --- | --- | --- | --- | --- | --- | --- |
| TC-30 | View member detailed information | Logged in as Librarian<br>Member exists in the system | 1. Login as `Librarian`<br>2. Go to `Members` tab<br>3. Observe member list | `Librarian` account: `librarian@library.com` / `admin123`<br>Member IDs: `MEM002`. `MEM004`, `MEM005` | Each member's info includes name, ID, email, phone number, nb of books borrowed and status<br>Member status displays correctly(eg: `MEM002` = `Active`, `MEM004` = `Suspended`, `MEM005` = `Expired`) | REQ-07 | EP |
| TC-31 | Librarian adds valid member | Logged in as Librarian | 1. Members tab → Add New<br>2. Enter Name, Email, Phone<br>3. Click Add Member | Name: `Nguyen Test`<br>Email: `testnewuser99@gmail.com`<br>Phone: `0901234567` | New member added successfully, member code is generated and displayed. | REQ-07 | EP |
| TC-32 | Librarian fails to add member due to invalid format | Logged in as Librarian | 1. Members tab → Add New<br>2. Enter details with invalid email<br>3. Click Add Member | Name: `Test Invalid`<br>Email: `new@gmail` (missing `.`)<br>Phone: `0901234567` | System rejects creation, displays email format error message. | REQ-07 | EP |
| TC-33 | Librarian fails to add member due to duplicate Email | Logged in as Librarian | 1. Members tab → Add New<br>2. Enter details with existing email<br>3. Click Add Member | Name: `Nguyen Duplicate`<br>Email: `librarian@library.com`<br>Phone: `0901234567` | System rejects creation, displays duplicate email error message. | REQ-07 | EP |
| TC-34 | Librarian fails to add member — empty Full Name | Logged in as Librarian | 1. Members tab → Add New<br>2. Leave Name empty, fill other fields<br>3. Click Add | Name: `""`<br>Email: `valid@email.com`<br>Phone: `0901234567` | System rejects action, displays "Full name must not be blank". | REQ-07 | EP |
| TC-35 | Librarian fails to add member — empty Phone Number | Logged in as Librarian | 1. Members tab → Add New<br>2. Leave Phone empty, fill other fields<br>3. Click Add | Name: `Nguyen Phone`<br>Email: `valid2@email.com`<br>Phone: `""` | System rejects action, displays "Phone number must not be blank". | REQ-07 | EP |
| TC-36 | Librarian fails to add member — invalid Phone format | Logged in as Librarian | 1. Members tab → Add New<br>2. Enter Phone with characters<br>3. Click Add | Name: `Nguyen Character`<br>Email: `valid3@email.com`<br>Phone: `09abcde345` | System rejects action, displays "Invalid Phone number". | REQ-07 | EP |

### REQ-08: Borrow Record Lookup

| TC ID | Test Objective | Precondition | Steps | Input Data | Expected Result | REQ | Technique |
| --- | --- | --- | --- | --- | --- | --- | --- |
| TC-37 | Member can only view their own borrowing records | Logged in as `MEM002` | 1. Login as `MEM002`<br>2. Go to `Borrow/Return` tab<br>3. Observe borrowing records | `MEM002` account: `ba.nguyen@email.com` / `password123` | Only records `BR001`, `BR004` belonging to `MEM002` are visible. | REQ-08 | EP |
| TC-38 | Librarian can view all borrowing records | Logged in as `Librarian` | 1. Login as `Librarian`<br>2. Go to `Borrow/Return` tab | `librarian@library.com`<br>`admin123` | Displays a list of all borrowing records from all library members. | REQ-08 | EP |
| TC-39 | Member cannot look up other's records | Reset data, logged in as `MEM002` | 1. Login as `MEM002`<br>2. Go to `Borrow/Return` tab<br>3. Search another Member's ID | Member ID: `MEM006` | System rejects lookup or does not display `MEM006`'s records. | REQ-08 | EP |
| TC-40 | Borrow record displays full information | User logged in account is `Librarian` or record owner `Member` | 1. Login Library App<br>2. Open `Borrow / Return` tab<br>3. Observe a borrow record | Record ID: `BR001` | Display Record ID, book title, borrow date, due date, and status | REQ-08 | EP |

---

## Summary

| Feature Group | # TC | REQ Coverage | IDM Techniques Applied |
| --- | --- | --- | --- |
| Login | 7 | REQ-01 | EP |
| View Book List | 4 | REQ-02 | EP |
| Search & Filter Books | 6 | REQ-03 | EP |
| Borrow Book | 6 | REQ-04 | EP, BVA, DT |
| Return Book | 4 | REQ-05 | EP, BVA, DT |
| Overdue Handling | 2 | REQ-06 | EP, BVA |
| Member Management | 7 | REQ-07 | EP |
| Borrow Record Lookup | 4 | REQ-08 | EP |
| **Total** | **40** | **8 REQ** | **EP, BVA, DT** |
