---
type: Hub
title: "{{Tên nhóm chủ đề}}"
description: "{{1-2 câu tóm tắt nhanh}}"
status: "🔄 doing"
priority: {{🔴 urgent | 🟡 normal | 🟢 low}}
progress: "{{ví dụ: 2/8 items done}}"
timestamp: {{YYYY-MM-DDTHH:MM:SSZ}}
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
- [ ] 📖 [{{Title}}](/tracking/{{file-name}}.md) — {{ghi chú ngắn}}
- [ ] 🎓 [{{Title}}](/tracking/{{file-name}}.md)
- [ ] 🔬 [{{Title}}](/tracking/{{file-name}}.md)

### Phase 2: {{Tên phase}}
- [ ] ...

---

## 🔍 Dataview — Auto pull items

```dataview
TABLE type, status, progress
FROM "second-brain/tracking"
WHERE contains(groups, "{{Tên nhóm chủ đề}}")
SORT choice(status = "🔄 doing", 0, choice(status = "👀 reading", 1, choice(status = "⬜ todo", 2, 3))) ASC
```

---

## 🔗 Related Hubs
- [{{Related Group}}](/tracking/hubs/hub-{{related-group}}.md)

## Log & Notes

- **{{YYYY-MM-DD}}**: Khởi tạo hub
- ...
