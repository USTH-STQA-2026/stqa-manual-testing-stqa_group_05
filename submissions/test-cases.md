## REQ-01: Đăng nhập — Login

### Bước 1: IDM (Input Domain Modeling)

**EP (Equivalence Partitioning)**

**BVA (Boundary Value Analysis)**



| Đặc tính (Characteristic) | Phân vùng (Block) | Giá trị đại diện (Value) | Kết quả mong đợi |
| --- | --- | --- | --- |
| Email có tồn tại trong hệ thống? | Có — Thủ thư | `librarian@library.com` | Đăng nhập thành công |
|  | Có — Thành viên | `ba.nguyen@email.com` | Đăng nhập thành công |
|  | Không tồn tại | `noone@email.com` | "Không tìm thấy thành viên" |
| Trạng thái tài khoản khi đăng nhập | Hoạt động | `ba.nguyen@email.com` | Đăng nhập thành công |
|  | **Tạm ngưng / Hết hạn** | `cu.le@email.com` (Tạm ngưng) | **Đăng nhập thành công** |
| Ô email có rỗng? | Rỗng | `""` | "Vui lòng nhập email và mật khẩu" |
| Mật khẩu có đúng? | Đúng | `admin123` | Đăng nhập thành công |
|  | Sai | `wrongpass` | "Mật khẩu không đúng" |
| Ô mật khẩu có rỗng? | Rỗng | `""` | "Vui lòng nhập email và mật khẩu" |
| Cả hai ô có rỗng? | Cả hai rỗng | `""` + `""` | "Vui lòng nhập email và mật khẩu" |
| Sau đăng nhập thành công | Hiển thị thông tin | — | Tên + vai trò hiện trên AppBar |
### Bước 2: Test Cases cho REQ-01
| Mã TC | Mục tiêu kiểm thử | Tiền điều kiện | Bước thực hiện | Dữ liệu đầu vào | Kết quả mong đợi | REQ | Kỹ thuật |
| --- | --- | --- | --- | --- | --- | --- | --- |
| TC-01 | Đăng nhập thành công với tài khoản Thủ thư | Hệ thống mở tại màn hình đăng nhập | 1. Nhập email `librarian@library.com` vào ô Email 2. Nhập `admin123` vào ô Mật khẩu 3. Nhấn nút Đăng nhập | Email: `librarian@library.com` Password: `admin123` | Chuyển sang trang chủ. AppBar hiển thị tên và vai trò "Thủ thư" | REQ-01 | EP |
| TC-02 | Đăng nhập thành công với tài khoản Thành viên đang Hoạt động | Hệ thống mở tại màn hình đăng nhập | 1. Nhập email `ba.nguyen@email.com` 2. Nhập `password123` 3. Nhấn Đăng nhập | Email: `ba.nguyen@email.com` Password: `password123` | Chuyển sang trang chủ. AppBar hiển thị tên và vai trò "Thành viên" | REQ-01 | EP |
| TC-03 | Đăng nhập thành công với tài khoản bị Tạm ngưng/Hết hạn | Mở trang chủ hệ thống | 1. Nhập email của tài khoản Tạm ngưng.
2. Nhập mật khẩu đúng.
3. Nhấn nút "Đăng nhập". | Email: `cu.le@email.com`
Mật khẩu: `password123` | Chuyển sang trang chủ. Trên AppBar hiển thị tên "Lê Cần Cù" và vai trò "Thành viên". Không có thông báo lỗi đăng nhập. | REQ-01 | EP |
| TC-04 | Đăng nhập với email không tồn tại | Hệ thống mở tại màn hình đăng nhập | 1. Nhập email không có trong hệ thống 2. Nhập mật khẩu bất kỳ 3. Nhấn Đăng nhập | Email: `noone@email.com` Password: `abc123` | Thông báo lỗi: **"Không tìm thấy thành viên"**. Không chuyển trang. | REQ-01 | EP |
| TC-05 | Đăng nhập với mật khẩu sai | Hệ thống mở tại màn hình đăng nhập | 1. Nhập email hợp lệ 2. Nhập mật khẩu sai 3. Nhấn Đăng nhập | Email: `librarian@library.com` Password: `wrongpass` | Thông báo lỗi: **"Mật khẩu không đúng"**. Không chuyển trang. | REQ-01 | EP |
| TC-06 | Đăng nhập với ô email bỏ trống | Hệ thống mở tại màn hình đăng nhập | 1. Để trống ô Email 2. Nhập mật khẩu bất kỳ 3. Nhấn Đăng nhập | Email: `""` Password: `admin123` | Thông báo lỗi: **"Vui lòng nhập email và mật khẩu"**. Không chuyển trang. | REQ-01 | EP |
| TC-07 | Đăng nhập với ô mật khẩu bỏ trống | Hệ thống mở tại màn hình đăng nhập | 1. Nhập email hợp lệ 2. Để trống ô Mật khẩu 3. Nhấn Đăng nhập | Email: `librarian@library.com` Password: `""` | Thông báo lỗi: **"Vui lòng nhập email và mật khẩu"**. Không chuyển trang. | REQ-01 | EP |
| TC-08 | Đăng nhập khi cả hai ô đều bỏ trống | Hệ thống mở tại màn hình đăng nhập | 1. Để trống cả ô Email lẫn ô Mật khẩu 2. Nhấn Đăng nhập | Email: `""` Password: `""` | Thông báo lỗi: **"Vui lòng nhập email và mật khẩu"**. Không chuyển trang. | REQ-01 | EP |

