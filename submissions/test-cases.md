# Test Cases — Bảng trường hợp kiểm thử

> **Hướng dẫn**: Viết tối thiểu **20 TC** phủ đủ các chức năng chính (REQ-01 → REQ-08).
> Xem [examples/sample-test-case.md](../examples/sample-test-case.md) để hiểu cách viết TC tốt.
> Tự tổ chức và phân nhóm test case theo cách hợp lý nhất.

| Thông tin | |
|---|---|
| **Nhóm** | Nhóm 1 |
| **Ngày tạo** | 16/05/2026 |
| **Hệ thống** | https://stqa.rbc.vn |
| **Tham chiếu** | SRS v1.0 |

---

## Bước 1: Mô hình hóa miền đầu vào — Input Domain Modeling (IDM)

### IDM — Đăng nhập (REQ-01)
| Đặc tính (Characteristic) | Phân vùng (Block) | Giá trị đại diện (Value) | Kết quả mong đợi |
|---|---|---|---|
| Email có tồn tại trong DB? | Có | `librarian@library.com` | Đăng nhập thành công |
| | Không | `noone@email.com` | Thông báo lỗi |
| Mật khẩu có đúng? | Đúng | `admin123` | Đăng nhập thành công |
| | Sai | `wrongpass` | Thông báo lỗi |
| Ô nhập có rỗng? | Không rỗng | (giá trị bất kỳ) | Xử lý bình thường |
| | Rỗng | `""` | Thông báo "Vui lòng nhập..." |

### IDM — Tìm kiếm sách (REQ-03)
| Đặc tính (Characteristic) | Phân vùng (Block) | Giá trị đại diện (Value) | Kết quả mong đợi |
|---|---|---|---|
| Từ khóa có tồn tại trong DB? | Có (tên sách) | `"Flutter"` | Hiển thị sách chứa "Flutter" |
| | Có (tên tác giả) | `"Nguyễn"` | Hiển thị sách của tác giả Nguyễn |
| | Không | `"XYZ123"` | Danh sách rỗng |
| Phân biệt HOA/thường? | Chữ thường | `"flutter"` | Kết quả giống "Flutter" |
| | Chữ HOA | `"FLUTTER"` | Kết quả giống "Flutter" |

### IDM & Bảng Quyết Định (Decision Table) — Mượn sách (REQ-04)

**Bảng Quyết Định (Decision Table) cho chức năng Mượn Sách:**

| Điều kiện / Kết quả | R1 | R2 | R3 | R4 |
|---------------------|----|----|----|----|
| **Điều kiện (Conditions)** | | | | |
| C1: Trạng thái sách là "Có sẵn"? | Sai | Đúng | Đúng | Đúng |
| C2: Trạng thái tài khoản là "Hoạt động"? | - | Sai | Đúng | Đúng |
| C3: Số sách đang mượn < 3? | - | - | Sai | Đúng |
| **Kết quả (Actions)** | | | | |
| A1: Thông báo lỗi sách không có sẵn | X | | | |
| A2: Thông báo lỗi tài khoản không hợp lệ | | X | | |
| A3: Thông báo lỗi vượt quá giới hạn mượn | | | X | |
| A4: Cho phép mượn sách thành công | | | | X |

**Bảng IDM bổ sung (cho các thuộc tính khác):**
| Đặc tính (Characteristic) | Phân vùng (Block) | Giá trị đại diện (Value) | Kết quả mong đợi |
|---|---|---|---|
| Trạng thái sách? | Có sẵn | BOOK001 | Cho phép mượn |
| | Đang mượn | BOOK003 | Không cho phép |
| | Thất lạc | BOOK007 | Không cho phép |
| Trạng thái thành viên? | Hoạt động | MEM002 | Cho phép mượn |
| | Tạm ngưng | MEM004 | Từ chối, thông báo lỗi |
| | Hết hạn | MEM005 | Từ chối, thông báo lỗi |
| Số sách đang mượn? | < 3 (BVA: 0, 1, 2) | MEM006 (0 sách) | Cho phép mượn |
| | = 3 (BVA: giới hạn) | MEM đã mượn 3 sách | Từ chối, thông báo vượt giới hạn |

### IDM — Trả sách, Xử lý quá hạn, Quản lý thành viên, Tra cứu (REQ-05 đến REQ-08)
| Đặc tính (Characteristic) | Phân vùng (Block) | Giá trị đại diện (Value) | Kết quả mong đợi |
|---|---|---|---|
| Sách trả có quá hạn? (REQ-05) | Chưa/Đúng hạn | Trả trước hạn | Trả thành công, không cảnh báo |
| | Quá hạn | Trả sau hạn | Trả thành công, có cảnh báo |
| Nhấn "Kiểm tra quá hạn" (REQ-06) | Ngày hiện tại > dueDate | Quét phiếu BR001 | Cập nhật thành "Quá hạn" |
| Định dạng Email mới (REQ-07) | Hợp lệ | `new@gmail.com` | Thêm thành công |
| | Thiếu `@` | `newgmail.com` | Báo lỗi định dạng |
| | Thiếu `.` domain | `new@gmail` | Báo lỗi định dạng |
| Email trùng lặp (REQ-07) | Có | `librarian@library.com` | Báo lỗi đã tồn tại |
| Quyền xem phiếu mượn (REQ-08)| Thành viên xem phiếu mình | MEM002 xem BR001 | Xem thành công |
| | Thủ thư xem tất cả | LIB001 xem mọi phiếu | Xem thành công |

---

## Bước 2: Test Cases

| Mã TC | Mục tiêu kiểm thử | Tiền điều kiện | Bước thực hiện | Dữ liệu đầu vào | Kết quả mong đợi | REQ | Kỹ thuật |
|-------|-------------------|---------------|---------------|-----------------|------------------|-----|---------|
| TC-01 | Đăng nhập thành công với vai trò Thủ thư | Không có | 1. Truy cập trang web<br>2. Nhập Email<br>3. Nhập Password<br>4. Nhấn "Đăng nhập" | Email: `librarian@library.com`<br>Pass: `admin123` | Chuyển đến trang chủ, hiển thị tên "Nguyễn Thủ Thư" và vai trò Thủ thư. | REQ-01 | EP |
| TC-02 | Đăng nhập thành công với Thành viên | Không có | 1. Truy cập trang web<br>2. Nhập Email<br>3. Nhập Password<br>4. Nhấn "Đăng nhập" | Email: `ba.nguyen@email.com`<br>Pass: `password123` | Chuyển đến trang chủ, hiển thị tên "Nguyễn Học Bá" và vai trò Thành viên. | REQ-01 | EP |
| TC-03 | Đăng nhập thất bại do email không tồn tại | Không có | 1. Truy cập trang web<br>2. Nhập Email<br>3. Nhập Password<br>4. Nhấn Đăng nhập | Email: `nobody@test.com`<br>Pass: `admin123` | Hiển thị thông báo: "Không tìm thấy thành viên". | REQ-01 | EP |
| TC-04 | Đăng nhập thất bại do sai mật khẩu | Không có | 1. Nhập Email hợp lệ<br>2. Nhập sai Password<br>3. Nhấn Đăng nhập | Email: `ba.nguyen@email.com`<br>Pass: `wrongpass` | Hiển thị thông báo: "Mật khẩu không đúng". | REQ-01 | EP |
| TC-05 | Đăng nhập thất bại do bỏ trống | Không có | 1. Để trống Email và Password<br>2. Nhấn Đăng nhập | Email: `""`<br>Pass: `""` | Hiển thị thông báo "Vui lòng nhập email và mật khẩu". | REQ-01 | BVA |
| TC-06 | Xem danh sách sách hiển thị đúng dữ liệu | Đăng nhập thành công | 1. Chuyển sang Tab "Sách" | N/A | Danh sách sách hiển thị tên sách, tác giả, thể loại, năm XB, trạng thái. | REQ-02 | EP |
| TC-07 | Tìm kiếm sách theo tên (Có kết quả) | Đã đăng nhập | 1. Nhập từ khóa vào ô tìm kiếm | Từ khóa: `Flutter` | Hiển thị cuốn sách `BOOK001`. | REQ-03 | EP |
| TC-08 | Tìm kiếm sách theo tên tác giả | Đã đăng nhập | 1. Nhập tên tác giả vào ô tìm kiếm | Từ khóa: `Trần Văn Hùng` | Hiển thị cuốn sách `BOOK002`. | REQ-03 | EP |
| TC-09 | Tìm kiếm sách không phân biệt HOA/thường | Đã đăng nhập | 1. Nhập từ khóa viết thường/viết HOA | Từ khóa: `flutter` | Hiển thị cuốn sách `BOOK001` (giống TC-07). | REQ-03 | EP |
| TC-10 | Tìm kiếm sách không có kết quả | Đã đăng nhập | 1. Nhập từ khóa không tồn tại | Từ khóa: `XYZ123` | Hiển thị thông báo "Không tìm thấy sách". | REQ-03 | EP |
| TC-11 | Lọc sách theo thể loại | Đã đăng nhập | 1. Chọn thể loại "Quản trị" từ dropdown | Thể loại: `Quản trị` | Danh sách hiển thị BOOK004, BOOK012. | REQ-03 | EP |
| TC-12 | Mượn sách thành công (Hoạt động, Có sẵn, Mượn < 3) | Đăng nhập MEM006 | 1. Chọn sách Có sẵn<br>2. Nhấn Mượn | Sách: `BOOK001` | Mượn thành công, trạng thái sách chuyển sang "Đã mượn". | REQ-04 | DT |
| TC-13 | Không cho phép mượn sách đã có người mượn | Đăng nhập Thành viên | 1. Chọn sách Đã mượn<br>2. Nhấn Mượn | Sách: `BOOK003` | Nút mượn bị ẩn hoặc thông báo từ chối. | REQ-04 | EP |
| TC-14 | Không cho phép mượn sách bị thất lạc | Đăng nhập Thành viên | 1. Tìm sách Thất lạc<br>2. Nhấn Mượn | Sách: `BOOK007` | Nút mượn bị ẩn hoặc thông báo từ chối. | REQ-04 | EP |
| TC-15 | Thành viên Tạm ngưng không được mượn sách | Đăng nhập MEM004 | 1. Chọn sách Có sẵn<br>2. Nhấn Mượn | Sách: `BOOK001` | Từ chối mượn, thông báo tài khoản bị tạm ngưng. | REQ-04 | DT |
| TC-16 | Thành viên Hết hạn không được mượn sách | Đăng nhập MEM005 | 1. Chọn sách Có sẵn<br>2. Nhấn Mượn | Sách: `BOOK001` | Từ chối mượn, thông báo tài khoản đã hết hạn. | REQ-04 | DT |
| TC-17 | Không cho mượn quá 3 cuốn sách | Đăng nhập MEM002 | 1. Mượn 3 cuốn sách<br>2. Mượn cuốn thứ 4 | Sách: 3 cuốn hợp lệ, mượn cuốn 4 | Từ chối mượn cuốn thứ 4, thông báo vượt giới hạn mượn. | REQ-04 | BVA |
| TC-18 | Trả sách đúng hạn thành công | Đăng nhập MEM006 | 1. Vào Tab Mượn/Trả<br>2. Tìm phiếu đang mượn<br>3. Nhấn Trả sách | Phiếu: `BR003` | Sách trả thành công, chuyển về "Có sẵn". Không cảnh báo. | REQ-05 | EP |
| TC-19 | Trả sách quá hạn hiển thị cảnh báo | Đăng nhập MEM002 | 1. Vào Tab Mượn/Trả<br>2. Tìm phiếu quá hạn<br>3. Nhấn Trả | Phiếu quá hạn | Sách trả thành công nhưng có cảnh báo quá hạn. | REQ-05 | BVA |
| TC-20 | Thủ thư cập nhật trạng thái Quá hạn | Đăng nhập Thủ thư | 1. Vào Mượn/Trả<br>2. Nhấn "Kiểm tra quá hạn" | N/A | Phiếu BR001 chuyển từ Đang mượn sang Quá hạn. | REQ-06 | DT |
| TC-21 | Thủ thư thêm thành viên hợp lệ | Đăng nhập Thủ thư | 1. Tab Thành viên<br>2. Nhấn Thêm mới<br>3. Nhập dữ liệu | Email: `newuser@gmail.com`<br>Tên, SDT | Thành viên mới được thêm vào danh sách. | REQ-07 | EP |
| TC-22 | Thủ thư thêm thành viên thất bại do sai định dạng | Đăng nhập Thủ thư | 1. Tab Thành viên<br>2. Nhập Email không hợp lệ | Email: `new@gmail` | Hiển thị lỗi định dạng email. | REQ-07 | EP |
| TC-23 | Thủ thư thêm thành viên thất bại do trùng Email | Đăng nhập Thủ thư | 1. Nhập Email đã tồn tại | Email: `librarian@library.com` | Hiển thị lỗi email đã tồn tại. | REQ-07 | EP |
| TC-24 | Thành viên chỉ xem được phiếu mượn của mình | Đăng nhập MEM002 | 1. Vào Mượn/Trả | N/A | Chỉ thấy các phiếu BR001, BR004. Không thấy BR002. | REQ-08 | EP |
| TC-25 | Thủ thư xem được tất cả phiếu mượn | Đăng nhập Thủ thư | 1. Vào Mượn/Trả | N/A | Hiển thị danh sách tất cả phiếu mượn của các thành viên. | REQ-08 | EP |

---

## Tổng hợp

| Nhóm chức năng | Số TC | REQ phủ | Kỹ thuật IDM áp dụng |
|----------------|-------|---------|----------------------|
| Đăng nhập | 5 | REQ-01 | EP, BVA |
| Tìm kiếm, lọc | 6 | REQ-02, REQ-03 | EP |
| Mượn, trả, quá hạn | 9 | REQ-04, REQ-05, REQ-06| EP, BVA, Decision Table (DT) |
| Quản lý thành viên, Tra cứu| 5 | REQ-07, REQ-08 | EP |
| **Tổng** | **25** | **8 REQ** | **EP, BVA, Decision Table** |
