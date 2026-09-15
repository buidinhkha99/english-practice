# Phase 5 — Writing & Shadowing

> Mức chi tiết: **khung**. Sẽ đặc tả đầy đủ khi tới lượt.

## Context Links
PRD §7.8 · Phụ thuộc: [Phase 3](phase-03-dictionary-enrichment.md) + [Phase 4](phase-04-grammar.md)

## Overview
- **Priority:** P2
- **Status:** ⬜ Chưa bắt đầu
- Luyện viết theo đúng mô tả ban đầu: shadow đoạn đã học trước, rồi mới tự viết phần còn lại.

## Key Insights
- Đoạn mẫu **chỉ lấy từ nguồn của chính người dùng** — shadow văn bản lạ thì mất hết lợi thế corpus riêng.
- Writing là **một hình thức ôn tập**, không phải module tách rời: item mục tiêu lấy từ hàng đợi
  đang học, dùng đúng trong bài viết nên được tính là một lần review thành công.
- Lỗi ngữ pháp lặp lại là tín hiệu học tập quý — tự động đề xuất thêm pattern đó vào hàng đợi.

## Requirements
Ba bước tăng dần độ tự do:
1. **Shadow** — gõ lại theo đoạn mẫu, có scaffold (điền chỗ trống, sắp xếp câu)
2. **Guided** — đề bài tương tự, cho khung + danh sách item bắt buộc dùng
3. **Free** — cùng chủ đề, không khung, LLM chấm

- Feedback có cấu trúc: item dùng đúng / dùng sai / bỏ sót + lỗi ngữ pháp theo catalog Phase 4
- Bài viết lưu lại, so sánh được giữa các lần
- Lỗi lặp lại → gợi ý thêm pattern vào hàng đợi

## Todo List
- [ ] Bảng `writing_tasks` + migration
- [ ] Chọn đoạn mẫu: ưu tiên nguồn có nhiều item đã Known
- [ ] UI 3 bước shadow → guided → free
- [ ] Prompt chấm bài + Zod schema feedback
- [ ] Liên kết feedback với catalog ngữ pháp
- [ ] Lịch sử bài viết + so sánh
- [ ] Đề xuất pattern từ lỗi lặp lại

## Success Criteria
- Hoàn thành trọn một bài 3 bước, nhận feedback chỉ rõ item nào dùng sai
- Đoạn mẫu đúng là văn bản người dùng đã nhập, không phải nội dung bịa
- Dùng đúng item mục tiêu được ghi nhận vào lịch sử review

## Risk Assessment
| Rủi ro | Giảm thiểu |
|---|---|
| Chấm bài tốn nhiều token, chạm free tier | Giới hạn độ dài bài; chấm theo yêu cầu, không tự động |
| Feedback chung chung, vô dụng | Ép Zod schema có cấu trúc; cấm nhận xét mơ hồ trong prompt |
| Không đủ nguồn đã học để lấy đoạn mẫu | Chặn tính năng đến khi có ≥ N nguồn đủ item Known; báo rõ lý do |

## Security Considerations
Bài viết của người dùng gửi qua LLM → áp dụng cùng redaction + cờ `sensitive` như Phase 1.

## Next Steps
Phase 6 — PWA & Notifications.
