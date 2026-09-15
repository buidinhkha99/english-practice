# PRD — English Practice (Work-Scoped English Learning)

| | |
|---|---|
| **Phiên bản** | 1.0 |
| **Ngày** | 2026-09-15 |
| **Trạng thái** | Draft — chờ review |
| **Owner** | buidinhkha99 |
| **Repo** | `buidinhkha99/english-practice` |

---

## 1. TL;DR

Ứng dụng web học tiếng Anh **lấy chính tài liệu công việc của người dùng làm giáo trình**.

Thay vì học từ vựng theo list có sẵn, người dùng dán vào app những gì họ thực sự gặp — biên bản họp,
email, tài liệu kỹ thuật, transcript. App phân tích văn bản đó, tách ra **từ / cụm từ / cấu trúc ngữ pháp**,
đối chiếu với những gì người dùng đã học để chỉ nổi bật phần *chưa biết*, để người dùng chọn,
rồi đưa vào **SRS (spaced repetition)** — mỗi item mang theo **chính câu thật** mà nó xuất hiện làm ví dụ.

Khác biệt cốt lõi: **corpus là của chính người dùng**. Phạm vi học tự thu hẹp vào đúng miền công việc,
nên mỗi phút học đổi trực tiếp thành khả năng hiểu cuộc họp tuần sau.

---

## 2. Bối cảnh & Vấn đề

Người dùng làm việc trong môi trường dùng tiếng Anh dày đặc: họp, trao đổi công việc, đọc tài liệu.
Vấn đề không phải là "không biết tiếng Anh" mà là:

| Vấn đề | Hệ quả |
|---|---|
| Từ/cụm gặp trong họp trôi qua, không được ghi lại | Gặp lại lần 3-4 vẫn không chắc nghĩa |
| App học tiếng Anh phổ thông dạy từ không liên quan công việc | Học nhiều, dùng được ít |
| Ngữ pháp học rời rạc, không gắn ngữ cảnh | Hiểu luật nhưng không nhận ra khi nghe |
| Không biết mình *đã* biết gì | Học lại từ đã thuộc, bỏ sót từ chưa thuộc |
| Tài liệu công việc là nguồn học tốt nhất nhưng không có công cụ khai thác | Nguồn học tốt nhất bị lãng phí |

**Insight trung tâm:** tài liệu công việc hàng ngày đã là một corpus học tập hoàn hảo — đúng miền,
đúng register, đúng tần suất, có sẵn ngữ cảnh. Thiếu duy nhất là công cụ biến nó thành bài học.

---

## 3. Mục tiêu & Phi mục tiêu

### 3.1 Mục tiêu

| # | Mục tiêu | Đo bằng |
|---|---|---|
| G1 | Biến văn bản công việc thành item học được, trong < 60 giây | Thời gian từ paste → item đầu tiên vào hàng đợi |
| G2 | Không bao giờ bắt học lại thứ đã thuộc | % item trùng lặp trong phiên ôn |
| G3 | Mỗi item học luôn có ví dụ thật từ nguồn của chính người dùng | % item có ≥ 1 occurrence thật |
| G4 | Tăng độ phủ hiểu văn bản công việc theo thời gian | Coverage score trên văn bản mới (§7.9) |
| G5 | Duy trì thói quen ôn tập hàng ngày | Streak, tỷ lệ ngày hoàn thành hàng đợi |

### 3.2 Phi mục tiêu (v1)

- ❌ Không làm app học tiếng Anh tổng quát (IELTS/TOEIC, luyện thi)
- ❌ Không làm social / leaderboard / gamification nặng
- ❌ Không làm speaking/pronunciation scoring (có thể xét ở v2)
- ❌ Không làm marketplace nội dung, không làm khoá học biên soạn sẵn
- ❌ Không multi-tenant thương mại hoá ở v1 (nhưng schema không chặn đường — §8)
- ❌ Không mobile native ở v1 (API-first để RN dùng lại — §7.10, Phase 7)

---

## 4. Người dùng

### 4.1 Persona chính — "Kỹ sư trong môi trường tiếng Anh"

- Trình độ đọc/hiểu khá, nhưng **nghe họp** và **diễn đạt chủ động** còn hụt
- Tiếp xúc 5-20 trang tiếng Anh công việc/ngày
- Thời gian học rất hạn chế: 10-20 phút/ngày, thường xen kẽ giữa các task
- Có kỷ luật kỹ thuật, chấp nhận công cụ hơi thô nếu hiệu quả cao
- Không muốn học từ vựng "cái bàn, con mèo" — muốn học `circle back`, `align on scope`, `it might be worth + V-ing`

### 4.2 Use cases

| ID | Tình huống | Hành vi mong muốn |
|---|---|---|
| UC1 | Vừa họp xong, có bản note/transcript | Dán vào, lấy 10 item đáng học nhất, thêm vào hàng đợi |
| UC2 | Sáng mở máy, có 30 thẻ đến hạn | Ôn 10 phút trên home, xong là đóng |
| UC3 | Đọc tài liệu, gặp cụm lạ | Tra nhanh trong app, thấy nghĩa + ngữ cảnh, một chạm thêm vào học |
| UC4 | Chuẩn bị họp về chủ đề mới | Dán agenda/tài liệu, xem mình chưa biết gì, học trước |
| UC5 | Muốn biết mình tiến bộ chưa | Dán văn bản mới, app báo "bạn đã biết 87% từ trong này" |
| UC6 | Muốn viết email/báo cáo tốt hơn | Shadow đoạn mẫu đã học, rồi tự viết phần còn lại, nhận feedback |

---

## 5. Nguyên lý sản phẩm

Năm nguyên tắc này quyết định mọi trade-off thiết kế phía sau:

1. **Corpus của người dùng là nguồn chân lý.** Mọi item học phải truy được về câu thật, nguồn thật, ngày thật.
   Không có ví dụ bịa khi chưa cạn ví dụ thật.
2. **Thu hẹp là tính năng, không phải giới hạn.** App chủ động *loại bỏ* thứ không đáng học
   (từ quá hiếm, từ đã thuộc, từ ngoài miền công việc) hơn là nhồi thêm.
3. **Ma sát thấp ở đầu vào, ma sát cao có chủ đích ở đầu ra.** Dán văn bản phải nhanh như Ctrl+V.
   Ngược lại, thẻ ôn nên buộc *sản sinh* (gõ, điền, nghe) chứ không chỉ nhận diện.
4. **Người dùng luôn giữ quyền quyết định học gì.** AI đề xuất và xếp hạng; người dùng chốt.
   Không bao giờ tự động nhồi item vào hàng đợi mà không xác nhận.
5. **Xuống cấp có duyên (graceful degradation).** Hết quota AI, mất mạng, model lỗi — app vẫn phải
   phân tích được ở mức cơ bản và vẫn ôn tập được. AI làm app *tốt hơn*, không phải điều kiện để app *chạy*.

---

## 6. Vòng lặp cốt lõi

```
┌─────────────┐
│  1. CAPTURE │  Dán / upload văn bản công việc thật
│             │  (note họp, email, doc, transcript, link)
└──────┬──────┘
       ▼
┌─────────────┐
│  2. ANALYZE │  Tầng NLP cục bộ  → câu, token, lemma, POS
│             │  Tầng LLM (Groq)  → cụm từ, ngữ pháp, nghĩa theo ngữ cảnh
└──────┬──────┘
       ▼
┌─────────────┐
│  3. TRIAGE  │  Đối chiếu kho đã học của user:
│             │    ✓ Đã thuộc   → ẩn bớt
│             │    ~ Đang học   → đánh dấu "gặp lại"
│             │    + Mới        → xếp hạng ưu tiên, user chọn
└──────┬──────┘
       ▼
┌─────────────┐
│  4. LEARN   │  Item vào SRS, mang theo câu gốc làm ví dụ
│             │  Thẻ: cloze câu thật / nghe / sản sinh / ngữ pháp
└──────┬──────┘
       ▼
┌─────────────┐
│  5. REVIEW  │  Hàng đợi đến hạn trên Home, nhắc qua web push
│             │  FSRS tính lịch, tự điều chỉnh theo độ khó thật
└──────┬──────┘
       ▼
┌─────────────┐
│  6. PRODUCE │  (Phase 5) Shadowing đoạn đã học → tự viết → AI feedback
└──────┬──────┘
       │
       └──────► quay lại 1 với văn bản mới, coverage tăng dần
```

---

## 7. Đặc tả tính năng

### 7.1 Capture — Nhập văn bản

**Mục đích:** đưa văn bản công việc vào hệ thống với ma sát tối thiểu.

| Yêu cầu | Chi tiết | Phase |
|---|---|---|
| F1.1 | Ô dán văn bản ngay trên Home, tối đa 20.000 ký tự | 1 |
| F1.2 | Gán metadata: tiêu đề (auto-gen nếu bỏ trống), loại nguồn, ngày, tag/project | 1 |
| F1.3 | Loại nguồn: `meeting` / `email` / `doc` / `chat` / `article` / `transcript` / `other` | 1 |
| F1.4 | Lưu nguyên văn bản gốc — mọi occurrence trỏ về offset trong bản gốc | 1 |
| F1.5 | Chống trùng: hash nội dung, cảnh báo nếu đã nhập nguồn giống hệt | 1 |
| F1.6 | Upload `.txt` / `.md` / `.docx` / `.pdf` | 3 |
| F1.7 | Nhập từ URL (fetch + readability extract) | 3 |
| F1.8 | **Redaction pass** trước khi gửi LLM — xem §11 | 1 |

**Giới hạn có chủ đích:** một lần nhập = một nguồn. Không hỗ trợ nhập hàng loạt ở v1 — nhập hàng loạt
tạo ra hàng trăm item và phá vỡ nguyên tắc #2 (thu hẹp).

---

### 7.2 Analyze — Phân tích văn bản

Đây là trái tim kỹ thuật của sản phẩm. Pipeline **2 tầng** để kiểm soát chi phí (§10).

#### Tầng A — NLP cục bộ (không tốn AI, chạy mọi lúc)

| Bước | Output | Thư viện |
|---|---|---|
| Tách câu | `source_sentences` với offset | `wink-nlp` |
| Tokenize + lemma + POS | token → lemma chuẩn hoá (`ran` → `run`) | `wink-nlp` + eng-lite model |
| Lọc stopword & tên riêng | loại `the`, `and`, tên người/công ty | wink NER + stoplist |
| Tra tần suất | gán `freq_band` 1-10 từ bảng tần suất tĩnh (top 20k) | dataset tĩnh |
| Bắt cụm ứng viên | n-gram 2-4 + đối chiếu **lexicon cụm công việc** có sẵn | lexicon tự seed |

Tầng A **luôn chạy được** kể cả offline/hết quota → thoả nguyên lý #5.

#### Tầng B — LLM (Groq, có cache)

LLM chỉ nhận **những gì tầng A không quyết được**, với structured output bắt buộc:

| Nhiệm vụ | Input | Output |
|---|---|---|
| B1. Xác nhận & chấm điểm cụm | danh sách n-gram ứng viên + câu chứa | giữ/loại, phân loại `phrasal_verb`/`collocation`/`idiom`/`business_term` |
| B2. Phân loại ngữ pháp | từng câu | danh sách `{pattern_slug, span, confidence}` — **chỉ từ catalog** (§7.7) |
| B3. Phân biệt nghĩa theo ngữ cảnh | từ đa nghĩa + câu chứa | `sense_id` đúng, hoặc định nghĩa ngắn nếu chưa có |
| B4. Gloss tiếng Việt | lemma/cụm + nghĩa EN | nghĩa VN ngắn gọn |
| B5. Sinh ví dụ bổ sung | lemma + miền công việc | 1-2 câu ví dụ (**chỉ khi < 2 occurrence thật**) |

**Ràng buộc chống ảo giác:** B2 bị ép chọn trong tập slug đóng của catalog. Confidence < 0.6 → loại.
Đây là cơ chế chặn "bùng nổ item ngữ pháp" — xem Rủi ro R2.

#### Chấm điểm ưu tiên

Mỗi item được chấm để trả lời "có đáng học không":

```
priority = w1·corpus_freq      (xuất hiện bao nhiêu lần trong corpus CỦA USER)
         + w2·recency          (mới gặp tuần này > gặp 6 tháng trước)
         + w3·general_utility  (freq_band — từ quá hiếm bị hạ điểm)
         + w4·level_fit        (CEFR item so với level user)
         + w5·source_weight    (nguồn kiểu `meeting` được ưu tiên hơn `article`)
         - p1·known_penalty    (đã ở trạng thái Known → gần như loại)
```

Trọng số để trong config, tinh chỉnh bằng dữ liệu thật sau vài tuần dùng. **Không hard-code rải rác.**

#### Yêu cầu vận hành

| ID | Yêu cầu |
|---|---|
| F2.1 | Phân tích 2.000 từ hoàn tất < 20 giây (p50), có progress streaming |
| F2.2 | Kết quả cache theo `content_hash` — dán lại y hệt không tốn AI lần hai |
| F2.3 | Cache theo lemma/cụm — một từ chỉ enrich (nghĩa, gloss, ví dụ) **một lần trọn đời** |
| F2.4 | Lỗi LLM → vẫn trả kết quả tầng A + cờ `degraded`, cho retry riêng phần LLM |
| F2.5 | Ghi log token/chi phí mỗi lần phân tích vào bảng `analyses` |

---

### 7.3 Triage — Chọn học gì

Màn hình sau phân tích. **Đây là màn hình quan trọng nhất của app** — nơi nguyên lý #2 và #4 gặp nhau.

Kết quả nhóm thành 4 khối:

| Khối | Nội dung | Hành vi mặc định |
|---|---|---|
| 🆕 **Mới** | Chưa từng có trong kho | Hiện đầy đủ, sắp xếp theo priority, **chưa tick sẵn** |
| 🔁 **Đang học** | Đã có `user_item`, chưa Known | Hiện gọn, ghi "gặp lần thứ N" — occurrence mới được gắn thêm tự động |
| ✅ **Đã thuộc** | Trạng thái Known | Thu gọn sau accordion, chỉ để user tự kiểm chứng |
| ⚪ **Bỏ qua** | User từng đánh dấu ignore | Ẩn, có nút hiện lại |

**Hai chế độ theo đúng mô tả ban đầu:**

- **Chế độ tự động (mặc định):** app dựa vào kho đã học + priority, đề xuất sẵn N item đáng học nhất
  (N mặc định 10, chỉnh được). User chỉ cần bỏ tick cái không muốn.
- **Chế độ thủ công:** liệt kê **toàn bộ** item trích được, user tự tick. Dành cho khi user muốn kiểm soát hết.

**Quét tự đánh giá (self-assessment sweep):**
Với item hệ thống chưa biết trạng thái, cho phép trả lời nhanh 3 mức — `Đã biết` / `Không chắc` / `Chưa biết`.
- `Đã biết` → tạo `user_item` status `known`, **không vào hàng đợi ôn** (nhưng vẫn để đối chiếu lần sau)
- `Không chắc` → vào hàng đợi với interval ngắn
- `Chưa biết` → vào hàng đợi như item mới

Đây chính là "hỏi người dùng đã thuộc gì chưa" — nhưng ở dạng quét nhanh, không phải bài kiểm tra.

| ID | Yêu cầu |
|---|---|
| F3.1 | Mỗi item hiển thị: dạng gốc gặp được, lemma, nghĩa ngắn, **câu thật** có highlight |
| F3.2 | Click item → panel chi tiết: mọi occurrence trong mọi nguồn, nghĩa đầy đủ, ngữ pháp liên quan |
| F3.3 | Thao tác hàng loạt: chọn tất cả trong khối, bỏ chọn tất cả, đảo chọn |
| F3.4 | Nút "Bỏ qua vĩnh viễn" (ignore) — không bao giờ đề xuất lại |
| F3.5 | **Trần item mới/ngày** (mặc định 20) — vượt thì cảnh báo, chống quá tải |
| F3.6 | Rời trang giữa chừng → giữ nguyên lựa chọn (draft), quay lại làm tiếp |

---

### 7.4 SRS — Ôn tập

**Thuật toán:** FSRS-5 qua `ts-fsrs`. Chọn FSRS thay vì SM-2 vì nó tự học độ khó thật của từng item
từ lịch sử review, phù hợp với người dùng ít thời gian — lịch ôn sát hơn = ít thẻ hơn cho cùng mức nhớ.

**Trạng thái item:** `new` → `learning` → `review` → `known` (+ `suspended`, `ignored`)

`known` = đạt interval ≥ 60 ngày và ≥ 3 lần đúng liên tiếp. Item `known` rời hàng đợi nhưng vẫn dùng
để đối chiếu ở Triage và tính Coverage.

#### Loại thẻ

Vì mục tiêu là **hiểu họp** và **diễn đạt chủ động**, thẻ nghiêng về sản sinh và nghe, không chỉ nhận diện:

| Loại thẻ | Mặt trước | Mặt sau | Dùng cho |
|---|---|---|---|
| **Cloze thật** ⭐ | Câu gốc từ nguồn, khoét item | Từ/cụm + nghĩa | Từ, cụm — *thẻ mặc định* |
| Nhận diện | Từ + câu chứa | Nghĩa EN + VN | Item mới, vòng đầu |
| Sản sinh | Nghĩa VN + gợi ý ngữ cảnh | Từ/cụm đúng | Item đã qua nhận diện |
| **Nghe** | TTS đọc câu gốc | Transcript + nghĩa | Ưu tiên cao — phục vụ nghe họp |
| Ngữ pháp | Câu gợi ý + yêu cầu áp dụng pattern | Câu đúng + giải thích | Item ngữ pháp (§7.7) |

Cloze là mặc định vì nó giữ ngữ cảnh thật — đúng nguyên lý #1.

| ID | Yêu cầu |
|---|---|
| F4.1 | Phiên ôn: kích thước hàng đợi hiển thị rõ, có thể dừng giữa chừng không mất tiến độ |
| F4.2 | Chấm 4 mức FSRS: `Again` / `Hard` / `Good` / `Easy`, phím tắt `1-4` và `Space` |
| F4.3 | Trộn item đến hạn + item mới theo tỷ lệ config (mặc định 80/20) |
| F4.4 | Thẻ luôn hiển thị **nguồn gốc**: "từ *Sprint planning 12/09*" — click về nguồn |
| F4.5 | Nút sửa/gắn cờ item ngay trong lúc ôn (nghĩa sai, ví dụ tệ) |
| F4.6 | Phiên ôn hoạt động offline sau khi tải hàng đợi; đồng bộ lại khi có mạng |
| F4.7 | Ghi `reviews` đầy đủ (rating, mode, duration) — cần cho tinh chỉnh FSRS sau |

---

### 7.5 Home — Dashboard

Theo đúng yêu cầu "SRS nhắn học từ có thể ở home hiển thị cho user biết".

Bố cục ưu tiên từ trên xuống:

1. **Hàng đợi hôm nay** — số thẻ đến hạn, nút `Bắt đầu ôn` cỡ lớn. Nếu 0 → trạng thái rỗng tích cực.
2. **Ô dán nhanh** — đường ngắn nhất vào vòng lặp cốt lõi, luôn nhìn thấy không cần cuộn.
3. **Chờ triage** — "12 item từ *Weekly sync 14/09* chưa phân loại" → click vào Triage.
4. **Tiến độ** — streak, số item lên `known` tuần này, độ chính xác 7 ngày.
5. **Nguồn gần đây** — 5 nguồn mới nhất, mỗi nguồn kèm số item đã rút.

| ID | Yêu cầu |
|---|---|
| F5.1 | Home tải < 1.5s, số liệu tính từ query tổng hợp, không N+1 |
| F5.2 | Badge số thẻ đến hạn hiện trên tab title và PWA icon |
| F5.3 | Trạng thái rỗng (chưa có nguồn nào) dẫn thẳng vào luồng nhập đầu tiên |

---

### 7.6 Dictionary — Từ điển

| ID | Yêu cầu | Phase |
|---|---|---|
| F6.1 | Tra cứu từ/cụm bất kỳ, không cần thuộc nguồn nào | 3 |
| F6.2 | Hiển thị: IPA, POS, các nghĩa, ví dụ, mức CEFR/tần suất | 3 |
| F6.3 | **Ưu tiên hiện occurrence thật của user trước ví dụ từ điển** | 3 |
| F6.4 | Một chạm "Thêm vào học" ngay từ kết quả tra | 3 |
| F6.5 | Nguồn dữ liệu: dictionaryapi.dev + Wiktionary, cache vĩnh viễn vào bảng `senses` | 3 |
| F6.6 | Gloss tiếng Việt sinh bởi LLM, cache theo sense — không gọi lại | 3 |
| F6.7 | Phát âm: Web Speech API (v1), TTS chất lượng cao (v2) | 3 |
| F6.8 | Lịch sử tra cứu — tra 3 lần cùng một từ mà chưa học → gợi ý thêm vào | 3 |

---

### 7.7 Grammar — Ngữ pháp

Phần khó nhất về thiết kế. Ngữ pháp **không thể** trích tự do như từ vựng: nếu để LLM tự mô tả cấu trúc,
cùng một pattern sẽ sinh ra 50 biến thể tên gọi khác nhau và SRS trở nên vô nghĩa.

**Giải pháp: Grammar Catalog cố định.**

- Catalog seed sẵn **~150-200 pattern** có `slug` ổn định, gắn CEFR (A2→C1)
- LLM chỉ **phân loại** câu vào slug có sẵn — không được tạo slug mới
- Mỗi pattern có: tên, công thức, giải thích tiếng Việt, ví dụ mẫu, pattern liên quan

**Trọng tâm catalog nghiêng về register công việc**, không phải ngữ pháp luyện thi:

| Nhóm | Ví dụ pattern | Vì sao quan trọng ở công sở |
|---|---|---|
| Hedging & lịch sự | `it might be worth + V-ing`, `I'd suggest + V-ing`, `we may want to` | Đề xuất không áp đặt — dùng liên tục trong họp |
| Câu chẻ (cleft) | `What we need is...`, `It was X that...` | Nhấn mạnh ý trong thuyết trình |
| Bị động & danh hoá | `it is said that`, `passive with get`, nominalization | Đặc trưng văn phong tài liệu kỹ thuật |
| Điều kiện | conditional 1/2/3, mixed, đảo ngữ `Had we known` | Thương lượng, phân tích rủi ro |
| Modal quá khứ | `should have + V3`, `must have + V3` | Retro, post-mortem |
| Mệnh đề quan hệ | defining/non-defining, rút gọn, `which` bình luận | Viết tài liệu chặt chẽ |
| Tường thuật | backshift, động từ tường thuật + pattern | Tóm tắt cuộc họp |
| So sánh | `the more... the more`, `as... as` | Báo cáo số liệu |

| ID | Yêu cầu |
|---|---|
| F7.1 | Catalog seed qua migration, có versioning — thêm pattern không phá dữ liệu cũ |
| F7.2 | LLM output ràng buộc bằng enum slug + confidence; < 0.6 thì loại |
| F7.3 | Mỗi pattern user học lưu **occurrence thật** — câu trong tài liệu của họ minh hoạ pattern đó |
| F7.4 | Thẻ ngữ pháp dạng transform/produce, không phải trắc nghiệm định nghĩa |
| F7.5 | Trang chi tiết pattern: giải thích + mọi câu thật của user dùng pattern đó |
| F7.6 | Lọc theo CEFR — user chọn dải level muốn nhận đề xuất |

---

### 7.8 Writing & Shadowing

Theo mô tả: *"writing cũng dựa vào các đoạn đã học mà đưa cho người dùng shadow theo trước rồi mới viết còn lại"*.

Bài tập gồm 3 bước tăng dần độ tự do:

```
Bước 1 — SHADOW      Đoạn mẫu lấy từ chính nguồn user đã học.
                     User gõ lại theo mẫu, có scaffold (điền chỗ trống, sắp xếp câu).
                     Mục tiêu: nạp nhịp câu và collocation vào trí nhớ cơ.

Bước 2 — GUIDED      Đề bài tương tự, cho sẵn khung + danh sách item bắt buộc dùng.
                     User viết, hệ thống kiểm tra đã dùng đủ item chưa.

Bước 3 — FREE        Đề bài cùng chủ đề, không khung. User tự viết.
                     LLM chấm: đúng/sai item mục tiêu, lỗi ngữ pháp, gợi ý nâng register.
```

| ID | Yêu cầu | Phase |
|---|---|---|
| F8.1 | Đoạn mẫu **chỉ lấy từ nguồn của user**, ưu tiên nguồn có nhiều item đã Known | 5 |
| F8.2 | Chọn item mục tiêu từ hàng đợi đang học — writing là một hình thức ôn tập | 5 |
| F8.3 | Feedback có cấu trúc: item dùng đúng / dùng sai / bỏ sót, + lỗi ngữ pháp theo catalog | 5 |
| F8.4 | Bài viết lưu lại, so sánh được giữa các lần | 5 |
| F8.5 | Lỗi ngữ pháp lặp lại → đề xuất thêm pattern đó vào hàng đợi học | 5 |

---

### 7.9 Coverage Report — Đo tiến bộ

Tính năng nhỏ, chi phí gần như bằng 0 (dùng lại tầng A), nhưng là **thước đo trung thực nhất** cho mục tiêu G4.

Dán một văn bản công việc bất kỳ → app trả về:

- **% token đã biết** (Known + Learning) trên tổng token có nghĩa
- Danh sách từ/cụm chưa biết, xếp theo priority
- So sánh với lần đo trước cùng loại nguồn

Đây là trả lời trực tiếp cho câu hỏi *"học mấy tháng rồi có khá hơn không?"* bằng dữ liệu thật
chứ không phải bằng số thẻ đã ôn.

---

### 7.10 Notifications

| ID | Yêu cầu | Phase |
|---|---|---|
| F10.1 | Badge đến hạn trên Home + tab title (không cần quyền gì) | 2 |
| F10.2 | PWA installable, chạy standalone | 6 |
| F10.3 | Web Push nhắc ôn theo khung giờ user chọn (iOS 16.4+ hỗ trợ) | 6 |
| F10.4 | Nhắc thông minh: không nhắc khi hàng đợi rỗng, không nhắc 2 lần/ngày | 6 |
| F10.5 | Email tóm tắt tuần (tuỳ chọn) | 6 |
| F10.6 | FCM push cho app React Native | 7 |

---

## 8. Mô hình dữ liệu

Thiết kế **solo-first nhưng không khoá đường multi-user**: mọi bảng thuộc về người dùng đều mang `user_id`
ngay từ migration đầu tiên. Các bảng catalog (`lexemes`, `phrases`, `grammar_patterns`, `senses`) là
**dùng chung**, không gắn user — đây là điểm then chốt giúp cache enrich dùng lại được khi mở multi-user.

```
┌── Nội dung của user ─────────────────────────────────────────┐
│ users               id, email, name, level_cefr, settings     │
│ sources             id, user_id, title, kind, raw_text,       │
│                     content_hash, source_date, tags[]         │
│ source_sentences    id, source_id, idx, text, char_start/end  │
│ analyses            id, source_id, status, provider, model,   │
│                     tokens_in/out, degraded, result_json      │
└───────────────────────────────────────────────────────────────┘

┌── Catalog dùng chung (không gắn user) ───────────────────────┐
│ lexemes             id, lemma, pos, ipa, freq_band, cefr      │
│ senses              id, lexeme_id, definition_en,             │
│                     definition_vi, examples[]                 │
│ phrases             id, text_norm, kind, cefr, definition_*   │
│ grammar_patterns    id, slug(UK), title, cefr, formula,       │
│                     explanation_vi, examples[], catalog_ver   │
│ items               id, kind, lexeme_id?, phrase_id?,         │
│                     grammar_pattern_id?   ← lớp hợp nhất      │
└───────────────────────────────────────────────────────────────┘

┌── Cầu nối user ↔ catalog ────────────────────────────────────┐
│ occurrences         id, user_id, item_id, source_id,          │
│                     sentence_id, surface_form,                │
│                     char_start/end, sense_id?                 │
│ user_items          id, user_id, item_id, status,             │
│                     fsrs_state(jsonb), due_at, stability,     │
│                     difficulty, reps, lapses,                 │
│                     first_source_id, priority_score           │
│ reviews             id, user_item_id, rating, card_mode,      │
│                     duration_ms, reviewed_at                  │
│ writing_tasks       id, user_id, source_id, stage,            │
│                     target_item_ids[], draft, feedback_json   │
└───────────────────────────────────────────────────────────────┘
```

### Quyết định thiết kế đáng chú ý

| Quyết định | Lý do |
|---|---|
| Bảng `items` hợp nhất 3 loại thay vì 3 bảng SRS riêng | Một hàng đợi ôn duy nhất, một thuật toán FSRS, tránh nhân ba logic |
| `occurrences` tách khỏi `user_items` | Một item có nhiều lần xuất hiện ở nhiều nguồn; xoá item không mất dấu vết nguồn |
| Catalog không gắn `user_id` | Enrich (nghĩa, IPA, gloss VN) trả tiền AI **một lần duy nhất**, dùng lại mãi mãi |
| `fsrs_state` lưu jsonb | Thuật toán FSRS còn tiến hoá; tránh migrate cột mỗi lần đổi version |
| `raw_text` giữ nguyên vẹn + offset | Ví dụ thật phải tái dựng được chính xác, kể cả sau khi đổi thuật toán tách câu |
| `content_hash` trên `sources` | Nền tảng của cache phân tích (F2.2) |
| `priority_score` lưu sẵn | Triage phải nhanh; không tính lại điểm mỗi lần render |

---

## 9. Kiến trúc kỹ thuật

```
┌──────────────────────────────────────────────────────────┐
│  Client — Next.js 16 App Router + React 19 + TypeScript  │
│  Tailwind + shadcn/ui · PWA (Phase 6)                     │
└────────────────────────┬─────────────────────────────────┘
                         │
┌────────────────────────▼─────────────────────────────────┐
│  API — Route Handlers + Server Actions                    │
│  ⚠️ Mọi logic nghiệp vụ nằm ở /lib, KHÔNG ở component     │
│     → app React Native (Phase 7) gọi lại được nguyên vẹn  │
└────┬──────────────┬──────────────┬───────────────────────┘
     │              │              │
┌────▼─────┐  ┌─────▼──────┐  ┌────▼──────────────────────┐
│ NLP cục  │  │ LLM        │  │ SRS engine                 │
│ bộ       │  │ Provider   │  │ ts-fsrs                    │
│ wink-nlp │  │ (abstract) │  │ thuần, testable            │
└──────────┘  └─────┬──────┘  └───────────────────────────┘
                    │
        ┌───────────┼───────────┬──────────────┐
        ▼           ▼           ▼              ▼
    ┌────────┐ ┌────────┐ ┌──────────┐ ┌────────────┐
    │ Groq   │ │ Ollama │ │ Anthropic│ │ Mock       │
    │ (mặc   │ │ (local,│ │ (dự      │ │ (test)     │
    │ định)  │ │ nhạy   │ │ phòng)   │ │            │
    │        │ │ cảm)   │ │          │ │            │
    └────────┘ └────────┘ └──────────┘ └────────────┘

┌──────────────────────────────────────────────────────────┐
│  Postgres (Neon) + Drizzle ORM                            │
│  Full-text search cho dictionary · jsonb cho fsrs_state   │
└──────────────────────────────────────────────────────────┘
```

### Lựa chọn công nghệ

| Lớp | Chọn | Lý do |
|---|---|---|
| Framework | Next.js 16 App Router, React 19, TypeScript | Streaming cho phân tích, RSC cho dashboard, deploy Vercel liền mạch |
| DB | Postgres trên Neon | Full-text search, jsonb, free tier đủ dùng, không khoá vendor |
| ORM | Drizzle | SQL-first, bundle nhẹ, migration minh bạch — hợp serverless |
| Auth | Auth.js v5, một provider Google | Solo-first nhưng đã có `user_id` xuyên suốt |
| SRS | `ts-fsrs` | FSRS-5, thuật toán tốt nhất hiện có dạng open source |
| NLP | `wink-nlp` + `wink-eng-lite-web-model` | Pure JS, chạy cả Node lẫn browser, không cần Python |
| LLM | Groq (free tier) qua lớp trừu tượng | §10 |
| UI | Tailwind + shadcn/ui | Tốc độ dựng, accessible sẵn |
| TTS | Web Speech API | Miễn phí, đủ cho v1 |
| Deploy | Vercel | Fluid Compute 300s đủ cho phân tích inline |

### Ràng buộc kiến trúc bắt buộc

1. **Logic nghiệp vụ sống ở `/lib`, không ở React component.** Phase 7 (React Native) sẽ gọi lại
   cùng những hàm đó qua API. Vi phạm điều này = viết lại backend khi làm app.
2. **`LlmProvider` là interface.** Không import SDK Groq trực tiếp ở bất kỳ đâu ngoài
   `lib/llm/providers/groq.ts`. Đổi sang local model phải là đổi một dòng config.
3. **SRS engine thuần hàm, không chạm DB.** Nhận state + rating, trả state mới. Dễ test, dễ đổi thuật toán.
4. **Mọi prompt LLM có schema output (Zod).** Parse fail → retry 1 lần → degrade, không bao giờ crash.
5. **File < 200 dòng** theo chuẩn codebase (`ck-docs/code-standards.md`).

---

## 10. AI Pipeline & Chi phí

### Chiến lược: Groq free tier, kiến trúc sẵn sàng đổi

**Mặc định:** Groq free tier. Model đề xuất — `llama-3.3-70b-versatile` cho phân loại ngữ pháp và
xác nhận cụm (cần chất lượng), model nhỏ hơn cho gloss/ví dụ. Groq nhanh bất thường (tokens/s rất cao),
rất hợp với UX streaming khi phân tích.

**Ràng buộc free tier:** giới hạn RPM/RPD/TPD. Kiến trúc phải sống được với nó:

| Cơ chế | Tác dụng |
|---|---|
| Tầng A làm hết việc không cần AI | Giảm ~60-70% khối lượng cần LLM |
| Cache theo `content_hash` | Dán lại cùng văn bản = 0 token |
| Cache enrich theo lemma/sense | Một từ chỉ trả tiền một lần trọn đời |
| Gộp batch theo câu | 1 request xử lý 10-20 câu thay vì 1 request/câu |
| Hàng đợi + backoff khi 429 | Chạm rate limit không mất dữ liệu, tự chạy tiếp |
| Cờ `degraded` | Hết quota → trả kết quả tầng A, user retry phần LLM sau |

### Đường local model

Đã tính sẵn trong kiến trúc, bật khi cần:

- Chạy Ollama cục bộ với model 7-8B (Qwen2.5 / Llama 3.1)
- **Ca dùng đắt giá nhất không phải tiết kiệm tiền mà là riêng tư** — tài liệu công ty nhạy cảm
  không rời máy (§11). Đánh dấu nguồn là `sensitive` → tự động định tuyến sang local provider.
- Đánh đổi thành thật: model 7-8B phân loại ngữ pháp kém hơn 70B rõ rệt. Chấp nhận được cho gloss
  và trích cụm; với ngữ pháp nên hạ ngưỡng kỳ vọng hoặc giữ Groq.

### Theo dõi chi phí

Bảng `analyses` ghi `provider`, `model`, `tokens_in`, `tokens_out` mỗi lần gọi. Có trang thống kê đơn giản
để biết mỗi nguồn tốn bao nhiêu — cần thiết nếu sau này mở multi-user.

---

## 11. Bảo mật & Riêng tư

> ⚠️ **Đây là rủi ro lớn nhất của sản phẩm, không phải rủi ro kỹ thuật.**

Bản chất app là **dán tài liệu công việc vào rồi gửi cho bên thứ ba**. Biên bản họp nội bộ, email khách hàng,
tài liệu kiến trúc — đây có thể là thông tin thuộc sở hữu công ty, có thể vi phạm NDA hoặc chính sách bảo mật
nội bộ khi gửi ra LLM bên ngoài.

**Phải xử lý ngay từ v1, không để sau:**

| ID | Yêu cầu | Phase |
|---|---|---|
| S1 | **Redaction pass trước khi gửi LLM** — tự phát hiện & thay thế email, số điện thoại, URL nội bộ, token/key, tên người (NER) bằng placeholder | 1 |
| S2 | Xem trước nội dung đã redact, cho user sửa trước khi gửi | 1 |
| S3 | Cờ `sensitive` trên nguồn → **không gửi LLM**, chỉ dùng tầng A (hoặc local provider) | 1 |
| S4 | Cảnh báo rõ ràng ở lần nhập đầu tiên: dữ liệu sẽ được gửi tới đâu | 1 |
| S5 | Xoá nguồn = xoá cascade cả `raw_text` và cache phân tích | 1 |
| S6 | Không log `raw_text` ra log hệ thống / telemetry / error tracking | 1 |
| S7 | Biến môi trường qua `vercel env`, không commit `.env` | 0 |
| S8 | Rate limit endpoint phân tích (chống lạm dụng khi mở multi-user) | 6 |
| S9 | Export toàn bộ dữ liệu ra JSON — không khoá người dùng vào app | 3 |

**Khuyến nghị thực hành:** nếu công ty có chính sách cấm đưa tài liệu nội bộ ra dịch vụ AI bên ngoài,
dùng đường Ollama local cho các nguồn đó. Kiến trúc provider (§9) tồn tại chính vì lý do này.

---

## 12. Metrics

### Metric bắc sao (North Star)

> **Coverage trên văn bản công việc mới** — % token có nghĩa mà user đã Known, đo trên văn bản họ
> chưa từng nhập. Đây là thứ duy nhất đo đúng mục tiêu "hiểu được công việc bằng tiếng Anh".

### Metric vận hành

| Nhóm | Metric | Mục tiêu v1 |
|---|---|---|
| Vòng lặp | % nguồn nhập vào → có ≥1 item được thêm | > 80% |
| Vòng lặp | Thời gian paste → item đầu vào hàng đợi | < 60s |
| Thói quen | Số ngày/tuần hoàn thành hàng đợi đến hạn | ≥ 5 |
| Chất lượng | Độ chính xác review 7 ngày | 80-90% (thấp hơn = quá khó, cao hơn = quá dễ) |
| Chất lượng | % item bị gắn cờ sai (nghĩa/ví dụ tệ) | < 5% |
| Giữ chân | Item đạt `known` và không lapse sau 60 ngày | > 70% |
| Kỹ thuật | Phân tích 2.000 từ, p50 | < 20s |
| Kỹ thuật | Tỷ lệ phân tích `degraded` | < 10% |

---

## 13. Roadmap

| Phase | Tên | Nội dung | Điều kiện hoàn thành |
|---|---|---|---|
| **0** | Foundation | Scaffold Next.js, Drizzle + Neon, Auth.js, CI, deploy Vercel | Deploy được, đăng nhập được, migration chạy |
| **1** | Capture & Analyze ⭐ | Nhập văn bản, pipeline NLP + LLM, trích item, **màn Triage**, redaction | Dán note họp → chọn được item để học |
| **2** | SRS Core ⭐ | `user_items`, FSRS, phiên ôn, thẻ cloze/nhận diện, **Home dashboard** | Ôn được hàng ngày, lịch đúng |
| **3** | Dictionary & Enrichment | Tra cứu, senses, gloss VN, TTS, thẻ nghe, export dữ liệu | Tra từ bất kỳ, một chạm thêm vào học |
| **4** | Grammar | Seed catalog, phân loại pattern, thẻ ngữ pháp, trang chi tiết pattern | Ngữ pháp vào chung hàng đợi SRS |
| **5** | Writing & Shadowing | Bài 3 bước shadow → guided → free, AI feedback | Hoàn thành 1 bài viết có feedback |
| **6** | PWA & Notifications | PWA, Web Push, nhắc thông minh, email tuần | Nhận được nhắc ôn trên điện thoại |
| **7** | Mobile (React Native) | App RN dùng lại API, FCM push, ôn offline | Ôn được trên app native |

**MVP = Phase 0 + 1 + 2.** Đó là vòng lặp nhỏ nhất mà vẫn có giá trị thật hàng ngày.
Phase 3-4 làm app thực sự tốt. Phase 5-7 là mở rộng.

Kế hoạch chi tiết từng phase: [`ck-plans/260915-1341-english-practice-mvp/plan.md`](../ck-plans/260915-1341-english-practice-mvp/plan.md)

---

## 14. Rủi ro

| ID | Rủi ro | Mức | Giảm thiểu |
|---|---|---|---|
| **R1** | **Rò rỉ tài liệu công ty ra LLM bên thứ ba** | 🔴 Cao | Redaction bắt buộc (S1), cờ `sensitive` (S3), đường local provider. Xem §11 |
| **R2** | Bùng nổ item ngữ pháp — LLM sinh pattern tự do, SRS thành rác | 🔴 Cao | Catalog slug đóng (§7.7), ngưỡng confidence, LLM chỉ phân loại không sáng tạo |
| **R3** | Quá tải item — nhập nhiều nguồn, hàng đợi phình, bỏ cuộc | 🟠 TB | Trần item mới/ngày (F3.5), priority score, mặc định **không** tick sẵn |
| **R4** | Groq rate limit chặn luồng chính | 🟠 TB | Tầng A độc lập, cache 2 lớp, hàng đợi + backoff, cờ `degraded` (§10) |
| **R5** | Chất lượng trích cụm kém → item vô giá trị, mất niềm tin | 🟠 TB | Lexicon cụm công việc seed sẵn, nút gắn cờ (F4.5), theo dõi tỷ lệ flag < 5% |
| **R6** | Dự án cá nhân chết yểu sau 2 tuần | 🟠 TB | MVP cắt xuống Phase 0-2; thời gian tới bản dùng được thật phải tính bằng tuần, không phải tháng |
| **R7** | Phân biệt nghĩa sai (từ đa nghĩa) → học nhầm nghĩa | 🟡 Thấp | Luôn hiện câu gốc kèm item, user tự sửa được |
| **R8** | Logic nghiệp vụ lẫn vào React → Phase 7 phải viết lại backend | 🟡 Thấp | Ràng buộc kiến trúc #1 (§9), review khi merge |

---

## 15. Câu hỏi mở

| # | Câu hỏi | Ảnh hưởng | Cần chốt trước |
|---|---|---|---|
| Q1 | Nguồn dữ liệu CEFR cho từ vựng? Các list chính thống (Oxford/Cambridge) có bản quyền. Dùng xấp xỉ từ freq_band? | Độ chính xác `level_fit` khi chấm priority | Phase 1 |
| Q2 | Lexicon cụm công việc seed từ đâu — tự soạn ~500-1000 mục, hay sinh bằng LLM rồi tự duyệt? | Chất lượng trích cụm (R5) | Phase 1 |
| Q3 | Ngưỡng `known`: 60 ngày + 3 lần đúng có hợp lý không, hay để tự tinh chỉnh sau khi có dữ liệu thật? | Coverage score, kích thước hàng đợi | Phase 2 |
| Q4 | Có cần lưu audio gốc cuộc họp để làm thẻ nghe bằng giọng thật thay vì TTS? | Chất lượng luyện nghe, độ phức tạp lưu trữ | Phase 3 |
| Q5 | Catalog ngữ pháp tự soạn hay lấy từ nguồn mở nào? Cần kiểm tra bản quyền | Phase 4 khởi động được hay không | Phase 4 |
| Q6 | Bao nhiêu phút/ngày là ngân sách thực tế? Quyết định trần item mới và kích thước hàng đợi mặc định | Thiết kế hàng đợi, chống burnout (R3) | Phase 2 |

---

## Phụ lục — Thuật ngữ

| Thuật ngữ | Nghĩa trong dự án |
|---|---|
| **Source** | Một văn bản người dùng nhập vào (note họp, email, doc...) |
| **Item** | Đơn vị học — một từ (lexeme), một cụm (phrase), hoặc một pattern ngữ pháp |
| **Occurrence** | Một lần xuất hiện cụ thể của item trong một câu của một source |
| **Triage** | Bước người dùng chọn học gì từ kết quả phân tích |
| **FSRS** | Free Spaced Repetition Scheduler — thuật toán xếp lịch ôn |
| **Tầng A / Tầng B** | Tầng NLP cục bộ (miễn phí) / tầng LLM (có chi phí) |
| **Degraded** | Trạng thái phân tích chỉ có tầng A do LLM lỗi hoặc hết quota |
| **Coverage** | % token có nghĩa trong một văn bản mà người dùng đã thuộc |
