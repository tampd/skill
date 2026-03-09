---
name: spec
description: "Tài liệu kỹ thuật nội bộ: architecture docs, API reference, data flow, ADR, BKNS repo setup. Kích hoạt khi người dùng nói /spec, /docs init, /docs adr, 'architecture', 'kiến trúc', 'data flow', 'api reference', 'ADR', 'khởi tạo repo', 'BKNS'."
metadata:
  author: tampd
  version: 7.0.0
  category: documentation
---

# Spec — Technical Documentation & Architecture

> **v7.0** — Gộp docs (technical parts) + BKNS templates
> Mục tiêu: Docs kỹ thuật chuẩn, machine-readable, BẤT KỲ AI/dev tiếp quản được 100%.

---

## 3 MODES

### Mode 1 — `/spec [scope]` (Architecture Docs)

```
STEP 1 — SCAN:
  So sánh docs/ hiện có vs template chuẩn 7 files:
  ├── ARCHITECTURE.md    ← Kiến trúc + RBAC + services
  ├── API_REFERENCE.md   ← Endpoints + request/response
  ├── DATA_FLOW.md       ← Mermaid diagrams luồng dữ liệu
  ├── SETUP.md           ← Cài đặt dev/staging/production
  ├── MASTER_PLAYBOOK.md ← Index tổng hợp
  ├── README.md          ← Quick start + badges
  └── adr/               ← Architecture Decision Records

  Báo cáo: ✅ Có | ❌ Thiếu | ⚠️ Cũ (> 30 ngày)

STEP 2 — GENERATE:
  Tạo files theo template chuẩn. Mỗi file có format riêng:
  > ARCHITECTURE.md: 10 sections (tổng quan → infra → RBAC → services → schema → security)
  > API_REFERENCE.md: base URL + auth + endpoints + error format + rate limiting
  > DATA_FLOW.md: Mermaid flowchart/sequence diagrams
  > Xem templates đầy đủ: references/architecture-template.md, references/api-template.md

STEP 3 — VERIFY:
  [ ] Routes trong API_REFERENCE khớp với code
  [ ] Controllers trong ARCHITECTURE khớp actual
  [ ] DB schema khớp migrations
  [ ] RBAC khớp giữa docs và code
  [ ] Setup commands thực sự chạy được

STEP 4 — INDEX:
  Cập nhật MASTER_PLAYBOOK.md liên kết tất cả docs
```

**Output:**
```
📚 SPEC REPORT — [Project]
  📊 SCAN: [N/7] files có | [M] thiếu | [K] ADRs
  📝 GENERATED: [list] | ✅ VERIFIED: [N/N] khớp code
```

---

### Mode 2 — `/docs init` (BKNS Repo Setup)

```
Tạo toàn bộ files bắt buộc từ templates/:
1. README.md          ← templates/README-BKNS.md
2. SPEC.md            ← templates/SPEC.md
3. DEPLOYMENT.md      ← templates/DEPLOYMENT.md
4. CHANGELOG.md       ← templates/CHANGELOG.md
5. PROJECT-META.md    ← templates/PROJECT-META.md
6. .env.example
7. docs/ directory + architecture.md, api.md, decisions.md
8. .gitignore

Hỏi user: project name, owner, tech stack
Output: ✅ BKNS Compliance: 6/6 files
> Xem cấu trúc: templates/REPO-STRUCTURE.md
> Rule 14 trong GEMINI.md
```

---

### Mode 3 — `/docs adr [decision]` (Architecture Decision Records)

```
Tạo docs/adr/ADR-NNN-[slug].md:

  # ADR-NNN: [Decision Title]
  - Date | Status: Proposed → Accepted → Deprecated
  - Context: [vấn đề gì?]
  - Decision: [chọn gì?]
  - Alternatives: [bảng so sánh]
  - Consequences: [tích cực + tiêu cực]

KHI NÀO TẠO ADR:
  - Framework/library mới
  - Thay đổi kiến trúc
  - DB schema quan trọng
  - Auth/security model thay đổi
  > Template đầy đủ: references/adr-template.md
```

---

## QUY TẮC

- Docs phải **machine-readable**: AI agent đọc và follow chính xác
- Mỗi update kèm `**Updated**: YYYY-MM-DD` ở header
- KHÔNG copy code chi tiết — chỉ reference file path + line
- ADR KHÔNG XÓA — chỉ thay đổi status
- Verify BẮT BUỘC sau mỗi generate/update
- Docs SAI còn tệ hơn KHÔNG CÓ docs
