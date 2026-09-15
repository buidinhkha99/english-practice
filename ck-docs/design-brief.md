# Design Brief — English Practice

| | |
|---|---|
| **Phiên bản** | 1.0 · 2026-09-15 |
| **Cho** | Thiết kế UI/UX — giai đoạn thiết kế trước khi code |
| **PRD nguồn** | [`product-requirements.md`](product-requirements.md) |
| **Đặc tả màn hình** | [`design-screens.md`](design-screens.md) |

---

## 1. Tài liệu này dùng để làm gì

Đây là **brief cho người thiết kế**, không phải spec kỹ thuật. Nó mô tả *phải giải quyết vấn đề thiết kế gì*,
không mô tả *giao diện trông thế nào* — phần đó là việc của thiết kế.

**Cần nhận lại:**

| # | Deliverable | Ghi chú |
|---|---|---|
| D1 | **Design tokens** — màu (đủ 2 theme), typography scale, spacing, radius, elevation | Dạng CSS custom properties, khớp §5 |
| D2 | **Hệ thống kind × state** — cách thể hiện 3 loại item và 5 trạng thái mà không đụng nhau | §4 — đây là phần khó nhất |
| D3 | **Thiết kế 6 màn MVP** ở cả desktop và mobile | Home, Capture, Analyzing, Triage, Review, Review Summary |
| D4 | **Component `ContextSentence`** — câu thật có highlight span | §7 — component chữ ký của sản phẩm |
| D5 | **Trạng thái rỗng / đang tải / lỗi / degraded** cho từng màn MVP | §9 — không được coi là phụ |
| D6 | Thiết kế phác 4 màn sau MVP | Từ điển, Thư viện, Chi tiết pattern, Cài đặt |

**Không cần:** brand identity, logo, landing page, marketing site, icon set tự vẽ (dùng bộ có sẵn).

---

## 2. Sản phẩm trong 60 giây

Người dùng làm việc trong môi trường tiếng Anh dày đặc — họp, email, tài liệu. App biến **chính tài liệu
công việc của họ** thành giáo trình: dán văn bản vào → app tách ra từ / cụm từ / cấu trúc ngữ pháp →
đối chiếu với những gì họ đã học để chỉ nổi bật phần chưa biết → họ chọn → item vào lịch ôn tập ngắt quãng,
**mỗi item mang theo đúng câu thật mà nó xuất hiện**.

Khác biệt với Duolingo/Anki: corpus là của chính người dùng, không phải list có sẵn.

---

## 3. Bối cảnh sử dụng

Thiết kế phải chịu được ba tình huống rất khác nhau:

| | Khi nào | Ở đâu | Tâm trạng | Hệ quả thiết kế |
|---|---|---|---|---|
| **A. Nhập** | Ngay sau cuộc họp, 2-5 phút | Laptop, tab bên cạnh Zoom/Notion | Vội, muốn xong nhanh | Dán → xong phải dưới 90 giây. Không hỏi nhiều |
| **B. Ôn** | Sáng sớm hoặc lúc chờ build | Laptop *hoặc* điện thoại | Tập trung ngắn, dễ bị ngắt | Bàn phím trên desktop, ngón cái trên mobile. Dừng giữa chừng không mất gì |
| **C. Tra** | Đang đọc tài liệu, gặp cụm lạ | Laptop, chuyển tab | Đang dở việc khác | Tra → hiểu → quay lại, dưới 15 giây |

**Ngân sách thời gian mặc định: 30 phút/ngày** (cấu hình được). Đây không phải người dùng rảnh rỗi —
mọi giây trong giao diện phải xứng đáng.

---

## 4. Hệ thống phân loại — hai trục

> **Đây là bài toán thiết kế trung tâm.** Mọi item đều mang đồng thời một *loại* và một *trạng thái*.
> Hai trục này xuất hiện cùng nhau trên gần như mọi màn hình và **không được dùng chung ngôn ngữ thị giác**.

### Trục 1 — Loại item (kind) · 3 giá trị, cố định

| Loại | Là gì | Ví dụ thật |
|---|---|---|
| `lexeme` | Một từ đơn, lưu dạng lemma | *mitigate*, *scoping*, *spike* |
| `phrase` | Cụm nhiều từ: phrasal verb, collocation, idiom, thuật ngữ công việc | *circle back on*, *align with*, *take ownership of* |
| `grammar` | Một cấu trúc trong catalog cố định | *it might be worth + V-ing*, *should have + V3* |

### Trục 2 — Trạng thái học (state) · 5 giá trị

| Trạng thái | Nghĩa | Xuất hiện ở |
|---|---|---|
| `new` | Chưa từng có trong kho | Triage, kết quả phân tích |
| `learning` | Đang học, chưa vững | Triage, hàng đợi ôn, thư viện |
| `review` | Đã vào chu kỳ ôn dài | Hàng đợi ôn, thư viện |
| `known` | Đã thuộc, rời hàng đợi | Triage (thu gọn), thư viện, coverage |
| `ignored` | Người dùng chủ động bỏ qua | Triage (ẩn), thư viện |

### Ràng buộc

- **Loại nên mã hoá bằng *hình dạng / ký hiệu*, trạng thái bằng *màu*** — hoặc ngược lại, nhưng phải
  chọn một và giữ nhất quán toàn app. Dùng màu cho cả hai trục là hỏng.
- **Không được chỉ dựa vào màu.** Mỗi trạng thái phải đọc được khi in đen trắng.
- `learning` và `review` có thể gộp thị giác thành "đang học" ở màn Triage, nhưng phải tách được
  ở Thư viện (nơi người dùng lọc).
- Hệ thống này còn phải hoạt động **bên trong một câu văn** (§7) — nơi không có chỗ cho badge.

---

## 5. Yêu cầu hệ thống thiết kế

### 5.1 Màu — theo vai trò, không chỉ định hex

Chọn bảng màu là việc của thiết kế. Đây là danh sách **vai trò phải có**:

```
ground · surface · surface-raised            nền, thẻ, lớp nổi
ink · ink-secondary · ink-muted · ink-faint  4 bậc chữ, không hơn
rule · rule-soft                             đường kẻ
accent · accent-soft · accent-line           hành động chính
state-new · state-learning · state-known · state-ignored
highlight-lexeme · highlight-phrase · highlight-grammar   ← §7
success · warning · danger · info
```

**Ràng buộc bắt buộc:**

| | |
|---|---|
| Hai theme | Sáng và tối đều phải thiết kế đầy đủ, không đảo ngược máy móc |
| `state-*` ≠ `accent` | Trạng thái là ngữ nghĩa, accent là hành động — không được trùng hue |
| `highlight-*` | Phải là **wash nhạt dùng được sau chữ đang đọc**, không phá độ tương phản của text. Đây là ràng buộc khó nhất của bảng màu |
| Mù màu | `state-*` và `highlight-*` phải phân biệt được với deuteranopia — kiểm tra, đừng đoán |
| Tương phản | Chữ thường ≥ 4.5:1, chữ lớn ≥ 3:1, ở **cả hai** theme |

### 5.2 Typography — bài toán song ngữ

> Giao diện tiếng Việt, **nội dung học là tiếng Anh**. Hai ngôn ngữ nằm cạnh nhau trên mọi màn hình.

| Vai trò | Ngôn ngữ | Yêu cầu |
|---|---|---|
| UI, nhãn, nút, giải thích | Tiếng Việt | **Bắt buộc đủ dấu tiếng Việt.** Nhiều display font không có — kiểm tra `ế ộ ữ ỵ ẳ` trước khi chọn |
| Nội dung học (câu gốc, từ, ví dụ) | Tiếng Anh | **Nên khác face với UI** — giúp mắt tách ngay "cái đang học" khỏi "vỏ giao diện". Đây là lựa chọn mang thông tin, không phải trang trí |
| Phiên âm IPA | — | Font phải có glyph IPA: `/ˈmɪtɪɡeɪt/ /ˌɪntəˈɡreɪʃn/`. Đa số font sans thiếu |
| Số liệu, interval, slug, mã | — | Mono, `tabular-nums` cho mọi cột số |

**Line-height:** dấu tiếng Việt chồng cao (`ữ`, `ế`, `ộ`) — line-height chật sẽ cắt dấu hoặc làm dòng dính nhau.
Tối thiểu 1.6 cho body tiếng Việt.

**Đo dòng:** câu gốc tiếng Anh là thứ được đọc kỹ nhất trong app — giữ gần 65 ký tự/dòng.

### 5.3 Spacing, radius, elevation

- Thang spacing rõ ràng, dùng `gap` của flex/grid thay vì margin rời rạc
- **Không phải mọi thứ đều là card.** Border, nền, bo góc, đổ bóng mỗi thứ nói "đây là vật thể riêng" —
  tiêu chúng theo vai trò. Một radius + một shadow đóng dấu lên mọi khối sẽ làm phẳng hết phân cấp
- Lề hai bên tối thiểu 16px ở mọi bề rộng

### 5.4 Motion

Tiết chế. Có **đúng một chỗ** chuyển động xứng đáng: **lật thẻ trong phiên ôn** — vì nó đánh dấu ranh giới
giữa "đang cố nhớ" và "đã biết đáp án", một ranh giới nhận thức có thật.

Mọi thứ khác: transition ngắn hoặc không có. Tôn trọng `prefers-reduced-motion`.

---

## 6. Kiến trúc thông tin

```
Home                    hàng đợi hôm nay · dán nhanh · chờ triage · tiến độ · nguồn gần đây
├── Nhập                ô dán → xem trước redaction → đang phân tích → Triage
│   └── Triage          4 khối, chọn item → vào hàng đợi
├── Ôn tập              phiên ôn toàn màn hình → tổng kết phiên
├── Thư viện            item đã học (lọc theo kind × state) · nguồn · pattern ngữ pháp
│   ├── Chi tiết item   mọi occurrence, nghĩa, lịch ôn
│   ├── Chi tiết nguồn  văn bản gốc có highlight, item đã rút
│   └── Chi tiết pattern giải thích + mọi câu thật của user dùng pattern đó
├── Từ điển             tra cứu · (Phase 3)
└── Cài đặt             ngân sách phút/ngày · CEFR · thông báo · dữ liệu
```

**Điều hướng:** desktop — thanh bên trái hoặc trên. Mobile — tab dưới (Home / Ôn / Thư viện / Cài đặt),
riêng phiên ôn chiếm **toàn màn hình, không có tab** — đó là chế độ tập trung.

---

## 7. Component chữ ký — `ContextSentence`

> Nếu chỉ thiết kế được một component cho tử tế, chọn cái này.

Câu thật từ tài liệu của người dùng, có highlight các span là item. Nó xuất hiện ở **Triage, thẻ ôn,
chi tiết item, chi tiết nguồn, từ điển, chi tiết pattern** — tức là gần như mọi nơi.

Ví dụ thật:

> We should **circle back on** the *scoping* once we've **aligned with** the platform team —
> `it might be worth` *timeboxing* the *spike*.
>
> *(đậm = phrase · nghiêng = lexeme · code = grammar)*

**Yêu cầu:**

| # | Yêu cầu |
|---|---|
| C1 | Highlight **không được phá độ đọc** của câu — người dùng vẫn phải đọc trôi chảy toàn câu |
| C2 | Ba loại item phân biệt được **bên trong dòng chữ**, nơi không có chỗ cho badge |
| C3 | Span có thể **liền kề hoặc lồng nhau** (`it might be worth` chứa cả cấu trúc lẫn từ) — thiết kế phải chịu được |
| C4 | Một biến thể **khoét lỗ (cloze)** cho thẻ ôn: một span thành ô trống, phần còn lại giữ nguyên |
| C5 | Một biến thể **trung tính** khi hiển thị trong ngữ cảnh đã biết hết (không highlight gì) |
| C6 | Luôn có **xuất xứ**: "từ *Sprint planning · 12.09*" — click về được nguồn |
| C7 | Câu dài phải xuống dòng đẹp, không cắt span giữa chừng một cách khó đọc |

---

## 8. Component inventory

| Component | Dùng ở | Ghi chú |
|---|---|---|
| `ContextSentence` | Khắp nơi | §7 — quan trọng nhất |
| `ItemRow` | Triage, Thư viện | kind + state + từ + nghĩa ngắn + câu thật thu gọn |
| `ItemChip` | Inline, danh sách gọn | kind + state ở kích thước nhỏ nhất còn đọc được |
| `BucketSection` | Triage | Khối gập được, có đếm số, thao tác hàng loạt |
| `ClozeCard` | Phiên ôn | Mặt trước/sau, chuyển động lật |
| `RatingBar` | Phiên ôn | 4 nút FSRS + phím tắt hiện rõ trên desktop |
| `BudgetMeter` | Home, phiên ôn | Tiến độ so với ngân sách 30 phút — **không** phải thanh "hoàn thành 100%" |
| `SourceCard` | Home, Thư viện | Tiêu đề, loại, ngày, số item đã rút |
| `RedactionPreview` | Nhập | Văn bản có phần bị thay placeholder, sửa được — §9 |
| `AnalysisProgress` | Đang phân tích | Streaming theo bước, không phải spinner vô định |
| `CoverageBar` | Coverage report | % đã biết trên văn bản mới |
| `EmptyState` | Mọi màn | Luôn dẫn tới hành động tiếp theo |
| `StateBadge` | Khắp nơi | 5 trạng thái, đọc được khi đen trắng |
| `KindMark` | Khắp nơi | 3 loại, hoạt động cả trong dòng chữ |

---

## 9. Trạng thái bắt buộc

> Trạng thái không phải phần phụ. App này có một trạng thái đặc thù (`degraded`) mà thiết kế thông thường không có.

| Trạng thái | Xảy ra khi | Yêu cầu thiết kế |
|---|---|---|
| **Rỗng lần đầu** | Chưa có nguồn nào | Dẫn thẳng vào luồng dán văn bản đầu tiên. Không phải hình minh hoạ suông |
| **Rỗng tích cực** | Hàng đợi hôm nay đã xong | Phải thấy *đã hoàn thành*, không phải *không có gì*. Đây là phần thưởng hàng ngày |
| **Đang phân tích** | 5-20 giây | Streaming theo bước có tên ("đang tách câu" → "đang tra cụm" → "đang phân loại ngữ pháp"). Không spinner câm |
| **`degraded`** ⚠️ | LLM lỗi / hết quota Groq | Kết quả **vẫn hiện** (tầng NLP cục bộ vẫn chạy), kèm dải báo rõ: thiếu gì, nút thử lại phần thiếu. **Không phải màn lỗi** |
| **`sensitive`** ⚠️ | Nguồn đánh dấu nhạy cảm | Trạng thái nhìn thấy được và yên tâm được: người dùng phải *tin* là dữ liệu không rời máy |
| **Redaction** ⚠️ | Trước mọi lần gửi LLM | Cho thấy chính xác cái gì bị thay, sửa được trước khi gửi. Đây là màn tạo niềm tin |
| **Offline** | Mất mạng giữa phiên ôn | Vẫn ôn tiếp được; báo trạng thái đồng bộ chứ không chặn |
| **Tồn đọng** | Nghỉ vài ngày, hàng đợi dồn | Hiện số đã **giãn theo ngân sách**, không hiện con số 400 gây bỏ cuộc |
| **Lỗi thật** | Lưu hỏng, mất kết nối DB | Nói rõ hỏng gì, sửa thế nào. Không xin lỗi, không mơ hồ |

---

## 10. Responsive

| | |
|---|---|
| **Desktop** (≥ 1024px) | Bối cảnh chính cho *nhập* và *triage* — hai việc cần nhiều chữ trên màn. Triage có thể dùng hai cột: danh sách + panel chi tiết |
| **Tablet** | Không tối ưu riêng. Hoạt động đúng là đủ |
| **Mobile** (~390px) | Bối cảnh chính cho *ôn tập*. Nút chấm điểm phải nằm trong tầm ngón cái. Triage dùng được nhưng không phải nơi tối ưu |

Không có bảng nào được tràn ngang ra khỏi màn — cho vào container cuộn riêng.

---

## 11. Accessibility

- Toàn bộ phiên ôn **điều khiển được bằng bàn phím**: `Space` lật thẻ, `1-4` chấm điểm, `Esc` thoát.
  Phím tắt phải **hiện trên giao diện** ở desktop, không phải kiến thức ẩn
- Focus nhìn thấy rõ ở mọi control
- Trạng thái và loại item không bao giờ chỉ dựa vào màu
- Tương phản WCAG AA ở cả hai theme
- `prefers-reduced-motion` tắt chuyển động lật thẻ

---

## 12. Ràng buộc kỹ thuật ảnh hưởng thiết kế

| Ràng buộc | Hệ quả thiết kế |
|---|---|
| Web trước, **React Native sau** | Pattern điều hướng nên dịch được sang native. Tránh tương tác chỉ có ở web |
| Phase 6: **PWA + web push** | Cần icon set và splash; Home phải đẹp khi mở standalone |
| Groq free tier có rate limit | `degraded` là trạng thái **thường gặp**, không phải hiếm. Thiết kế nó tử tế |
| Câu gốc có offset chính xác | Highlight span phải bám đúng ký tự, không được "làm đẹp" bằng cách nới rộng vùng bôi |
| Ngân sách 30 phút/ngày | Mọi màn phải trả lời được "còn bao nhiêu" chứ không chỉ "đã làm bao nhiêu" |

---

## 13. Nội dung mẫu thật

> **Dùng đúng nội dung này khi dựng thiết kế.** Không lorem ipsum, không "Từ vựng 1 / Từ vựng 2".
> Nội dung thật là thứ để lộ ngay layout nào không chịu nổi.

### Nguồn mẫu

```
Tiêu đề : Sprint planning — Q4 platform migration
Loại    : meeting
Ngày    : 12.09.2026
Tag     : platform, q4
Độ dài  : 1.847 từ
```

### Câu gốc mẫu

1. *We should circle back on the scoping once we've aligned with the platform team — it might be worth timeboxing the spike.*
2. *I'd suggest we push back on that deadline; the dependency on the auth service hasn't been scoped yet.*
3. *What we really need is a rollback plan before we cut over to the new cluster.*
4. *Had we known about the rate limits earlier, we would have front-loaded the migration work.*
5. *Let's take this offline and loop in the infra folks — I don't want to derail the agenda.*

### Item mẫu

| Từ / cụm | Loại | Trạng thái | Nghĩa ngắn | Gặp |
|---|---|---|---|---|
| circle back on | phrase | new | quay lại bàn tiếp về (việc gì) | 3 lần / 2 nguồn |
| align with | phrase | learning | thống nhất với, đồng bộ với | 7 lần / 4 nguồn |
| push back on | phrase | new | phản đối, đẩy lùi (deadline, yêu cầu) | 2 lần / 2 nguồn |
| take this offline | phrase | new | bàn riêng sau, không bàn ở đây | 1 lần / 1 nguồn |
| scoping | lexeme | learning | việc xác định phạm vi công việc | 5 lần / 3 nguồn |
| spike | lexeme | new | task nghiên cứu ngắn có giới hạn thời gian | 2 lần / 1 nguồn |
| mitigate | lexeme | review | giảm thiểu (rủi ro, tác động) | 4 lần / 3 nguồn |
| front-load | lexeme | new | dồn công việc về giai đoạn đầu | 1 lần / 1 nguồn |
| it might be worth + V-ing | grammar | new | đề xuất nhẹ nhàng, không áp đặt | 3 lần / 3 nguồn |
| What + clause + is... (cleft) | grammar | learning | câu chẻ để nhấn mạnh | 2 lần / 2 nguồn |
| Had + S + V3 (đảo ngữ) | grammar | new | điều kiện loại 3 đảo ngữ | 1 lần / 1 nguồn |

### Số liệu mẫu cho Home

```
Đến hạn hôm nay     34 thẻ   (~7 phút / ngân sách 30 phút)
Chờ triage          12 item  từ "Sprint planning — Q4 platform migration"
Streak              9 ngày
Lên `known` tuần này 6 item
Độ chính xác 7 ngày  84%
Tổng item đang học   218
```

---

## 14. Không làm

| | |
|---|---|
| ❌ | Gamification nặng — huy hiệu, cấp độ, linh vật, hiệu ứng ăn mừng ồn ào |
| ❌ | Hero khổng lồ chiếm hết màn đầu. Đây là công cụ dùng hàng ngày, không phải landing page |
| ❌ | Tile số to cho mọi chỉ số. Chỉ số duy nhất xứng đáng nổi bật là **số thẻ đến hạn** |
| ❌ | Emoji làm ký hiệu phân loại hay dấu mục |
| ❌ | Onboarding nhiều bước. Người dùng là chính lập trình viên làm ra app này |
| ❌ | Màu trạng thái trùng với màu accent |
| ❌ | Thiết kế chỉ cho theme sáng rồi đảo ngược cơ học sang tối |

---

## 15. Thứ tự ưu tiên khi thiết kế

```
1. ContextSentence + hệ thống kind × state      ← nền tảng, mọi thứ khác phụ thuộc
2. Triage                                       ← màn quan trọng nhất của sản phẩm
3. Phiên ôn (ClozeCard + RatingBar)             ← màn dùng nhiều nhất
4. Home                                         ← điểm vào hàng ngày
5. Nhập + Redaction preview                     ← màn tạo niềm tin
6. Đang phân tích + trạng thái degraded         ← thường gặp hơn tưởng
─────────────────────────────────────────────── MVP ─
7. Thư viện · Từ điển · Chi tiết pattern · Cài đặt
```
