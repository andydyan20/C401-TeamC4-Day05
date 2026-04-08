# SPEC draft — VinUni Onboarding Agent (Nhóm)

## Track: VinUni

## Problem statement
Tân sinh viên hoặc nhân viên mới tại VinUni thường gặp khó khăn khi “onboarding” vì thông tin nằm rải rác (email, handbook, portal, form nội bộ), mỗi phòng ban có quy trình khác nhau và các bước phụ thuộc thời gian (deadline, điều kiện, giấy tờ). Họ thường phải:

- Hỏi lại bạn bè/mentor/HR/Student Affairs nhiều lần (mất thời gian, câu trả lời không đồng nhất).
- Tự mò quy định học vụ/thủ tục hành chính (dễ bỏ sót bước quan trọng).
- Lặp lại các câu hỏi cơ bản theo ngữ cảnh cá nhân (khoa/ngành, hệ, vai trò, kỳ học, visa, ký túc xá…).

Nhóm mình muốn xây dựng **Hệ thống AI Onboarding toàn diện**: một **agent tự động** hỗ trợ hội nhập cho tân sinh viên hoặc nhân viên mới, tích hợp **RAG** để trả lời **quy định học vụ** và **thủ tục hành chính**, đồng thời dẫn dắt theo **luồng việc** (checklist theo vai trò & thời gian).

---

## Canvas draft

| VALUE | TRUST | FEASIBILITY |
| :--- | :--- | :--- |
| **User nào? Pain nào?**<br>- **User**: Tân sinh viên (UG/PG), nhân viên mới, mentor/HR/Student Affairs.<br>- **Pain**: Info phân tán, hỏi đi hỏi lại, trễ deadline, sai thủ tục. | **Precision hay recall?**<br>- **Precision**. RAG cho hành chính/học vụ không được sai sót. Không tìm ra thông tin thì trả lời "Tôi không biết" thay vì bịa. | **Cost / latency?**<br>- **Latency**: Mục tiêu 2–5s (RAG cache + retrieval).<br>- **Cost**: Theo token + retrieval; có giới hạn lượt/phiên. |
| **Automation hay augmentation?**<br>- **Augmentation**: AI gợi ý, kéo luồng trích nguồn. User quyết định cuối cùng vì các bước onboarding có yếu tố "high-stakes". | **Khi sai → user biết/sửa thế nào?**<br>- **Biết sai**: Agent hiện "nguồn trích dẫn", user thấy mâu thuẫn với portal/email.<br>- **Sửa**: Nút "Báo sai/Không đúng" + đính kèm link, hoặc "Escalate" đến bộ phận liên quan. | **Dependency / risk chính?**<br>- **Dependency**: Phụ thuộc nội dung từ tài liệu nội bộ VinUni (FAQ, policy, form...).<br>- **Risk**: Lộ thông tin nội bộ/PII, tài liệu lỗi thời, hallucination, thiếu phân quyền (RBAC). |
| **Value khi AI đúng?**<br>- Trả lời tức thời theo ngữ cảnh cá nhân hóa (vai trò/kỳ/đơn vị).<br>- Setup checklist quy trình cụ thể, bám nguồn chính thức. | **Trust recovery?**<br>- Cho phép nhanh chóng "Báo sai" và route/escalate sang đúng phòng ban để xử lý. | |

> **Learning Signal:**
> **(1) User correction đi vào đâu?** Dữ liệu từ nút Helpful/Not helpful và luồng Báo sai sẽ được đẩy thẳng vào Event Logs lưu tại Database. Những câu hỏi bị bot hỏi lại (mơ hồ) hoặc bot từ chối trả lời sẽ được tổng hợp để bộ phận Đào tạo/HR cập nhật thêm "Data fix cứng" (FAQ mới).  
> **(2) Product thu signal gì để biết đang tốt lên hay tệ đi?** Tỉ lệ “resolved without escalation”, tỉ lệ click form/link, thời gian hoàn thành checklist, số lần hỏi lặp lại cùng intent, % câu trả lời có nguồn trích dẫn.  
> **(3) Data thuộc loại nào?** Thuộc loại User-specific, Domain-specific, Real-time và Human-judgment. **Có marginal value không?** Sẽ có marginal value vì: tài liệu nội bộ thay đổi theo kỳ, lượng câu hỏi theo ngữ cảnh lớn, feedback khám phá ra lỗ hổng policy để cải thiện knowledge base.

---

## Hướng đi chính

- **Prototype**: agent chat + checklist onboarding theo vai trò (tân sinh viên / nhân viên mới)  
  - Hỏi 3–5 câu “setup” (vai trò, đơn vị/khoa, kỳ, tình trạng visa/ký túc xá nếu có)  
  - Trả lời bằng RAG (kèm trích dẫn) + gợi ý “next step” (link form/portal/contact)  
  - Có nút “Báo sai” + “Chuyển bộ phận”
- **Eval**:
  - Answer grounded rate (có trích dẫn) ≥ 90%
  - Task success (hoàn thành bước đúng theo checklist) ≥ 80%
  - Escalation rate hợp lý (không quá cao, nhưng đúng lúc) + giảm câu hỏi lặp
- **Main failure modes**:
  - Tài liệu cũ/khác kỳ → trả lời sai deadline/quy định
  - Câu hỏi thiếu ngữ cảnh (vai trò/kỳ) → trả lời “đúng nhưng không đúng với user”
  - Truy cập vượt quyền (lộ nội dung nội bộ) nếu không có RBAC

---

## Phân công

- **Đặng Tiến Dũng — 2A202600024**: Prototype research + prompt test + demo flow
- **Phạm Hữu Hoàng Hiệp — 2A202600415**: Canvas + failure modes + risk/RBAC
- **Phạm Việt Cường — 2A202600420**: Eval metrics + ROI/cost estimate + instrumentation
- **Phạm Trần Thanh Lâm — 2A202600270**: User stories 4 paths + UX sketch (as-is/to-be)

