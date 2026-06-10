# ⚙️ CONFIG — Second Brain Configuration
> Chỉ **Human** được sửa file này. Agent chỉ đọc.

---

## 📥 INBOX Sources

Agent quét các file này khi yêu cầu "process inbox":

```yaml
inbox_sources:
  - path: "../../Master.md"
    description: "Dump chính: links, projects, todo, random notes"
  - path: "../../Quick notes.md"
    description: "Ghi chú nhanh từ browsing, social media, articles"
  # Thêm source mới:
  # - path: "../../NewFile.md"
  #   description: "Mô tả"
```

---

## 🏷️ Tags

```yaml
# Domain
domain_tags:
  - database
  - caching
  - system-design
  - networking
  - security
  - runtime
  - architecture
  - devops
  - golang
  - nodejs
  - ai-ml
  - blockchain
  - frontend

# Type
type_tags:
  - concept
  - pattern
  - anti-pattern
  - tool
  - framework
  - case-study
  - tutorial
  - interview
  - roadmap
  - project-idea
  - open-source
  - book
  - course
  - paper
  - usecase      # 🆕 Use case / hands-on application
```

---

## 📦 Categories (Tracking)

```yaml
categories:
  - book
  - project
  - course
  - habit
  - task
  - research
  - usecase      # 🆕 Use case thực tế — khác task ở chỗ gắn liền với context
  - hub          # 🆕 Hub note (Map of Content) — nhóm chủ đề

---

## 🗂️ Wiki Taxonomy

```yaml
wiki_paths:
  engineering/database:      "SQL, NoSQL, transactions, indexes, replication"
  engineering/caching:       "Redis, CDN, cache strategies"
  engineering/system-design: "Kafka, RabbitMQ, NATS, distributed systems"
  engineering/networking:    "HTTP, gRPC, protocols, OSI"
  engineering/security:      "JWT, auth, encryption, OAuth"
  engineering/runtime:       "NodeJS, Go, JVM internals"
  engineering/architecture:  "DDD, Clean Arch, microservices, patterns"
  devops/containers:         "Docker, K8s"
  devops/cicd:               "CI/CD pipelines, deployment"
  devops/cloud:              "AWS, GCP, infrastructure"
  concepts:                  "Cross-cutting terms, general CS"
  interview:                 "Q&A, prep cards, mock questions"
  ai-ml:                     "LLM, RAG, agents, ML ops"
```

---

## 📊 Statuses

```yaml
# Knowledge Pipeline
knowledge_statuses:
  - "📥 raw"          # Đã thu thập, chưa tóm tắt
  - "🧠 staged"       # AI đã tóm tắt
  - "👀 reading"      # Human đang đọc
  - "✅ wiki"          # Đã tạo wiki entry
  - "📌 deferred"     # Tạm hoãn
  - "🗑️ archived"     # Không còn relevant

# General
task_statuses:
  - "⬜ todo"
  - "🔄 doing"
  - "✅ done"
  - "📌 deferred"

# Reading
reading_statuses:
  - "📖 reading"
  - "⏸️ paused"
  - "✅ done"
  - "📌 deferred"

# Projects
project_statuses:
  - "🔄 in-progress"
  - "⏸️ paused"
  - "✅ done"
  - "📌 deferred"
  - "💀 abandoned"

# Courses
course_statuses:
  - "🎯 active"
  - "⏸️ paused"
  - "✅ completed"
  - "📌 deferred"

# Habits
habit_statuses:
  - "🔥 active"       # Đang duy trì streak
  - "❄️ broken"        # Streak bị đứt
  - "✅ established"   # Đã thành thói quen (> 30 ngày)
  - "📌 deferred"

# Research
research_statuses:
  - "🔍 exploring"
  - "📝 noted"
  - "🧠 staged"
  - "✅ understood"
  - "📌 deferred"
```

---

## ⏰ Priorities

```yaml
priorities:
  - "🔴 urgent"       # Cần làm ngay
  - "🟡 normal"       # Khi có thời gian
  - "🟢 low"          # Tham khảo
  - "⚪ someday"      # Chưa biết khi nào
```

---

## 🔔 Nudge Thresholds

```yaml
# Agent nhắc nhở khi:
nudge_rules:
  raw_stale_days: 7         # Raw chưa staged > 7 ngày
  deferred_remind_days: 14  # Deferred > 14 ngày → hỏi lại
  task_overdue_alert: true  # Task quá deadline → nhắc ngay
  daily_log_check: true     # Kiểm tra daily log hôm nay
  reading_stale_days: 7     # Sách chưa update > 7 ngày
  habit_break_alert: true   # Streak bị gián đoạn → nhắc
  project_stale_days: 14    # Project chưa update > 14 ngày
```

---

## 📦 Groups (Free-form)

> `groups` là trường trong frontmatter của tracking entry, dùng để gán item vào 1 hoặc nhiều nhóm chủ đề.
> **Free-form** — không có danh sách cố định. Đặt tên tự do, Dataview tự query.

```yaml
# Ví dụ các groups đang dùng (chỉ để tham khảo, không phải whitelist):
groups_examples:
  - "Learn AI"              # Nhóm học AI: sách + khóa + use case + project
  - "System Design"         # Nhóm System Design
  - "Career 2026"           # Mục tiêu career
  - "Homelab"               # Homelab setup
  - "Database Mastery"      # Đi sâu về database

# Quy tắc đặt tên groups:
# - Tiếng Anh, Title Case
# - Ngắn gọn (tối đa 3 từ)
# - Khi nhóm lớn (>3 items) → tạo Hub Note tương ứng trong tracking/hubs/
```
