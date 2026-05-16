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
| TC-15 | Pass | Bị từ chối mượn, có thông báo lỗi trạng thái Tạm ngưng | |
| TC-16 | Pass | Bị từ chối mượn, có thông báo lỗi trạng thái Hết hạn | |
| TC-17 | **Fail** | **Hệ thống vẫn cho phép mượn cuốn sách thứ 4 dù đã đạt giới hạn 3 cuốn.** | [BUG-001] |
| TC-18 | Pass | Trả sách thành công, sách chuyển về trạng thái Có sẵn | |
| TC-19 | Pass | Trả sách thành công, có thông báo cảnh báo quá hạn | |
| TC-20 | Pass | Hệ thống quét và tự cập nhật phiếu BR001 sang trạng thái Quá hạn | |
| TC-21 | Pass | Thêm thành viên thành công | |
| TC-22 | Pass | Báo lỗi định dạng email khi thiếu dấu `.` | |
| TC-23 | Pass | Báo lỗi trùng email khi dùng email của Thủ thư | |
| TC-24 | Pass | Thành viên chỉ nhìn thấy phiếu mượn của chính mình | |
| TC-25 | Pass | Thủ thư nhìn thấy toàn bộ phiếu mượn của hệ thống | |
