---
type: Reference
title: AGENTS Gateway
description: Entry point and rules for AI agents operating within the framework
timestamp: 2026-06-15T14:55:00Z
---

# AGENTS.md — Second Brain Gateway
> **Version:** 1.1 | **Updated:** 2026-06-16
> 
> **Đây là GATEWAY** — AI Agent đọc file này ĐẦU TIÊN.
> Nắm quy tắc ở đây, đọc chi tiết qua các link bên dưới.

---

## Hệ thống này là gì

Personal Life & Knowledge Management System kết hợp AI.

- **Human** = Kiểm soát, quyết định, viết wiki
- **Orchestrator/Agent layer** = Thu thập, xử lý, tóm tắt, tracking, gợi ý, nhắc nhở, **và đề xuất cải tiến framework**


---

## Đọc thêm

| Cần gì | Đọc ở đâu |
|--------|-----------|
| Tổng quan framework, pipeline, ý tưởng, cách dùng | [[README.md]] ← **Source docs chính thức** |
| Chi tiết 6 bước pipeline | [[GUIDE/WORKFLOW.md]] |
| Cấu hình: inbox, tags, nudge thresholds | [[CONFIG.md]] |
| Dashboard tracking | [[INDEX.md]] |
| Templates | [[GUIDE/TEMPLATES/]] |

---

## QUY TẮC

### R1: Permissions

| Quyền | Agent | Human |
|-------|-------|-------|
| Tạo file `raw/`, `staging/` | yes | yes |
| Tạo file `tracking/`, `dailylogs/` | yes | yes |
| Tạo file `wiki/` | no (chỉ đề xuất draft) | yes |
| Cập nhật `INDEX.md` | yes (Qua Dataview/Tracking files) | yes |
| Xóa/Move file | yes (Archive/mark staging, move raw sau khi tạo wiki) | yes |
| Sửa `CONFIG.md` | no (chỉ đề xuất) | yes |
| Sửa `README.md` | no (chỉ đề xuất) | yes |
| Nhắc nhở chủ động | yes | — |

### R2: Ngôn ngữ
- **File names, Tags:** Tiếng Anh
- **Nội dung:** Việt-Anh mix OK

### R3: Atomic notes
1 file = 1 topic chính. Source nhiều items → tách.

### R4: Không bịa
Không fetch được → ghi rõ. Thiếu info → ghi "cần tìm hiểu thêm".

### R5: Source of truth
- Markdown files + YAML frontmatter là source of truth cho state.
- `INDEX.md` là Dataview dashboard/query view, không phải database thủ công.
- Khi cần cập nhật tracking/status, sửa file item/frontmatter tương ứng thay vì sửa bảng trong `INDEX.md`.

### R6: Human confirmation for governance
- Agent được phép đề xuất thay đổi `CONFIG.md`, `README.md`, `GUIDE/WORKFLOW.md`, templates.
- Chỉ apply thay đổi framework sau khi Human xác nhận.
- Mọi framework change phải được ghi vào `log.md`.

---

## HÀNH VI CHỦ ĐỘNG

### Mỗi session — Kiểm tra & nhắc nhở

```
1. ĐỌC INDEX.md → kiểm tra overdue, stale, broken streaks
2. NẾU có vấn đề → nhắc nhở đầu conversation
3. NẾU Human "không biết làm gì" → phân tích INDEX → suggest TOP 3
```

| Trigger | Hành động |
|---------|-----------|
| Raw > 7 ngày chưa staged | "N items chưa xử lý..." |
| Task quá deadline | "[X] quá hạn N ngày" |
| Deferred > 14 ngày | "[X] hoãn 2 tuần, vẫn giữ?" |
| Chưa daily log hôm nay | "Chưa có log hôm nay" |
| Item stale (book, project...) | "[X] chưa update N ngày" |
| Human nói "rảnh" / "không biết" | Suggest TOP 3 theo priority |

> Ngưỡng cấu hình: xem [[CONFIG.md]] `nudge_rules`

---

## SELF-EVOLVE — Tự phát triển framework

> **Rule quan trọng nhất:** Framework này đang phát triển. Agent phải chủ động giúp nó tốt hơn.

### Khi nào trigger

Khi Human đưa ra yêu cầu hoặc hỏi đáp mà:
- Chưa có workflow/pipeline phù hợp
- Chưa có category trong INDEX phù hợp
- Chưa có template phù hợp
- Có pattern sử dụng lặp lại nhưng chưa được formalize

### Agent phải làm gì

```
1. XỬ LÝ yêu cầu của Human trước (giải quyết vấn đề ngay)

2. SAU ĐÓ chủ động đề xuất cải tiến framework:
   "Tôi nhận thấy [pattern/gap]. Đề xuất:
    - Thêm [X] vào CONFIG.md
    - Cập nhật workflow cho case [Y]
    - Thêm template cho [Z]
    Bạn đồng ý thì tôi sẽ đề xuất nội dung cụ thể."

3. NẾU Human đồng ý → đề xuất nội dung thay đổi cụ thể cho:
   - CONFIG.md (tags, categories, rules mới)
   - GUIDE/WORKFLOW.md (steps mới hoặc sửa steps)
   - GUIDE/TEMPLATES/ (template mới)
   - README.md (cập nhật docs — BẮT BUỘC)

4. CHỈ apply sau khi Human xác nhận nội dung thay đổi.

5. LUÔN cập nhật `log.md` khi framework thay đổi.
```

### Ví dụ

```
Human: "Track tiến độ đọc paper cho tôi, mỗi paper có nhiều sections"
Agent:
  → Xử lý: Thêm entries vào INDEX.md với category "paper"
  → Đề xuất: "Tôi thấy tracking paper khác sách — paper có sections
    nhỏ hơn chapters. Đề xuất:
    - Thêm Progress format 'S.3/7 (43%)' cho papers vào CONFIG
    - Bạn đồng ý không?"
```

```
Human: "Tôi muốn track calories hàng ngày"
Agent:
  → Xử lý: Thêm entry vào INDEX.md category "health"
  → Đề xuất: "Category 'health' chưa có trong hệ thống.
    Đề xuất thêm vào CONFIG.md. Đồng ý?"
```

---

## CHECKLIST (Mỗi session)

```
[ ] Đọc file này (AGENTS.md) trước
[ ] Kiểm tra INDEX.md — overdue/stale items → nhắc
[ ] NẾU Human tạo wiki thành công → archive/mark staging tương ứng, move raw vào archive/raw/
[ ] Xử lý yêu cầu Human
[ ] Cập nhật files trong tracking/ nếu có thay đổi
[ ] NẾU phát hiện gap trong framework → đề xuất cải tiến
[ ] NẾU framework thay đổi → cập nhật README.md và log.md
```

---

*Source docs: [[README.md]] | Config: [[CONFIG.md]] | Dashboard: [[INDEX.md]] | Workflow: [[GUIDE/WORKFLOW.md]]*
