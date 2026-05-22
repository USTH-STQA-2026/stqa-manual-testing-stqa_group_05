# BẢN GHI CHÚ KIỂM TOÁN HỢP NHẤT (MERGE AUDIT NOTES)

**Áp dụng cho việc gộp Nhánh B vào Nhánh A (Tệp tin: `test-cases.md`)**

> 📑 **Mục đích:** Ghi nhận các điểm xung đột logic, sự thiếu hụt dữ liệu thực thi và các hạng mục bắt buộc phải rà soát thủ công (Audit) bởi tác giả nhánh B và Đội ngũ Phát triển trước khi tiến hành gộp (Merge) vào nhánh chính (`main`).

---

### 1. Giải trình về việc Tối ưu hóa số lượng Test Cases (45 TC giảm còn 33 TC)

* **Vấn đề:** Trong tệp gốc, nhánh B có 45 TC và nhánh A có 28 TC. Sau khi hợp nhất, tổng số ca kiểm thử được chuẩn hóa thành **33 TC**.
* **Nguyên nhân:** * **Khử trùng lặp (Deduplication):** Do cả hai nhánh cùng kiểm thử một hệ thống (`https://stqa.rbc.vn`) theo tài liệu SRS v1.0, các kịch bản cốt lõi (Đăng nhập, Tìm kiếm, Luồng mượn sách cơ bản) bị trùng lặp hoàn toàn về mục tiêu kiểm thử.
* **Gom nhóm ca kiểm thử biên:** Các ca kiểm thử biên chi tiết của nhánh B (như tách nhỏ 5-6 trường hợp lỗi định dạng Email hoặc để trống từng trường đơn lẻ khi Đăng nhập/Thêm thành viên) đã được gom lại thành các ca kiểm thử đại diện (áp dụng kỹ thuật Phân vùng tương đương - EP và Phân tích giá trị biên - BVA) để tối ưu hiệu suất chạy test mà không làm giảm độ phủ yêu cầu (REQ-01 $\rightarrow$ REQ-08).

* **Hạng mục cần Audit:** Tác giả nhánh B cần rà soát lại danh sách từ **TC-01 đến TC-33** trong file đã gộp để đảm bảo không có ý đồ kiểm thử đặc biệt nào bị bỏ sót trong quá trình tinh gọn.

### 2. Thiếu hụt dữ liệu Báo cáo lỗi (`bug-reports.md`) và Tổng hợp (`summary.md`) từ Nhánh B

* **Vấn đề:** Qua đối chiếu, hai tệp tin `bug-reports.B.md` và `summary.B.md` của nhánh B hiện tại hoàn toàn trống (chỉ chứa các thẻ giữ chỗ/template của hệ thống, không có nội dung thực tế). Nhánh A hiện đang lưu vết 6 lỗi nghiêm trọng (từ Bug-001 đến Bug-006).
* **Hạng mục cần Audit:** Tác giả nhánh B cần xác nhận lại xem đã ghi nhận lỗi (Bug) ở một tệp tin hoặc nền tảng quản lý khác chưa. Nếu nhánh B có phát hiện lỗi trong quá trình thực thi, bắt buộc phải bổ sung thủ công các báo cáo lỗi đó vào tệp `bug-reports.md` chung của dự án.

### 3. Cập nhật kết quả thực thi (Test Execution) cho các ca kiểm thử mới

* **Vấn đề:** Tệp `test-execution.B.md` của nhánh B là tệp trống. Khi gộp các ca kiểm thử biên chi tiết của nhánh B vào bộ TC chung (đặc biệt là các ca kiểm tra điều kiện trống trường dữ liệu và định dạng ở REQ-01 và REQ-07), chúng tôi chưa có dữ liệu thực tế hệ thống trả về cho các case này là **Pass**, **Fail** hay **Blocked**.
* **Hạng mục cần Audit:** Tác giả nhánh B cần phối hợp kiểm thử lại các kịch bản bổ sung này trên hệ thống live, sau đó cập nhật trạng thái thực tế (Actual Result & Status) vào tệp `test-execution.md` chung. Nếu xuất hiện hành vi sai lệch so với tài liệu, cần log bổ sung Bug Report ngay lập tức.

### 4. Xung đột về "Kết quả mong đợi" (Expected Result) đối với Thông báo lỗi

* **Vấn đề:** Có sự bất đồng bộ về nội dung chuỗi thông báo lỗi (Error Message Strings) giữa hai nhánh, rõ nhất ở tính năng Mượn sách (REQ-04) khi tài khoản bị Đình chỉ (Suspended) hoặc Hết hạn (Expired):
* **Nhánh B:** Kỳ vọng hệ thống hiển thị text thông báo cụ thể (Ví dụ: `“Suspended member. Cannot borrow books!”`).
* **Nhánh A:** Ghi nhận thực tế hệ thống đang trả về sai thông báo (Bug-002 - Tài khoản bị đình chỉ nhưng hệ thống lại báo là hết hạn).

* **Hạng mục cần Audit:** Toàn nhóm hoặc Product Owner (PO) cần đối chiếu lại với tài liệu đặc tả yêu cầu phần mềm (SRS v1.0) để chốt từ khóa/câu thông báo lỗi chuẩn của hệ thống. Nếu thông báo của nhánh B là chuẩn thiết kế, thông tin này sẽ được dùng làm căn cứ (Expected Result) để Đội phát triển (Dev) sửa lỗi Bug-002.

### 5. Đồng bộ hóa Ngôn ngữ và Dịch thuật (Localization Audit)

* **Vấn đề:** Nhánh A sử dụng cấu trúc tài liệu bằng Tiếng Anh làm chuẩn, trong khi nhánh B viết phần lớn nội dung IDM và Test Cases bằng Tiếng Việt. Để đảm bảo tính nhất quán của mã nguồn tài liệu, các nội dung từ nhánh B đã được dịch thuật và chuẩn hóa sang Tiếng Anh.
* **Hạng mục cần Audit:** Tác giả nhánh B cần rà soát lại ngữ nghĩa của các ca kiểm thử mới được dịch (đặc biệt là phần mô tả dữ liệu đầu vào và các phân vùng biên) để đảm bảo quá trình dịch thuật không làm sai lệch ý đồ kiểm thử gốc.

---

**Tóm lại hành động khẩn cấp (Action Items):** Yêu cầu đại diện Nhánh B vào rà soát file `test-cases.md` sau gộp, xác minh lại trạng thái các file báo cáo trống, và bổ sung kết quả chạy test thực tế cho các ca kiểm thử biên vừa được tích hợp.
