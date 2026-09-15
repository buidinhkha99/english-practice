# Phase 1 — Capture & Analyze ⭐

## Context Links
- PRD §7.1 Capture, §7.2 Analyze, §7.3 Triage, §10 AI Pipeline, §11 Bảo mật
- Phụ thuộc: [Phase 0](phase-00-foundation.md)

## Overview
- **Priority:** P0 — đây là lõi sản phẩm, phase quyết định app có giá trị hay không
- **Status:** ⬜ Chưa bắt đầu
- Nhập văn bản công việc → phân tích 2 tầng → trích từ/cụm/ngữ pháp → đối chiếu kho đã học
  → màn Triage cho người dùng chọn học gì.

## Key Insights
- **Pipeline 2 tầng là quyết định kiến trúc quan trọng nhất phase này.** Tầng A (NLP cục bộ) làm
  ~60-70% khối lượng miễn phí và luôn chạy được; tầng B (LLM) chỉ xử lý phần khó. Không có tầng A,
  app chết ngay khi chạm Groq rate limit.
- **Triage là màn hình quan trọng nhất của toàn app.** Nó hiện thực hoá cả nguyên lý "thu hẹp là
  tính năng" lẫn "người dùng giữ quyền quyết định". Làm ẩu màn này thì mọi thứ phía sau vô nghĩa.
- **Redaction không được để sang phase sau.** Ngay từ lần dán đầu tiên, tài liệu công ty đã rời máy.
  Đây là rủi ro R1 — mức Cao.
- Ngữ pháp ở phase này chỉ **trích và lưu occurrence**, chưa làm thẻ ôn (đó là Phase 4).
  Nhưng catalog phải seed ở đây vì LLM cần enum slug để phân loại.

## Requirements
**Functional**
- Dán văn bản ≤ 20.000 ký tự + metadata (tiêu đề, loại nguồn, ngày, tag)
- Redaction preview trước khi gửi LLM, sửa được
- Cờ `sensitive` → bỏ qua hoàn toàn tầng B
- Phân tích streaming có progress; kết quả nhóm 4 khối (Mới / Đang học / Đã thuộc / Bỏ qua)
- Hai chế độ triage: tự động đề xuất N item, hoặc liệt kê toàn bộ
- Quét tự đánh giá 3 mức: Đã biết / Không chắc / Chưa biết
- Trần item mới/ngày suy ra từ ngân sách 30 phút/ngày (PRD §7.4.1) — mặc định ~15
- Chống trùng nguồn bằng `content_hash`

**Non-functional**
- Phân tích 2.000 từ < 20s (p50)
- Dán lại văn bản y hệt = 0 token LLM
- Lỗi LLM → trả kết quả tầng A + cờ `degraded`, không crash

## Architecture
```
lib/nlp/          wink-nlp: tách câu, tokenize, lemma, POS, NER
  sentences.ts    tách câu + giữ offset
  tokens.ts       lemma + POS + lọc stopword
  frequency.ts    tra freq_band từ dataset tĩnh
  candidates.ts   n-gram + lexicon cụm công việc
lib/redact/       phát hiện & thay thế PII trước khi gửi LLM
lib/llm/
  provider.ts     interface LlmProvider          ← KHÔNG import SDK ở ngoài đây
  providers/groq.ts | ollama.ts | anthropic.ts | mock.ts
  schemas.ts      Zod schema cho mọi output
  tasks/          confirm-phrases · classify-grammar · disambiguate · gloss-vi
lib/analysis/
  pipeline.ts     điều phối tầng A → tầng B → ghi DB
  priority.ts     công thức chấm điểm (trọng số ở config)
  cache.ts        cache theo content_hash + theo lemma
app/analyze/      UI nhập + progress
app/triage/[id]/  màn Triage
```

## Related Code Files
**Tạo mới**
- `lib/db/schema.ts` (mở rộng): `lexemes`, `phrases`, `grammar_patterns`, `items`, `occurrences`,
  `senses`, `analyses`, `user_items` (chỉ cột trạng thái, FSRS ở Phase 2)
- Toàn bộ `lib/nlp/`, `lib/llm/`, `lib/analysis/`, `lib/redact/`
- `app/analyze/page.tsx`, `app/triage/[sourceId]/page.tsx`
- `app/api/analyze/route.ts` — streaming
- `data/frequency-bands.json`, `data/business-phrases.json`, `data/grammar-patterns.json`

## Implementation Steps
1. Mở rộng schema Drizzle theo §8 PRD; migration
2. `lib/nlp/` với wink-nlp: tách câu giữ offset, lemma + POS, lọc stopword/tên riêng
3. Dataset tần suất tĩnh (top 20k) → `freq_band` 1-10
4. Seed `data/business-phrases.json` — LLM sinh theo chủ đề công việc → **duyệt tay** → chuẩn hoá lemma
   → commit. Mục tiêu ~600-800 mục. Quy trình đầy đủ ở PRD §7.2.1
5. Bắt cụm ứng viên: n-gram 2-4 + đối chiếu lexicon + phrasal verb detection
6. Seed `grammar_patterns` (~150-200 slug, CEFR A2-C1) — nghiêng về register công việc
7. `lib/redact/`: email, phone, URL nội bộ, token/key, tên người qua wink NER → placeholder
8. `lib/llm/provider.ts` + `providers/groq.ts` + `providers/mock.ts`; chọn provider bằng env
9. 4 task LLM với Zod schema: xác nhận cụm, phân loại ngữ pháp (enum slug đóng), phân biệt nghĩa, gloss VN
10. Batching theo câu (10-20 câu/request) + retry backoff khi 429
11. Cache 2 lớp: `content_hash` cho phân tích, lemma/sense cho enrich
12. `lib/analysis/priority.ts` — công thức §7.2, trọng số trong config
13. API streaming `/api/analyze` — phát progress theo bước
14. UI nhập + redaction preview + cờ `sensitive`
15. Màn Triage: 4 khối, 2 chế độ, quét tự đánh giá, thao tác hàng loạt, lưu draft
16. Ghi `user_items` khi người dùng chốt (status only — lịch FSRS ở Phase 2)

## Todo List
- [ ] Schema mở rộng + migration
- [ ] `lib/nlp/` tách câu + lemma + POS + offset chính xác
- [ ] Dataset tần suất + `freq_band`
- [ ] Lexicon cụm công việc: sinh theo lô chủ đề, duyệt tay từng lô, ~600-800 mục đã duyệt
- [ ] Seed grammar catalog ~150-200 pattern
- [ ] `lib/redact/` + preview UI
- [ ] `LlmProvider` interface + Groq + mock
- [ ] 4 task LLM với Zod schema, enum slug đóng cho ngữ pháp
- [ ] Batching + backoff 429
- [ ] Cache 2 lớp
- [ ] Priority scoring
- [ ] API streaming + UI progress
- [ ] Màn Triage đầy đủ (4 khối, 2 chế độ, self-assessment, bulk, draft)
- [ ] Trần item mới/ngày
- [ ] Test: pipeline chạy trọn với mock provider, không cần mạng

## Success Criteria
- Dán một biên bản họp thật 1.500-2.500 từ → trong 20s thấy danh sách item phân 4 khối
- Bỏ tick vài item, chốt → `user_items` được tạo đúng, mỗi item có ≥ 1 occurrence trỏ về câu thật
- Dán lại y hệt văn bản đó → 0 token LLM tiêu thụ (kiểm qua bảng `analyses`)
- Tắt mạng / dùng key Groq sai → vẫn ra kết quả tầng A kèm cờ `degraded`
- Bật cờ `sensitive` → xác nhận không có request nào rời máy
- Redaction: dán văn bản có email + số điện thoại → chúng bị thay placeholder trước khi gửi

## Risk Assessment
| Rủi ro | Mức | Giảm thiểu |
|---|---|---|
| Rò rỉ tài liệu công ty (R1) | 🔴 | Redaction bắt buộc + `sensitive` + cảnh báo lần đầu |
| Bùng nổ item ngữ pháp (R2) | 🔴 | Enum slug đóng, confidence ≥ 0.6, LLM chỉ phân loại |
| Groq rate limit (R4) | 🟠 | Tầng A độc lập, cache 2 lớp, batching, backoff |
| Chất lượng trích cụm kém (R5) | 🟠 | Lexicon seed sẵn, đo tỷ lệ flag sau 2 tuần dùng thật |
| Offset lệch sau tách câu → ví dụ hỏng | 🟠 | Test tính bất biến: `raw_text.slice(start,end) === surface_form` |

## Security Considerations
- Redaction chạy **trước** mọi lời gọi LLM, không có đường tắt
- `raw_text` không bao giờ vào log/telemetry/error tracking (S6)
- Cờ `sensitive` phải được kiểm ở tầng `pipeline.ts`, không phải ở UI
- Xoá nguồn → cascade xoá `analyses`, `occurrences`, `source_sentences`

## Next Steps
Phase 2 — SRS Core. Cần `user_items` và `occurrences` từ phase này để dựng hàng đợi ôn.
