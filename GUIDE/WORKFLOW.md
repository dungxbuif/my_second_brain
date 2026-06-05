# ⚙️ WORKFLOW v2 — Pipeline Chi Tiết
> Đọc trước: [[../AGENTS.md]] | Config: [[../CONFIG.md]]

---

## 🔁 Full Pipeline

```
INBOX Sources ──┐
                ├──► raw/ ──► staging/ ──► wiki/ (human)
Human dump ─────┘  ▲  │          │            │
                   │  └──── INDEX.md ◄────────┘
                   │
             raw/ cũng là inbox
          (human có thể ném thẳng vào đây)
```

---

## 📥 PHASE 1: COLLECT (Thu thập)

### 1.1 Khi User yêu cầu "Process inbox"
Agent quét **3 nguồn** theo thứ tự:

```
Nguồn 1: INBOX files (CONFIG.md)
  → Master.md, Quick notes.md, ...
  → Tách items → tạo file raw/ mới (nếu chưa có)

Nguồn 2: raw/ (file chưa chuẩn hoá)
  → Scan toàn bộ raw/
  → Tìm file THIẾU frontmatter hoặc frontmatter không đầy đủ
  → Chuẩn hoá tại chỗ (thêm header, rename nếu cần)

Nguồn 3: User chỉ định trực tiếp
  → Link, text, file cụ thể → tạo raw/ mới
```

### 1.2 Cách phát hiện file raw/ chưa chuẩn hoá

File được coi là **chưa chuẩn hoá** nếu:
- Không có frontmatter `---` block ở đầu file
- Có frontmatter nhưng thiếu fields bắt buộc (`title`, `source`, `captured_at`, `status`)
- Tên file không theo convention `YYYY-MM-DD_slug.md`

Khi phát hiện → Agent **chuẩn hoá tại chỗ**:
1. Đọc nội dung → phân tích topic, source
2. Thêm/sửa frontmatter đầy đủ
3. Rename file theo convention (nếu cần)
4. Giữ nguyên nội dung gốc bên dưới

### 1.3 Khi User ném link/text trực tiếp
- Tạo file raw/ ngay với nội dung được cung cấp
- Nếu là URL → cố gắng fetch nội dung, nếu không được thì ghi `type: link-only`

### 1.4 Khi User chỉ định file cụ thể
- Đọc file đó, xử lý như INBOX source

---

## 📄 PHASE 2: NORMALIZE (Chuẩn hóa vào raw/)

### 2.1 Template cho raw/ file

```markdown
---
title: "Mô tả ngắn gọn mục đích tài liệu"
source: "URL chính hoặc 'Master.md' hoặc 'conversation'"
type: link | article | video | pdf | note | code | job-posting
captured_at: YYYY-MM-DD
tags: [system-design, database]
status: unread
priority: 🟡 normal
---

## Mô tả
> [1-3 câu] Nguồn này nói về gì. Tại sao đáng lưu. Phân loại sơ bộ.

## Nội dung gốc

<!-- OPTION A: Nguyên văn nếu ngắn (<500 dòng) -->
[Paste nội dung gốc ở đây]

<!-- OPTION B: Link + tóm tắt nếu dài (>500 dòng) -->
> 📎 Full content: [Link đến source](url)
> 
> **Preview (5 dòng đầu):**
> ...
```

### 2.2 Naming Convention
```
Format: YYYY-MM-DD_<slug-mô-tả>.md
Slug: kebab-case, tiếng Anh, tối đa 6 từ

✅ 2026-06-05_kafka-io-disk-vs-ram.md
✅ 2026-06-05_replication-lag-sync-async.md
✅ 2026-06-05_db-design-roadmap-senior.md
❌ 2026-06-05_bài_viết_hay.md (tiếng Việt)
❌ 2026-06-05_article.md (quá chung chung)
```

### 2.3 Xử lý edge cases

| Case | Cách xử lý |
|------|------------|
| URL không fetch được | `type: link-only`, ghi URL trong nội dung gốc |
| Ảnh/Video/PDF | `type: video/pdf`, link đến file, ghi mô tả nếu có |
| Item không liên quan đến tech | Vẫn tạo raw/ nhưng tag `#personal` hoặc bỏ qua, hỏi User |
| Job posting / quảng cáo | Skip hoặc tạo với `type: job-posting` nếu có giá trị |
| Item trùng lặp | Kiểm tra raw/ hiện có, nếu trùng → skip, ghi log |

---

## 🧠 PHASE 3: DIGEST (AI tóm tắt vào staging/)

Đây là bước **quan trọng nhất** — Agent biến raw thành kiến thức cô đọng.

### 3.1 Template cho staging/ file

```markdown
---
title: "Tên concept / topic"
raw_source: "[[raw/YYYY-MM-DD_slug.md]]"
created: YYYY-MM-DD
tags: [tag1, tag2]
wiki_path: "engineering/system-design"
status: 🧠 staged
priority: 🟡 normal
---

# {Title}

> **TL;DR:** 1 câu tóm bản chất. Đọc xong câu này phải hiểu ngay topic là gì.

## Definition (Định nghĩa)
<!-- 1-2 câu chuẩn xác -->

## How it works (Cơ chế)
<!-- Step-by-step hoặc diagram -->

```
Flow / Diagram nếu cần
```

## Trade-offs (Đánh đổi)

| Pros ✅ | Cons ❌ |
|---------|---------|
| ... | ... |

## Key Terms (Thuật ngữ mới)
- **Term A** — giải thích ngắn
- **Term B** — giải thích ngắn

## Related (Liên kết)
- [[staging/related-entry]] hoặc [[wiki/path/to/entry]] nếu đã có
- 🔍 Cần tìm hiểu thêm: [topic chưa có trong vault]

---
*Raw source: [[raw/YYYY-MM-DD_slug.md]] | Staged: YYYY-MM-DD*
```

### 3.2 Quy tắc viết staging

| Quy tắc | Chi tiết |
|---------|----------|
| Độ dài | ~1 trang A4 (300-500 từ). Không viết quá dài |
| Giọng văn | Cô đọng, mang tính gợi nhớ. Như flashcard nâng cao |
| Diagram | Luôn thêm nếu có flow/process/comparison |
| New terms | Liệt kê rõ ở cuối — đây là input cho lần học tiếp |
| wiki_path | Phải ghi rõ đề xuất phân loại |
| Không bịa | Nếu source không đủ info → ghi "cần tìm hiểu thêm" |

---

## 📊 PHASE 4: INDEX (Tracking)

### 4.1 Dataview Tracking
Dataview sẽ tự động quét các file `raw/` và `staging/` thông qua frontmatter (`status`, `priority`, `category`). Không cần phải thêm row thủ công vào `INDEX.md` nữa.

### 4.2 Cập nhật Tracking (Sách, Khóa học, Tasks, Projects...)

Nếu có việc mới cần track, tạo 1 file riêng trong thư mục `tracking/` (dùng template `tracking_entry.md`).
- File: `tracking/ten-du-an.md`
- Frontmatter: `category: project`, `status: 🔄 doing`

Dataview trong `INDEX.md` sẽ tự động hiển thị nó lên bảng To-Do/Doing.

---

## 💡 PHASE 5: SUGGEST (Khi được hỏi)

### 5.1 "Gợi ý plan hôm nay / tuần này"
Agent phân tích:
- Items trong staging/ có `status: 🧠 staged` → ưu tiên `priority: 🔴 urgent`
- Items trong Backlog → gợi ý pick up
- TODO tasks sắp deadline

Output: Danh sách 3-5 items nên focus, chia theo ngày.

### 5.2 "Đề xuất draft wiki"
Agent đọc staging entry → viết draft wiki entry theo template wiki_entry.md
→ KHÔNG tự tạo file → Trả nội dung cho Human review

### 5.3 "Phân tích knowledge gaps"
Agent so sánh wiki taxonomy (CONFIG.md) với nội dung hiện có trong wiki/
→ Liệt kê categories còn trống hoặc thiếu

---

## 🧹 PHASE 6: CLEANUP (Dọn dẹp)

Sau khi Human hoàn tất việc chuyển đổi kiến thức từ `staging/` sang `wiki/` thành công:

1. **Delete Staging**: Agent sẽ XÓA (delete) file `staging/` tương ứng (vì nội dung đã được lưu vĩnh viễn ở wiki).
2. **Archive Raw**: Agent sẽ CHUYỂN (move) file `raw/` tương ứng sang thư mục `archive/raw/`. Việc này giữ `raw/` không bị phình to nhưng vẫn lưu được tài liệu gốc để đối chiếu trong tương lai.

---

## ⏱️ TRIGGERS (Khi nào chạy gì)

| Human nói | Agent làm |
|-----------|-----------|
| "Process inbox" | Phase 1→2→3→4: Quét INBOX → raw/ → staging/ → INDEX |
| "Tóm tắt file X" | Phase 2→3→4: raw/ → staging/ → INDEX |
| "Plan tuần này" | Phase 5.1: Phân tích staged + backlog → suggest |
| "Draft wiki cho [topic]" | Phase 5.2: Đọc staging → viết draft |
| "Thêm [text/link] vào inbox" | Phase 2→3→4: Tạo raw/ → staging/ → INDEX |
| "Status check" | Đọc INDEX.md → báo cáo dashboard |

---

*Xem thêm: [[../AGENTS.md]] | [[../CONFIG.md]] | [[TEMPLATES/]]*
