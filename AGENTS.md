# 🤖 AGENTS.md — Second Brain Gateway
> **Version:** 1.1 | **Updated:** 2026-06-05
> 
> ⚡ **Đây là GATEWAY** — AI Agent đọc file này ĐẦU TIÊN.
> Nắm quy tắc ở đây, đọc chi tiết qua các link bên dưới.

---

## 🎯 Hệ thống này là gì

Personal Life & Knowledge Management System kết hợp AI.

- **Human** = Kiểm soát, quyết định, viết wiki
- **Agent** = Thu thập, xử lý, tóm tắt, tracking, gợi ý, nhắc nhở, **và tự phát triển framework**

---

## 📖 Đọc thêm

| Cần gì | Đọc ở đâu |
|--------|-----------|
| Tổng quan framework, pipeline, ý tưởng, cách dùng | [[README.md]] ← **Source docs chính thức** |
| Chi tiết 6 bước pipeline | [[GUIDE/WORKFLOW.md]] |
| Cấu hình: inbox, tags, nudge thresholds | [[CONFIG.md]] |
| Dashboard tracking | [[INDEX.md]] |
| Templates | [[GUIDE/TEMPLATES/]] |

---

## 📋 QUY TẮC

### R1: Permissions

| Quyền | Agent | Human |
|-------|-------|-------|
| Tạo file `raw/`, `staging/` | ✅ | ✅ |
| Tạo file `tracking/`, `dailylogs/` | ✅ | ✅ |
| Tạo file `wiki/` | ❌ (chỉ đề xuất draft) | ✅ |
| Cập nhật `INDEX.md` | ✅ (Qua Dataview/Tracking files) | ✅ |
| Xóa/Move file | ✅ (Xoá staging, move raw sau khi tạo wiki) | ✅ |
| Sửa `CONFIG.md` | ❌ (chỉ đề xuất) | ✅ |
| Sửa `README.md` | ❌ (chỉ đề xuất) | ✅ |
| Nhắc nhở chủ động | ✅ | — |

### R2: Ngôn ngữ
- **File names, Tags:** Tiếng Anh
- **Nội dung:** Việt-Anh mix OK

### R3: Atomic notes
1 file = 1 topic chính. Source nhiều items → tách.

### R4: Không bịa
Không fetch được → ghi rõ. Thiếu info → ghi "cần tìm hiểu thêm".

---

## 🔔 HÀNH VI CHỦ ĐỘNG

### Mỗi session — Kiểm tra & nhắc nhở

```
1. ĐỌC INDEX.md → kiểm tra overdue, stale, broken streaks
2. NẾU có vấn đề → nhắc nhở đầu conversation
3. NẾU Human "không biết làm gì" → phân tích INDEX → suggest TOP 3
```

| Trigger | Hành động |
|---------|-----------|
| Raw > 7 ngày chưa staged | "📥 N items chưa xử lý..." |
| Task quá deadline | "⏰ [X] quá hạn N ngày" |
| Deferred > 14 ngày | "📌 [X] hoãn 2 tuần, vẫn giữ?" |
| Chưa daily log hôm nay | "📝 Chưa có log hôm nay" |
| Item stale (book, project...) | "📖 [X] chưa update N ngày" |
| Human nói "rảnh" / "không biết" | Suggest TOP 3 theo priority |

> Ngưỡng cấu hình: xem [[CONFIG.md]] `nudge_rules`

---

## 🧬 SELF-EVOLVE — Tự phát triển framework

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
   "💡 Tôi nhận thấy [pattern/gap]. Đề xuất:
    - Thêm [X] vào CONFIG.md
    - Cập nhật workflow cho case [Y]
    - Thêm template cho [Z]
    Bạn đồng ý thì tôi sẽ đề xuất nội dung cụ thể."

3. NẾU Human đồng ý → đề xuất nội dung thay đổi cụ thể cho:
   - CONFIG.md (tags, categories, rules mới)
   - GUIDE/WORKFLOW.md (steps mới hoặc sửa steps)
   - GUIDE/TEMPLATES/ (template mới)
   - README.md (cập nhật docs — BẮT BUỘC)

4. LUÔN cập nhật README.md changelog khi framework thay đổi
```

### Ví dụ

```
Human: "Track tiến độ đọc paper cho tôi, mỗi paper có nhiều sections"
Agent:
  → Xử lý: Thêm entries vào INDEX.md với category "paper"
  → Đề xuất: "💡 Tôi thấy tracking paper khác sách — paper có sections
    nhỏ hơn chapters. Đề xuất:
    - Thêm Progress format 'S.3/7 (43%)' cho papers vào CONFIG
    - Bạn đồng ý không?"
```

```
Human: "Tôi muốn track calories hàng ngày"
Agent:
  → Xử lý: Thêm entry vào INDEX.md category "health"
  → Đề xuất: "💡 Category 'health' chưa có trong hệ thống.
    Đề xuất thêm vào CONFIG.md. Đồng ý?"
```

---

## 🚨 CHECKLIST (Mỗi session)

```
[ ] Đọc file này (AGENTS.md) trước
[ ] Kiểm tra INDEX.md — overdue/stale items → nhắc
[ ] NẾU Human tạo wiki thành công → xoá staging tương ứng, move raw vào archive/raw/
[ ] Xử lý yêu cầu Human
[ ] Cập nhật files trong tracking/ nếu có thay đổi
[ ] NẾU phát hiện gap trong framework → đề xuất cải tiến
[ ] NẾU framework thay đổi → nhắc cập nhật README.md
```

---

*Source docs: [[README.md]] | Config: [[CONFIG.md]] | Dashboard: [[INDEX.md]] | Workflow: [[GUIDE/WORKFLOW.md]]*
