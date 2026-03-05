# GEMINI — Skill Repository v5.0 (Hybrid Memory + TDD + Spec-Driven)

> **GitHub**: https://github.com/tampd/skill
> **Local path**: /root/skill
> **Cập nhật**: 2026-03-05 — **v5.0 HYBRID UPGRADE**

---

## 🔑 THÔNG TIN QUAN TRỌNG

### Skill System v5.0 — Hybrid Upgrade
Phiên bản 5.0 nâng cấp từ v4.1 (15 skills), tập trung **methodology**:
- ⬆ **build**: 🔥 **TDD Iron Law** (RED→GREEN→REFACTOR) + Spec Compliance Check
- ⬆ **plan**: 🔥 **Change Folder** (proposal/specs/design/tasks/delta-specs) + Brainstorming Hard Gate
- ⬆ **fix**: 🔥 **4-Phase Systematic Debugging** (Root Cause Investigation mandatory)
- ⬆ **save**: 🔥 **2-Stage Review** (Spec Compliance + Code Quality) + Change Archive + Delta Merge
- Giữ nguyên: start, design, guard, integrate, n8n-pro, brainstorm, web-security, review-website, docs, seo, memory
- Lấy cảm hứng từ: **Superpowers** (TDD, systematic-debugging) + **OpenSpec** (spec-driven, change folders)

### Triết lý v5.0: "Test First, Spec First, Root Cause First"
```
✅ TDD Iron Law: Viết test TRƯỚC code. Viết code trước test? XÓA.
✅ Spec-Driven: Mỗi change = 1 folder (proposal + specs + design + tasks)
✅ Systematic Debug: Tìm root cause TRƯỚC khi fix. KHÔNG fix symptoms.
✅ 5-Layer Memory: Working → Semantic → Episodic → Vector → Task Graph
✅ Graceful fallback: Layer 4-5 không available → bỏ qua im lặng
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

### Rule 4: ARCHITECTURE SPEC TRƯỚC KHI CODE
> Project mới hoặc module mới ≥ 3 files → BẮT BUỘC tạo `.spec.md` trước.
> Spec gồm: Site Map, Component Tree, Data Flow, File Structure, Design Tokens.
> Xem chi tiết: `/build` Step 0.5.

### Rule 5: BEADS CHO TASK TRACKING, NEXT-TODO LÀM FALLBACK
> Nếu dự án có `.beads/` → dùng `bd` CLI cho task management.
> Nếu Beads chưa init hoặc không available → dùng NEXT-TODO.md như bình thường.
> Layer 4-5 (Qdrant/Beads) KHÔNG bao giờ gây lỗi nếu không available.

### Rule 6: TDD IRON LAW (MỚI v5.0)
> Viết test TRƯỚC code. RED (fail) → GREEN (pass) → REFACTOR → COMMIT.
> Viết code trước test? → XÓA code, bắt đầu lại. Không ngoại lệ.
> **Exceptions** (hỏi user): throwaway prototype, config files, generated code, CSS-only.

### Rule 7: BRAINSTORMING HARD GATE (MỚI v5.0)
> `/plan` PHẢI hỏi clarifying questions + propose 2-3 approaches TRƯỚC.
> KHÔNG tạo spec/change folder khi chưa được user approve approach.

### Rule 8: ROOT CAUSE TRƯỚC FIX (MỚI v5.0)
> `/fix` PHẢI xong Phase 1 (Root Cause Investigation) TRƯỚC KHI fix.
> "Nó có vẻ rõ ràng" KHÔNG phải root cause. CHỨNG MINH bằng evidence.

---

## 📁 CẤU TRÚC SKILL REPOSITORY v5.0

```
/root/skill/
├── GEMINI.md                ← File này — brain & rules
├── README.md                ← Hướng dẫn cài đặt
│
│── ─── 8 CORE SKILLS (v5.0 upgraded) ───
├── start/SKILL.md           ← /start [task]          Beads Ready + Qdrant Recall
├── build/SKILL.md           ← /build [task] ⭐       🔥 TDD Iron Law + Spec Check
├── fix/SKILL.md             ← /fix [bug]             🔥 4-Phase Systematic Debug
├── save/SKILL.md            ← /save                  🔥 2-Stage Review + Archive
├── plan/SKILL.md            ← /plan [feature]        🔥 Change Folder + Hard Gate
├── design/SKILL.md          ← /design [task]         Token System + Component API
├── guard/SKILL.md           ← /guard [scope]
├── integrate/SKILL.md       ← /integrate [svc]
│
│── ─── 4 SKILLS (v3) ───
├── n8n-pro/SKILL.md         ← /n8n [task]            MCP Server + Sub-workflows
├── brainstorm/SKILL.md      ← /brainstorm [idea]
├── web-security/SKILL.md    ← /security [target]
├── review-website/SKILL.md  ← /review-web [url]
│
│── ─── 2 SKILLS (v4) ───
├── docs/SKILL.md            ← /docs [scope]          Documentation + ADR
├── seo/SKILL.md             ← /seo [topic]           SEO + GEO Writer
│
│── ─── 1 SKILL (v4.1) ───
├── memory/SKILL.md          ← /memory /checkpoint /recall
│
│── ─── INFRASTRUCTURE ───
├── qdrant-memory/SKILL.md   ← Qdrant Layer 4 setup guide
└── archive/                 ← Skills cũ v1+v2 (preserved)
```

---

## 🎯 15 SKILLS — QUICK REFERENCE

| # | Skill | Lệnh | Mục đích | v5.0 |
|---|---|---|---|---|
| 1 | **start** | `/start [task]` | Khởi động phiên + Memory recall | |
| 2 | **build** ⭐ | `/build [task]` | 🔥 **TDD** RED→GREEN→REFACTOR + Spec Check | ⬆ |
| 3 | **fix** | `/fix [bug]` | 🔥 **4-Phase Systematic Debug** (root cause first) | ⬆ |
| 4 | **save** | `/save` | 🔥 **2-Stage Review** + Change Archive + Delta Merge | ⬆ |
| 5 | **plan** | `/plan [feature]` | 🔥 **Change Folder** + Brainstorming Hard Gate | ⬆ |
| 6 | **design** | `/design [task]` | UI/UX + Design Tokens + Component API | |
| 7 | **guard** | `/guard [scope]` | Test + security + performance | |
| 8 | **integrate** | `/integrate [svc]` | API + 3rd-party + webhook | |
| 9 | **n8n-pro** | `/n8n [task]` | N8N + MCP Server + Sub-workflows | |
| 10 | **brainstorm** | `/brainstorm [idea]` | Ideation → Spec → Prototype | |
| 11 | **web-security** | `/security [target]` | OWASP audit + CVE + hardening | |
| 12 | **review-website** | `/review-web [url]` | Website review toàn diện 7 chiều | |
| 13 | **docs** | `/docs [scope]` | Documentation + ADR + handoff | |
| 14 | **seo** | `/seo [topic]` | SEO + GEO content writer | |
| 15 | **memory** | `/memory` `/checkpoint` `/recall` | 5-Layer Memory management | |

---

## 🔄 SESSION LIFECYCLE v5.0

```
/start [task]  →  [auto-select skill]  →  /save
   │                    │                    │
   ├─ Load context      ├─ /build ⭐        ├─ 2-Stage Review 🔥
   ├─ Check LESSONS     │  TDD: RED→GREEN   │  Stage 1: Spec compliance
   ├─ Beads ready       │  →REFACTOR→COMMIT │  Stage 2: Code quality (7)
   ├─ Qdrant recall     ├─ /plan            ├─ Change Archive 🔥
   ├─ Scan changes/     │  Hard Gate → Ask  │  delta merge → docs/
   └─ Auto-select       │  → Change Folder  ├─ Write LESSONS
      15 skills         ├─ /fix             ├─ Beads close + compact
                        │  4-Phase Debug 🔥  ├─ Qdrant store
                        │  Root Cause FIRST  ├─ Atomic commit
                        ├─ /design           └─ Push
                        ├─ /n8n
                        ├─ /guard
                        └─ ...
```

---

## 🧠 MEMORY ARCHITECTURE (5 Tầng — Hybrid v4.1)

| Tầng | Engine/File | Mục đích | Skill tương tác |
|---|---|---|---|
| **1 — Working** | `ACTIVE_CONTEXT.md` | Working memory (xóa sau /save) | /checkpoint, /recall |
| **2 — Semantic** | `GEMINI.md`, `STATE.md`, `NEXT-TODO.md` | Não bộ dự án | /start, /save |
| **3 — Episodic** | `LESSONS.md`, `CHANGE_LOG.md` | Bộ nhớ dài hạn (append-only) | /fix, /save |
| **4 — Vector** 🆕 | **Qdrant** (MCP: qdrant_find/store) | Semantic search cross-session | /start, /build, /fix, /save |
| **5 — Task Graph** 🆕 | **Beads/Dolt** (CLI: bd ready/create/close) | Dependency-aware task tracking | /start, /build, /save, /plan |

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

### Cách viết Change Folder (v5.0 — tạo bởi /plan)

```
changes/<feature-name>/
├── proposal.md      ← WHY: Business context, impact, scope
├── specs/
│   └── spec.md      ← WHAT: Requirements & scenarios
├── design.md        ← HOW: Technical approach, architecture
├── tasks.md         ← TDD bite-sized tasks (2-5 min each)
└── delta-specs.md   ← Merge into docs/ when archived
```

> Xem chi tiết: `/plan` SKILL.md

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

## 📋 CHEAT SHEET v5.0

```
Bắt đầu phiên?           → /start [task]
Viết code?                → /build [task]         🔥 TDD Iron Law
Gặp bug?                  → /fix [bug]            🔥 4-Phase Systematic
Feature phức tạp?         → /plan [feature]       🔥 Change Folder + Hard Gate
Design / UI?              → /design [task]
Test / audit code?        → /guard [scope]
API / webhook?            → /integrate [service]
N8N workflow?             → /n8n [task]
Cần ý tưởng / spec?      → /brainstorm [idea]
Audit bảo mật sâu?       → /security [target]
Review website?           → /review-web [url]
Viết docs / handoff?      → /docs [scope]
Viết bài SEO / content?   → /seo [topic]
Lưu context giữa phiên?  → /checkpoint
Khôi phục context?        → /recall
Xem trạng thái memory?    → /memory
Kết thúc phiên?           → /save                 🔥 2-Stage Review + Archive
```

---

## 📝 CHANGE LOG

| Date | Change |
|---|---|
| **2026-03-05** | **v5.0 HYBRID UPGRADE**: Nâng cấp 4 skills core từ Superpowers + OpenSpec. /build: TDD Iron Law (RED→GREEN→REFACTOR), Spec Compliance Check. /plan: Change Folder (proposal+specs+design+tasks+delta-specs), Brainstorming Hard Gate. /fix: 4-Phase Systematic Debugging, Root Cause Iron Law. /save: 2-Stage Review (Spec+Quality), Change Archive + Delta Merge. Thêm Rule 6-8. |
| 2026-03-04 | v4.1 HYBRID MEMORY: +1 skill (/memory). Layer 5 Beads + Layer 4 Qdrant. Thêm Rule 5. |
| 2026-03-04 | v4.0 STRUCTURED VIBE: +2 skills (/docs, /seo). Architecture Spec Phase. Thêm Rule 4. |
| 2026-03-04 | v3.0 EXTENDED: +4 skills (brainstorm, n8n-pro, web-security, review-website). |
| 2026-02-28 | v2.0: Gộp 26 skills → 8 unified skills. Thêm /plan, /guard. |
| 2026-02-27 | v1.x: 26 skills, memory-optimizer, qdrant integration |
