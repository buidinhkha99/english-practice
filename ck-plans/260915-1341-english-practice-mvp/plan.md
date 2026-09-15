# Plan — English Practice

> PRD nguồn: [`ck-docs/product-requirements.md`](../../ck-docs/product-requirements.md)
> Ngày tạo: 2026-09-15 · Trạng thái: chưa khởi động

## Mục tiêu

Ứng dụng web học tiếng Anh lấy tài liệu công việc của chính người dùng làm giáo trình:
nhập văn bản → phân tích từ/cụm/ngữ pháp → đối chiếu kho đã học → chọn học → SRS với ví dụ thật.

## Quyết định đã chốt

| Hạng mục | Quyết định |
|---|---|
| Phạm vi | Solo-first, schema có `user_id` sẵn để mở multi-user sau |
| Stack | Next.js 16 App Router + React 19 + TS, Postgres (Neon) + Drizzle, deploy Vercel |
| AI | Groq free tier, sau lớp `LlmProvider` — đổi sang Ollama local hoặc Anthropic bằng config |
| Mobile | Web trước, API-first; React Native ở Phase 7 |
| SRS | FSRS-5 qua `ts-fsrs` |

## Phases

| # | Phase | Trạng thái | Ghi chú |
|---|---|---|---|
| 0 | [Foundation](phase-00-foundation.md) | ⬜ Chưa bắt đầu | Scaffold, DB, auth, CI/deploy |
| 1 | [Capture & Analyze](phase-01-capture-and-analyze.md) | ⬜ Chưa bắt đầu | ⭐ Lõi sản phẩm — pipeline + Triage |
| 2 | [SRS Core](phase-02-srs-core.md) | ⬜ Chưa bắt đầu | ⭐ FSRS, phiên ôn, Home dashboard |
| 3 | [Dictionary & Enrichment](phase-03-dictionary-enrichment.md) | ⬜ Chưa bắt đầu | Tra cứu, gloss VN, TTS, thẻ nghe |
| 4 | [Grammar](phase-04-grammar.md) | ⬜ Chưa bắt đầu | Catalog cố định + phân loại pattern |
| 5 | [Writing & Shadowing](phase-05-writing-shadowing.md) | ⬜ Chưa bắt đầu | Shadow → guided → free + feedback |
| 6 | [PWA & Notifications](phase-06-pwa-notifications.md) | ⬜ Chưa bắt đầu | Web Push nhắc ôn |
| 7 | [Mobile React Native](phase-07-mobile-react-native.md) | ⬜ Chưa bắt đầu | App native + FCM |

**MVP = Phase 0 + 1 + 2.** Chỉ khi ba phase này xong mới có vòng lặp dùng được hàng ngày.

## Phụ thuộc

```
Phase 0 ──► Phase 1 ──► Phase 2 ──┬──► Phase 3 ──► Phase 5
                                   ├──► Phase 4 ──┘
                                   └──► Phase 6 ──► Phase 7
```

- Phase 4 (Grammar) cần Phase 2 vì thẻ ngữ pháp dùng chung hàng đợi SRS
- Phase 5 (Writing) cần Phase 3 + 4 để có đủ item và pattern làm mục tiêu bài viết
- Phase 7 chỉ khả thi nếu ràng buộc kiến trúc "logic ở `/lib`" được giữ từ Phase 0

## Mức chi tiết

Phase 0-2 đặc tả đầy đủ (sắp thực thi). Phase 3-7 ở mức khung — sẽ chi tiết hoá khi tới lượt,
tránh lên kế hoạch giả định quá xa khi chưa có dữ liệu sử dụng thật.

## Rủi ro cần theo dõi xuyên suốt

| Rủi ro | Phase chịu trách nhiệm |
|---|---|
| 🔴 Rò rỉ tài liệu công ty ra LLM | Phase 1 (redaction, cờ `sensitive`) |
| 🔴 Bùng nổ item ngữ pháp | Phase 4 (catalog slug đóng) |
| 🟠 Quá tải hàng đợi → bỏ cuộc | Phase 2 (trần item mới/ngày) |
| 🟠 Groq rate limit | Phase 1 (tầng A độc lập + cache 2 lớp) |
| 🟡 Logic lẫn vào React → viết lại cho mobile | Mọi phase (review khi merge) |

## Câu hỏi mở

6 câu hỏi chưa chốt — xem §15 của PRD. Ảnh hưởng sớm nhất: **Q2** (nguồn lexicon cụm công việc,
cần trước Phase 1) và **Q6** (ngân sách phút/ngày, cần trước Phase 2).
