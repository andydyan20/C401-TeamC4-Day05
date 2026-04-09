# Individual reflection — Phạm Trần Thanh Lâm (2A202600270)

## 1. Role
Frontend UX Designer + User Experience Specialist. Phụ trách thiết kế trải nghiệm người dùng (UX), vẽ luồng tương tác và xây dựng giao diện (UI) cho Chatbot & Checklist.

## 2. Đóng góp cụ thể
- Thiết kế 4 luồng User Stories chi tiết, bao quát các kịch bản: Tân sinh viên, Nhân viên mới, và Đơn vị hỗ trợ (HR/Student Affairs).
- Vẽ UX Sketch chuyển đổi từ quy trình onboarding rời rạc (As-is) sang quy trình tập trung có AI hỗ trợ (To-be).
- Thiết kế giao diện Chat UI hỗ trợ hiển thị **Nguồn trích dẫn (Citations)** và **Mức độ tự tin (Confidence level)** để kiểm soát sự không chắc chắn của AI.
- Xây dựng Panel Checklist cá nhân hóa theo vai trò/đơn vị và tích hợp các nút phản hồi an toàn: "Báo sai" & "Chuyển bộ phận".

## 3. SPEC mạnh/yếu
- **Mạnh nhất: Trust (Sự tin tưởng)** — Nhóm đã thiết kế cơ chế Augmentation rất kỹ. Thay vì bot khẳng định 100%, bot luôn đưa ra nguồn để user kiểm soát thủ công. Việc tích hợp nút "Báo sai" giúp người dùng cảm thấy luôn có quyền kiểm soát khi hệ thống gặp lỗi.
- **Yếu nhất: ROI (Hiệu quả kinh tế)** — Các kịch bản ROI vẫn còn khá chung chung, dựa nhiều vào giả định về số lượng user mà chưa tính toán kỹ chi phí vận hành API thực tế. Đặc biệt là chưa có dữ liệu cụ thể về chi phí vận hành AI khi quy mô dữ liệu Knowledge Base phình to. Ngoài ra cũng chưa có thông tin về chi phí vận hành hệ thống(token, nhân sự bảo trì,...). Chưa có đưa ra được thời gian giảm được nếu sử dụng chatbot thay vì hỏi trực tiếp nhân viên hỗ trợ.

## 4. Đóng góp khác
- Xác định và thiết kế hệ thống **Learning Signal**: Quy định cách thu thập feedback từ nút Helpful/Not helpful để đẩy vào Event Logs phục vụ việc tinh chỉnh Knowledge Base.
- Review UX cho phần output của Prompt để đảm bảo bot luôn thực hiện bước "clarify" (hỏi lại vai trò, kỳ học) trước khi trả lời, tránh phản hồi sai ngữ cảnh.

## 5. Điều học được
Trước đây mình nghĩ AI tốt là phải tự động hóa hoàn toàn (Automation). Sau Lab 5, mình nhận ra trong các lĩnh vực "high-stakes" như hành chính học vụ, **Augmentation** mới là yếu tố then chốt. UX cho AI không chỉ là hiển thị kết quả đúng, mà là hiển thị bằng chứng (evidence) và đường thoát (escalation) khi AI sai. Metric về sự hài lòng của user đôi khi quan trọng hơn độ chính xác tuyệt đối của model.

## 6. Nếu làm lại
Sẽ đầu tư hơn vào việc detect input người dùng. 

## 7. AI giúp gì / AI sai gì
- **Giúp:** Dùng Gemini để brainstorm các **failure modes** thực tế như "Tài liệu lỗi thời/hết hạn" hoặc "Rò rỉ dữ liệu định danh (PII)". Những case này rất khó tự liệt kê đầy đủ nếu không có sự hỗ trợ của AI. Ngoài ra AI giúp tôi debug trong quá trình xây dựng UI của app
- **Sai/mislead:** Claude từng gợi ý thêm tính năng "Tự động nộp hồ sơ thay cho sinh viên". Ý tưởng này nghe hấp dẫn nhưng lại vi phạm nguyên tắc "Human-managed" của nhóm và có rủi ro pháp lý. Bài học: AI brainstorm rất tốt nhưng không biết giới hạn scope và các rào cản thực tế, cần con người giữ vai trò định hướng.