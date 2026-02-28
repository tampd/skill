# GEMINI — Skill Repository v2 (Production Code Framework)

> **GitHub**: https://github.com/tampd/skill
> **Local path**: /root/skill
> **Cập nhật**: 2026-02-28 — **v2.0 MAJOR REWRITE**

---

## 🔑 THÔNG TIN QUAN TRỌNG

### Skill System v2 — Unified Production Code Framework
Phiên bản 2.0 gộp 26 skills rời rạc thành **8 skills tập trung**, kết hợp:
- **Antigravity**: LESSONS.md persistence, 5-tier memory, session lifecycle
- **bkns-minicrm (Claude Code)**: PR blueprints, mandatory testing, architecture rules density

### GitHub Repository
```
URL:    https://github.com/tampd/skill
Remote: origin → https://github.com/tampd/skill.git
Branch: main
```

---

## ⚠️ GLOBAL RULE: AI PHẢI HỎI KHI CHƯA RÕ

> **RULE BẮT BUỘC CHO MỌI SKILL:**
> Nếu trong quá trình làm việc, yêu cầu của user **chưa rõ nghĩa**, **chưa mô tả chuẩn ý định**, hoặc có **thông tin mâu thuẫn/thiếu**:
>
> � **AI PHẢI HỎI user TRƯỚC KHI**:
> - Lập kế hoạch làm việc (`/plan`)
> - Viết code (`/build`)
> - Ghi vào docs (`GEMINI.md`, `LESSONS.md`, `CHANGE_LOG.md`, bất kỳ file .md nào)
>
> **Câu hỏi phải cụ thể**, ví dụ:
> - "Bạn muốn KPI tính theo doanh thu thô hay doanh thu sau feedback?"
> - "Module này cần RBAC cho role nào? ADMIN only hay cả EMPLOYEE?"
>
> ❌ KHÔNG ĐƯỢC giả định và code/ghi docs khi chưa xác nhận.

---

## �📁 CẤU TRÚC SKILL REPOSITORY v2

```
/root/skill/
├── GEMINI.md              ← File này — rules & context
├── README.md              ← Hướng dẫn sử dụng skill system v2
│
├── start/SKILL.md         ← /start [task] — Khởi động phiên
├── build/SKILL.md         ← /build [task] — ⭐ SUPER SKILL viết code
├── fix/SKILL.md           ← /fix [bug] — Debug + ghi bài học
├── save/SKILL.md          ← /save — Review + Save + Push
├── plan/SKILL.md          ← /plan [feature] — Tạo blueprint
├── design/SKILL.md        ← /design [task] — UI/UX workflow
├── guard/SKILL.md         ← /guard [scope] — Test + Security + Perf
├── integrate/SKILL.md     ← /integrate [svc] — API + 3rd-party
│
├── qdrant-memory/SKILL.md ← Qdrant Layer 4 Memory (giữ nguyên)
│
└── archive/               ← 27 skills cũ v1 (preserved)
    ├── start/SKILL.md
    ├── save/SKILL.md
    ├── vibe-code/SKILL.md
    └── ... (24 more)
```

---

## 🎯 8 SKILLS — QUICK REFERENCE

| # | Skill | Lệnh | Mục đích | Thay thế v1 |
|---|---|---|---|---|
| 1 | **start** | `/start [task]` | Khởi động phiên + chọn skill | start + recall + supper |
| 2 | **build** ⭐ | `/build [task]` | Viết code production-grade | vibe-code + gsd + 5 skills |
| 3 | **fix** | `/fix [bug]` | Debug + ghi LESSONS | debugger + bug-journal |
| 4 | **save** | `/save` | Review + save + push | review + save + checkpoint |
| 5 | **plan** | `/plan [feature]` | Tạo task blueprint | MỚI (từ bkns-minicrm) |
| 6 | **design** | `/design [task]` | UI/UX workflow | 4 design skills |
| 7 | **guard** | `/guard [scope]` | Test + security + perf | 3 quality skills |
| 8 | **integrate** | `/integrate [svc]` | API + 3rd-party | 5 integration skills |

---

## 🔄 SESSION LIFECYCLE v2

```
/start [task]  →  /build [task]  →  /save
   │                  │               │
   ├─ Load context    ├─ 8 steps      ├─ 7-criteria review
   ├─ Check LESSONS   ├─ Mandatory    ├─ Write LESSONS
   ├─ Scan blueprints │   testing     ├─ Update docs
   └─ Auto-select     ├─ Blueprint    ├─ Atomic commit
      skill            │   mode       └─ Push + archive
                       └─ Arch rules
                          enforced

ALTERNATIVES:
/fix [bug]      → For bugs: LESSONS check → debug → fix → record
/plan [feature] → For complex features: spec → blueprint → /build
/design [task]  → For UI/UX: brief → implement → verify
/guard [scope]  → For quality: test → security → performance
/integrate [svc]→ For APIs: design → implement → test → document
```

---

## 📚 DOCUMENTATION STANDARDS (Học từ bkns-minicrm)

### Nguyên tắc viết docs

Mỗi dự án PHẢI có hệ thống docs theo chuẩn sau:

| File | Viết gì | KHÔNG viết gì |
|---|---|---|
| `GEMINI.md` | Tech stack, Architecture Rules (✅❌ code), Version, Links | Task TODO, logs phiên |
| `LESSONS.md` | Bug #WARN-XXX + ✅❌ code examples + quy tắc rút ra | Feature code, changelog |
| `CHANGE_LOG.md` | Timeline thay đổi: Added/Fixed/Changed/Removed | Task future |
| `NEXT-TODO.md` | Task backlog (bảng tracking: #, Size, Status, Deps) | Task đã xong (xóa ngay) |
| `docs/architecture.md` | System diagrams, module tree, data flow | Task backlog |
| `docs/business-rules.md` | Logic nghiệp vụ thuần (KHÔNG code) | UI details |
| `docs/api-endpoints.md` | REST API: method + URL + auth + request/response | Business logic |
| `docs/setup.md` | Cài đặt môi trường dev/prod | Business rules |

### Cách viết Architecture Rules (bắt buộc)

Mỗi project PHẢI define Architecture Rules trong GEMINI.md theo format:

```markdown
### Rule N: [Tên rule] (VI PHẠM = BUG)
[Giải thích 1 dòng tại sao rule này tồn tại]

// ✅ Đúng — [giải thích]
[code example đúng]

// ❌ Sai — [giải thích]
[code example sai]
```

### Cách viết LESSONS.md entries (bắt buộc)

Mỗi entry PHẢI có ✅❌ code examples:

```markdown
### 🟡 #WARN-NNN — Tiêu đề (YYYY-MM-DD)
- **Mức độ:** 🟡 Warning
- **Thẻ:** #tag1, #tag2
- **Triệu chứng:** [mô tả quan sát]
- **Nguyên nhân gốc:** [WHY]
- **Cách fix:**
  // ✅ Đúng
  [code]
  // ❌ Sai
  [code]
- **Quy tắc rút ra:** [Rule áp dụng mãi]
- **File liên quan:** `path/to/file`
```

### Cách viết Task Blueprint (.agent/commands/task-NNN.md)

Tạo bởi `/plan`, follow bởi `/build`:

```markdown
> **Complexity: [S|M|L]**

# Task-NNN: [Feature Name]

## Step 1 — Read Rules & Docs
- `GEMINI.md` (Rules: [chỉ rõ rule nào])
- `LESSONS.md` (grep: [keywords])
- `docs/[doc].md` (Section: [chỉ rõ])

## Step 2 — Implementation
[File tree + logic flow + error handling table]

## Step 3 — Tests
[Liệt kê từng test case cụ thể]

## Step 4 — Documentation
[Chỉ rõ files nào cần update]
```

### Cách viết NEXT-TODO.md (bảng tracking)

```markdown
| # | Task | Size | Status | Deps | Notes |
|---|------|------|--------|------|-------|
| T-001 | [name] | M | ⬜ | - | |
| T-002 | [name] | L | 🔄 | T-001 | Đang làm |
| T-003 | [name] | S | ✅ | - | Done 28/02 |
```

Status: ⬜ Not started | 🔄 In progress | ✅ Done | ⏸️ Blocked

---

## 🧠 MEMORY ARCHITECTURE (5 Tầng)

| Tầng | File | Mục đích |
|---|---|---|
| **1 — Session** | `ACTIVE_CONTEXT.md` | Working memory (xóa sau /save) |
| **2 — Project** | `GEMINI.md`, `LESSONS.md`, `CHANGE_LOG.md`, `NEXT-TODO.md` | Não bộ dự án |
| **3 — Knowledge** | `docs/*.md` | Tài liệu kỹ thuật |
| **4 — Vector** | Qdrant (curl scripts) | Cross-session semantic memory |
| **5 — Skills** | `~/.gemini/antigravity/skills/` + `.agent/commands/` | Skill + Blueprints |

---

## 📋 CHEAT SHEET v2

```
Bắt đầu phiên?           → /start [mô tả task]
Viết bất kỳ code nào?    → /build [task]
Gặp bug?                 → /fix [mô tả bug]
Feature phức tạp?         → /plan [feature] → /build
Design / UI?              → /design [task]
Viết test / audit?        → /guard [scope]
API / webhook / payment?  → /integrate [service]
Kết thúc phiên?           → /save
```

---

## 📝 CHANGE LOG

| Date | Change |
|---|---|
| **2026-02-28** | **v2.0 MAJOR**: Gộp 26 skills → 8 unified skills. Thêm /plan (blueprint), /guard (QA). Nâng /build thành super skill 8 bước với mandatory testing. Archive 27 skills cũ. Thêm Documentation Standards (từ bkns-minicrm). Thêm AI Clarification Rule. |
| 2026-02-27 | v1.x: 26 skills, memory-optimizer, qdrant integration |
