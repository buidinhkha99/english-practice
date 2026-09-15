# Design — Đặc tả màn hình

> Đi kèm [`design-brief.md`](design-brief.md). Brief nói *tại sao*, tài liệu này nói *cái gì phải có trên màn*.
> Nội dung mẫu dùng ở đây lấy từ §13 của brief — **dùng đúng nội dung đó**, không thay bằng lorem.
>
> Mỗi màn liệt kê thành phần **theo thứ tự ưu tiên thị giác**, không phải theo thứ tự đặt trên layout.
> Thiết kế được tự do về bố cục miễn giữ đúng thứ tự quan trọng.

---

# PHẦN A — MVP

## A1. Home

**Mục đích:** trả lời hai câu trong 2 giây — *hôm nay phải ôn bao nhiêu?* và *có gì mới cần xử lý không?*

### Nội dung theo thứ tự ưu tiên

| # | Khối | Nội dung mẫu | Ghi chú |
|---|---|---|---|
| 1 | **Hàng đợi hôm nay** | `34 thẻ · ~7 phút` + nút `Bắt đầu ôn` | Chỉ số duy nhất được phép to. Có `BudgetMeter` so với 30 phút |
| 2 | **Ô dán nhanh** | placeholder: *"Dán note họp, email, tài liệu..."* | Đường ngắn nhất vào vòng lặp. **Không được nằm dưới màn đầu** |
| 3 | **Chờ triage** | `12 item từ "Sprint planning — Q4 platform migration"` | Chỉ hiện khi có. Click → Triage |
| 4 | **Tiến độ** | streak `9 ngày` · lên known tuần này `6` · chính xác 7 ngày `84%` | Nhỏ, không phải tile số to |
| 5 | **Nguồn gần đây** | 5 `SourceCard`, mỗi cái kèm số item đã rút | Click → chi tiết nguồn |

### Trạng thái

- **Lần đầu (chưa có nguồn):** cả màn thu về một việc — dán văn bản đầu tiên. Nói rõ chuyện gì sẽ xảy ra sau khi dán.
- **Hàng đợi rỗng:** *đã xong hôm nay*, không phải *không có gì*. Vẫn giữ ô dán nhanh nổi bật — đây là lúc người dùng rảnh để nhập nguồn mới.
- **Tồn đọng:** nghỉ 5 ngày, hàng đợi thật 412 thẻ → hiện **số đã giãn theo ngân sách** (`34 thẻ hôm nay`) kèm ghi chú nhỏ *"đang giãn dần 412 thẻ tồn"*. Không bao giờ hiện 412 ở vị trí số 1.

### Mobile
Ô dán nhanh có thể thu thành một nút mở sheet. Hàng đợi + nút ôn vẫn phải nằm trên fold.

---

## A2. Nhập văn bản

**Mục đích:** từ Ctrl+V tới "đang phân tích" trong dưới 20 giây.

### Nội dung

| # | Thành phần | Chi tiết |
|---|---|---|
| 1 | **Vùng dán** | Tối đa 20.000 ký tự. Hiện số ký tự/từ khi gõ. Nhận cả paste và kéo-thả file (Phase 3) |
| 2 | **Metadata** | Tiêu đề (tự sinh nếu bỏ trống) · Loại nguồn · Ngày · Tag |
| 3 | **Cờ `sensitive`** | Toggle, kèm giải thích một dòng: *"Chỉ phân tích cục bộ, không gửi dữ liệu đi đâu"* |
| 4 | **Nút chính** | `Phân tích` |

**Loại nguồn:** `meeting` · `email` · `doc` · `chat` · `article` · `transcript` · `other`
Mặc định là `meeting` — đó là ca dùng thường gặp nhất.

### Trạng thái
- **Trùng nguồn:** đã nhập văn bản giống hệt → cảnh báo, cho phép mở nguồn cũ hoặc nhập đè.
- **Quá dài:** vượt 20.000 ký tự → báo rõ số dư, đề nghị cắt làm nhiều nguồn.

### Ghi chú thiết kế
Metadata **không được chặn đường**. Người dùng phải dán được rồi bấm phân tích ngay mà không điền gì —
tiêu đề tự sinh, ngày mặc định hôm nay, loại mặc định `meeting`.

---

## A3. Xem trước redaction ⚠️

**Mục đích:** cho người dùng thấy **chính xác** cái gì sẽ rời khỏi máy, trước khi nó rời khỏi máy.

> Đây là màn tạo niềm tin. Người dùng sắp gửi biên bản họp nội bộ công ty cho một dịch vụ bên thứ ba.
> Nếu màn này làm họ lo, họ sẽ không dùng app. Nếu nó làm họ chủ quan, app gây hại thật.

### Nội dung

| # | Thành phần | Chi tiết |
|---|---|---|
| 1 | **Văn bản có đánh dấu** | Phần bị thay hiện rõ: `[EMAIL]` `[PERSON]` `[PHONE]` `[URL]` `[TOKEN]` |
| 2 | **Tóm tắt** | *"Đã thay 7 mục: 3 tên người, 2 email, 1 URL nội bộ, 1 token"* |
| 3 | **Sửa được** | Click vào một mục để khôi phục bản gốc, hoặc chọn thêm đoạn để thay thủ công |
| 4 | **Điểm đến** | Nói thẳng: *"Bản đã che sẽ được gửi tới Groq để phân tích"* |
| 5 | **Hai nút** | `Gửi phân tích` · `Chỉ phân tích cục bộ` (= bật `sensitive`) |

### Ghi chú thiết kế
- Đánh dấu redaction phải **khác hẳn** highlight item (§7 brief) — hai hệ thống bôi khác nhau trên cùng loại bề mặt, không được nhầm.
- Không dùng màu cảnh báo đỏ cho toàn bộ — redaction là hành vi bình thường, không phải sự cố.
- Bỏ qua được, nhưng bỏ qua phải là hành động có ý thức, không phải mặc định.

---

## A4. Đang phân tích

**Mục đích:** 5-20 giây chờ phải cảm thấy như tiến trình, không như treo máy.

### Nội dung
Streaming theo bước có tên thật, mỗi bước xong thì đánh dấu:

```
✓ Tách câu                 127 câu
✓ Phân tích từ             1.847 từ → 412 từ có nghĩa
✓ Bắt cụm ứng viên         38 cụm
⟳ Phân loại ngữ pháp       đang xử lý 84/127 câu
○ Tra nghĩa theo ngữ cảnh
○ Đối chiếu kho đã học
```

### Trạng thái
- **`degraded` giữa chừng:** bước LLM lỗi → bước đó chuyển sang cảnh báo, các bước cục bộ vẫn chạy tiếp, kết thúc vẫn vào được Triage.
- **Cache hit:** dán lại văn bản cũ → gần như tức thì. Nói rõ *"Đã phân tích trước đó — dùng lại kết quả"*, đừng giả vờ đang tính.
- **`sensitive`:** các bước LLM hiện là "bỏ qua (chế độ cục bộ)", không phải lỗi.

### Ghi chú thiết kế
Không spinner vô định. Người dùng là lập trình viên — họ muốn biết đang làm gì và còn bao lâu.

---

## A5. Triage ⭐

**Mục đích:** từ kết quả phân tích → chọn ra thứ đáng học. **Màn quan trọng nhất của sản phẩm.**

### Bố cục
Desktop nên hai cột: **danh sách item** (trái) + **panel chi tiết** (phải, A6).
Mobile: một cột, chi tiết mở dạng sheet.

### Bốn khối

| Khối | Mặc định | Nội dung mẫu |
|---|---|---|
| **Mới** | Mở, đầy đủ, sắp theo priority, **chưa tick sẵn** | `circle back on` · `push back on` · `spike` · `front-load` · `it might be worth + V-ing` |
| **Đang học** | Mở, hiện gọn hơn | `align with` *(gặp lần thứ 7)* · `scoping` *(lần thứ 5)* |
| **Đã thuộc** | Thu gọn sau accordion | `mitigate` · `deadline` · `dependency` |
| **Bỏ qua** | Ẩn, có nút hiện lại | — |

### Mỗi `ItemRow` phải có

```
[kind]  circle back on                      [state]
        quay lại bàn tiếp về (việc gì)
        "We should circle back on the scoping once we've..."   ← ContextSentence thu gọn
        3 lần · 2 nguồn                                 [checkbox]
```

### Điều khiển

| # | Thành phần | Chi tiết |
|---|---|---|
| 1 | **Chuyển chế độ** | `Đề xuất` (N item đáng học nhất) ↔ `Tất cả` (liệt kê hết, tự tick) |
| 2 | **Quét tự đánh giá** | Với item chưa rõ: 3 nút `Đã biết` / `Không chắc` / `Chưa biết` — quét nhanh, không phải kiểm tra |
| 3 | **Thao tác hàng loạt** | Chọn tất cả trong khối · bỏ chọn · đảo chọn |
| 4 | **Bỏ qua vĩnh viễn** | Trên từng item |
| 5 | **Thanh chốt** | Cố định dưới: `Đã chọn 9 item` + cảnh báo nếu vượt trần + nút `Thêm vào học` |

### Trạng thái
- **Vượt trần item mới/ngày:** chọn 22 item khi trần là 15 → cảnh báo mềm, cho phép tiếp tục nhưng nói rõ hệ quả (*"sẽ dồn sang ngày mai"*). **Không chặn cứng.**
- **Không có item mới:** cả văn bản đều đã biết → đây là tin vui, không phải màn rỗng buồn. Hiện coverage: *"Bạn đã biết 94% văn bản này"*.
- **Rời trang giữa chừng:** lựa chọn được giữ dạng draft, quay lại làm tiếp.
- **`degraded`:** dải báo trên đầu — *"Chưa phân loại được ngữ pháp và nghĩa theo ngữ cảnh"* + nút thử lại phần thiếu. Item từ/cụm vẫn chọn được bình thường.

---

## A6. Panel chi tiết item

**Mục đích:** đủ thông tin để quyết định *"có học cái này không"* mà không rời Triage.

### Nội dung

| # | Thành phần | Nội dung mẫu |
|---|---|---|
| 1 | Từ/cụm + loại + trạng thái | `align with` · phrase · đang học |
| 2 | Phiên âm + nghe | `/əˈlaɪn wɪð/` 🔊 |
| 3 | Nghĩa | EN: *to agree on an approach or goal* · VN: *thống nhất với, đồng bộ với* |
| 4 | **Mọi occurrence thật** | 7 lần trong 4 nguồn, mỗi cái là một `ContextSentence` kèm tên nguồn + ngày |
| 5 | Mức độ | CEFR `B2` · tần suất chung `band 4` · trong corpus của bạn `7 lần` |
| 6 | Liên quan | Pattern ngữ pháp trong cùng câu · cụm cùng họ (`align on`, `alignment`) |
| 7 | Hành động | `Thêm vào học` · `Đánh dấu đã biết` · `Bỏ qua` |

### Ghi chú thiết kế
Occurrence thật là phần dài nhất và **quan trọng nhất** của panel — đừng nén nó xuống dưới để nhường chỗ
cho định nghĩa từ điển. Nguyên lý #1: corpus của người dùng là nguồn chân lý.

---

## A7. Phiên ôn ⭐

**Mục đích:** màn được dùng nhiều nhất. Toàn màn hình, không phân tâm, thoát được bất cứ lúc nào.

### Khung màn

| Vị trí | Nội dung |
|---|---|
| Trên | Tiến độ `12/34` · `BudgetMeter` · nút thoát |
| Giữa | Thẻ |
| Dưới | `RatingBar` (sau khi lật) hoặc nút `Hiện đáp án` |
| Góc | Gắn cờ item · xem nguồn |

### Năm loại thẻ

**1. Cloze thật** *(mặc định)*
```
Mặt trước:  We should circle back on the [____] once we've aligned
            with the platform team.
            ↳ từ "Sprint planning — Q4 platform migration" · 12.09

Mặt sau:    scoping  /ˈskəʊpɪŋ/  🔊
            việc xác định phạm vi công việc
            + câu đầy đủ, ô trống đã điền, highlight
```

**2. Nhận diện** — từ + câu chứa → nghĩa EN + VN
**3. Sản sinh** — nghĩa VN + gợi ý ngữ cảnh → từ/cụm đúng *(có ô gõ)*
**4. Nghe** — nút phát TTS câu gốc, không hiện chữ → transcript + nghĩa
**5. Ngữ pháp** — yêu cầu áp dụng pattern → câu đúng + giải thích

### `RatingBar`

```
[1] Again        [2] Hard        [3] Good        [4] Easy
    < 1 phút        2 ngày          5 ngày         12 ngày
```

Hiện **khoảng thời gian thật** dưới mỗi nút — đó là thông tin người dùng cần để chấm đúng.
Phím tắt `1-4` hiện rõ trên desktop; mobile là 4 nút trong tầm ngón cái.

### Trạng thái
- **Chạm ngân sách 30 phút:** đề nghị dừng, hiện thành quả, **cho phép học tiếp** nếu muốn.
- **Offline:** dải trạng thái nhỏ, ôn tiếp bình thường, rating xếp hàng chờ đồng bộ.
- **Hết thẻ giữa chừng do chấm `Again` liên tục:** thẻ quay lại trong cùng phiên — tiến độ không được chạy lùi một cách khó hiểu.

### Ghi chú thiết kế
Lật thẻ là **chuyển động duy nhất** xứng đáng trong app (§5.4 brief). Nó đánh dấu ranh giới nhận thức
giữa "đang cố nhớ" và "đã biết đáp án".

---

## A8. Tổng kết phiên

**Mục đích:** đóng vòng lặp hàng ngày. Ngắn — người dùng đang muốn đóng tab.

### Nội dung
```
Xong 34 thẻ trong 8 phút 12 giây

Chính xác        29/34  (85%)
Lên `known`      2 item:  mitigate · dependency
Cần chú ý        3 item chấm `Again` nhiều lần:  front-load · take this offline · Had + S + V3

Ngày mai: 28 thẻ
```

### Ghi chú thiết kế
Không confetti, không huy hiệu. Thông tin hữu ích là phần thưởng đủ cho persona này.
"Cần chú ý" là phần giá trị nhất — nó nói cho người dùng biết nên xem lại gì.

---

# PHẦN B — Sau MVP

## B1. Thư viện

**Mục đích:** duyệt và lọc toàn bộ kho đã học.

- Lọc theo **kind × state** (hai trục §4 brief), CEFR, nguồn, tag, khoảng ngày
- Sắp xếp: mới thêm · sắp đến hạn · gặp nhiều nhất · khó nhất (nhiều lapse)
- Ba tab: `Item` · `Nguồn` · `Pattern ngữ pháp`
- Thao tác hàng loạt: tạm dừng · bỏ qua · đặt lại tiến độ

Đây là nơi hệ thống kind × state bị thử thách nặng nhất — hàng trăm dòng, hai trục cùng hiện.

## B2. Chi tiết nguồn

- Văn bản gốc đầy đủ, **highlight mọi item đã rút** theo kind × state
- Cột bên: danh sách item từ nguồn này, click → cuộn tới vị trí trong văn bản
- Thống kê: đã rút bao nhiêu, đã thuộc bao nhiêu, coverage của nguồn này
- Hành động: phân tích lại · xoá nguồn *(cảnh báo cascade)*

## B3. Từ điển

- Ô tra cứu, kết quả tức thì
- **Occurrence thật của người dùng lên trước ví dụ từ điển** — khác biệt so với mọi từ điển khác
- Không có occurrence → hiện ví dụ từ điển, ghi rõ *"bạn chưa gặp từ này trong tài liệu của mình"*
- Một chạm `Thêm vào học`
- Lịch sử tra cứu; tra 3 lần chưa học → gợi ý thêm vào

## B4. Chi tiết pattern ngữ pháp

- Tên pattern · công thức · CEFR · giải thích tiếng Việt
- **Mọi câu thật của người dùng dùng pattern này** — đây là phần chính, không phải phần phụ
- Ví dụ mẫu từ catalog *(chỉ khi chưa đủ câu thật)*
- Pattern liên quan
- Trạng thái học + lịch ôn

## B5. Coverage report

- Ô dán văn bản mới → `CoverageBar`: *"Bạn đã biết 87% văn bản này"*
- Danh sách từ/cụm chưa biết, sắp theo priority, chọn học được ngay
- So sánh với lần đo trước cùng loại nguồn

Đây là metric bắc sao của sản phẩm — thiết kế phải làm nó cảm thấy như một cột mốc, không như một công cụ phụ.

## B6. Cài đặt

| Nhóm | Nội dung |
|---|---|
| **Học** | **Ngân sách phút/ngày** (5-120, mặc định 30) — hiện rõ trần item mới suy ra từ đó · tỷ lệ trộn · dải CEFR |
| **Riêng tư** | Provider AI · mặc định cờ `sensitive` · quy tắc redaction · xoá dữ liệu |
| **Thông báo** | Khung giờ nhắc · bật/tắt · email tuần *(Phase 6)* |
| **Dữ liệu** | Export JSON · thống kê chi phí AI theo nguồn |
| **Giao diện** | Theme sáng / tối / theo hệ thống |

Ô ngân sách phút/ngày là control quan trọng nhất của cả màn — nó điều khiển mọi giới hạn khác.
Khi đổi giá trị, trần item mới phải cập nhật ngay trước mắt người dùng.
