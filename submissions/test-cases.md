# Test Cases

> **Instructions**: Write at least **20 TCs** covering all main features (REQ-01 → REQ-08).
> See [examples/sample-test-case.md](../examples/sample-test-case.md) to understand how to write a good TC.
> Organize and group your test cases in the most logical way.

| Information | |
|---|---|
| **Group** | STQA_GROUP_05 |
| **Date Created** | 21/05/2026 |
| **System** | <https://stqa.rbc.vn> |
| **Reference** | SRS v1.0 |

---

## Step 1: Input Domain Modeling (IDM)

## IDM — Login (REQ-01)

| Characteristic | Block | Representative Value | Expected Result |
|---|---|---|---|
| Does the email exist in the DB? | Yes | `librarian@library.com` | Login successful |
| | No | `noone@email.com` | Error message "Member not found" |
| Is the password correct? | Correct | `admin123` | Login successful |
| | Incorrect | `wrongpass` | Error message "Incorrect password" |
| Is the input field empty? | Not empty | (any value) | Processed normally |
| Input Space / Fields | Both fields empty | Email `""`, Pass `""` | Message "Please enter email and password" |
| | Email empty, Pass filled | Email `""`, Pass `admin123` | Message "Please enter email" |
| | Email filled, Pass empty | Email `ba.nguyen@email.com`, Pass `""` | Message "Please enter password" |

---

## IDM — View Book List (REQ-02)

| Characteristic | Block | Representative Value | Expected Result |
|---|---|---|---|
| Access rights (Role) | Librarian | `librarian@library.com` | Sees the full book list |
| | Member | `ba.nguyen@email.com` | Sees the full book list |
| Book status after borrowing? | Book just borrowed | `BOOK001` (Available -> Borrowed) | Status updates to "Borrowed" immediately (real-time) |
| Book status after returning? | Book just returned | `BOOK003` (Borrowed -> Available) | Status updates back to "Available" immediately (real-time) |
| Book information display | Complete | (any book) | Displays: title, author, category, publish year, code, status |
| | Missing field | N/A | No field may be hidden |

---

## IDM — Search for Books (REQ-03)

| Characteristic | Block | Representative Value | Expected Result |
|---|---|---|---|
| Does the keyword exist in the DB? | Yes (book title) | `"Flutter"` | Displays books containing "Flutter" |
| | Yes (author name) | `"Nguyen"` | Displays books by author Nguyen |
| | No | `"XYZ123"` | Empty list, displays "No books found" |
| Case-sensitive? | Lowercase | `"flutter"` | Same result as "Flutter" |
| | UPPERCASE | `"FLUTTER"` | Same result as "Flutter" |
| Category/Genre filter | Exist | `"Management"` / `"Công nghệ"` / `"Kinh tế"` | Displays books in selected category |
| | Non-exist | `"Psychology"` / `"Tâm lý học"` | Empty list, displays "No books found" |

---

## IDM & Decision Table — Borrow Book (REQ-04)

### Decision Table for Borrow Book Feature

| Conditions / Actions | R1 | R2 | R3 | R4 | R5 | R6 |
|---------------------|----|----|----|----|----|----|
| **Conditions** | | | | | | |
| C1: Book status is "Available"? | True | False (Borrowed) | False (Lost) | True | True | True |
| C2: Account status is "Active"? | True | True | True | False (Suspended) | False (Expired) | True |
| C3: Number of books borrowed < 3? | True | - | - | - | - | False |
| **Actions** | | | | | | |
| A1: Allow borrow successfully | X | | | | | |
| A2: Reject — book already borrowed | | X | | | | |
| A3: Reject — book lost | | | X | | | |
| A4: Reject — suspended member | | | | X | | |
| A5: Reject — expired member | | | | | X | |
| A6: Reject — limit exceeded | | | | | | X |

### Supplementary IDM Table (for other attributes)

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
| | > 3 (BVA: above limit) | Attempting a 4th borrow | Reject, show limit exceeded message |

---

## IDM — Return, Overdue Handling, Member Management, Lookup (REQ-05 to REQ-08)

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

## REQ-01: Login

| TC ID | Test Objective | Precondition | Steps | Input Data | Expected Result | Technique |
|---|---|---|---|---|---|---|
| TC-01 | Successful login as Librarian | None | 1. Navigate to website<br>2. Enter Email & Password<br>3. Click Login | `librarian@library.com` / `admin123` | Redirect to homepage, display Librarian info | EP |
| TC-02 | Successful login as Member | None | Same as TC-01 | `ba.nguyen@email.com` / `password123` | Redirect to homepage, display Member info | EP |
| TC-03 | Failed login — email does not exist | None | Enter invalid email and password | `nobody@test.com` / `admin123` | Display "Member not found" | EP |
| TC-04 | Failed login — wrong password | None | Enter valid email and wrong password | `ba.nguyen@email.com` / `wrongpass` | Display "Incorrect password" | EP |
| TC-05 | Failed login — both fields empty | None | Leave Email & Password empty | `""` / `""` | Display "Please enter email and password" | BVA |
| TC-06 | Failed login — empty email only | None | Leave Email blank | `""` / `admin123` | Display "Please enter email" | EP |
| TC-07 | Failed login — empty password only | None | Leave Password blank | `librarian@library.com` / `""` | Display "Please enter password" | EP |

---

## REQ-02: View Book List

| TC ID | Test Objective | Precondition | Steps | Input Data | Expected Result | Technique |
|---|---|---|---|---|---|---|
| TC-08 | Book list displays correct information | Logged in | Open Books tab | N/A | Display title, author, genre, year, status | EP |
| TC-09 | Book status updates immediately after borrowing | Logged in as MEM003 | Borrow available book | `BOOK002` | Status changes to "Borrowed" without reload | EP |

---

## REQ-03: Search & Filter Books

| TC ID | Test Objective | Precondition | Steps | Input Data | Expected Result | Technique |
|---|---|---|---|---|---|---|
| TC-10 | Search book by title | Logged in | Enter keyword | `Flutter` | Display `BOOK001` | EP |
| TC-11 | Search book by author | Logged in | Enter author name | `Tran Van Hung` | Display `BOOK002` | EP |
| TC-12 | Search is case-insensitive | Logged in | Enter lowercase keyword | `flutter` | Display `BOOK001` | EP |
| TC-13 | Search with no result | Logged in | Enter non-existing keyword | `XYZ123` | Display "No books found" | EP |
| TC-14 | Filter books by genre | Logged in | Filter by genre | `Management` / `Kinh tế` | Display correct matching books | EP |

---

## REQ-04: Borrow Book

| TC ID | Test Objective | Precondition | Steps | Input Data | Expected Result | Technique |
|---|---|---|---|---|---|---|
| TC-15 | Borrow available book successfully | Logged in as MEM006 | Select available book → Borrow | `BOOK001` | Borrow success, status becomes Borrowed | DT |
| TC-16 | Cannot borrow already borrowed book | Logged in as Member | Borrow unavailable book | `BOOK003` | Borrow rejected or button hidden | EP |
| TC-17 | Cannot borrow lost book | Logged in as Member | Borrow lost book | `BOOK007` | Borrow rejected or button hidden | EP |
| TC-18 | Suspended member cannot borrow | Logged in as MEM004 | Borrow available book | `BOOK001` | Display suspended-account message | DT |
| TC-19 | Expired member cannot borrow | Logged in as MEM005 | Borrow available book | `BOOK001` | Display expired-account message | DT |
| TC-20 | Cannot borrow more than 3 books | Reset data, login MEM002 | Borrow until reaching 4th book | `BOOK001`, `BOOK002`, `BOOK004` | Reject 4th borrow request | BVA / DT |

---

## REQ-05 & REQ-06: Return Book & Overdue

| TC ID | Test Objective | Precondition | Steps | Input Data | Expected Result | Technique |
|---|---|---|---|---|---|---|
| TC-21 | Return book on time successfully | Reset data, login MEM006 | Borrow then return book | `BOOK001` | Status becomes Available, no warning | EP |
| TC-22 | Return overdue book | Reset data, login MEM002 | Return overdue slip | `BR001` | Return success with overdue warning | BVA |
| TC-23 | Member cannot return another member’s book | Reset data, login MEM002 | Attempt to return other member slip | `BR003` | Action rejected | EP |
| TC-24 | Librarian updates overdue status | Reset data, login Librarian | Click "Check Overdue" | N/A | Overdue slips updated correctly | DT |

---

## REQ-07: Member Management

| TC ID 	| Test Objective                                       	| Precondition                                    	| Steps                                                       	| Input Data                                                                        	| Expected Result                                                       	| Technique 	|    	|
|-------	|------------------------------------------------------	|-------------------------------------------------	|-------------------------------------------------------------	|-----------------------------------------------------------------------------------	|-----------------------------------------------------------------------	|-----------	|----	|
| TC-25 	| Librarian adds valid member successfully             	| Logged in as Librarian, in `Add new member` tab 	| 1. Enter Name, Email, Phone<br>2. Click Add Member          	| Name: `Nguyen Test`<br>Email: `testnewuser99@gmail.com`<br>Phone: `0901234567`    	| New member added successfully, member code is generated and displayed 	| EP        	|    	|
| TC-26 	| Librarian fails to add member due to invalid format  	| Logged in as Librarian, in `Add new member` tab 	| 1. Enter details with invalid email<br>2. Click Add Member  	| Name: `Test Invalid`<br>Email: `new@gmail` (missing `.`)<br>Phone: `0901234567`   	| System rejects creation, displays email format error message.         	| REQ-07    	| EP 	|
| TC-26 	| Librarian fails to add member due to invalid format  	| Logged in as Librarian, in `Add new member` tab 	| 1. Enter details with invalid email<br>2. Click Add Member  	| Name: `Test Invalid`<br>Email: `new@gmail` (missing `.`)<br>Phone: `0901234567`   	| System rejects creation, displays email format error message.         	| REQ-07    	| EP 	|
| TC-27 	| Librarian fails to add member due to duplicate Email 	| Logged in as Librarian, in `Add new member` tab 	| 1. Enter details with existing email<br>2. Click Add Member 	| Name: `Nguyen Duplicate`<br>Email: `librarian@library.com`<br>Phone: `0901234567` 	| System rejects creation, displays duplicate email error message.      	| REQ-07    	| EP 	|
| TC-28 	| Librarian fails to add member — empty Full Name      	| Logged in as Librarian, in `Add new member` tab 	| 1. Leave Name empty, fill other fields<br>2. Click Add      	| Name: `""`<br>Email: `valid@email.com`<br>Phone: `0901234567`                     	| System rejects action, displays "Full name must not be blank".        	| REQ-07    	| EP 	|
| TC-29 	| Librarian fails to add member — empty Phone Number   	| Logged in as Librarian, in `Add new member` tab 	| 1. Leave Phone empty, fill other fields<br>2. Click Add     	| Name: `Nguyen Phone`<br>Email: `valid2@email.com`<br>Phone: `""`                  	| System rejects action, displays "Phone number must not be blank".     	| REQ-07    	| EP 	|
| TC-30 	| Librarian fails to add member — invalid Phone format 	| Logged in as Librarian, in `Add new member` tab 	| 1. Enter Phone with characters<br>2. Click Add              	| Name: `Nguyen Character`<br>Email: `valid3@email.com`<br>Phone: `09abcde345`      	| System rejects action, displays "Invalid Phone number".               	| REQ-07    	| EP 	|

---

## REQ-08: Borrow Record Lookup

| TC ID 	| Test Objective                             	| Precondition                                   	| Steps                  	| Input Data 	| Expected Result                                                                	| Technique 	|
|-------	|--------------------------------------------	|------------------------------------------------	|------------------------	|------------	|--------------------------------------------------------------------------------	|-----------	|
| TC-31 	| Member can view only own borrowing slips   	| Login as `MEM002`                              	| Open Borrow/Return tab 	| N/A        	| Only slips `BR001`, `BR004` belonging to `MEM002` are visible.                 	| EP        	|
| TC-32 	| Librarian can view all borrowing slips     	| Login as Librarian                             	| Open Borrow/Return tab 	| N/A        	| Displays a comprehensive list of all borrowing slips from all library members. 	| EP        	|
| TC-33 	| Member cannot lookup another member’s slip 	| Reset data, login MEM002, in Borrow/Return tab 	| Lookup other member ID 	| `MEM006`   	| System rejects lookup or does not display MEM006's records                     	| EP        	|

---

# Summary

| Feature Group | # TC | REQ Coverage | Techniques |
|---|---|---|---|
| Login | 7 | REQ-01 | EP, BVA |
| View Book List | 2 | REQ-02 | EP |
| Search & Filter | 5 | REQ-03 | EP |
| Borrow Book | 6 | REQ-04 | EP, BVA, DT |
| Return & Overdue | 4 | REQ-05, REQ-06 | EP, BVA, DT |
| Member Management | 6 | REQ-07 | EP |
| Borrow Record Lookup | 3 | REQ-08 | EP |
| **TOTAL** | **33 TC** | **REQ-01 → REQ-08** | **EP, BVA, DT** |
