---
title: "{{Tên nhóm chủ đề}}"
category: hub
status: "🔄 doing"
priority: {{🔴 urgent | 🟡 normal | 🟢 low}}
progress: "{{ví dụ: 2/8 items done}}"
created_at: {{YYYY-MM-DD}}
tags: [{{tag1, tag2}}]
---

# 🗂️ {{Tên nhóm chủ đề}}

> **Mục tiêu:** {{1-2 câu — nhóm này để làm gì, đạt được gì?}}

---

## 📋 Resources

> Liệt kê tất cả items thuộc nhóm này. Có thể chia phases nếu cần.
> Agent có thể dùng Dataview block bên dưới thay vì liệt kê thủ công.

<!-- Tuỳ chọn: Chia phases nếu nhóm có lộ trình -->

### Phase 1: {{Tên phase}}
- [ ] 📖 [[tracking/{{file-name}}|{{Title}}]] — {{ghi chú ngắn}}
- [ ] 🎓 [[tracking/{{file-name}}|{{Title}}]]
- [ ] 🔬 [[tracking/{{file-name}}|{{Title}}]]

### Phase 2: {{Tên phase}}
- [ ] ...

---

## 🔍 Dataview — Auto pull items

```dataview
TABLE category, status, progress
FROM "second-brain/tracking"
WHERE contains(groups, "{{Tên nhóm chủ đề}}")
SORT choice(status = "🔄 doing", 0, choice(status = "👀 reading", 1, choice(status = "⬜ todo", 2, 3))) ASC
```

---

## 🔗 Related Hubs
- [[tracking/hubs/hub-{{related-group}}|{{Related Group}}]]

## Log & Notes

- **{{YYYY-MM-DD}}**: Khởi tạo hub
- ...
