# Bug Reports — Báo cáo lỗi

> **Hướng dẫn**: Với mỗi TC bị **Fail** ở bước thực thi, hãy viết một Bug Report hoàn chỉnh vào file này. Xem [examples/sample-bug-report.md](../examples/sample-bug-report.md) để biết cách viết.

---

## BUG-001: Thành viên có thể mượn quá giới hạn 3 cuốn sách cùng lúc

**1. Thông tin chung**
- **Test Case bị Fail**: TC-17
- **Yêu cầu (REQ) liên quan**: REQ-04 (Giới hạn mượn sách)
- **Mức độ nghiêm trọng (Severity)**: High
  *(Giải thích: Lỗi này vi phạm trực tiếp luồng nghiệp vụ cốt lõi, khiến thành viên có thể gom hết sách của thư viện, ảnh hưởng nghiêm trọng đến vận hành)*
- **Môi trường**: Chrome / Windows (https://stqa.rbc.vn)

**2. Các bước tái hiện (Steps to Reproduce)**
1. Mở trình duyệt và truy cập hệ thống tại `https://stqa.rbc.vn`.
2. Đăng nhập bằng tài khoản Thành viên `ba.nguyen@email.com` (mật khẩu: `password123`).
3. Xác nhận ở Tab "Mượn/Trả" rằng thành viên này đã có sẵn 1 cuốn sách đang mượn (BR001 — BOOK003).
4. Chuyển sang Tab "Sách", mượn thêm 2 cuốn sách có sẵn (VD: BOOK001, BOOK002) để đạt tổng 3 cuốn.
5. Tiếp tục nhấn nút (+) để mượn thêm cuốn thứ 4 (Ví dụ: `BOOK004` hoặc `BOOK005`).
6. Trong hộp thoại xác nhận, nhấn "Mượn".

**3. Kết quả thực tế (Actual Result)**
Hệ thống hiển thị thông báo **"Mượn sách thành công!"** (màu xanh) và thêm cuốn sách thứ 4 vào danh sách đang mượn của thành viên. Không có bất kỳ cảnh báo hay ngăn chặn nào từ hệ thống.

![Minh chứng BUG-001: Mượn thành công cuốn sách thứ 4](bug001_proof.png)

**4. Kết quả mong đợi (Expected Result)**
Theo REQ-04, hệ thống phải từ chối yêu cầu mượn cuốn thứ 4 và hiển thị thông báo lỗi vượt quá giới hạn 3 cuốn sách.

**5. Đề xuất / Khuyến nghị (Recommendation)**
Logic điều khiển cần thực hiện kiểm tra `currentBorrowedBooksCount >= 3` trước khi thực thi hàm mượn sách. Nút bấm Mượn sách cũng nên bị vô hiệu hóa (disabled) nếu người dùng đã đạt giới hạn.

---

## BUG-002: Thông báo lỗi sai khi tài khoản "Tạm ngưng" cố gắng mượn sách

**1. Thông tin chung**
- **Test Case bị Fail**: TC-15
- **Yêu cầu (REQ) liên quan**: REQ-04 (Thông báo lỗi phải mô tả đúng lý do từ chối: "tạm ngưng ≠ hết hạn")
- **Mức độ nghiêm trọng (Severity)**: Medium
  *(Giải thích: Chức năng ngăn chặn mượn sách hoạt động đúng, nhưng thông báo sai gây hiểu lầm cho người dùng về trạng thái tài khoản của họ. Vi phạm yêu cầu cụ thể trong SRS REQ-04.)*
- **Môi trường**: Chrome / Windows (https://stqa.rbc.vn)

**2. Các bước tái hiện (Steps to Reproduce)**
1. Mở trình duyệt và truy cập hệ thống tại `https://stqa.rbc.vn`.
2. Đăng nhập bằng tài khoản Thành viên **Tạm ngưng**: `cu.le@email.com` (mật khẩu: `password123`).
3. Xác nhận đăng nhập thành công (vào được trang chính).
4. Chuyển sang Tab "Sách".
5. Nhấn nút (+) để mượn một cuốn sách có trạng thái "Có sẵn" (Ví dụ: `BOOK001`).
6. Trong hộp thoại xác nhận, nhấn "Mượn".

**3. Kết quả thực tế (Actual Result)**
Hệ thống từ chối mượn sách (đúng), nhưng hiển thị thông báo lỗi:
> **"Thành viên đã hết hạn. Không thể mượn sách."**

Đây là thông báo dành cho tài khoản **Hết hạn (MEM005)**, không phải cho tài khoản **Tạm ngưng (MEM004)**. Hai trạng thái này hoàn toàn khác nhau về mặt nghiệp vụ.

![Minh chứng BUG-002: Thông báo "hết hạn" sai khi tài khoản bị "tạm ngưng"](bug002_proof.png)

**4. Kết quả mong đợi (Expected Result)**
Theo SRS REQ-04: *"Thông báo lỗi phải mô tả **đúng lý do** từ chối (tạm ngưng ≠ hết hạn)"*.
Hệ thống phải hiển thị thông báo phân biệt rõ trạng thái, ví dụ: **"Tài khoản đang bị tạm ngưng. Không thể mượn sách."**

**5. Đề xuất / Khuyến nghị (Recommendation)**
Kiểm tra lại logic hiển thị thông báo lỗi cho chức năng mượn sách. Cần phân tách điều kiện kiểm tra `status == "suspended"` (tạm ngưng) và `status == "expired"` (hết hạn) để trả về đúng nội dung thông báo tương ứng.

---

## BUG-003: Thủ thư không thể thêm thành viên mới do validation email bị lỗi

**1. Thông tin chung**
- **Test Case bị Fail**: TC-21
- **Yêu cầu (REQ) liên quan**: REQ-07 (Quản lý thành viên — Thêm thành viên mới)
- **Mức độ nghiêm trọng (Severity)**: Critical
  *(Giải thích: Lỗi này hoàn toàn chặn chức năng "Thêm thành viên mới" — chức năng cốt lõi của Thủ thư. Không có thành viên mới nào có thể được đăng ký vào hệ thống, ảnh hưởng trực tiếp đến hoạt động của thư viện.)*
- **Môi trường**: Chrome / Windows (https://stqa.rbc.vn)

**2. Các bước tái hiện (Steps to Reproduce)**
1. Mở trình duyệt và truy cập hệ thống tại `https://stqa.rbc.vn`.
2. Đăng nhập bằng tài khoản Thủ thư: `librarian@library.com` (mật khẩu: `admin123`).
3. Chuyển sang Tab **"Thành viên"**.
4. Nhấn nút **"+"** (Thêm thành viên) ở góc trên bên phải.
5. Hộp thoại "Thêm thành viên mới" xuất hiện. Điền đầy đủ thông tin:
   - Họ và tên: `Nguyễn Test`
   - Email: `testnewuser99@gmail.com`
   - Số điện thoại: `0901234567`
6. Nhấn nút **"Thêm thành viên"** (Submit).

**3. Kết quả thực tế (Actual Result)**
Hệ thống **từ chối** yêu cầu và hiển thị thông báo lỗi màu đỏ ngay dưới ô nhập email:
> **"Email không hợp lệ."**

Địa chỉ email `testnewuser99@gmail.com` hoàn toàn hợp lệ theo mọi tiêu chuẩn (có `@`, có dấu `.` trong phần domain). Lỗi này tái hiện với **mọi địa chỉ email hợp lệ** được nhập vào form, khiến chức năng thêm thành viên **hoàn toàn không thể sử dụng được**.

![Minh chứng BUG-003: Email hợp lệ nhưng bị báo lỗi "Email không hợp lệ"](bug003_proof.png)

**4. Kết quả mong đợi (Expected Result)**
Theo SRS REQ-07, email hợp lệ (có `@` VÀ có dấu `.` trong phần domain, ví dụ `user@domain.com`) phải được chấp nhận. Thành viên mới phải được tạo thành công và xuất hiện trong danh sách thành viên.

**5. Đề xuất / Khuyến nghị (Recommendation)**
Kiểm tra lại biểu thức chính quy (regex) hoặc hàm xác thực email trong form "Thêm thành viên". Regex hiện tại bị cấu hình sai (quá nghiêm ngặt hoặc logic bị đảo ngược), dẫn đến việc từ chối mọi đầu vào. Nên sử dụng regex chuẩn RFC 5322 hoặc package kiểm tra email được cập nhật đúng phiên bản.

---
