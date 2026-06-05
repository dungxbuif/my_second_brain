# 🧠 Second Brain — Documentation
> **Version:** 1.1 | **Status:** 🟢 Released | **Updated:** 2026-06-05
>
> ⚠️ **Đây là source docs chính thức** của toàn bộ framework này.
> Mọi thay đổi về thiết kế, quy tắc, cấu trúc đều phải được phản ánh ở đây.
> Agent phải đọc file này trước khi thao tác với hệ thống.

---

## Mục lục

- [Ý tưởng](#-ý-tưởng)
- [Mục đích](#-mục-đích)
- [Nguyên tắc thiết kế](#-nguyên-tắc-thiết-kế)
- [Pipeline](#-pipeline)
- [Cấu trúc thư mục](#-cấu-trúc-thư-mục)
- [File Reference](#-file-reference)
- [INDEX.md — Tracking System](#-indexmd--tracking-system)
- [Cách dùng — Example Prompts](#-cách-dùng--example-prompts)
- [Vai trò AI Agent](#-vai-trò-ai-agent)
- [Templates](#-templates)
- [Hướng phát triển](#-hướng-phát-triển)
- [Changelog](#-changelog)

---

## 💡 Ý tưởng

Tôi tiếp nhận hàng tá thông tin mỗi ngày — links trên Facebook, bài viết Viblo, paper trên ACM, video YouTube, ý tưởng dự án, sách muốn đọc — nhưng tất cả nằm rải rác, không hệ thống, và dần bị quên.

Second Brain giải quyết vấn đề đó bằng một **pipeline có cấu trúc**, kết hợp **AI Agent** để tự động hoá phần nặng nhọc nhất: thu thập, phân loại, tóm tắt. Tôi chỉ cần dump thông tin thô vào — AI xử lý — tôi đọc bản cô đọng khi rảnh — và tự tay viết wiki khi đã hiểu sâu.

Ngoài kiến thức, hệ thống tracking **mọi thứ đang dở dang** — sách, dự án, khóa học, thói quen, ý tưởng — để khi tôi "không biết làm gì", AI có data để gợi ý.

---

## 🎯 Mục đích

| Vấn đề | Giải pháp |
|--------|-----------|
| Thông tin rải rác | Pipeline thu thập → chuẩn hoá vào `raw/` |
| Không có thời gian đọc kỹ | AI tóm tắt cô đọng vào `staging/` |
| Kiến thức đọc rồi quên | Wiki cá nhân (`wiki/`) — tự viết = tự hiểu |
| Không biết đang dở bao nhiêu thứ | `INDEX.md` tracking linh hoạt |
| Hay trì hoãn, quên deadline | AI chủ động nhắc nhở |
| "Rảnh mà không biết làm gì" | AI phân tích data → suggest plan |

---

## 🧬 Nguyên tắc thiết kế

> Những quyết định nền tảng định hình cách framework hoạt động.

### 1. Human owns knowledge, Agent owns process
- Human quyết định cái gì đáng giữ, cái gì viết thành wiki
- Agent lo thu thập, phân loại, tóm tắt, nhắc nhở

### 2. Flexible over rigid
- INDEX.md dùng **1 bảng tracking chung** thay vì hardcode nhiều sections
- Category chỉ là 1 field — thêm loại mới = thêm row, không cần sửa cấu trúc
- Hệ thống phải thích ứng theo cách dùng thực tế, không ép user theo template

### 3. Raw data is sacred
- `raw/` luôn giữ nguyên văn source — không bao giờ sửa nội dung gốc
- `staging/` là bản AI xử lý — có thể tạo lại bất cứ lúc nào từ raw

### 4. Single source of truth
- `INDEX.md` = nơi duy nhất tracking trạng thái mọi thứ
- `CONFIG.md` = nơi duy nhất cấu hình hệ thống
- File này (`README.md`) = nơi duy nhất mô tả thiết kế

### 5. Progressive — bắt đầu đơn giản, phức tạp dần
- Không cần setup hết tất cả features ngày đầu
- Dùng đến đâu, mở rộng đến đó
- AI thích ứng gợi ý dựa trên data thực tế có trong INDEX

---

## 🏗️ Pipeline

```
INBOX Sources ──► raw/ ──► staging/ ──► wiki/ (human)
     │              │          │            │
     │              └──── INDEX.md ◄────────┘
     │                         │
     │                    AI phân tích
     │                         │
     │                    Gợi ý / Nhắc nhở
     │                         │
     └─────────────────────────┘
```

### 5 bước:

| # | Phase | Ai làm | Mô tả |
|---|-------|--------|-------|
| 1 | **COLLECT** | Agent | Quét INBOX sources → tách từng item |
| 2 | **NORMALIZE** | Agent | Tạo file `raw/` chuẩn hoá (metadata + nguyên văn) |
| 3 | **DIGEST** | Agent | Đọc raw → tóm tắt cô đọng → `staging/` |
| 4 | **INDEX** | Agent | Cập nhật tracking board trong `INDEX.md` |
| 5 | **SUGGEST** | Agent | Phân tích data → gợi ý plan / nhắc nhở |
| 6 | **CLEANUP** | Agent | Xóa staging & archive raw sau khi tạo wiki thành công |

> Chi tiết từng bước: xem [[GUIDE/WORKFLOW.md]]

---

## 📂 Cấu trúc thư mục

```
second-brain/
├── README.md        ← ★ Source docs chính thức (file này)
├── AGENTS.md        ← Quy tắc & hành vi cho AI Agent
├── CONFIG.md        ← Cấu hình: inbox sources, tags, nudge thresholds
├── INDEX.md         ← Master Tracking (Dataview Dashboard)
├── raw/             ← Tài liệu gốc đã chuẩn hoá header
├── staging/         ← Bản AI tóm tắt cô đọng
├── wiki/            ← Kiến thức hoàn thiện (human viết)
├── tracking/        ← Các files quản lý sách, task, dự án, khóa học
├── dailylogs/       ← Nhật ký hàng ngày
├── archive/         ← Nơi lưu trữ (vd: archive/raw/)
└── GUIDE/
    ├── WORKFLOW.md  ← Chi tiết 6 bước pipeline
    └── TEMPLATES/   ← Templates cho raw, staging, wiki, tracking entries
```

---

## 📄 File Reference

| File | Vai trò | Ai sửa |
|------|---------|--------|
| [[AGENTS.md]] | **Gateway** — entry point cho AI Agent, link đến docs chi tiết | Human |
| [[CONFIG.md]] | Cấu hình: inbox paths, allowed tags, taxonomy, nudge thresholds | Human |
| [[INDEX.md]] | Dataview Dashboard tracking + plan | Agent + Human |
| [[GUIDE/WORKFLOW.md]] | Chi tiết kỹ thuật 6 bước pipeline | Human |
| [[GUIDE/TEMPLATES/raw_entry.md]] | Template cho file raw/ | Human |
| [[GUIDE/TEMPLATES/staging_entry.md]] | Template cho file staging/ | Human |
| [[GUIDE/TEMPLATES/wiki_entry.md]] | Template cho wiki entry | Human |
| [[GUIDE/TEMPLATES/tracking_entry.md]] | Template cho tracking item | Human |
| [[GUIDE/TEMPLATES/daily_log.md]] | Template cho daily log | Human |
| **README.md** (file này) | Source docs chính thức | Human |

---

## 📊 INDEX.md — Tracking System

### Thiết kế (V1.1 - Dataview)

INDEX không còn dùng Markdown table thủ công. Thay vào đó, dùng plugin **Dataview** để tự động query từ các file markdown.
Mỗi item cần track (sách, task, khóa học) sẽ là **1 file riêng** nằm trong thư mục `tracking/`.

- **Category** = tự do: `knowledge`, `book`, `project`, `course`, `habit`, `research`, `task`, `idea`
- Thêm item mới = Tạo file mới trong `tracking/` (dùng `tracking_entry.md`)
- AI phân tích toàn bộ bảng → gợi ý thích ứng theo data thực tế

### Kèm theo

- **Quick Stats** — Agent tự đếm và cập nhật
- **Daily Log** — Nhật ký ngắn gọn mỗi ngày
- **Plan** — AI suggest khi được hỏi

---

## 🧑‍💻 Cách dùng — Example Prompts

### 📥 Thu thập & xử lý

| Muốn làm | Prompt |
|----------|--------|
| Xử lý toàn bộ inbox | *"Process inbox"* |
| Thêm 1 link/bài viết | *"Thêm link này vào inbox: [URL]"* |
| Tóm tắt 1 file cụ thể | *"Đọc và tóm tắt [file] vào staging"* |
| Tóm tắt từ conversation | *"Tóm tắt discussion về [topic] vào staging"* |

### 📊 Tracking

| Muốn làm | Prompt |
|----------|--------|
| Xem tình hình | *"Status check"* |
| Thêm item tracking | *"Track [item], category: [loại], status: [trạng thái]"* |
| Cập nhật progress | *"Update [item]: [progress mới]"* |
| Hoãn 1 item | *"Defer [item]"* |

### 🧠 AI gợi ý

| Muốn làm | Prompt |
|----------|--------|
| Không biết làm gì | *"Gợi ý gì đi"* / *"Plan hôm nay"* |
| Plan tuần | *"Lên plan tuần này"* |
| Draft wiki | *"Draft wiki cho [topic]"* |
| Phân tích gaps | *"Tôi đang thiếu gì?"* |
| Research topic | *"Tìm hiểu về [topic]"* |

### 📝 Nhật ký

| Muốn làm | Prompt |
|----------|--------|
| Log hôm nay | *"Log: [đã làm gì]"* |
| Review tuần | *"Review tuần này"* |

---

## 🤖 Vai trò AI Agent

### Permissions

| Quyền | Agent | Human |
|-------|-------|-------|
| Tạo `raw/`, `staging/` | ✅ | ✅ |
| Tạo `wiki/` | ❌ (chỉ đề xuất draft) | ✅ |
| Cập nhật `INDEX.md` | ✅ (Tạo/sửa file tracking/ thay vì sửa table) | ✅ |
| Xoá file / Move file | ✅ (Xoá staging, move raw sau khi có wiki) | ✅ |
| Sửa `CONFIG.md`, `README.md` | ❌ (chỉ đề xuất) | ✅ |
| Nhắc nhở chủ động | ✅ | — |

### Hành vi chủ động (Proactive)

Agent **tự nhắc** khi phát hiện (ngưỡng cấu hình trong CONFIG.md):

| Trigger | Mặc định |
|---------|---------|
| Raw chưa staged | > 7 ngày |
| Task quá deadline | ngay lập tức |
| Item deferred | > 14 ngày → hỏi lại |
| Item chưa update (sách, project...) | > 7-14 ngày |
| Chưa daily log | hôm nay |

### Khi suggest plan

```
Agent đọc toàn bộ INDEX.md → phân tích:
  1. 🔴 Urgent + Overdue → ưu tiên đầu
  2. 🔄 Items đang dở → tiếp tục
  3. ⚡ Quick wins (< 30 phút) → chen giữa
  4. 🧠 Deep work → block thời gian
  5. 📌 Backlog → pick 1 nếu rảnh
```

---

## 🗂️ Templates

| Template | Dùng cho |
|----------|---------|
| [[GUIDE/TEMPLATES/raw_entry.md]] | File raw/ — metadata + nguyên văn |
| [[GUIDE/TEMPLATES/staging_entry.md]] | File staging/ — AI tóm tắt cô đọng |
| [[GUIDE/TEMPLATES/wiki_entry.md]] | Wiki entry — human viết |
| [[GUIDE/TEMPLATES/tracking_entry.md]] | File tracking (sách, khoá học, dự án...) |
| [[GUIDE/TEMPLATES/daily_log.md]] | File nhật ký hàng ngày |
| [[GUIDE/TEMPLATES/review_card.md]] | Interview prep card |

---

## 🧬 Self-Evolving Framework

> Framework này **tự phát triển** theo cách dùng thực tế.

### Cơ chế

Khi Human đưa ra yêu cầu mà framework chưa cover (chưa có workflow, category, template phù hợp), Agent phải:

1. **Xử lý yêu cầu trước** — giải quyết vấn đề ngay
2. **Đề xuất cải tiến** — suggest thêm config/workflow/template
3. **Nếu Human đồng ý** — đề xuất nội dung cụ thể cho các file liên quan
4. **Cập nhật README.md** — ghi changelog (BẮT BUỘC)

### Ví dụ

```
Human: "Track tiến độ đọc paper, mỗi paper có sections"
Agent:
  → Xử lý: thêm entries vào INDEX
  → Đề xuất: "💡 Paper tracking khác sách. Đề xuất thêm
    progress format 'S.3/7' vào CONFIG. Đồng ý?"
```

```
Human: "Tôi muốn track calories"
Agent:
  → Xử lý: thêm entry category 'health'
  → Đề xuất: "💡 Category 'health' mới. Thêm vào CONFIG?"
```

### File nào bị ảnh hưởng khi framework evolve

| File | Khi nào cần cập nhật |
|------|---------------------|
| CONFIG.md | Thêm tags, categories, statuses, rules mới |
| GUIDE/WORKFLOW.md | Thêm/sửa steps pipeline |
| GUIDE/TEMPLATES/ | Thêm template mới |
| **GUIDE/README.md** | **LUÔN LUÔN** — ghi changelog |

> Rule: [[AGENTS.md]] phần SELF-EVOLVE quy định chi tiết hành vi agent.

---

## 🔮 Hướng phát triển

> Ý tưởng mở rộng — chưa implement, sẽ đánh giá lại khi framework ổn định.

- [ ] Spaced Repetition — review schedule tự động
- [ ] Knowledge Graph — visualize connections
- [ ] Auto-import bookmarks → raw/
- [ ] Daily digest — "5 điều nên ôn hôm nay"
- [ ] Obsidian Dataview integration
- [ ] Multi-vault sync (KNOWLEDGE/, QUESTIONS_BANK/)
- [ ] Progress analytics — streak charts, velocity

---

## 📜 Changelog

> Mỗi lần thay đổi thiết kế lớn, ghi lại ở đây.

### v1.1 (2026-06-05)
**Status:** 🟢 Released

**Thay đổi kiến trúc (Scalability):**
- Đổi từ Markdown table sang Dataview block trong `INDEX.md`.
- Mỗi tracking item giờ là 1 file riêng trong `tracking/`.
- Tách Daily Log ra thư mục `dailylogs/` riêng biệt.
- Thêm quy tắc Cleanup: Sau khi wiki được tạo, Agent tự xóa file staging tương ứng và di chuyển file raw vào `archive/raw/`.

### v1.0 (2026-06-05)
**Status:** 🟢 Released

**Thiết kế ban đầu:**
- Pipeline 5 bước: COLLECT → NORMALIZE → DIGEST → INDEX → SUGGEST
- Cấu trúc: raw/ → staging/ → wiki/
- INDEX.md = 1 bảng tracking linh hoạt (không hardcode sections)
- CONFIG.md = cấu hình tập trung (inbox sources, tags, nudge thresholds)
- AGENTS.md = **gateway** cho AI Agent, link đến docs chi tiết
- Templates: raw, staging, wiki, review_card

**Quyết định thiết kế:**
- Human owns wiki/ — Agent chỉ đề xuất draft
- raw/ giữ nguyên văn — staging/ là AI distillation
- INDEX dùng 1 bảng chung + category field → linh hoạt mở rộng
- Agent chủ động nhắc nhở, ngưỡng configurable
- Framework thích ứng theo cách dùng, không ép rigid structure
- **Self-evolve:** Agent chủ động đề xuất cải tiến khi phát hiện gap
- **README.md = source docs chính thức** — mọi thay đổi phải ghi changelog

<!-- 
### v1.1 (YYYY-MM-DD)
**Changes:**
- ...

### v2.0 (YYYY-MM-DD)
**Breaking changes:**
- ...
-->

---

*Workflow chi tiết: [[GUIDE/WORKFLOW.md]] | Config: [[CONFIG.md]] | Agent rules: [[AGENTS.md]] | Dashboard: [[INDEX.md]]*
