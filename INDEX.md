# SECOND BRAIN — Master Index
> **Updated:** 2026-06-05 | [[README.md]] | [[AGENTS.md]] | [[CONFIG.md]]

---

## Quick Stats (Dataview)

```dataview
TABLE length(rows) as Count
FROM "raw" OR "staging" OR "tracking"
GROUP BY status
```

---

## Master Tracking

> **Tracking tự động qua Dataview**. Để thêm item mới, tạo file trong thư mục `tracking/` hoặc tạo `raw/`, `staging/`. Dataview sẽ tự quét dựa trên frontmatter.

### Urgent / Doing

```dataview
TABLE category, priority, progress, deadline
FROM "tracking" OR "raw" OR "staging"
WHERE priority = "urgent" OR status = "doing" OR status = "reading"
SORT priority desc, file.mtime desc
```

### Inbox & Staging

```dataview
TABLE status, priority, file.ctime as Created
FROM "raw" OR "staging"
WHERE status = "raw" OR status = "staged"
SORT status desc, file.ctime desc
```

### To-Do / Backlog (Active)

```dataview
TABLE category, status, priority, progress
FROM "tracking"
WHERE status != "done" AND status != "archived" AND status != "doing"
SORT priority desc, file.mtime desc
```

### Groups & Hubs

> Items theo nhóm chủ đề. Nhóm lớn có Hub Note riêng trong `tracking/hubs/`.

```dataview
TABLE status, progress
FROM "tracking/hubs"
SORT status ASC, file.mtime desc
```

---

## Daily Logs

> Nhật ký hàng ngày được lưu thành các file riêng biệt trong thư mục: **[[dailylogs/]]**
> 
> *Bạn có thể xem trực tiếp trong folder hoặc bảo Agent tạo/xem log.*

---

## Plan

> Hỏi agent: *"Plan hôm nay"* / *"Tuần này làm gì"* / *"Không biết làm gì"*
> Agent phân tích dữ liệu Dataview → gợi ý dựa trên status, priority, deadline, progress.

_Chưa có plan — hỏi agent để được gợi ý._

---

> **Agent:** 
> - Tạo file tracking mới vào thư mục `tracking/` khi Human yêu cầu track item mới.
> - Dataview sẽ tự động cập nhật bảng, Agent không cần parse file INDEX.md này để đếm số liệu nữa.
> - Nhắc Human khi phát hiện overdue, stale, hoặc broken streak (Agent tự quét file trong `tracking/` và `raw/`).
> - Khi suggest plan: đọc toàn bộ files → phân tích → ưu tiên theo context.
