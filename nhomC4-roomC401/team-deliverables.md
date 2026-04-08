# VinUni Onboarding Agent — Team Deliverables (Ready-to-use)

## 1) Mục tiêu chung

Xây dựng AI agent hỗ trợ onboarding cho tân sinh viên và nhân viên mới tại VinUni:
- Trả lời quy định học vụ và thủ tục hành chính bằng RAG.
- Dẫn luồng theo checklist cá nhân hóa theo vai trò.
- Có cơ chế xác minh nguồn, phản hồi sai, và chuyển bộ phận khi cần.

---

## 2) Deliverables theo thành viên

## A. Đặng Tiến Dũng — `2A202600024`
**Phụ trách:** Prototype research + prompt test + demo flow

### A1. Kiến trúc prototype đề xuất (v1)
- **Frontend:** Chat UI đơn giản + checklist panel.
- **Backend agent:** Intent routing + RAG + response policy.
- **Knowledge base:** Handbook, FAQ, policy, quy trình biểu mẫu.
- **Guardrails:** Chặn trả lời khi thiếu dữ liệu, yêu cầu hỏi rõ ngữ cảnh.
- **Escalation:** Chuyển sang Student Affairs/HR theo intent.

### A2. Luồng demo (happy path)
1. User chọn vai trò: `Tân sinh viên` hoặc `Nhân viên mới`.
2. Agent hỏi 3-5 câu setup (khoa/đơn vị, kỳ, tình trạng hồ sơ).
3. Agent đưa checklist onboarding cá nhân hóa.
4. User hỏi câu cụ thể -> agent trả lời kèm trích dẫn.
5. Agent đề xuất `Next step` + link form/portal chính xác.

### A3. Bộ prompt test (bản dùng ngay)
**System prompt (rút gọn):**
- Bạn là AI Onboarding Assistant của VinUni.
- Chỉ trả lời dựa trên context truy xuất được từ knowledge base.
- Nếu thiếu dữ liệu hoặc câu hỏi mơ hồ, hỏi lại tối đa 2 câu làm rõ.
- Luôn kèm nguồn trích dẫn ngắn (tên tài liệu + mục).
- Không bịa thông tin. Nếu không chắc, đề xuất kênh hỗ trợ chính thức.

**5 test prompts mẫu:**
1. "Em là freshman ngành CS, cần làm gì tuần đầu?"
2. "Deadline nộp hồ sơ bảo hiểm y tế kỳ Fall là khi nào?"
3. "Tôi là nhân viên mới, cần quy trình nhận email nội bộ."
4. "Nếu em đổi ký túc xá giữa kỳ thì thủ tục nào ưu tiên?"
5. "Học bổng bị thiếu chứng từ thì em nộp bổ sung ở đâu?"

---

## B. Phạm Hữu Hoàng Hiệp — `2A202600415`
**Phụ trách:** Canvas + failure modes + risk/RBAC

### B1. Canvas tóm tắt
| Hạng mục | Nội dung |
|---|---|
| **Value** | Giảm thời gian tìm thông tin onboarding, giảm hỏi lặp lại cho Student Affairs/HR, giảm lỗi sai thủ tục. |
| **Trust** | Trả lời có trích dẫn, ghi ngày cập nhật, có mức tự tin, có nút báo sai/chuyển bộ phận. |
| **Feasibility** | RAG trên tài liệu nội bộ, có thể triển khai theo phase nhỏ, chi phí token + retrieval trong mức kiểm soát. |

### B2. Top failure modes + xử lý
| Failure mode | Tác động | Cách giảm thiểu |
|---|---|---|
| Tài liệu cũ | Trả lời sai deadline/quy định | ETL cập nhật theo lịch, version tài liệu, hiển thị ngày cập nhật |
| Thiếu ngữ cảnh user | Trả lời đúng chung nhưng sai với cá nhân | Bắt buộc bước setup profile trước khi trả lời |
| Hallucination | User làm sai thủ tục | Chỉ trả lời từ context, không có context thì từ chối + escalate |
| Truy cập sai quyền | Rò rỉ thông tin nội bộ | RBAC theo role + audit log |
| Link chết/form đổi URL | Mất niềm tin, tốn thời gian | Link checker hằng ngày + fallback trang trung tâm hỗ trợ |

### B3. RBAC đề xuất
- `Student`: chỉ xem thông tin onboarding sinh viên.
- `Staff`: chỉ xem thông tin onboarding nhân sự.
- `Admin/Support`: xem dashboard feedback, duyệt cập nhật tài liệu.
- Mọi truy cập nhạy cảm ghi log: `user_id`, `intent`, `doc_id`, `timestamp`.

---

## C. Phạm Việt Cường — `2A202600420`
**Phụ trách:** Eval metrics + ROI/cost estimate + instrumentation

### C1. Bộ metrics đánh giá
| Nhóm chỉ số | Metric | Mục tiêu v1 |
|---|---|---|
| Chất lượng trả lời | Grounded answer rate (% câu có trích dẫn hợp lệ) | >= 90% |
| Hoàn thành tác vụ | Checklist step completion success | >= 80% |
| Trải nghiệm | Median response latency | <= 5 giây |
| Vận hành | Escalation accuracy (chuyển đúng bộ phận) | >= 85% |
| Tin cậy | User-reported incorrect rate | <= 10% |

### C2. ROI ước lượng sơ bộ (order of magnitude)
- **Giả định:** 1,000 lượt hỏi/tuần onboarding.
- Nếu agent xử lý tự phục vụ 60% câu hỏi cơ bản:
  - Giảm tải cho Student Affairs/HR khoảng 600 lượt/tuần.
  - Nếu mỗi lượt tiết kiệm trung bình 3-5 phút nhân sự -> tiết kiệm 1,800-3,000 phút/tuần.
- **Chi phí chính:** inferencing + retrieval + lưu trữ vector + giám sát.
- **Kết luận sơ bộ:** khả thi khi triển khai augmentation và giới hạn phạm vi use case ở giai đoạn đầu.

### C3. Instrumentation cần gắn
- Log chuẩn cho mỗi phiên:
  - `session_id`, `role`, `intent`, `retrieved_docs`, `confidence`, `escalated`.
- Event tracking:
  - `answer_viewed`, `citation_clicked`, `feedback_negative`, `task_completed`.
- Dashboard:
  - Theo dõi theo tuần: intent top, tỷ lệ sai theo chủ đề, tài liệu gây lỗi nhiều nhất.

---

## D. Phạm Trần Thanh Lâm — `2A202600270`
**Phụ trách:** User stories 4 paths + UX sketch (as-is / to-be)

### D1. 4 paths thực tế
| Path | Hiện trạng as-is | To-be đề xuất |
|---|---|---|
| **Đúng -> user thấy gì?** | User nhận câu trả lời nhanh nhưng đôi lúc thiếu bước tiếp theo | Câu trả lời + nút `Next step` + checklist cập nhật ngay |
| **Không chắc -> hỏi lại?** | Bot hỏi lại chưa trọng tâm hoặc trả lời chung chung | Hỏi rõ 1-2 câu ngữ cảnh (role/kỳ/đơn vị) bằng quick options |
| **Sai -> sửa thế nào?** | User đổi cách hỏi nhiều lần hoặc bỏ sang kênh khác | Có nút `Báo sai`, đề xuất câu đúng, và sửa câu trả lời theo nguồn |
| **Mất tin -> có exit?** | User tự tìm email/đầu mối hỗ trợ | Luôn hiển thị `Chuyển bộ phận` + contact đúng theo intent |

### D2. User stories mẫu
1. Là tân sinh viên, tôi muốn biết checklist tuần đầu để không bỏ sót bước.
2. Là nhân viên mới, tôi muốn biết hồ sơ cần nộp để hoàn tất onboarding đúng hạn.
3. Là mentor, tôi muốn gửi link hướng dẫn chuẩn cho mentee chỉ bằng 1 thao tác.
4. Là Student Affairs/HR, tôi muốn giảm câu hỏi lặp để tập trung case phức tạp.

### D3. UX sketch đề xuất (text wireframe)
- **Header:** "VinUni Onboarding Assistant"
- **Step 1:** chọn vai trò (`Student` / `Staff`)
- **Step 2:** hỏi setup nhanh (khoa/đơn vị, kỳ, loại thủ tục)
- **Main chat:** câu trả lời + trích dẫn + mức tự tin
- **Side panel:** checklist cá nhân hóa và trạng thái từng bước
- **Footer actions:** `Báo sai` | `Chuyển bộ phận` | `Mở portal`

---

## 3) Kế hoạch triển khai 2 tuần (gợi ý)

| Tuần | Mục tiêu | Output |
|---|---|---|
| **Tuần 1** | Chốt scope + dữ liệu + prototype chat | Intent list, KB đầu tiên, demo Q/A có trích dẫn |
| **Tuần 2** | Eval + chỉnh UX + chuẩn bị slide demo | Dashboard metrics v1, 4 paths as-is/to-be, script demo 5 phút |

---

## 4) Checklist nộp bài

- [ ] Có problem statement rõ pain thật.
- [ ] Có Canvas (Value/Trust/Feasibility) đã điền đầy đủ.
- [ ] Có 4 paths as-is/to-be rõ ràng.
- [ ] Có metrics + giả định ROI.
- [ ] Có demo flow end-to-end + kịch bản failure.
- [ ] Có phân công rõ theo thành viên.

