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

|   | Value | Trust | Feasibility |
|---|-------|-------|-------------|
| **Câu hỏi guide** | User nào? Pain gì? AI giải quyết gì mà cách hiện tại không giải được? | Khi AI sai thì user bị ảnh hưởng thế nào? User biết AI sai bằng cách nào? User sửa bằng cách nào? | Cost bao nhiêu/request? Latency bao lâu? Risk chính là gì? |
| **Trả lời** | **User**: tân sinh viên (UG/PG), nhân viên mới, mentor/HR/Student Affairs. **Pain**: info phân tán, hỏi đi hỏi lại, trễ deadline, sai thủ tục. **AI giúp**: trả lời tức thời theo ngữ cảnh + checklist onboarding cá nhân hóa (theo vai trò/kỳ/đơn vị) mà vẫn bám nguồn chính thức. | **Sai**: hướng dẫn sai quy định/điền nhầm form/trễ hạn → ảnh hưởng học vụ/nhân sự. **Biết sai**: agent hiển thị “nguồn trích dẫn”, ngày cập nhật, mức tự tin; user thấy mâu thuẫn với portal/email. **Sửa**: nút “Báo sai/Không đúng” + chọn câu trả lời đúng/đính kèm link; hoặc “Escalate” đến bộ phận liên quan. | **Feasibility**: RAG trên tài liệu nội bộ (handbook, FAQ, policy, form). Latency mục tiêu 2–5s (cache + retrieval). Cost theo token + retrieval; giới hạn lượt/phiên. **Risk**: dữ liệu nội bộ & PII, tài liệu lỗi thời, hallucination, quyền truy cập theo vai trò (RBAC). |

---

## Automation hay augmentation?

☐ Automation — AI làm thay, user không can thiệp  
☑ Augmentation — AI gợi ý, user quyết định cuối cùng

**Justify:** Onboarding có nhiều bước “high-stakes” (deadline, giấy tờ, quyết định học vụ). Agent nên **gợi ý + dẫn luồng + trích nguồn**, còn user/đơn vị phụ trách vẫn là điểm quyết định và xác nhận cuối.

---

## Learning signal

| # | Câu hỏi | Trả lời |
|---|---------|---------|
| 1 | User correction đi vào đâu? | Lưu feedback theo Q/A: “câu trả lời hữu ích/không”, chọn lý do sai (lỗi nguồn, lỗi hiểu câu hỏi, tài liệu cũ), và link/đoạn trích đúng → cập nhật bộ tri thức + prompt/routing. |
| 2 | Product thu signal gì để biết tốt lên hay tệ đi? | Tỉ lệ “resolved without escalation”, tỉ lệ click đúng form/link, thời gian hoàn thành checklist, số lần hỏi lại cùng intent, tỉ lệ feedback tiêu cực theo chủ đề, và % câu trả lời có nguồn trích dẫn. |
| 3 | Data thuộc loại nào? ☐ User-specific · ☐ Domain-specific · ☐ Real-time · ☐ Human-judgment · ☐ Khác: ___ | ☑ User-specific (vai trò/kỳ/khoa) · ☑ Domain-specific (policy/FAQ) · ☑ Real-time (deadline/đợt tuyển/portal status) · ☑ Human-judgment (phản hồi đúng/sai) |

**Có marginal value không?** Có. Dữ liệu có giá trị vì: (1) tài liệu nội bộ thay đổi theo kỳ/đợt, (2) câu hỏi lặp lại theo ngữ cảnh VinUni, (3) feedback giúp phát hiện “chỗ policy gây hiểu lầm” và ưu tiên cập nhật knowledge base.

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

