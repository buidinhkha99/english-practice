# Phase 2 — SRS Core ⭐

## Context Links
- PRD §7.4 SRS, §7.5 Home dashboard, §12 Metrics
- Phụ thuộc: [Phase 1](phase-01-capture-and-analyze.md)

## Overview
- **Priority:** P0 — hoàn tất MVP
- **Status:** ⬜ Chưa bắt đầu
- Biến item đã chọn ở Triage thành lịch ôn thật: FSRS scheduling, phiên ôn với thẻ cloze từ câu gốc,
  Home dashboard hiển thị hàng đợi đến hạn.

## Key Insights
- **Thẻ cloze dùng câu thật là mặc định**, không phải thẻ định nghĩa. Đây là điểm khác biệt so với Anki:
  ngữ cảnh đến từ chính tài liệu của người dùng (nguyên lý #1).
- **SRS engine phải là hàm thuần**, không chạm DB. Nhận `(state, rating)` trả `state mới`.
  Cần thiết để test được và để đổi thuật toán sau này mà không đụng tầng dữ liệu.
- `fsrs_state` lưu jsonb — FSRS còn tiến hoá, tránh migrate cột mỗi lần đổi version.
- **Ngân sách thời gian là knob duy nhất** (mặc định 30 phút/ngày, PRD §7.4.1). Trần item mới/ngày
  được *suy ra* từ nó, không phải cài đặt riêng — hai knob độc lập chắc chắn mâu thuẫn sau vài tuần.
- **Giãn hàng đợi tồn đọng** (F4.12) quyết định người dùng có quay lại sau kỳ nghỉ hay không.
  Mở app thấy 400 thẻ dồn một ngày là bỏ luôn.

## Requirements
**Functional**
- Chuyển trạng thái `new` → `learning` → `review` → `known` (+ `suspended`, `ignored`)
- FSRS-5 qua `ts-fsrs`, 4 mức chấm `Again` / `Hard` / `Good` / `Easy`
- Thẻ cloze từ câu gốc (mặc định) + thẻ nhận diện + thẻ sản sinh
- Ngân sách phút/ngày trong settings (dải 5-120, mặc định 30); trần item mới suy ra và hiển thị
- Trộn item đến hạn và item mới theo tỷ lệ config (mặc định 80/20), tổng không vượt ngân sách
- Tồn đọng giãn ra nhiều ngày thay vì dồn một hôm; chạm ngân sách thì đề nghị dừng, không chặn cứng
- Mỗi thẻ hiển thị nguồn gốc, click về được nguồn
- Gắn cờ item sai ngay trong lúc ôn
- Dừng phiên giữa chừng không mất tiến độ
- Home: hàng đợi hôm nay, ô dán nhanh, chờ triage, tiến độ, nguồn gần đây

**Non-functional**
- Home tải < 1.5s, số liệu từ query tổng hợp, không N+1
- Phiên ôn hoạt động offline sau khi tải hàng đợi
- Phím tắt `1-4` chấm điểm, `Space` lật thẻ

## Architecture
```
lib/srs/
  scheduler.ts    hàm thuần bọc ts-fsrs — KHÔNG import DB
  states.ts       chuyển trạng thái + định nghĩa `known`
  queue.ts        dựng hàng đợi: đến hạn + item mới theo tỷ lệ
  cards.ts        sinh nội dung thẻ từ item + occurrence
app/review/       phiên ôn (client-heavy, cache hàng đợi để chạy offline)
app/page.tsx      Home dashboard (RSC, một query tổng hợp)
lib/stats/        số liệu dashboard
```

## Related Code Files
**Sửa**
- `lib/db/schema.ts` — `user_items` thêm `fsrs_state` jsonb, `due_at`, `stability`, `difficulty`,
  `reps`, `lapses`; bảng `reviews` mới
- `app/page.tsx` — thay trang rỗng bằng dashboard thật

**Tạo mới**
- Toàn bộ `lib/srs/`, `lib/stats/`
- `app/review/page.tsx`, `components/review/*`
- `app/api/review/route.ts` — ghi rating, trả state mới

## Implementation Steps
1. Mở rộng `user_items` + tạo bảng `reviews`; migration
2. `lib/srs/scheduler.ts` — bọc `ts-fsrs`, hàm thuần, unit test đầy đủ trước khi nối DB
3. `lib/srs/states.ts` — ngưỡng `known` (interval ≥ 60 ngày + ≥ 3 lần đúng liên tiếp, xem Q3)
4. `lib/srs/queue.ts` — query item đến hạn + item mới, trộn theo tỷ lệ config, tôn trọng trần/ngày
5. `lib/srs/cards.ts` — sinh cloze từ occurrence: khoét `surface_form` khỏi câu gốc bằng offset
6. UI phiên ôn: lật thẻ, 4 nút chấm, phím tắt, progress, nút gắn cờ
7. Cache hàng đợi phía client để ôn offline; hàng chờ ghi rating khi mất mạng, đồng bộ lại sau
8. `lib/stats/` — một query tổng hợp cho Home (đến hạn, streak, độ chính xác 7 ngày, chờ triage)
9. Home dashboard theo thứ tự ưu tiên §7.5 PRD
10. Badge đến hạn trên tab title
11. Trạng thái rỗng dẫn vào luồng nhập đầu tiên

## Todo List
- [ ] Schema `user_items` FSRS + bảng `reviews`
- [ ] `lib/srs/scheduler.ts` hàm thuần + unit test
- [ ] Chuyển trạng thái + ngưỡng `known` (chốt Q3)
- [ ] Ngân sách phút/ngày + công thức suy ra trần item mới
- [ ] Dựng hàng đợi + tỷ lệ trộn + giãn tồn đọng
- [ ] Sinh thẻ cloze từ occurrence bằng offset
- [ ] Thẻ nhận diện + sản sinh
- [ ] UI phiên ôn + phím tắt + gắn cờ
- [ ] Ôn offline + hàng chờ đồng bộ
- [ ] `lib/stats/` một query tổng hợp
- [ ] Home dashboard đầy đủ 5 khối
- [ ] Badge tab title

## Success Criteria
- Chốt 20 item ở Triage → hôm sau Home báo đúng 20 thẻ đến hạn
- Ôn hết phiên, chấm hỗn hợp 4 mức → `due_at` các item giãn ra khác nhau theo đúng FSRS
- Chấm `Again` → item quay lại trong cùng phiên
- Đóng tab giữa phiên, mở lại → tiến độ còn nguyên
- Bật chế độ máy bay giữa phiên → vẫn ôn tiếp được, bật mạng lại thì rating đồng bộ đủ
- Thẻ cloze hiển thị đúng câu gốc, ô khoét đúng vị trí từ
- Home tải < 1.5s với 2.000 `user_items`

## Risk Assessment
| Rủi ro | Mức | Giảm thiểu |
|---|---|---|
| Quá tải hàng đợi → bỏ cuộc (R3) | 🟠 | Ngân sách 30 phút/ngày suy ra trần item mới; giãn tồn đọng |
| Cloze khoét sai vị trí do offset lệch | 🟠 | Test bất biến từ Phase 1; fallback sang thẻ nhận diện nếu offset không khớp |
| Đồng bộ offline gây ghi trùng rating | 🟠 | Idempotency key trên mỗi review; ghi trùng thì bỏ qua |
| Ngưỡng `known` sai → coverage score ảo | 🟡 | Đánh dấu Q3 là giả định, xem lại sau 4-6 tuần dữ liệu thật |

## Security Considerations
- Mọi query hàng đợi lọc theo `user_id` — kể cả khi hiện chỉ có một người dùng
- API `/api/review` xác thực chủ sở hữu `user_item` trước khi ghi
- Hàng đợi cache phía client chứa câu trích từ tài liệu công việc → dùng `sessionStorage`, xoá khi đăng xuất

## Next Steps
MVP hoàn tất. Dùng thật ít nhất 2-4 tuần trước khi sang Phase 3 — dữ liệu sử dụng thật sẽ trả lời
Q1/Q3 và điều chỉnh trọng số priority chính xác hơn mọi phỏng đoán lúc này. `thời gian/thẻ` thật
sẽ thay hằng số 12s, làm trần item mới sát với người dùng hơn.
