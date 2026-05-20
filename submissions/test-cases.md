# Test Cases — Bảng trường hợp kiểm thử

> **Hướng dẫn**: Viết tối thiểu **20 TC** phủ đủ các chức năng chính (REQ-01 → REQ-08).
> Xem [examples/sample-test-case.md](../examples/sample-test-case.md) để hiểu cách viết TC tốt.
> Tự tổ chức và phân nhóm test case theo cách hợp lý nhất.

| Thông tin | |
|---|---|
| **Group** | STQA_GROUP_05 |
| **Date create** | 17/05/2026 |
| **System** | https://stqa.rbc.vn |
| **Reference** | SRS v1.0 |

---

## Bước 1: Input Domain Modeling (Mô hình hóa miền đầu vào) - IDM

> 📖 **Textbook:** Chương 6 — *Input Domain Modeling*, Paul Ammann & Jeff Offutt.
>
> **Trước khi viết Test Case**, nhóm **phải** phân tích miền đầu vào bằng bảng IDM bên dưới.
> Mỗi chức năng cần xác định: **Đặc tính (Characteristic)**, **Phân vùng (Block/Partition)**, và **Giá trị đại diện (Value)**.

### IDM — Login (Đăng nhập) (REQ-01)

| Characteristic | Block | Value | Expected result |
|---|---|---|---|
| Email | Exist | `librarian@library.com` | Login successully |
| | Non-exist | `noone@email.com` | Display error "Member Not Found" |
| Password | Correct | `admin123` | Login successfully |
| | Incorrect | `wrongpass` | Display error "Incorrect Password" |
| Input Space | Empty | `""` | Display "Please enter..." |
| | Non-empty | (random) | Handle normally |

### IDM - View Book List (Xem danh sách sách) (REQ-02)

| Characteristic | Block | Value | Expected result |
|---|---|---|---|
| Access rights | Librarian | `librarian.@library.com` | View all book list |
| | Member | `ba.nguyen@email.com` | View all book list |
| Book list | Yes | `BOOK001` --> `BOOK020` | Display full book list |
| Book information | Full info | name, author, category, publish year, initial state | Display full book information |
| Book state | Available | `BOOK001` | Display state "Available" |
| | Borrowed | `BOOK003` (by `MEM002`) | Display state "Borrowed" |
| | Lost | `BOOK007` | Display state "Lost" |
| Real-time update | After borrow | `BOOK001`: available --> borrowed | Immediate update state |
| | After return | `BOOK003`: borrowed --> available | Immediate update state |

### IDM — Search & Filter Books (Tìm kiếm & lọc sách) (REQ-03)

| Characteristic | Block | Value | Expected result |
|---|---|---|---|
| Keyword | Exist (book) | `"Flutter"` | Display books' name have "Flutter" |
| | Exist (author) | `"Nguyễn"` | Display Nguyễn's book |
| | Non-exist | `"XYZ123"` | Display "No books found" |
| Letter | Lowercase | `"flutter"` | Result shows "Flutter" |
| | Uppercase | `"FLUTTER"` | Result shows "Flutter" |
| Category | Exist | `Công nghệ` | Display books in the `Công nghệ` category |
| | Non-exist | `Tâm lý học` | Display "No books found" |

### IDM — Borrow & Return Book (Mượn & Trả sách) (REQ-04, REQ-05)

| Characteristic | Block | Value | Expected result |
|---|---|---|---|
| Book status | Available | `BOOK001` | Accept borrowing |
| | Borrowed | `BOOK003` | Reject borrowing |
| | Lost | `BOOK007` | Reject borrowing |
| Member status | Active | `MEM002` | Accept borrowing |
| | Suspended | `MEM004` | Reject borrowing, display error |
| | Expired | `MEM005` | Reject borrowing, display error |
| Number of borrowed books (BVA) | Below limit | 2 books | Accept borrowing |
| | At limit | 3 books | Reject borrowing, display limit error |
| | Above limit | 4 books | Reject borrowing, display limit error |
| Due date | accept borrowing | borrow date | borrow date + 14 days |
| Book belongs to the member borrowed | Yes | `MEM006` return `BOOK013` | Accept returning |
| | No | `MEM002` return `BOOK001` | Reject returning |
| Book state after returning | Available | `BOOK013` | Book state return to "Available" |
| returnDate v/s dueDate | before dueDate | returnDate < dueDate | Accept return, no overdue warning |
| | on dueDate | returnDate = dueDate | Accept return, no overdue warning|
| | after dueDate | returnDate > dueDate | Accept return, display overdue warning |

### Decision Table - Borrow Book (REQ-04)

| | | R1 | R2 | R3 | R4 | R5 | R6 |
|---|---|---|---|---|---|---|---|
| Book status | available | x | | | x | x | x |
| | borrowed | | x | | | | |
| | lost | | | x | | | |
| Member status | active | x | x | x | | | x |
| | suspended | | | | x | | |
| | expired | | | | | x | |
| Borrow Count | < 3 | x | x | x | x | x | |
| | = 3 | | | | | | x |
| Action | **Accept** | X | | | | | |
| | **Reject (reason)** | | X <br>(book already borrowed) | X <br>(book lost) | x<br>(Suspended Member) | X <br>(Expired Member) | X <br>(Exceed Limit Borrow Count) |


### IDM - Overdue Handling (Xử lý sách quá hạn) (REQ-06)

| Characteristic | Block | Value | Expected result |
|---|---|---|---|
| Role check Overdue | Librarian | `librarian@library.com` | Accept checking |
| | Member | `ba.nguyen@email.com` | Reject checking |
| "Check overdue books" button | clicked | click | Display notification the number of overdue books |
| | unclicked | N/A | N/A |
| currentDate v/s dueDate | currentDate > dueDate | dueDate: past | Overdue |
| | currentDate = dueDate | dueDate: present | Overdue |
| | currentDate < dueDate | dueDate: future | Not overdue |
| Borrow Record Status | borrowed | status `Borrowed` | If dueDate ≤ currentDate, mark as Overdue |
| | returned | status `Returned` | Do not mark as "Overdue" |
| The right to view Overdue Borrow Records | Librarian | `librarian@library.com` | Can view all |
| | Members with their own records | `MEM002` view `MEM002`'s record | Can view own |
| | Member with other records | `MEM002` view `MEM006`'s record | Cannot view other's | 

### IDM - Member Management (Quản lý thành viên) (REQ-07)

| Characteristic | Block | Value | Expected result |
|---|---|---|---|
| Add member | Librarian | `librarian@library.com` | Accept adding |
| | Member | `ba.nguyen@email.com` | Reject adding |
| Full name | Empty | `""` | Require "Full name must not be left blank" |
| | Nonempty | `Nguyễn Văn A` | Continue enter Email |
| Email | Valid | `new.member@email.com` | Continue enter Phone Number |
| | Invalid (miss `@`) | `new.memberemail.com` | Display "Invalid Email" |
| | Invalid (miss `.` in domain part) | `new.member@email` | Display "Invalid Email" |
| | Invalid (miss domain part) | `new.member@` | Display "Invalid Email" |
| | Invalid (miss local part) | `@email.com` | Display "Invalid Email" |
| | Invalid (include " " space) | `new member@email.com` | Display "Invalid Email" |
| | Invalid (empty) | `""` | Display "Email must not be left blank" |
| Email Existent | Yes | `ba.nguyen@email.com` | Display "Email existed. Please enter another email" |
| | No | `new.member@email.com` | Continue enter Phone Number |
| Phone Number | Nonempty | `0901234567` | Click "Add member" |
| | Empty | `""` | Display "Phone number must not be left blank" |
| | Include non-number character | `09abcde345` | Display "Invalid Phone number" |
| Member Creation result | Success | `Nguyễn Văn A`, `new.member@email.com`, `0901234567` | Add Member successfully |
| | Fail | Invalid email or Phone number | Cannot Add New Member |

### IDM - Borrow Record Lookup (Tra cứu phiếu mượn) (REQ-08)

| Characteristic | Block | Value | Expected result |
|---|---|---|---|
| View Borrow Record | Librarian | `librarian@library.com` | View all members' borrow record |
| | Member with their own records | `MEM002` view `MEM002`'s record | Display record IDs `BR001`, `BR004` |
| | Member with other records | `MEM002` view `MEM003`'s record | Cannot view other's |
| Existence | Found | `MEM002` | Display borrow record information |
| | Not found | `MEM100` | Display "No borrow records found |
| Info | Full info | `BR001` | Display Record ID, Book name, borrow date, due date, status |
| Status | Borrowing | Record ID `BR003` | Display status "Borrowing" |
| | Returned | Record ID `BR002` | Display "Returned" |
| | Overdue | Record ID `BR001` after click "Check overdue books" button | Display "Overdue" |

---

> 💡 **Gợi ý kỹ thuật**: Sử dụng **Phân lớp tương đương (EP)** cho các phân vùng rời rạc, **Phân tích giá trị biên (BVA)** cho các phân vùng số (ví dụ: giới hạn 3 sách). Xem textbook §6.1–6.3.

---

## Bước 2: Test Cases

<!-- Tự tổ chức bảng test case: có thể chia nhóm theo chức năng, theo REQ, hoặc theo luồng nghiệp vụ — tùy nhóm quyết định. -->
<!-- Mỗi TC phải ánh xạ ngược về ít nhất 1 dòng trong bảng IDM ở Bước 1. -->

| Mã TC | Mục tiêu kiểm thử | Tiền điều kiện | Bước thực hiện | Dữ liệu đầu vào | Kết quả mong đợi | REQ | Kỹ thuật |
|-------|-------------------|---------------|---------------|-----------------|------------------|-----|---------|
| TC-01 | Login successfully with Librarian account | Library App is open and account exists in the system | 1. Access to web<br>2. Enter email address<br>3. Enter password<br>4. Click "Log in" | Email: `librarian@library.com`<br>Password: `admin123`| Reach Homepage, display the name "Nguyễn Thủ Thư", role "Librarian" on the AppBar | REQ-01 | EP |
| TC-02 | Login successfully with Member account | Library App is open and account exists in the system | 1. Access to web<br>2. Enter email address<br>3. Enter password<br>4. Click "Log in" | Email: `ba.nguyen@email.com`<br>Password: `password123` | Reach Homepage, display the name "Nguyễn Học Bá", role "Member" on the AppBar | REQ-01 | EP |
| TC-03 | Login failed when entering a non-existent email address | Library App open, Email address not in the list | 1. Access to web<br>2. Enter a nonexistent email address<br>3. Enter password<br>4. Click "Log in" | Email: `nobody@test.com`<br>Password: `anything` | Display error message "Không tìm thấy thành viên", cannot reach Homepage | REQ-01 | EP |
| TC-04 | Login failed when entering the wrong password | Library App open, exist email address | 1. Access to web<br>2. Enter email address<br>3. Enter wrong password<br>4. Click "Log in" | Email: `ba.nguyen@email.com`<br>Password: `wrongpassword` | Display error message "Incorrect password", cannot reach Homepage | REQ-01 | EP |
| TC-05 | Login failed when email is blank | Library App open | 1. Access to web<br>2. Leave Email blank<br>3. Enter Password<br>4. Click "Log in" | Email: `""`<br>Password: `password123` | Display message "Please enter email and password", cannot reach Homepage | REQ-01 | EP |
| TC-06 | Login failed when password is blank | Library App open | 1. Access to web<br>2. Enter Email<br>3. Leave Password blank<br>4. Click "Log in" | Email: `ba.nguyen@email.com`<br>Password: `""` | Display message "Please enter email and password", cannot reach Homepage | REQ-01 | EP |
| TC-07 | Login failed when both email and password is blank | Library App open | 1. Access to web<br>2. Leave Email and Password blank<br>4. Click "Log in" | Email: `""`<br>Password: `""` | Display message "Please enter email and password", cannot reach Homepage | REQ-01 | EP |
| TC-08 | View Book list with Librarian account | User account login is Librarian | 1. Login as Librarian account<br> 2. Open "Books" tab | Email: `librarian@library.com`<br>Password: `admin123` | Display book list from `BOOK001` to `BOOK020` | REQ-02 | EP |
| TC-09 | View Book list with Member account | User account login is Member | 1. Login as Member account<br> 2. Open "Books" tab | Email: `ba.nguyen@email.com`<br>Password: `password123` | Display book list from `BOOK001` to `BOOK020` | REQ-02 | EP |
| TC-10 | Book displays full info | User account login successfully | 1. Login user account<br>2. Open "Books" tab<br>3. Observe `BOOK001` | Book ID: `BOOK001` | Display  `BOOK001` with title `Lập trình Flutter cơ bản`, author `Nguyễn Minh Đức`, category `Công nghệ`, publish year `2023`, code `BOOK001`, status `Available` | REQ-02 | EP |
| TC-11 | Display books' state | User account login successfully | 1. Login Library App<br>2. Open "Books" tab<br>3. Observe book statuses | Book IDs: `BOOK001`, `BOOK003`, `BOOK007` | `BOOK001` displays `Available`<br>`BOOK003` displays `Borrowed`<br>`BOOK007` displays `Lost` | REQ-02 | EP |
| TC-12 | Check book status after borrowing | User logged in as Active member<br>`BOOK001` is `Available` | 1. Login as Member<br>2. Open "Books" tab<br>3. Borrow `BOOK001`<br>4. Observe book list | Member: `ba.nguyen@email.com`<br>Book ID: `BOOK001` | `BOOK001` status changes from `Available` to `Borrowed` immediately | REQ-02 | EP |
| TC-13 | Check book status after returning | User logged in as Active member<br>`BOOK001` is `Borrowed` | 1. Login as Member<br>2. Open `Borrow / Return` tab<br>3. Return `BOOK001`<br>4. Observe book list | Member: `ba.nguyen@email.com`<br>Book ID: `BOOK001` | `BOOK001` status changes from `Borrowed` to `Available` immediately | REQ-02 | EP |
| TC-14 | Search by book title with result | User account login successfully | 1. Login Library App<br>2. Open "Books" tab<br>3. Observe Search Bar `Search by book title or author...`<br>4. Enter keyword in Search Bar | User account<br>Keyword: `Flutter` | Display book `BOOK001` and its full info | REQ-03 | EP |
| TC-15 | Search by author with result | User account login successfully | 1. Login Library App<br>2. Open "Books" tab<br>3. Observe Search Bar `Search by book title or author...`<br>4. Enter keyword in Search Bar | User account<br>Keyword: `Nguyễn Minh Đức` | Display books `BOOK001`, `BOOK009` and its full info | REQ-03 | EP |
| TC-16 | Search by book title or author with no result | User account login successfully | 1. Login Library App<br>2. Open "Books" tab<br>3. Observe Search Bar `Search by book title or author...`<br>4. Enter keyword in Search Bar | User account<br>Keyword: `XY123` | Display `No books found` | REQ-03 | EP |
| TC-17 | Search by book title or author regardless of UPPERCASE/lowercase | User account login successfully | 1. Login Library App<br>2. Open "Books" tab<br>3. Observe Search Bar `Search by book title or author...`<br>4. Enter keyword in Search Bar | User account<br>Keyword: `FLUTTER`<br>or `flutter` | Display book `BOOK001` and its full info | REQ-03 | EP |
| TC-18 | Filter books by category with result| User account login successfully | 1. Login Library App<br>2. Open "Books" tab<br>3. Observe Filter Bar `Filter by category`<br>4. Enter category in Filter Bar | User account<br>Category: `Kinh tế` | Display books `BOOK007`, `BOOK014`, `BOOK015` and their full info | REQ-03 | EP |
| TC-19 | Filter books by category with no result| User account login successfully | 1. Login Library App<br>2. Open "Books" tab<br>3. Observe Filter Bar `Filter by category`<br>4. Enter keyword in Filter Bar | User account<br>Category: `Tâm lý học` | Display `No books found` | REQ-03 | EP |
| TC-20 | Borrow books successfully | Active member<br>Available book<br>borrowCount < 3 | 1. Login Library App<br>2. View books in book list<br>3. Click (+) button next to `Available` status | Active member account: `ba.nguyen@email.com`, password: `password123`<br>Available book: `BOOK001`| Display `Book borrowed successfully!`<br>`BOOK001` status changes from `Available` to `Borrowed`<br>A borrow record is created with due date = borrow date + 14 days | REQ-04 | EP + Decision Table (R1) |
| TC-21 | Borrow book failed (Book already borrowed) |  `Active` member<br>`Borrowed` book<br>Borrow Count < 3 | 1. Login Library App<br>2. View books in book list<br>3. Display `Borrowed` status only, no (+) button | Active member account: `ba.nguyen@email.com`, password: `password123`<br>Borrowed book: `BOOK003`| `BOOK003` displays status `Borrowed`; the (+) borrow button is not displayed; user cannot borrow this book | REQ-04 | EP + Decision Table (R2) |
| TC-22 | Borrow book failed (Book lost) |  `Active` member<br>`Lost` book<br>Borrow Count < 3 | 1. Login Library App<br>2. View books in book list <br>3. Display `Lost` status only, no (+) button | Active member account: `ba.nguyen@email.com`, password: `password123`<br>Lost book: `BOOK007`| `BOOK007` displays status `Lost`; the (+) borrow button is not displayed; user cannot borrow this book | REQ-04 | EP + Decision Table (R3) |
| TC-23 | Borrow book failed (Suspended member) |  `Suspended` member<br>`Available` book<br>Borrow Count < 3 | 1. Login Library App<br>2. View books in book list <br>3. Click (+) button next to `Available` status | Suspended member account: `cu.le@email.com`, password: `password123`<br>Available book: `BOOK001`| Borrow Failed<br>Display warning `Suspended member. Cannot borrow books!` | REQ-04 | EP + Decision Table (R4) |
| TC-24 | Borrow book failed (Expired member) |  `Expired` member<br>`Available` book<br>Borrow Count < 3 | 1. Login Library App<br>2. View books in book list <br>3. Click (+) button next to `Available` status | Expired member account: `binh.pham@email.com`, password: `password123`<br>Available book: `BOOK001`| Borrow Failed<br>Display warning `Expired member. Cannot borrow books!` | REQ-04 | EP + Decision Table (R5) |
| TC-25 | Borrow book failed (Exceed Limit Borrow Count) |  Active member has already borrowed 3 books | 1. Login Library App<br>2. View books in book list <br>3. Select an `Available` book<br>4. Click (+) button to borrow 4th book |`Active` user account<br>Book: 1 `Available` book | Borrow Failed<br>Display warning `Books borrowed reach its maximum (3 books)` | REQ-04 | BVA + Decision Table (R6) |


---

## Tổng hợp

| Nhóm chức năng | Số TC | REQ phủ | Kỹ thuật IDM áp dụng |
|----------------|-------|---------|----------------------|
| Login | 7 | REQ-01 | EP |
| | | | |
| **Tổng** | **<!-- ≥ 20 -->** | | |
