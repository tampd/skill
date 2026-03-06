# GEMINI — Skill Repository v6.0 (Memory-First Architecture)

> **GitHub**: https://github.com/tampd/skill
> **Local path**: /root/skill
> **Cập nhật**: 2026-03-05 — **v6.0 MEMORY-FIRST ARCHITECTURE**

---

## 🔑 THÔNG TIN QUAN TRỌNG

### Skill System v6.0 — Memory-First Architecture
Phiên bản 6.0 nâng cấp từ v5.0, tập trung **zero-overlap + composable skills**:
- 🆕 **craft**: Gộp design + frontend → 1 UI lifecycle (setup/component/css/a11y)
- 🆕 **quality**: Gộp guard + perf + review-website → 1 quality gate (test/a11y/perf/all)
- 🆕 **ship**: Pre-launch checklist + CI/CD + Monitoring + Rollback
- ⬆ **plan**: Gộp brainstorm → Ideation Step 0 + Spec-driven
- ⬆ **memory**: Context Compression Protocol + 3-Tier Load Order
- Giữ nguyên: start, build, fix, save, integrate, n8n-pro, web-security, docs, seo
- **Giảm 18→14 skills**: Loại bỏ overlap, tiết kiệm context window

### Triết lý v6.0: "Zero Overlap, Layered Context, Memory as Backbone"
```
✅ ZERO OVERLAP: Mỗi knowledge chỉ nằm ở 1 nơi duy nhất
✅ LAYERED CONTEXT: AI chỉ load context cần thiết (3-Tier)
✅ SKILL COMPOSITION: Skill nhỏ, composable, modes thay vì tách skill
✅ MEMORY AS BACKBONE: Bộ nhớ 5 tầng + Context Compression
✅ TDD Iron Law: Viết test TRƯỚC code (giữ từ v5.0)
✅ Spec-Driven: Mỗi change = 1 folder (giữ từ v5.0)
✅ Graceful fallback: Layer 4-5 không available → bỏ qua im lặng
✅ SELF-REASONING GATE: AI tự hỏi 3 câu trước MỌI quyết định (v6.1)
```

### GitHub Repository
```
URL:    https://github.com/tampd/skill
Remote: origin → git@github.com:tampd/skill.git (SSH)
Branch: main
SSH Key: SHA256:nTSlO07MbIplXX/j2FAHlyuSb+MJxPO1yboDHJJidFs
```

> **RULE**: Mọi dự án đều push qua SSH (`git@github.com:`), KHÔNG dùng HTTPS.

---

## ⚠️ GLOBAL RULES (13 Rules)

### Rule 1: AI PHẢI HỎI KHI CHƯA RÕ
> 🛑 AI PHẢI HỎI user TRƯỚC KHI lập kế hoạch, viết code, ghi docs.
> ❌ KHÔNG giả định và code khi chưa xác nhận.

### Rule 2: ĐỌC LESSONS TRƯỚC KHI CODE
> Mỗi task PHẢI bắt đầu bằng việc đọc LESSONS.md + grep tags liên quan.

### Rule 3: EVIDENCE-BASED VERIFICATION
> Không chấp nhận "nó chạy rồi". Phải có curl output, test results, screenshot, hoặc log.

### Rule 4: ARCHITECTURE SPEC TRƯỚC KHI CODE
> Module mới ≥ 3 files → BẮT BUỘC `/plan` tạo change folder trước.

### Rule 5: BEADS CHO TASK TRACKING
> `.beads/` → dùng `bd` CLI. Không có → dùng NEXT-TODO.md.

### Rule 6: TDD IRON LAW
> Viết test TRƯỚC code. RED → GREEN → REFACTOR → COMMIT.
> Viết code trước test? → XÓA code. **Exceptions**: prototype, config, CSS-only.

### Rule 7: BRAINSTORMING HARD GATE
> `/plan` PHẢI hỏi + propose ≥ 2 approaches TRƯỚC khi tạo spec.

### Rule 8: ROOT CAUSE TRƯỚC FIX
> `/fix` PHẢI xong Phase 1 (Root Cause Investigation) TRƯỚC khi fix.

### Rule 9: ACCESSIBILITY WCAG AA (v5.1→v6.0)
> Mọi user-facing page PHẢI đạt WCAG 2.2 AA.
> Xem `/craft a11y` → axe DevTools 0 violations.

### Rule 10: SECURITY HEADERS (v5.1→v6.0)
> HSTS, CSP, X-Frame-Options, X-Content-Type-Options là BẮT BUỘC.
> Xem `/ship check` → security headers checklist.

### Rule 11: DESIGN TOKEN MANDATORY (v5.1→v6.0)
> KHÔNG hardcode colors, fonts, spacing.
> Mọi giá trị visual PHẢI qua semantic design tokens (var(--xxx)).

### Rule 12: PERFORMANCE BUDGET (v5.1→v6.0)
> Lighthouse Performance (mobile) ≥ 90. LCP < 2.5s. CLS < 0.1. INP < 200ms.
> First Load JS < 150KB gzip.

### Rule 13: SELF-REASONING GATE (v6.1) 🆕
> 🛑 Trước MỌI quyết định thực thi, AI PHẢI chạy **3-Question Self-Check**:
>
> **Q1 — "Đây đã là phương án tốt nhất chưa?"**
> → Liệt kê ≥ 2 alternatives. So sánh nhanh pros/cons/effort.
> → Nếu phương án hiện tại KHÔNG rõ ràng tốt nhất → dừng, đề xuất options cho user.
>
> **Q2 — "Có risk hoặc side-effect nào tôi đang bỏ qua không?"**
> → Check: breaking changes? Regression? Performance? Security?
> → Nếu CÓ risk → ghi rõ và đề xuất mitigation TRƯỚC khi thực thi.
>
> **Q3 — "User có cần approve quyết định này không?"**
> → Nếu thay đổi ≤ 2 files, không breaking → tự thực thi + giải thích.
> → Nếu thay đổi ≥ 3 files, hoặc có breaking/risky → PHẢI hỏi user trước.

---

## 📁 CẤU TRÚC SKILL REPOSITORY v6.0

```
/root/skill/
├── GEMINI.md                ← File này — brain & rules
├── README.md                ← Hướng dẫn cài đặt
│
│── ─── 6 CORE SKILLS ───
├── start/SKILL.md           ← /start [task]          Memory Bootstrap + Skill Select
├── build/SKILL.md           ← /build [task] ⭐       🔥 TDD Iron Law + Spec Check
├── fix/SKILL.md             ← /fix [bug]             🔥 4-Phase Systematic Debug
├── save/SKILL.md            ← /save                  🔥 2-Stage Review + Archive
├── plan/SKILL.md            ← /plan [feature]        🔥 Ideation + Change Folder
├── memory/SKILL.md          ← /memory                5-Layer + Compression
│
│── ─── 3 WEB SKILLS (v6.0 merged) ───
├── craft/SKILL.md           ← /craft [task] 🆕       design+frontend → UI lifecycle
├── quality/SKILL.md         ← /quality [scope] 🆕    guard+perf+review → quality gate
├── ship/SKILL.md            ← /ship [scope] 🆕       Pre-launch + CI/CD + Monitor
│
│── ─── 5 SPECIALIST SKILLS ───
├── integrate/SKILL.md       ← /integrate [svc]       API + 3rd-party
├── n8n-pro/SKILL.md         ← /n8n [task]            N8N + MCP Server
├── web-security/SKILL.md    ← /security [target]     OWASP deep audit
├── docs/SKILL.md            ← /docs [scope]          Documentation + ADR
├── seo/SKILL.md             ← /seo [topic]           SEO + GEO content
│
│── ─── INFRASTRUCTURE ───
├── qdrant-memory/SKILL.md   ← Qdrant Layer 4 setup
│
│── ─── ARCHIVED (preserved) ───
├── design/SKILL.md          ← → merged into /craft
├── guard/SKILL.md           ← → merged into /quality
├── brainstorm/SKILL.md      ← → merged into /plan
├── review-website/SKILL.md  ← → merged into /quality all
└── archive/                 ← Skills cũ v1+v2
```

---

## 🎯 14 SKILLS — QUICK REFERENCE

| # | Skill | Lệnh | Mục đích | Version |
|---|---|---|---|---|
| 1 | **start** | `/start [task]` | Memory bootstrap + Skill select | v6.0 |
| 2 | **build** ⭐ | `/build [task]` | 🔥 TDD Iron Law + Spec Compliance | v5.0 |
| 3 | **fix** | `/fix [bug]` | 🔥 4-Phase Systematic Debug | v5.0 |
| 4 | **save** | `/save` | 🔥 2-Stage Review + Archive | v5.0 |
| 5 | **plan** | `/plan [feature]` `/brainstorm` | 🔥 Ideation + Change Folder | v6.0 |
| 6 | **memory** | `/memory` `/checkpoint` `/recall` | 5-Layer + Compression | v6.0 |
| 7 | **craft** 🆕 | `/craft [task]` | UI + Atomic Design + Tokens + WCAG | 🆕 v6.0 |
| 8 | **quality** 🆕 | `/quality [scope]` | Test + A11y + Perf + Lighthouse | 🆕 v6.0 |
| 9 | **ship** 🆕 | `/ship [scope]` | Pre-launch + CI/CD + Monitor | 🆕 v6.0 |
| 10 | **integrate** | `/integrate [svc]` | API + 3rd-party + webhook | v5.0 |
| 11 | **n8n-pro** | `/n8n [task]` | N8N + MCP Server | v3.0 |
| 12 | **web-security** | `/security [target]` | OWASP deep audit | v5.0 |
| 13 | **docs** | `/docs [scope]` | Documentation + ADR | v4.0 |
| 14 | **seo** | `/seo [topic]` | SEO + GEO content | v4.0 |

---

## 🌐 WEBSITE DEVELOPMENT WORKFLOW v6.0

```
/craft setup     →  /craft component  →  /quality a11y  →  /quality perf  →  /ship check
      │                    │                    │                │                │
   Stack choice        Token system         WCAG audit     Web Vitals      Pre-launch
   Atomic Design       ARIA attrs           axe 0 err      Lighthouse ≥90  Security hdrs
   TS strict           Animation            Contrast OK    Bundle <150KB   CI/CD pipeline
```

### Typical Sprint:
```
FEATURE MỚI:
/plan [feature]         → Ideation + Change folder + Tasks
/craft component        → Atomic Design + Tokens + ARIA
/build [task]           → TDD implementation
/quality test           → Unit + integration tests
/quality a11y           → WCAG verification
/quality perf           → Web Vitals check
/save                   → 2-stage review + commit

PRÉ-LAUNCH:
/security [target]      → OWASP deep audit
/ship check             → Full pre-launch checklist
/ship ci                → CI/CD pipeline setup
/ship monitor           → Monitoring + alerts
```

---

## 🧠 MEMORY ARCHITECTURE (5 Tầng + Compression)

| Tầng | Engine/File | Mục đích | Skill |
|---|---|---|---|
| **1 — Working** | `ACTIVE_CONTEXT.md` | Session memory (xóa sau /save) | /checkpoint, /recall |
| **2 — Semantic** | `GEMINI.md`, `STATE.md`, `NEXT-TODO.md` | Project brain | /start, /save |
| **3 — Episodic** | `LESSONS.md`, `CHANGE_LOG.md` | Long-term (append-only) | /fix, /save |
| **4 — Vector** | **Qdrant** MCP | Semantic search cross-session | /start, /build, /fix |
| **5 — Task Graph** | **Beads** CLI | Dependency-aware tasks | /start, /build, /save |

### 3-Tier Document Load Order (v6.0)
```
TIER 1 — LUÔN LOAD (/start):  GEMINI.md + LESSONS grep tags
TIER 2 — LOAD KHI CẦN:        docs/architecture.md, docs/business-rules.md
TIER 3 — LOAD KHI /build:     changes/<feature>/specs/ (chỉ spec đang build)
```

---

## 📚 DOCUMENTATION — ZERO OVERLAP

| File | Viết gì | KHÔNG viết gì |
|---|---|---|
| `GEMINI.md` | Tech stack, Rules (✅❌ code), Version | Tasks, logs, state |
| `LESSONS.md` | Bug #WARN-XXX + ✅❌ code + quy tắc | Features, changelog |
| `CHANGE_LOG.md` | Timeline: Added/Fixed/Changed/Removed | Tasks, rules |
| `NEXT-TODO.md` | Task backlog (⬜🔄✅ tracking) | Tasks đã xong (XÓA) |
| `ACTIVE_CONTEXT.md` | Working memory phiên hiện tại | Persistent info |
| `docs/` | Architecture, business rules, API, setup | Task lists |
| `changes/` | Spec-driven: proposal + specs + design + tasks | |

---

## 📋 CHEAT SHEET v6.0

```
Bắt đầu phiên?           → /start [task]
Ý tưởng mơ hồ?           → /plan [idea]          (= /brainstorm)
Feature phức tạp?         → /plan [feature]       🔥 Ideation + Change Folder
Viết code?                → /build [task]         🔥 TDD Iron Law
Gặp bug?                  → /fix [bug]            🔥 4-Phase Systematic
Setup project frontend?   → /craft setup          🆕 Atomic Design + TS
Design UI component?      → /craft component      🆕 Tokens + WCAG + Animation
Test / audit a11y?        → /quality test|a11y     🆕 axe-core + semantic tests
Check performance?        → /quality perf          🆕 Lighthouse + Web Vitals
Chuẩn bị deploy?          → /ship check           🆕 Pre-launch checklist
Setup CI/CD?              → /ship ci              🆕 GitHub Actions
API / webhook?            → /integrate [service]
N8N workflow?             → /n8n [task]
Audit bảo mật sâu?        → /security [target]
Viết docs / handoff?      → /docs [scope]
Viết bài SEO?             → /seo [topic]
Lưu context giữa phiên?  → /checkpoint
Kết thúc phiên?           → /save                 🔥 2-Stage Review
```

---

## 📝 CHANGE LOG

| Date | Change |
|---|---|
| **2026-03-05** | **v6.0 MEMORY-FIRST**: Gộp thông minh 18→14 skills. +3 merged skills (craft=design+frontend, quality=guard+perf+review, ship=new). Gộp brainstorm→plan (Ideation Step 0). Thêm Context Compression Protocol + 3-Tier Load Order vào memory. 12 Global Rules (thêm 9-12: A11y, Security Headers, Design Tokens, Perf Budget). Website Development Workflow. Zero-overlap documentation model. |
| 2026-03-05 | v5.0 HYBRID UPGRADE: TDD Iron Law, Spec-Driven, 4-Phase Debug, 2-Stage Review. |
| 2026-03-04 | v4.1 HYBRID MEMORY: Beads + Qdrant 5-Layer Memory. |
| 2026-03-04 | v4.0 STRUCTURED VIBE: +docs, +seo skills. |
| 2026-03-04 | v3.0 EXTENDED: +brainstorm, +n8n-pro, +web-security, +review-website. |
| 2026-02-28 | v2.0: Gộp 26 skills → 8 unified skills. |
