# Báo cáo tổng hợp — Test Summary Report

> **Hướng dẫn**: Điền vào các bảng và trả lời câu hỏi bên dưới để đánh giá tổng quan về chất lượng của hệ thống.

---

## 1. Thống kê số liệu (Statistics)

| Hạng mục | Số lượng | Tỷ lệ (%) |
|----------|----------|-----------
| **Tổng số Test Case đã viết** | 28 | 100% |
| **Số TC Pass** | 20 | 71.4% |
| **Số TC Fail** | 8 | 28.6% |
| **Số Bug đã report** | 6 | N/A |

## 2. Phân tích theo nhóm chức năng (Analysis by Feature)

| Nhóm chức năng | Số TC | Số Bug | Mức độ ổn định | Ghi chú |
|----------------|-------|--------|----------------|---------|
| Đăng nhập | 5 | 0 | 🟢 Tốt | Xử lý đầy đủ trường hợp hợp lệ/không hợp lệ. |
| Xem danh sách sách | 2 | 0 | 🟢 Tốt | Hiển thị đúy đủ thông tin, cập nhật real-time đúng theo thiết kế. |
| Tìm kiếm & Lọc | 5 | 0 | 🟢 Tốt | Chạy đúng thiết kế, kết quả trả về khớp. |
| Mượn, trả, quá hạn | 9 | 3 | 🔴 Kém | BUG-001: Vượt giới hạn 3 sách không bị chặn. BUG-002: Thông báo lỗi sai cho tài khoản Tạm ngưng. BUG-006: Trả sách quá hạn không có cảnh báo. |
| Quản lý thành viên, Tra cứu | 7 | 3 | 🔴 Kém | BUG-003: Logic email validation bị lỗi. BUG-004/BUG-005: Thành viên xem và trả được phiếu mượn của thành viên khác. |

## 3. Danh sách Bug phát hiện (Bug Summary)

| Mã Bug | Mô tả ngắn | REQ liên quan | Severity | Trạng thái |
|--------|-----------|---------------|----------|------------|
| BUG-001 | Thành viên có thể mượn quá 3 cuốn sách | REQ-04 | High | Mở |
| BUG-002 | Thông báo lỗi sai khi tài khoản Tạm ngưng cố mượn sách | REQ-04 | Medium | Mở |
| BUG-003 | Logic xác thực email bị lỗi, gồm cả trường hợp email trùng bị báo sai lỗi | REQ-07 | Critical | Mở |
| BUG-004 | Thành viên tra cứu được phiếu mượn của thành viên khác | REQ-08 | High | Mở |
| BUG-005 | Thành viên trả được phiếu mượn của thành viên khác | REQ-05, REQ-08 | Critical | Mở |
| BUG-006 | Trả sách quá hạn không hiển thị cảnh báo quá hạn | REQ-05 | Medium | Mở |

## 4. Đánh giá chất lượng (Quality Assessment)

**Q1. Tính ổn định chung của hệ thống:**
Hệ thống hoạt động ổn định ở các chức năng đọc cơ bản (xem sách, tìm kiếm, đăng nhập), chiếm 12/28 TC (42.9% test cases). Tuy nhiên, có 6 lỗi được phát hiện trong các luồng nghiệp vụ quan trọng, trong đó có lỗi Critical ở chức năng thêm thành viên và phân quyền trả phiếu mượn.

**Q2. Lỗi nào nghiêm trọng nhất? Tại sao?**
**BUG-003** (Critical) — Thủ thư không thể thêm thành viên mới. Đây là lỗi nghiêm trọng nhất vì nó **hoàn toàn vô hiệu hóa** một chức năng cốt lõi của Thủ thư. Mọi email hợp lệ đều bị hệ thống báo là "không hợp lệ", khiến thư viện không thể mở rộng danh sách thành viên trong suốt thời gian lỗi tồn tại.

**BUG-001** (High) — Thành viên có thể mượn quá 3 cuốn sách cùng lúc. Đây là lỗi nghiêm trọng thứ hai vì vi phạm trực tiếp quy tắc nghiệp vụ, có khả năng gây ra tình trạng thiếu sách cho thành viên khác.

**BUG-005** (Critical) — Thành viên có thể trả phiếu mượn của thành viên khác. Đây là lỗi phân quyền nghiêm trọng vì cho phép người dùng thay đổi dữ liệu mượn/trả không thuộc quyền sở hữu của mình.

**Q3. Đề xuất cải tiến (Recommendation) và Ưu tiên sửa lỗi:**
- **[ƯU TIÊN KHẨN CẤP / CRITICAL]** Sửa BUG-003: Kiểm tra và fix logic regex xác thực email trong form "Thêm thành viên". Đây là lỗi chặn toàn bộ chức năng, cần được fix ngay lập tức.
- **[ƯU TIÊN KHẨN CẤP / CRITICAL]** Sửa BUG-005: Chặn thành viên trả phiếu mượn của người khác ở cả giao diện và logic nghiệp vụ.
- **[ƯU TIÊN CAO / HIGH]** Sửa BUG-001: Thêm kiểm tra `currentBorrowedBooksCount >= 3` trước khi cho phép mượn sách. Cần khóa nút mượn khi đã đạt giới hạn.
- **[ƯU TIÊN CAO / HIGH]** Sửa BUG-004: Chặn thành viên tra cứu phiếu mượn của người khác, chỉ Thủ thư được xem toàn bộ phiếu.
- **[ƯU TIÊN TRUNG BÌNH / MEDIUM]** Sửa BUG-002: Phân tách logic thông báo lỗi cho trạng thái "Tạm ngưng" và "Hết hạn" để hiển thị đúng lý do từ chối theo từng trường hợp.
- **[ƯU TIÊN TRUNG BÌNH / MEDIUM]** Sửa BUG-006: Khi trả sách quá hạn, thông báo kết quả phải kèm cảnh báo quá hạn theo REQ-05.
- **[ƯU TIÊN THẤP / LOW]** Về mặt UI/UX: Nên thiết kế thêm tính năng làm mờ (disable) nút Mượn nếu tài khoản bị khoá, quá hạn, hoặc đã đạt hạn mức 3 cuốn, nhằm cải thiện trải nghiệm người dùng.

## 5. Khai báo sử dụng AI (AI Usage Declaration)

- Nhóm đã sử dụng AI (ChatGPT) để gợi ý một số kịch bản kiểm thử (Test Cases) cơ bản và hỗ trợ căn chỉnh định dạng các bảng (Markdown tables).
- Toàn bộ việc thực thi kiểm thử trên trình duyệt, phân tích kết quả và viết báo cáo lỗi đều do nhóm tự thực hiện thủ công.
