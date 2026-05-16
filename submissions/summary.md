# Báo cáo tổng hợp — Test Summary Report

> **Hướng dẫn**: Điền vào các bảng và trả lời câu hỏi bên dưới để đánh giá tổng quan về chất lượng của hệ thống.

---

## 1. Thống kê số liệu (Statistics)

| Hạng mục | Số lượng | Tỷ lệ (%) |
|----------|----------|-----------|
| **Tổng số Test Case đã viết** | 25 | 100% |
| **Số TC Pass** | 24 | 96% |
| **Số TC Fail** | 1 | 4% |
| **Số Bug đã report** | 1 | N/A |

## 2. Phân tích theo nhóm chức năng (Analysis by Feature)

| Nhóm chức năng | Số TC | Số Bug | Mức độ ổn định | Ghi chú |
|----------------|-------|--------|----------------|---------|
| Đăng nhập | 5 | 0 | 🟢 Tốt | Xử lý đầy đủ trường hợp hợp lệ/không hợp lệ. |
| Tìm kiếm & Lọc | 6 | 0 | 🟢 Tốt | Chạy đúng thiết kế, kết quả trả về khớp. |
| Mượn, trả, quá hạn | 9 | 1 | 🟡 Trung bình | Bị lỗi nghiêm trọng ở giới hạn số lượng mượn sách. |
| Quản lý TV, Tra cứu| 5 | 0 | 🟢 Tốt | Hiển thị phiếu chính xác, quản lý chặt chẽ. |

## 3. Đánh giá chất lượng (Quality Assessment)

**Q1. Tính ổn định chung của hệ thống:**
Nhìn chung, hệ thống hoạt động khá mượt mà, thực hiện đúng các chức năng cơ bản đã nêu trong SRS (96% Pass). UI rõ ràng, thời gian phản hồi nhanh.

**Q2. Lỗi nào nghiêm trọng nhất? Tại sao?**
Lỗi nghiêm trọng nhất (và duy nhất phát hiện) là việc không giới hạn số lượng sách được mượn (BUG-001). Đây là một Critical Bug vì nó ảnh hưởng trực tiếp đến nghiệp vụ phân bổ tài nguyên của Thư viện, có khả năng dẫn đến việc thất thoát sách do một người ôm đồm quá nhiều sách.

**Q3. Đề xuất cải tiến (Recommendation) và Ưu tiên sửa lỗi:**
- **[ƯU TIÊN CAO / HIGH]** Cần khẩn trương Fix lỗi BUG-001: Chặn ngay thao tác mượn khi số lượng sách >= 3. Đây là lỗi nghiệp vụ nghiêm trọng nhất cần được khắc phục trước tiên.
- **[ƯU TIÊN THẤP / LOW]** Về mặt UI/UX: Nên thiết kế thêm tính năng làm mờ (disable) nút Mượn nếu tài khoản bị khoá, quá hạn, hoặc đã đạt hạn mức 3 cuốn. Việc này giúp cải thiện trải nghiệm người dùng, có thể làm sau khi đã fix xong logic ở phía trên.

## 4. Khai báo sử dụng AI (AI Usage Declaration)

- Nhóm đã sử dụng AI (ChatGPT) để gợi ý một số kịch bản kiểm thử (Test Cases) cơ bản và hỗ trợ căn chỉnh định dạng các bảng (Markdown tables).
- Toàn bộ việc thực thi kiểm thử trên trình duyệt, phân tích kết quả và viết báo cáo lỗi đều do nhóm tự thực hiện thủ công.
