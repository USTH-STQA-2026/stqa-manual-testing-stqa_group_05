# Báo cáo tổng hợp — Test Summary Report

> **Hướng dẫn**: Điền vào các bảng và trả lời câu hỏi bên dưới để đánh giá tổng quan về chất lượng của hệ thống.

---

## 1. Thống kê số liệu (Statistics)

| Hạng mục | Số lượng | Tỷ lệ (%) |
|----------|----------|-----------
| **Tổng số Test Case đã viết** | 26 | 100% |
| **Số TC Pass** | 22 | 84.6% |
| **Số TC Fail** | 4 | 15.4% |
| **Số Bug đã report** | 3 | N/A |

## 2. Phân tích theo nhóm chức năng (Analysis by Feature)

| Nhóm chức năng | Số TC | Số Bug | Mức độ ổn định | Ghi chú |
|----------------|-------|--------|----------------|---------|
| Đăng nhập | 5 | 0 | 🟢 Tốt | Xử lý đầy đủ trường hợp hợp lệ/không hợp lệ. |
| Xem danh sách sách | 2 | 0 | 🟢 Tốt | Hiển thị đúy đủ thông tin, cập nhật real-time đúng theo thiết kế. |
| Tìm kiếm & Lọc | 5 | 0 | 🟢 Tốt | Chạy đúng thiết kế, kết quả trả về khớp. |
| Mượn, trả, quá hạn | 9 | 2 | 🔴 Kém | BUG-001: Vượt giới hạn 3 sách không bị chặn. BUG-002: Thông báo lỗi sai cho tài khoản Tạm ngưng. |
| Quản lý thành viên, Tra cứu | 5 | 1 (BUG-003 — 2 biểu hiện, cùng 1 root cause) | 🔴 Kém | BUG-003: Logic email validation bị đảo ngược — từ chối email hợp lệ (TC-21) và chấp nhận email không hợp lệ (TC-22). |

## 3. Danh sách Bug phát hiện (Bug Summary)

| Mã Bug | Mô tả ngắn | REQ liên quan | Severity | Trạng thái |
|--------|-----------|---------------|----------|------------|
| BUG-001 | Thành viên có thể mượn quá 3 cuốn sách | REQ-04 | High | Mở |
| BUG-002 | Thông báo lỗi sai khi tài khoản Tạm ngưng cố mượn sách | REQ-04 | Medium | Mở |
| BUG-003 | Thủ thư không thể thêm thành viên mới (email validation bị lỗi) | REQ-07 | Critical | Mở |

## 4. Đánh giá chất lượng (Quality Assessment)

**Q1. Tính ổn định chung của hệ thống:**
Hệ thống hoạt động ổn định ở các chức năng đọc (xem sách, tìm kiếm, đăng nhập), chiếm 11/25 TC (44% test cases). Tuy nhiên, có 3 lỗi được phát hiện trong các luồng nghiệp vụ quan trọng, với 1 lỗi Critical làm tê liệt hoàn toàn chức năng thêm thành viên.

**Q2. Lỗi nào nghiêm trọng nhất? Tại sao?**
**BUG-003** (Critical) — Thủ thư không thể thêm thành viên mới. Đây là lỗi nghiêm trọng nhất vì nó **hoàn toàn vô hiệu hóa** một chức năng cốt lõi của Thủ thư. Mọi email hợp lệ đều bị hệ thống báo là "không hợp lệ", khiến thư viện không thể mở rộng danh sách thành viên trong suốt thời gian lỗi tồn tại.

**BUG-001** (High) — Thành viên có thể mượn quá 3 cuốn sách cùng lúc. Đây là lỗi nghiêm trọng thứ hai vì vi phạm trực tiếp quy tắc nghiệp vụ, có khả năng gây ra tình trạng thiếu sách cho thành viên khác.

**Q3. Đề xuất cải tiến (Recommendation) và Ưu tiên sửa lỗi:**
- **[ƯU TIÊN KHẨN CẤP / CRITICAL]** Sửa BUG-003: Kiểm tra và fix logic regex xác thực email trong form "Thêm thành viên". Đây là lỗi chặn toàn bộ chức năng, cần được fix ngay lập tức.
- **[ƯU TIÊN CAO / HIGH]** Sửa BUG-001: Thêm kiểm tra `currentBorrowedBooksCount >= 3` trước khi cho phép mượn sách. Cần khóa nút mượn khi đã đạt giới hạn.
- **[ƯU TIÊN TRUNG BÌNH / MEDIUM]** Sửa BUG-002: Phân tách logic thông báo lỗi cho trạng thái "Tạm ngưng" và "Hết hạn" để hiển thị đúng lý do từ chối theo từng trường hợp.
- **[ƯU TIÊN THẤP / LOW]** Về mặt UI/UX: Nên thiết kế thêm tính năng làm mờ (disable) nút Mượn nếu tài khoản bị khoá, quá hạn, hoặc đã đạt hạn mức 3 cuốn, nhằm cải thiện trải nghiệm người dùng.

## 5. Khai báo sử dụng AI (AI Usage Declaration)

- Nhóm đã sử dụng AI (ChatGPT) để gợi ý một số kịch bản kiểm thử (Test Cases) cơ bản và hỗ trợ căn chỉnh định dạng các bảng (Markdown tables).
- Toàn bộ việc thực thi kiểm thử trên trình duyệt, phân tích kết quả và viết báo cáo lỗi đều do nhóm tự thực hiện thủ công.
