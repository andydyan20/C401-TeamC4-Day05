# UX exercise — MoMo Moni AI

## Sản phẩm: MoMo — Moni AI Assistant (phân loại chi tiêu)

## 4 paths

### 1. AI đúng
- User trả 120k cho GrabFood → Moni tự tag "Ăn uống"
- User thấy đúng vì đây là đơn đồ ăn
- UI: hiện icon 🍜 + tag "Ăn uống", không cần xác nhận
- User trả tiền điện 850k → Moni tag "Hóa đơn"
- User không cần chỉnh lại
- UI: hiện icon hóa đơn + merchant EVN
- User đặt xe Grab 90k → Moni tag "Di chuyển"
- User thấy hợp lý vì đây là chi phí đi lại
- UI: icon xe + tag "Di chuyển"

### 2. AI không chắc
- User chuyển 500k cho bạn cùng phòng
- Moni không biết đây là trả tiền nhà, ăn uống chung hay trả nợ
- UI: để "Chưa phân loại" + gợi ý: Nhà ở, Ăn uống, Trả nợ
User thanh toán 300k cho một merchant lạ
- Merchant chưa từng xuất hiện trước đây
- UI: hiện message: “Đây là giao dịch mới, bạn muốn phân loại là gì?”
- Gợi ý: Mua sắm, Giải trí, Khác
- User nhận chuyển khoản 2 triệu từ người thân
- Moni không rõ đây là lương, quà hay hoàn tiền
- UI: hiện 3 lựa chọn nhanh:Thu nhập, Quà tặng, Hoàn tiền

### 3. AI sai
- User mua thuốc ở nhà thuốc Long Châu
- Moni tag "Mua sắm" thay vì "Sức khỏe"
- User phải vào giao dịch → đổi category → lưu lại
- User thanh toán Netflix
- Moni tag "Mua sắm" thay vì "Giải trí"
- User phát hiện khi xem báo cáo tháng thấy nhóm mua sắm tăng bất thường
- User mua vé máy bay
- Moni tag "Di chuyển" nhưng user muốn "Du lịch"
- User phải sửa thủ công
- Không rõ lần sau Moni có nhớ không

### 4. User mất niềm tin
- Moni liên tục tag các giao dịch Shopee thành "Mua sắm" dù user chủ yếu mua sách và khóa học
- User thấy báo cáo tháng không còn phản ánh đúng chi tiêu thật
- Cuối cùng user tự phân loại thủ công tất cả giao dịch
- User thấy Moni gợi ý tiết kiệm dựa trên category sai
- Ví dụ Moni nói “Bạn chi quá nhiều cho mua sắm” trong khi thực tế phần lớn là học tập
- User bắt đầu bỏ qua mọi insight của AI
- Sau nhiều lần sửa category nhưng AI vẫn tiếp tục sai
- User không thấy hệ thống học từ mình
- Cuối cùng user tắt auto-tag hoặc liên hệ hỗ trợ để hỏi cách reset category rule

## Path yếu nhất: Path 3
- Thiếu explainability.
- Thiếu loop sửa sai nhanh.
- Thiếu transparency về độ chắc chắn.
- User phải tự nhận ra lỗi và tự sửa.

## Gap marketing vs thực tế
- Marketing positioning của Moni là “trợ thủ tài chính AI”, “người quản gia tài chính”, “hiểu người dùng”, “gợi ý thông minh”
- Thực tế: 
Moni mạnh ở việc tổng hợp dữ liệu, phân loại chi tiêu, gợi ý cơ bản. Nhưng chưa đạt mức “trợ lý tài chính thông minh toàn diện” như kỳ vọng marketing. AI hiện tại giống “smart dashboard + conversational layer” hơn là một financial advisor thực thụ.
- Gap lớn nhất: Khả năng xử lý ambiguity.
Khả năng giải thích vì sao AI đưa ra kết luận.
Khả năng sửa sai nhanh và học từ user.
Trải nghiệm fallback khi AI không đủ tốt.

## Sketch
Path yếu nhất: Path 3 - Khi AI sai 
- As-is: user nhìn báo cáo chi tiêu(nhận thấy AI phân loại sai) → mở chi tiêu giao dịch(chọn "Sửa") → Chọn category đúng(Lưu) → AI cập nhật 
- To-be: user nhìn báo cáo chi tiêu(AI highlight giao dịch không chắc) → Tooltip/promptAI(đề xuất xem xét, sửa đổi) → user click "Sửa" (UI hiện tị đề xuất trong category) → user chọn category mới(AI học ngay) → AI cập nhật