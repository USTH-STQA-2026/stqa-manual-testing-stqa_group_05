# Test Execution — Kết quả thực thi kiểm thử

> **Hướng dẫn**: Chạy từng TC trên hệ thống [https://stqa.rbc.vn](https://stqa.rbc.vn/), ghi lại kết quả thực tế.
Kết luận: **Pass** (kết quả đúng), **Fail** (kết quả sai → tạo bug report), **Blocked** (không thực hiện được vì lỗi khác chặn), **Not Run** (chưa chạy).
> 

| Thông tin |  |
| --- | --- |
| **Nhóm** | stqa-manual-testing-stqa_group_05 |
| **Ngày thực thi** | 18/05/2026 |
| **Trình duyệt** | Brave: 1.90.121 Chromium: 148.0.7778.96 (Official Build) (64-bit)  |
| **Hệ điều hành** | Arch Linux BTW |

---

## Kết quả chi tiết

| **Mã TC** | **Nhóm chức năng** | **Kết quả mong đợi (tóm tắt)** | **Kết quả thực tế** | **Kết luận** |
| --- | --- | --- | --- | --- |
| TC-01 | Đăng nhập | Chuyển sang trang chủ. AppBar hiển thị tên và vai trò "Thủ thư" | Đăng nhập thành công, hiển thị đúng tên "Nguyễn Thủ Thư" và vai trò trên AppBar | **Pass** |
| TC-02 | Đăng nhập | Chuyển sang trang chủ. AppBar hiển thị tên và vai trò "Thành viên" | Đăng nhập thành công, hiển thị đúng tên "Nguyễn Học Bá" và vai trò "Thành viên" | **Pass** |
| TC-03 | Đăng nhập | Chuyển sang trang chủ. Trên AppBar hiển thị tên "Lê Cần Cù" và vai trò "Thành viên". Không báo lỗi | Đăng nhập thành công, hiển thị đúng tên "Lê Cần Cù" và không bị chặn | **Pass** |
| TC-04 | Đăng nhập | Thông báo lỗi: "Không tìm thấy thành viên". Không chuyển trang | Hệ thống chặn đăng nhập và hiển thị chính xác thông báo "Không tìm thấy thành viên" | **Pass** |
| TC-05 | Đăng nhập | Thông báo lỗi: "Mật khẩu không đúng". Không chuyển trang | Hệ thống chặn đăng nhập và hiển thị chính xác thông báo "Mật khẩu không đúng" | **Pass** |
| TC-06 | Đăng nhập | Thông báo lỗi: "Vui lòng nhập email và mật khẩu". Không chuyển trang | Hệ thống không cho đăng nhập và báo lỗi nhập thiếu thông tin | **Pass** |
| TC-07 | Đăng nhập | Thông báo lỗi: "Vui lòng nhập email và mật khẩu". Không chuyển trang | Hệ thống không cho đăng nhập và báo lỗi nhập thiếu thông tin | **Pass** |
| TC-08 | Đăng nhập | Thông báo lỗi: "Vui lòng nhập email và mật khẩu". Không chuyển trang | Hệ thống không cho đăng nhập và báo lỗi nhập thiếu thông tin | **Pass** |
|  |  |  |  |  |
| TC-71 | Quản lý thành viên | Thông báo thành công, thành viên mới hiển thị | Form xử lý thất bại, hiển thị “Email không hợp lệ” | Fail |
| TC-72 | Quản lý thành viên | Thành viên không thấy chức năng thêm thành viên mới, xem các thành viên | Form xử lý thất bại, hiển thị “Email không hợp lệ” | Fail |
| TC-73 | Quản lý thành viên | Báo lỗi email không hợp lệ (thiếu `@`) | Form xử lý thất bại, hiển thị “Email không hợp lệ” | Fail |
| TC-74 | Quản lý thành viên | Báo lỗi email đã tồn tại | Form xử lý thất bại, hiển thị “Email không hợp lệ” | Fail |
| TC-75 | Quản lý thành viên | Báo lỗi SĐT không hợp lệ (sai đầu số) | Form xử lý thất bại, hiển thị “Email không hợp lệ” | Fail |
| TC-76 | Quản lý thành viên | Báo lỗi SĐT không hợp lệ (thiếu số) | Form xử lý thất bại, hiển thị “Email không hợp lệ” | Fail |
| TC-77 | Quản lý thành viên | Báo lỗi yêu cầu điền đầy đủ thông tin | Form xử lý thất bại, hiển thị “Email không hợp lệ” | Fail |

---

## Tổng hợp kết quả

| Chỉ số | Giá trị |
| --- | --- |
| Tổng số test case | 15 |
| Pass | 8 |
| Fail | 7 |
| Blocked | 0 |
| Not Run | 0 |
| **Tỷ lệ Pass** | % |

### Kết quả theo nhóm chức năng

| Nhóm | Tổng TC | Pass | Fail | Tỷ lệ Pass |
| --- | --- | --- | --- | --- |
| Đăng nhập (REQ-01) | 8 | 8 | 0 | 100% |
| Quản lý Thành viên (REQ-07) | 7 | 0 | 7 | 0% |
