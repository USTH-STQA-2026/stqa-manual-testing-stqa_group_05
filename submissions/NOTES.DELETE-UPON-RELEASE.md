# **Đây là ghi chú nội bộ, cần xoá khi gộp vào main**

Branch A: draft
Branch B: mchn

Dựa vào quá trình phân tích và gộp dữ liệu từ hai nhánh, có một số phần **không thể gộp tự động** hoặc **chỉ gộp được một nửa** do thiếu dữ liệu hoặc xung đột logic. Những phần này bắt buộc phải có sự kiểm tra lại (audit) từ tác giả của nhánh B hoặc sự thống nhất của cả nhóm.

Dưới đây là các phần cần audit chi tiết:

### 1. File `bug-reports.md` và `summary.md` của nhánh B đang trống (Chỉ chứa Template)

* **Vấn đề:** Các file `bug-reports.B.md` và `summary.B.md` dường như chưa được làm hoặc chỉ toàn chứa các thẻ giữ chỗ (placeholder) như `hay`. Không có dữ liệu thực tế nào về Bug hay Đánh giá chất lượng từ nhánh B để gộp sang nhánh A.
* **Cần Audit:** Tác giả nhánh B cần kiểm tra lại xem họ đã viết Bug Report và Summary ở file/nơi khác chưa. Nếu nhánh B có phát hiện lỗi trong quá trình test thì phải bổ sung thủ công các lỗi đó vào `bug-reports.md` hiện tại.

### 2. Kết quả thực thi (Test Execution) của các Test Case lấy từ nhánh B

* **Vấn đề:** Tôi đã lấy các Test Case mới/hay từ nhánh B gộp vào nhánh A (như TC-29, 30, 31). Tuy nhiên, do file `test-execution.B.md` của nhánh B chỉ là bảng trống, tôi không biết kết quả thực tế khi chạy các case này trên hệ thống là Pass, Fail hay Blocked.
* **Cần Audit:** Tác giả nhánh B cần vào file `test-execution.md` mới, tìm các dòng có trạng thái **"Not Run"** (TC-29 đến TC-31) để chạy lại trên hệ thống và điền kết quả (Actual Result + Status) thực tế. Nếu Fail thì phải viết thêm Bug Report.

### 3. Xung đột về "Kết quả mong đợi" (Expected Result) ở chức năng Mượn sách

* **Vấn đề:** Cả hai nhánh đều test các luồng lỗi của tính năng Mượn sách (REQ-04) như: User bị Suspended (đình chỉ), User bị Expired (hết hạn), mượn quá 3 cuốn.
* **Nhánh A** ghi nhận thực tế hệ thống đang lỗi (Bug-001, Bug-002).
* **Nhánh B** (ví dụ TC-23, TC-24, TC-25 của B) lại ghi rõ thông báo kỳ vọng bằng text cụ thể (VD: `"Suspended member. Cannot borrow books!"`).

* **Cần Audit:** Nhóm cần xem lại tài liệu gốc (SRS) để chốt xem **câu thông báo lỗi chuẩn** xác đáng lẽ ra phải là gì? Nếu nhánh B đúng, cần cập nhật lại thông tin "Expected Result" trong file `bug-reports.md` (Bug-002) của nhánh A cho chuẩn xác.

### 4. Đồng bộ ngôn ngữ (Tiếng Việt -> Tiếng Anh)

* **Vấn đề:** Nhánh A dùng tiếng Anh, nhánh B dùng tiếng Việt. Vì lấy nhánh A làm chuẩn, tôi đã phải tự động dịch các kịch bản của nhánh B sang tiếng Anh (ví dụ bảng Thống kê tổng hợp hoặc mô tả Test Case).
* **Cần Audit:** Tác giả nhánh B cần đọc qua lại các TC mới được thêm vào (TC-29, 30, 31) xem quá trình dịch thuật có làm sai lệch ý đồ test hay "Expected Result" ban đầu của bạn hay không.

**Tóm lại:** Việc cấp bách nhất bây giờ là **gọi tác giả nhánh B** vào xác nhận xem tại sao các file Report/Execution của họ lại trống, và nhờ họ điền kết quả cho các case "Not Run" trong file Execution mới.
