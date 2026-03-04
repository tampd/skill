---
name: Docs — Documentation Generation & Maintenance
description: "Quy trình tạo và duy trì documentation hoàn chỉnh: scan thiếu → generate → ADR → verify → handoff → index. Đảm bảo người/AI tiếp quản dự án 100% không sai sót. Kích hoạt bằng /docs [scope]."
---

# /docs [scope]

> **Mục tiêu**: Documentation hoàn chỉnh để BẤT KỲ AI hoặc developer nào cũng tiếp quản được 100%.
> Dựa trên template chuẩn đã verified từ production projects.

---

## KHI NÀO DÙNG

| Tình huống | Dùng /docs? |
|---|---|
| Dự án mới cần docs từ đầu | ✅ `/docs init` |
| Sau khi ship feature lớn, docs bị outdated | ✅ `/docs update` |
| Chuẩn bị handoff cho dev/AI mới | ✅ `/docs handoff` |
| Cần ghi lại quyết định kiến trúc quan trọng | ✅ `/docs adr [decision]` |
| Fix bug nhỏ | ❌ → `/fix` |

---

## TEMPLATE CHUẨN — 9 FILE DOCS

Mọi dự án production PHẢI có đủ bộ docs sau:

```
docs/
├── ARCHITECTURE.md       ← Kiến trúc hệ thống + sơ đồ + RBAC + services
├── API_REFERENCE.md      ← REST/GraphQL endpoints + request/response examples
├── DATA_FLOW.md          ← Mermaid diagrams luồng dữ liệu chính
├── SETUP.md              ← Cài đặt môi trường dev/staging/production
├── ONBOARDING.md         ← Hướng dẫn người mới tiếp quản (đọc TRƯỚC TIÊN)
├── USER_GUIDE.md         ← Hướng dẫn end-user sử dụng
├── MASTER_PLAYBOOK.md    ← Index tổng hợp + quy trình làm việc
├── README.md             ← Quick start + badges + feature list
└── adr/                  ← Architecture Decision Records
    └── ADR-001-*.md
```

---

## QUY TRÌNH 6 BƯỚC

### Step 1 — SCAN (Auto-detect docs thiếu/cũ)

```
1. Liệt kê files trong docs/ hiện có
2. So sánh với template chuẩn 9 files ở trên
3. Báo cáo:

   📋 DOCS SCAN REPORT
   ✅ Có: ARCHITECTURE.md, SETUP.md, README.md
   ❌ Thiếu: API_REFERENCE.md, DATA_FLOW.md, ONBOARDING.md
   ⚠️ Cũ (>30 ngày): USER_GUIDE.md (last updated: 2026-01-15)
   📁 ADR: 0 records

4. Recommend: files nào cần tạo/update trước
```

---

### Step 2 — GENERATE (Tạo docs theo template)

Mỗi file có format chuẩn:

#### ARCHITECTURE.md — Cấu trúc bắt buộc
```markdown
# 🏗️ ARCHITECTURE.md — [Project Name]
> **Version**: vX.Y.Z | **Updated**: YYYY-MM-DD | **Framework**: [stack]

## 1. Tổng quan hệ thống
[Mô tả 3-5 dòng: hệ thống làm gì, cho ai, tính năng chính]

## 2. Infrastructure
| Component | Thông tin |
|---|---|
| Domain | [url] |
| Server | [IP/Cloud] |
| Database | [type + version] |
| Framework | [name + version] |

## 3. Kiến trúc tổng thể
[ASCII diagram hoặc mermaid: Request → Server → App → DB]

## 4. RBAC — Phân quyền
[Bảng roles + quyền hạn]

## 5. Service Layer
[Mô tả từng service chính: input → process → output]

## 6. Database Schema
[Nhóm bảng + mô tả + quan hệ quan trọng]

## 7. Module Map — Controllers
[Bảng controllers: name, route prefix, roles required]

## 8. Core Business Flow
[Sequence diagram luồng nghiệp vụ chính]

## 9. Config System
[Config keys quan trọng + override mechanism]

## 10. Security Architecture
[Auth flow + rate limiting + input validation]
```

#### API_REFERENCE.md — Cấu trúc bắt buộc
```markdown
# 📡 API Reference — [Project Name]
> **Base URL**: `https://domain.com/api`
> **Auth**: [Bearer Token / API Key / etc.]

## 1. Authentication
### POST `/api/login`
| Param | Type | Required | Mô tả |
**Request**: curl example
**Response (200)**: JSON example
**Error (401)**: JSON example

## N. Error Response Format
[Chuẩn format: { message, errors }]

## N+1. Rate Limiting
[Bảng endpoint → limit]
```

#### ONBOARDING.md — Cấu trúc bắt buộc
```markdown
# 🚀 ONBOARDING.md — Hướng dẫn Tiếp quản
> Đọc TRƯỚC TIÊN nếu bạn là developer hoặc AI mới

## Bước 1 — Nắm nhanh toàn cảnh (5 phút)
[Dự án là gì? Stack chính? 3 file đọc ngay]

## Bước 2 — Hiểu phân quyền
[Role hierarchy + tài khoản test]

## Bước 3 — Luồng nghiệp vụ chính
[Core flow 5-10 bước]

## Bước 4 — Cấu trúc thư mục
[File tree có annotation]

## Bước 5 — Quy tắc KHÔNG được vi phạm
[DO's and DON'T's + code examples]

## Bước 6 — Debug nhanh
[Common commands: logs, queue, database]

## Bước 7 — Tài liệu tham khảo
[Links tới các docs khác]
```

#### DATA_FLOW.md — Cấu trúc bắt buộc
```markdown
# 🔄 Data Flow — [Project Name]

## 1. [Core Flow Name]
```mermaid flowchart diagram```

## 2. [Secondary Flow Name]
```mermaid stateDiagram```

## N. Sơ đồ Tổng hợp
```mermaid flowchart TB with subgraphs: Input → Process → Storage → Output```
```

---

### Step 3 — ADR (Architecture Decision Records)

Format mỗi ADR:

```markdown
# ADR-NNN: [Decision Title]

- **Date**: YYYY-MM-DD
- **Status**: Proposed | Accepted | Deprecated | Superseded by ADR-XXX
- **Deciders**: [ai đưa ra quyết định]

## Context
[Tại sao cần quyết định này? Vấn đề gì đang gặp?]

## Decision
[Chọn gì? Mô tả chi tiết]

## Alternatives Considered
| Option | Ưu điểm | Nhược điểm |
|---|---|---|
| A: [chosen] | [pros] | [cons] |
| B: [rejected] | [pros] | [cons] |

## Consequences
### Tích cực
- [positive effect]

### Tiêu cực / Rủi ro
- [negative effect / risk]

## References
- [links, tickets, PRs]
```

**Khi nào tạo ADR:**
- Chọn framework/library mới
- Thay đổi kiến trúc (monolith → microservice, thêm queue, v.v.)
- Quyết định database schema quan trọng
- Thay đổi auth/security model
- Bất kỳ quyết định nào mà "tại sao" không rõ ràng từ code

---

### Step 4 — VERIFY (Cross-reference docs vs code)

```
Checklist kiểm tra:
[ ] Routes trong API_REFERENCE.md khớp với routes/*.php hoặc router config
[ ] Controllers trong ARCHITECTURE.md khớp với actual controllers
[ ] Database schema trong ARCHITECTURE.md khớp với migrations
[ ] RBAC roles khớp giữa ARCHITECTURE.md và code
[ ] Setup commands trong SETUP.md thực sự chạy được
[ ] Version numbers nhất quán giữa tất cả docs
```

> 🛑 **RULE**: Docs SAI còn tệ hơn KHÔNG CÓ docs. Luôn verify.

---

### Step 5 — HANDOFF (Tạo handoff package hoàn chỉnh)

```
Handoff package = ONBOARDING.md + checklist:

HANDOFF CHECKLIST:
[ ] ONBOARDING.md có "5 phút nắm toàn cảnh"
[ ] Tài khoản test liệt kê đầy đủ (→ PASSWORD.md)
[ ] Core business flow mô tả rõ ràng
[ ] File tree có annotation mỗi folder
[ ] DO's and DON'T's có code examples ✅❌
[ ] Debug commands sẵn sàng copy-paste
[ ] Links tới tất cả docs khác hoạt động
```

---

### Step 6 — INDEX (Cập nhật Master Playbook)

Cập nhật `MASTER_PLAYBOOK.md` để liên kết tất cả docs:

```markdown
## Kiến trúc tài liệu
| Tầng | File | Mục đích |
|---|---|---|
| Brain | GEMINI.md | Master context, rules |
| Brain | LESSONS.md | Bugs & bài học |
| Brain | CHANGE_LOG.md | Timeline thay đổi |
| Brain | NEXT-TODO.md | Task backlog |
| Docs | ARCHITECTURE.md | Kiến trúc chi tiết |
| Docs | API_REFERENCE.md | REST API |
| Docs | DATA_FLOW.md | Luồng dữ liệu |
| Docs | SETUP.md | Cài đặt |
| Docs | ONBOARDING.md | Tiếp quản |
| Docs | USER_GUIDE.md | End-user |
| ADR | adr/ADR-NNN-*.md | Quyết định kiến trúc |
```

---

## OUTPUT FORMAT

```
📚 DOCS REPORT — [Project Name]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

📊 SCAN: [N/9] files có | [M] files thiếu | [K] ADRs
📝 GENERATED: [list files tạo mới]
✏️ UPDATED: [list files cập nhật]
✅ VERIFIED: [N/N] docs khớp với code
📋 HANDOFF: [Ready / Not ready]

🔜 NEXT:
   Docs đầy đủ?  → /build [task]
   Cần thêm ADR? → /docs adr [decision]
   Review tổng?  → /review-web [url]
```

---

## QUY TẮC

- Docs phải **machine-readable**: AI agent đọc được và follow chính xác
- Mỗi docs update PHẢI kèm `**Updated**: YYYY-MM-DD` ở header
- KHÔNG copy code chi tiết vào docs — chỉ reference file path + line
- ADR KHÔNG XÓA — chỉ thay đổi status (Deprecated / Superseded)
- ONBOARDING.md luôn viết cho "người đọc Zero Context"
- Mermaid diagrams ưu tiên cho DATA_FLOW (visual > text)
- Verify BẮT BUỘC sau mỗi lần generate/update
