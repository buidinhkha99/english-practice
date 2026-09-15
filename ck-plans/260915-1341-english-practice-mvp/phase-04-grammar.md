# Phase 4 — Grammar

> Mức chi tiết: **khung**. Sẽ đặc tả đầy đủ khi tới lượt.

## Context Links
PRD §7.7 Grammar · Phụ thuộc: [Phase 2](phase-02-srs-core.md) (hàng đợi SRS dùng chung)

## Overview
- **Priority:** P1 — người dùng nêu ngữ pháp là nhu cầu cốt lõi ngang từ vựng
- **Status:** ⬜ Chưa bắt đầu
- Biến pattern ngữ pháp đã trích ở Phase 1 thành item học được thật sự, với thẻ ôn riêng.

## Key Insights
- **Catalog slug đóng là toàn bộ lý do phase này khả thi.** Nếu để LLM tự mô tả cấu trúc,
  cùng một pattern sẽ sinh ra hàng chục tên gọi và SRS thành rác (rủi ro R2, mức Cao).
- Catalog phải nghiêng về **register công việc**, không phải ngữ pháp luyện thi:
  hedging, cleft, bị động/danh hoá, modal quá khứ, điều kiện thương lượng.
- Thẻ ngữ pháp phải là **transform/produce**, không phải trắc nghiệm định nghĩa —
  nhận ra luật khác hoàn toàn với dùng được luật.

## Requirements
- Seed catalog ~150-200 pattern, CEFR A2-C1, có versioning (F7.1)
- LLM phân loại ràng buộc enum slug, confidence ≥ 0.6 (F7.2)
- Mỗi pattern user học lưu occurrence thật từ tài liệu của họ (F7.3)
- Thẻ ngữ pháp dạng transform/produce (F7.4)
- Trang chi tiết pattern: giải thích VN + mọi câu thật của user dùng pattern đó (F7.5)
- Lọc theo dải CEFR (F7.6)

## Todo List
- [ ] Chốt nguồn catalog + kiểm tra bản quyền (câu hỏi mở Q5)
- [ ] Seed ~150-200 pattern qua migration, có `catalog_version`
- [ ] Tinh chỉnh prompt phân loại + đo độ chính xác trên tập câu tự gán nhãn
- [ ] Thẻ ngữ pháp transform/produce trong phiên ôn
- [ ] Trang chi tiết pattern
- [ ] Bộ lọc CEFR trong cài đặt

## Success Criteria
- Dán một tài liệu → pattern trích ra ánh xạ đúng slug, không sinh slug lạ
- Học một pattern → thẻ ôn yêu cầu *tạo* câu dùng pattern, không phải chọn định nghĩa
- Trang pattern hiện đủ mọi câu thật của user dùng cấu trúc đó
- Đo trên 100 câu tự gán nhãn: độ chính xác phân loại > 80%

## Risk Assessment
| Rủi ro | Mức | Giảm thiểu |
|---|---|---|
| Bùng nổ item ngữ pháp (R2) | 🔴 | Enum slug đóng + ngưỡng confidence + không cho tạo slug mới |
| Catalog có bản quyền (Q5) | 🟠 | Tự soạn hoặc dùng nguồn mở; kiểm tra trước khi seed |
| Phân loại sai → học nhầm cấu trúc | 🟠 | Luôn hiện câu gốc; nút gắn cờ; đo độ chính xác trước khi bật rộng |
| Model nhỏ (local) phân loại kém | 🟡 | Ngữ pháp giữ ở Groq 70B; local chỉ dùng cho nguồn `sensitive` |

## Security Considerations
Không phát sinh mới ngoài §11 PRD — pipeline phân loại đã đi qua redaction từ Phase 1.

## Next Steps
Phase 5 — Writing & Shadowing, cần catalog pattern để chấm lỗi ngữ pháp có cấu trúc.
