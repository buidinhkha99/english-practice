# Phase 7 — Mobile (React Native)

> Mức chi tiết: **khung sơ bộ**. Chỉ khởi động sau khi PWA (Phase 6) đã dùng thật và vẫn thấy chưa đủ.

## Context Links
PRD §9 ràng buộc kiến trúc · Phụ thuộc: [Phase 6](phase-06-pwa-notifications.md)

## Overview
- **Priority:** P3
- **Status:** ⬜ Chưa bắt đầu
- App React Native dùng lại toàn bộ backend, thêm FCM push và ôn offline đầy đủ.

## Key Insights
- **Phase này khả thi hay không đã được quyết định từ Phase 0**, không phải ở đây. Nếu logic
  nghiệp vụ đã lẫn vào React component thay vì nằm ở `lib/`, phase này biến thành viết lại backend.
- Trước khi bắt đầu, hãy hỏi thật: PWA thiếu gì cụ thể? Nếu chỉ vì push thì Phase 6 đã giải quyết rồi.
  Lý do chính đáng thường là: ôn offline dài ngày, widget, tích hợp hệ thống sâu.

## Requirements (sơ bộ)
- App RN gọi lại API hiện có, không có endpoint riêng cho mobile
- FCM push thay Web Push
- Ôn offline đầy đủ + đồng bộ hai chiều
- Đăng nhập dùng chung tài khoản với web

## Todo List
- [ ] Rà soát: logic nghiệp vụ còn nằm trong React component không? Rút về `lib/` trước
- [ ] Chốt lý do làm native (PWA thiếu gì) — nếu không trả lời được thì dừng
- [ ] Scaffold RN + auth dùng chung
- [ ] Màn ôn tập + đồng bộ offline
- [ ] FCM push
- [ ] Build + phát hành nội bộ

## Success Criteria
- Ôn tập trên app native, không mạng, rồi đồng bộ đúng khi có mạng
- Không phát sinh endpoint nào chỉ dành riêng cho mobile

## Risk Assessment
| Rủi ro | Giảm thiểu |
|---|---|
| Phải viết lại backend vì logic lẫn trong UI | Ràng buộc kiến trúc từ Phase 0; rà soát trước khi bắt đầu |
| Đồng bộ hai chiều gây xung đột state FSRS | Idempotency key + last-write-wins theo `reviewed_at` |
| Chi phí duy trì hai client cho dự án cá nhân | Cân nhắc kỹ — PWA có thể đã đủ; đây là lý do phase này xếp cuối |

## Security Considerations
- Token lưu trong secure storage của thiết bị
- Dữ liệu cache offline chứa trích đoạn tài liệu công việc → mã hoá at-rest, xoá khi đăng xuất

## Next Steps
Kết thúc roadmap hiện tại. Đánh giá lại toàn bộ sản phẩm dựa trên dữ liệu sử dụng thật.
