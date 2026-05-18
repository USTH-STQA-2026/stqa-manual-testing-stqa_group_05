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
| TC-19 | Pass | Trả sách thành công, có thông báo cảnh báo quá hạn | |
| TC-20 | Pass | Hệ thống quét và tự cập nhật phiếu BR001 sang trạng thái Quá hạn | |
| TC-21 | **Fail** | **Nhập email hợp lệ `testnewuser99@gmail.com` nhưng hệ thống báo "Email không hợp lệ." — Không thể thêm thành viên mới với bất kỳ email nào.** | [BUG-003] |
| TC-22 | Pass | Báo lỗi định dạng email khi thiếu dấu `.` (lỗi này đúng theo SRS) | |
| TC-23 | Pass | Báo lỗi trùng email khi dùng email của Thủ thư | |
| TC-24 | Pass | Thành viên chỉ nhìn thấy phiếu mượn của chính mình | |
| TC-25 | Pass | Thủ thư nhìn thấy toàn bộ phiếu mượn của hệ thống | |
| TC-26 | Pass | Sau khi MEM003 mượn BOOK002, trạng thái sách chuyển sang "Đã mượn" ngay lập tức trên danh sách mà không cần tải lại trang. | |
