# Phase 6 — PWA & Notifications

> Mức chi tiết: **khung**. Sẽ đặc tả đầy đủ khi tới lượt.

## Context Links
PRD §7.10 · Phụ thuộc: [Phase 2](phase-02-srs-core.md)

## Overview
- **Priority:** P2 — quyết định trực tiếp tới việc duy trì thói quen (mục tiêu G5)
- **Status:** ⬜ Chưa bắt đầu
- Cài được lên màn hình chính, nhắc ôn qua Web Push. Không cần app native.

## Key Insights
- Web Push trên iOS chỉ hoạt động khi PWA **đã được cài lên màn hình chính** (iOS 16.4+) —
  luồng onboarding phải hướng dẫn rõ, nếu không tính năng coi như không tồn tại trên iPhone.
- Nhắc sai còn tệ hơn không nhắc: nhắc khi hàng đợi rỗng sẽ bị tắt thông báo vĩnh viễn.

## Requirements
- PWA installable, chạy standalone, có offline shell
- Web Push nhắc ôn theo khung giờ người dùng chọn
- Nhắc thông minh: không nhắc khi hàng đợi rỗng, tối đa 1 lần/ngày
- Email tóm tắt tuần (tuỳ chọn)
- Rate limit endpoint phân tích (S8)

## Todo List
- [ ] Manifest + service worker + icon set
- [ ] Offline shell (hàng đợi ôn đã cache từ Phase 2)
- [ ] Đăng ký Web Push + lưu subscription
- [ ] Cron gửi nhắc theo múi giờ người dùng
- [ ] Logic nhắc thông minh
- [ ] Cài đặt thông báo trong settings
- [ ] Email tóm tắt tuần
- [ ] Rate limit `/api/analyze`

## Success Criteria
- Cài PWA lên iPhone, nhận được nhắc ôn đúng khung giờ đã đặt
- Hàng đợi rỗng → không có thông báo nào
- Mở app từ thông báo → vào thẳng phiên ôn

## Risk Assessment
| Rủi ro | Giảm thiểu |
|---|---|
| Web Push iOS nhiều hạn chế | Test sớm trên thiết bị thật; hướng dẫn cài PWA rõ ràng trong onboarding |
| Cron trên Vercel sai múi giờ | Lưu timezone người dùng; cron chạy theo giờ, lọc người nhận theo local time |
| Người dùng tắt thông báo vì bị làm phiền | Mặc định tối đa 1 lần/ngày; tắt/bật dễ dàng trong settings |

## Security Considerations
- VAPID key qua `vercel env`
- Nội dung push **không chứa văn bản tài liệu** — chỉ số lượng thẻ đến hạn
- Rate limit chống lạm dụng khi mở multi-user

## Next Steps
Phase 7 — React Native, chỉ làm khi PWA đã dùng thật và vẫn thấy thiếu.
