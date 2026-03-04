# GEMINI — Skill Repository v4 (Production Code Framework + Extended Arsenal)

> **GitHub**: https://github.com/tampd/skill
> **Local path**: /root/skill
> **Cập nhật**: 2026-03-04 — **v4.0 STRUCTURED VIBE**

---

## 🔑 THÔNG TIN QUAN TRỌNG

### Skill System v4 — Structured Vibe
Phiên bản 4.0 nâng cấp từ v3 (12 skills) lên **14 skills**, tập trung:
- 🆕 **docs**: Documentation generation + ADR + handoff (9 template files chuẩn)
- 🆕 **seo**: SEO + GEO content writer (keyword → outline → draft → optimize → publish)
- ⬆ **build**: Thêm **Architecture Spec Phase** — `.spec.md` BẮT BUỘC cho project/module mới
- ⬆ **design**: Thêm **Design Token System** + **Component API Docs**
- ⬆ **n8n-pro**: Thêm **MCP Server** + **Sub-workflows** + **Scaling Patterns**
- ⬆ **start**: Auto-select nâng lên **14 rules** + fallback logic

### Triết lý v4: "Vibe With Structure"
```
❌ KHÔNG code vibe không cấu trúc → dẫn tới tech debt
✅ .spec.md trước mọi project/module mới (site map, component tree, data flow)
✅ 14 skills chuyên biệt, mỗi skill có quy trình evidence-based
✅ Documentation-as-Code: docs tạo tự động, verify vs code
✅ GEO-optimized content: viết cho cả Google + AI search engines
```

### GitHub Repository
```
URL:    https://github.com/tampd/skill
Remote: origin → git@github.com:tampd/skill.git (SSH)
Branch: main
SSH Key: SHA256:nTSlO07MbIplXX/j2FAHlyuSb+MJxPO1yboDHJJidFs
```

> **RULE**: Mọi dự án đều push qua SSH (`git@github.com:`), KHÔNG dùng HTTPS.
> Nếu remote đang là HTTPS → chạy `git remote set-url origin git@github.com:{user}/{repo}.git`

---

## ⚠️ GLOBAL RULES

### Rule 1: AI PHẢI HỎI KHI CHƯA RÕ
> Nếu yêu cầu chưa rõ nghĩa, thiếu thông tin, hoặc mâu thuẫn:
> 🛑 AI PHẢI HỎI user TRƯỚC KHI lập kế hoạch, viết code, ghi docs.
> ❌ KHÔNG giả định và code khi chưa xác nhận.

### Rule 2: ĐỌC LESSONS TRƯỚC KHI CODE
> Mỗi task PHẢI bắt đầu bằng việc đọc LESSONS.md + grep tags liên quan.
> Không có ngoại lệ. Đây là cách AI "nhớ" để không lặp sai lầm.

### Rule 3: EVIDENCE-BASED VERIFICATION
> Không chấp nhận "nó chạy rồi". Phải có bằng chứng cụ thể:
> curl output, test results, screenshot, hoặc log.

### Rule 4: ARCHITECTURE SPEC TRƯỚC KHI CODE (MỚI v4)
> Project mới hoặc module mới ≥ 3 files → BẮT BUỘC tạo `.spec.md` trước.
> Spec gồm: Site Map, Component Tree, Data Flow, File Structure, Design Tokens.
> Xem chi tiết: `/build` Step 0.5.

---

## 📁 CẤU TRÚC SKILL REPOSITORY v4

```
/root/skill/
├── GEMINI.md                ← File này — brain & rules
├── README.md                ← Hướng dẫn cài đặt
│
│── ─── 8 CORE SKILLS (từ v2, nâng cấp v4) ───
├── start/SKILL.md           ← /start [task]          ⬆ 14-rule auto-select
├── build/SKILL.md           ← /build [task] ⭐       ⬆ Architecture Spec Phase
├── fix/SKILL.md             ← /fix [bug]
├── save/SKILL.md            ← /save
├── plan/SKILL.md            ← /plan [feature]
├── design/SKILL.md          ← /design [task]          ⬆ Token System + Component API
├── guard/SKILL.md           ← /guard [scope]
├── integrate/SKILL.md       ← /integrate [svc]
│
│── ─── 4 SKILLS (v3) ───
├── n8n-pro/SKILL.md         ← /n8n [task]             ⬆ MCP Server + Sub-workflows
├── brainstorm/SKILL.md      ← /brainstorm [idea]
├── web-security/SKILL.md    ← /security [target]
├── review-website/SKILL.md  ← /review-web [url]
│
│── ─── 2 NEW SKILLS (v4) ───
├── docs/SKILL.md            ← /docs [scope]           🆕 Documentation + ADR
├── seo/SKILL.md             ← /seo [topic]            🆕 SEO + GEO Writer
│
│── ─── INFRASTRUCTURE ───
├── qdrant-memory/SKILL.md   ← Qdrant Layer 4 Memory
└── archive/                 ← Skills cũ v1+v2 (preserved)
```

---

## 🎯 14 SKILLS — QUICK REFERENCE

| # | Skill | Lệnh | Mục đích | Từ v |
|---|---|---|---|---|
| 1 | **start** | `/start [task]` | Khởi động phiên + chọn skill (14 rules) | v2 ⬆ |
| 2 | **build** ⭐ | `/build [task]` | Code + Architecture Spec Phase | v2 ⬆ |
| 3 | **fix** | `/fix [bug]` | Debug + ghi LESSONS | v2 |
| 4 | **save** | `/save` | Review 7 tiêu chí + save + push | v2 |
| 5 | **plan** | `/plan [feature]` | Tạo task blueprint | v2 |
| 6 | **design** | `/design [task]` | UI/UX + Design Tokens + Component API | v2 ⬆ |
| 7 | **guard** | `/guard [scope]` | Test + security + performance | v2 ⬆ |
| 8 | **integrate** | `/integrate [svc]` | API + 3rd-party + webhook | v2 |
| 9 | **n8n-pro** | `/n8n [task]` | N8N + MCP Server + Sub-workflows + Scaling | v3 ⬆ |
| 10 | **brainstorm** | `/brainstorm [idea]` | Ideation → Spec → Prototype | v3 |
| 11 | **web-security** | `/security [target]` | OWASP audit + CVE + hardening | v3 |
| 12 | **review-website** | `/review-web [url]` | Website review toàn diện 7 chiều | v3 |
| 13 | **docs** 🆕 | `/docs [scope]` | Documentation + ADR + handoff | v4 |
| 14 | **seo** 🆕 | `/seo [topic]` | SEO + GEO content writer | v4 |

---

## 🔄 SESSION LIFECYCLE v4

```
/start [task]  →  [auto-select skill]  →  /save
   │                    │                    │
   ├─ Load context      ├─ /build ⭐        ├─ 7-criteria review
   ├─ Check LESSONS     ├─ /fix             ├─ Write LESSONS
   ├─ Qdrant recall     ├─ /design          ├─ Update docs
   ├─ Scan blueprints   ├─ /n8n             ├─ Qdrant store
   └─ Auto-select       ├─ /guard           ├─ Atomic commit
      14 skills         ├─ /integrate       └─ Push
                        ├─ /plan
                        ├─ /brainstorm
                        ├─ /security
                        ├─ /review-web
                        ├─ /docs 🆕
                        └─ /seo 🆕
```

---

## 🧠 MEMORY ARCHITECTURE (5 Tầng)

| Tầng | File/System | Mục đích |
|---|---|---|
| **1 — Session** | `ACTIVE_CONTEXT.md` | Working memory (xóa sau /save) |
| **2 — Project** | `GEMINI.md`, `LESSONS.md`, `CHANGE_LOG.md`, `NEXT-TODO.md` | Não bộ dự án |
| **3 — Knowledge** | `docs/*.md` | Tài liệu kỹ thuật |
| **4 — Vector** | Qdrant (MCP hoặc curl) | Cross-session semantic memory |
| **5 — Skills** | `~/.gemini/antigravity/skills/` + `.agent/commands/` | Skill + Blueprints |

---

## 📚 DOCUMENTATION STANDARDS

### Nguyên tắc viết docs

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
| `docs/security-audit.md` | Audit results, CVEs, remediation plan | Code patches |

### Cách viết Architecture Rules (bắt buộc)

```markdown
### Rule N: [Tên rule] (VI PHẠM = BUG)
[Giải thích 1 dòng tại sao rule này tồn tại]

// ✅ Đúng — [giải thích]
[code example đúng]

// ❌ Sai — [giải thích]
[code example sai]
```

### Cách viết LESSONS.md entries (bắt buộc)

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

## 📋 CHEAT SHEET v4

```
Bắt đầu phiên?           → /start [task]
Viết code?                → /build [task]         ⭐ Architecture Spec
Gặp bug?                  → /fix [bug]
Feature phức tạp?         → /plan [feature] → /build
Design / UI?              → /design [task]         ⬆ Token System
Test / audit code?        → /guard [scope]
API / webhook?            → /integrate [service]
N8N workflow?             → /n8n [task]            ⬆ MCP Server
Cần ý tưởng / spec?      → /brainstorm [idea]
Audit bảo mật sâu?       → /security [target]
Review website?           → /review-web [url]
Viết docs / handoff?      → /docs [scope]          🆕
Viết bài SEO / content?   → /seo [topic]           🆕
Kết thúc phiên?           → /save
```

---

## 📝 CHANGE LOG

| Date | Change |
|---|---|
| **2026-03-04** | **v4.0 STRUCTURED VIBE**: +2 skills mới (/docs, /seo). Nâng cấp /build (Architecture Spec Phase), /design (Token System + Component API), /n8n (MCP Server + Sub-workflows + Scaling), /start (14-rule auto-select). Thêm Rule 4 (Architecture Spec). |
| 2026-03-04 | v3.0 EXTENDED: +4 skills mới (brainstorm, n8n-pro, web-security, review-website). Nâng cấp start + guard. |
| 2026-02-28 | v2.0: Gộp 26 skills → 8 unified skills. Thêm /plan, /guard. Archive 27 skills cũ. |
| 2026-02-27 | v1.x: 26 skills, memory-optimizer, qdrant integration |
