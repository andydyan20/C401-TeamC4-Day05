# Individual reflection — Phạm Trần Thanh Lâm (2A202600270)

## 1. Role
Frontend UX Designer + User Experience Specialist. Phụ trách thiết kế trải nghiệm người dùng (UX), vẽ luồng tương tác và xây dựng giao diện (UI) cho Chatbot & Checklist.

## 2. Đóng góp cụ thể
- Thiết kế 4 luồng User Stories chi tiết bao hàm các kịch bản: Tân sinh viên, Nhân viên mới, Đơn vị hỗ trợ (HR/Student Affairs).
- Vẽ UX Sketch chuyển đổi từ quy trình onboarding rời rạc (As-is) sang quy trình tập trung có AI hỗ trợ (To-be).
- Thiết kế giao diện Chat UI hỗ trợ hiển thị **Nguồn trích dẫn (Citations)** và **Mức độ tự tin (Confidence level)** để xử lý sự không chắc chắn của AI.
- Xây dựng Panel Checklist cá nhân hóa theo vai trò/đơn vị và các nút phản hồi an toàn: "Báo sai" & "Chuyển bộ phận".

## 3. SPEC mạnh/yếu
- **Mạnh nhất: Trust (Sự tin tưởng)** — Nhóm đã thiết kế cơ chế Augmentation rất kỹ. Thay vì bot khẳng định 100%, bot luôn đưa ra nguồn để user kiểm soát thủ công. Việc tích hợp nút "Báo sai" giúp phục hồi niềm tin ngay lập tức khi AI gặp lỗi.
- **Yếu nhất: ROI (Hiệu quả kinh tế)** — Các kịch bản ROI hiện tại vẫn còn khá chung chung, dựa nhiều vào assumption về số lượng user mà chưa tính toán kỹ chi phí vận hành API thực tế khi dữ liệu Knowledge Base phình to. Chưa có dữ liệu về chi phí vận hành AI.


## 4. Đóng góp khác
- Xác định và thiết kế hệ thống **Learning Signal**: Quy định cụ thể cách thu thập feedback từ nút Helpful/Not helpful để đẩy vào Event Logs phục vụ việc cải thiện Knowledge Base sau này.
- Review UX cho phần output của Prompt để đảm bảo bot luôn hỏi lại những câu "setup" (vai trò, kỳ học) trước khi trả lời, tránh việc trả lời sai ngữ cảnh.

## 5. Điều học được
Trước đây mình nghĩ AI tốt là AI phải làm hết mọi thứ cho người dùng (Automation). Sau Lab 5, mình nhận ra trong các lĩnh vực "high-stakes" như hành chính học vụ, **Augmentation** mới là chìa khóa. UX cho AI không chỉ là hiển thị kết quả, mà là hiển thị bằng chứng (evidence) và đường thoát (escalation path) khi AI sai. Niềm tin không đến từ sự hoàn hảo, mà đến từ sự minh bạch.

## 6. Nếu làm lại
Sẽ dành thêm thời gian để phác thảo kỹ hơn phần "History & Session state". Hiện tại thiết kế tập trung vào phiên chat đơn lẻ, nếu làm lại mình sẽ thiết kế cách lưu trữ trạng thái checklist giữa các tab hoặc giữa các ngày khác nhau để user không bị mất tiến độ khi tắt trình duyệt.

## 7. AI giúp gì / AI sai gì
- **Giúp:** Sử dụng Gemini để brainstorm các **failure modes** tiềm ẩn như "Dữ liệu lỗi thời theo từng kỳ" hoặc "Lộ thông tin nhạy cảm định danh (PII)". Những case này nếu tự nghĩ sẽ rất dễ bỏ sót.
- **Sai/mislead:** AI từng gợi ý thiết kế tính năng "Tự động điền form và nộp hộ sinh viên". Ý tưởng này nghe rất hiện đại nhưng lại vi phạm nguyên tắc "Human-in-control" của nhóm và tiềm ẩn rủi ro pháp lý cao. Bài học: Cần tỉnh táo để không bị AI dẫn dắt vào các tính năng "over-engineered" làm giảm tính an toàn của sản phẩm.