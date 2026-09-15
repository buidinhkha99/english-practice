# Phase 3 — Dictionary & Enrichment

> Mức chi tiết: **khung**. Sẽ đặc tả đầy đủ khi tới lượt — sau 2-4 tuần dùng MVP thật.

## Context Links
PRD §7.6 Dictionary · Phụ thuộc: [Phase 2](phase-02-srs-core.md)

## Overview
- **Priority:** P1 — làm app thực sự dùng được hàng ngày, không chỉ sau mỗi lần dán văn bản
- **Status:** ⬜ Chưa bắt đầu
- Tra cứu từ/cụm bất kỳ ngoài luồng phân tích, làm giàu dữ liệu nghĩa, thêm thẻ nghe.

## Key Insights
- Điểm khác biệt so với từ điển thường: **occurrence thật của người dùng hiện trước ví dụ từ điển**.
  Tra "leverage" thì thấy ngay câu sếp mình đã dùng trong họp tuần trước.
- Enrich cache vào bảng catalog dùng chung → mỗi từ chỉ trả tiền AI một lần trọn đời.
- Thẻ nghe quan trọng hơn vẻ ngoài của nó: mục tiêu số một của người dùng là **hiểu cuộc họp**.

## Requirements
- Tra cứu từ/cụm, hiển thị IPA + POS + các nghĩa + CEFR/tần suất
- Occurrence thật ưu tiên trên ví dụ từ điển
- Một chạm "Thêm vào học" từ kết quả tra
- Gloss tiếng Việt cache theo sense
- TTS qua Web Speech API + thẻ nghe trong phiên ôn
- Upload `.txt` / `.md` / `.docx` / `.pdf` và nhập từ URL (F1.6, F1.7)
- Export toàn bộ dữ liệu ra JSON (S9)
- Lịch sử tra cứu; tra 3 lần chưa học → gợi ý thêm vào

## Architecture (phác thảo)
```
lib/dictionary/   lookup, nguồn ngoài, cache
lib/tts/          bọc Web Speech API
lib/import/       parser docx/pdf/url
app/dictionary/   UI tra cứu
```

## Todo List
- [ ] Tích hợp dictionaryapi.dev + Wiktionary, cache vào `senses`
- [ ] UI tra cứu + occurrence thật lên đầu
- [ ] Gloss VN qua LLM, cache theo sense
- [ ] TTS + thẻ nghe trong phiên ôn
- [ ] Import file + URL
- [ ] Export JSON
- [ ] Lịch sử tra cứu + gợi ý

## Success Criteria
- Tra một từ đã gặp trong tài liệu → thấy câu thật của mình trước tiên
- Thêm từ vào học trực tiếp từ màn tra cứu, không cần qua Triage
- Xuất JSON và đọc lại được đầy đủ dữ liệu

## Risk Assessment
| Rủi ro | Giảm thiểu |
|---|---|
| API từ điển miễn phí rate limit / chết | Cache vĩnh viễn; nhiều nguồn dự phòng; LLM là đường cuối |
| Web Speech API giọng kém trên một số nền tảng | Chấp nhận ở v1; đánh giá TTS trả phí ở v2 |
| Parse PDF ra text rác | Xem trước text đã parse, cho sửa trước khi lưu |

## Security Considerations
- Import từ URL: chặn địa chỉ nội bộ (SSRF), giới hạn kích thước, timeout
- Export JSON chỉ chứa dữ liệu của chính người dùng

## Next Steps
Phase 4 — Grammar. Có thể làm song song nếu ưu tiên ngữ pháp hơn từ điển.
