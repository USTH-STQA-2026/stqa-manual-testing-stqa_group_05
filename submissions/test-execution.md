# Test Execution — Kết quả thực thi

> **Hướng dẫn**: Chạy 20+ TC đã viết trên hệ thống https://stqa.rbc.vn và ghi nhận kết quả vào đây.

| Mã TC | Trạng thái (Pass/Fail/Block) | Kết quả thực tế (Actual Result) | Ghi chú / Link Bug |
|-------|------------------------------|---------------------------------|--------------------|
| TC-01 | Pass | Đăng nhập thành công vào vai trò Thủ thư | |
| TC-02 | Pass | Đăng nhập thành công vào vai trò Thành viên | |
| TC-03 | Pass | Hiển thị lỗi "Không tìm thấy thành viên." | |
| TC-04 | Pass | Hiển thị lỗi "Mật khẩu không đúng." | |
| TC-05 | Pass | Hiển thị lỗi "Vui lòng nhập email và mật khẩu" bên dưới trường nhập | |
| TC-06 | Pass | Danh sách sách hiển thị đầy đủ thông tin | |
| TC-07 | Pass | Hiển thị đúng cuốn BOOK001 | |
| TC-08 | Pass | Hiển thị đúng cuốn BOOK002 | |
| TC-09 | Pass | Tìm kiếm với từ khóa "FLUTTER" vẫn ra BOOK001 (Không phân biệt HOA/thường) | |
| TC-10 | Pass | Hiển thị "Không tìm thấy sách" | |
| TC-11 | Pass | Chỉ hiển thị sách thuộc thể loại Quản trị | |
| TC-12 | Pass | Mượn thành công, thông báo màu xanh hiển thị dưới màn hình | |
| TC-13 | Pass | Sách Đã mượn không có nút mượn (hoặc bị disable) | |
| TC-14 | Pass | Sách Thất lạc không có nút mượn | |
| TC-15 | **Fail** | **Hệ thống từ chối mượn sách (đúng), nhưng hiển thị thông báo sai: "Thành viên đã hết hạn. Không thể mượn sách." thay vì thông báo về trạng thái Tạm ngưng.** | [BUG-002] |
| TC-16 | Pass | Bị từ chối mượn, có thông báo lỗi "Thành viên đã hết hạn. Không thể mượn sách." | |
| TC-17 | **Fail** | **Hệ thống vẫn cho phép mượn cuốn sách thứ 4 dù đã đạt giới hạn 3 cuốn. Hiển thị "Mượn sách thành công!"** | [BUG-001] |
| TC-18 | Pass | Trả sách thành công, sách chuyển về trạng thái Có sẵn | |
| TC-19 | **Fail** | **Trả phiếu quá hạn BR001 thành công nhưng hệ thống chỉ hiển thị "Trả sách thành công.", không có cảnh báo quá hạn như REQ-05 yêu cầu.** | [BUG-006] |
| TC-20 | Pass | Hệ thống quét và tự cập nhật các phiếu đang mượn đã quá hạn, gồm BR001 và BR003, sang trạng thái Quá hạn | |
| TC-21 | **Fail** | **Nhập email hợp lệ `testnewuser99@gmail.com` nhưng hệ thống báo "Email không hợp lệ." — Không thể thêm thành viên mới với bất kỳ email nào.** | [BUG-003] |
| TC-22 | **Fail** | **Hệ thống chấp nhận email `new@gmail` (thiếu `.` trong domain) và tạo thành viên thành công với mã MEM007. Không có cảnh báo lỗi nào.** | [BUG-003] |
| TC-23 | **Fail** | **Nhập email đã tồn tại `librarian@library.com` nhưng hệ thống báo "Email không hợp lệ." thay vì báo lỗi email đã tồn tại.** | [BUG-003] |
| TC-24 | Pass | Thành viên chỉ nhìn thấy phiếu mượn của chính mình | |
| TC-25 | Pass | Thủ thư nhìn thấy toàn bộ phiếu mượn của hệ thống | |
| TC-26 | Pass | Sau khi MEM003 mượn BOOK002, trạng thái sách chuyển sang "Đã mượn" ngay lập tức trên danh sách mà không cần tải lại trang. | |
| TC-27 | **Fail** | **Thành viên MEM002 tra cứu mã MEM006 và hệ thống hiển thị phiếu BR003 của MEM006, gồm thông tin sách, ngày mượn, hạn trả và nút Trả sách.** | [BUG-004] |
| TC-28 | **Fail** | **Thành viên MEM002 trả được phiếu BR003 của MEM006. Hệ thống báo "Trả sách thành công", phiếu chuyển sang "Đã trả" và có ngày trả 19/05/2026.** | [BUG-005] |
