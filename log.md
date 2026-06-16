# Second Brain Framework — Changelog

Tài liệu ghi nhận toàn bộ lịch sử nâng cấp, cải tiến cấu trúc và quy tắc của Second Brain Framework.

---

## [v1.2] - 2026-06-16

### Added
- **Subsystem Boundary**: Formalize framework thành 4 subsystem: Knowledge Pipeline, Tracking System, Review & Planning, Governance.
- **Source-of-Truth Clarification**: Markdown files + YAML frontmatter là source of truth; `INDEX.md` là Dataview dashboard/query view, không phải database thủ công.
- **Traceability Principle**: Wiki phải truy vết được về staging/raw source khi có thể.
- **Canonical Status Mapping**: Thêm `canonical_statuses` trong `CONFIG.md` để API/domain logic không phụ thuộc vào plain labels.

### Changed
- Cleanup workflow không xóa cứng `staging/`; thay bằng archive/mark staging và preserve links.
- Cập nhật `AGENTS.md` yêu cầu human confirmation trước khi apply framework governance changes và ghi log bắt buộc.
- Cập nhật `README.md` version lên **v1.2**.

---

## [v1.1] - 2026-06-10

### Added
- **Generic Grouping (`groups` frontmatter)**: Hỗ trợ trường `groups` trong tracking entries. Giúp gom nhóm các item tự do (free-form), một item có thể thuộc nhiều nhóm cùng lúc (ví dụ: `["Learn AI", "Career 2026"]`).
- **Hub Notes (Map of Content - MOC)**: Thư mục `tracking/hubs/` và template `GUIDE/TEMPLATES/hub_note.md` để quản lý các nhóm chủ đề lớn. Hub note tự động kéo các tracking entries liên quan thông qua Dataview query.
- **Related Link (`related` frontmatter)**: Thêm trường `related` vào template tracking để kết nối trực tiếp các tài liệu liên quan trong vault, giúp tự động tạo Knowledge Graph nhẹ.
- **Usecase Category**: Thêm phân loại `usecase` cho các bài tập thực hành/use case thực tế đi kèm với chủ đề học tập.

### Changed
- Cập nhật `INDEX.md` thêm section `Groups & Hubs` để hiển thị danh sách các Hub Note đang active.
- Cập nhật `CONFIG.md` khai báo thêm loại category `usecase`, `hub` và tài liệu hóa cách dùng `groups`.
- Nâng phiên bản tài liệu framework trong `README.md` lên **v1.1**.

---

## [v1.0] - 2026-06-05

### Added
- **Core Pipeline**: Thiết kế quy trình 6 bước tự động hóa kiến thức: `COLLECT` → `NORMALIZE` → `DIGEST` → `INDEX` → `SUGGEST` → `CLEANUP`.
- **Directory Structure**:
  - `raw/`: Lưu trữ tài liệu gốc chưa sửa đổi.
  - `staging/`: Bản tóm tắt cô đọng (digest) do AI tạo.
  - `wiki/`: Nơi lưu trữ kiến thức hoàn chỉnh do con người viết.
  - `tracking/`: Quản lý các task, dự án, sách, khóa học qua các file markdown riêng lẻ.
  - `dailylogs/`: Lưu trữ nhật ký hàng ngày.
- **Master Index (Dataview)**: Tích hợp plugin Dataview để tự động quét trạng thái thay vì cập nhật bảng markdown thủ công.
- **Centralized Config**: Quản lý tập trung inbox, tags, wiki taxonomy và quy tắc nhắc nhở (nudge rules) trong `CONFIG.md`.
- **Self-Evolving Framework**: Quy định cơ chế cho Agent chủ động đề xuất nâng cấp hệ thống khi phát hiện khoảng trống thiết kế trong quá trình sử dụng thực tế.
- **Core Templates**: Khởi tạo templates cho `raw_entry`, `staging_entry`, `wiki_entry`, `tracking_entry`, `daily_log`, `review_card`.
