# GEMINI — Skill Repository v7.0 (Streamlined + Progressive Disclosure)

> **GitHub**: https://github.com/tampd/skill
> **Local path**: /root/skill
> **Cập nhật**: 2026-03-09 — **v7.0 STREAMLINED** (14→8 skills + Progressive Disclosure + Claude Guide aligned)

---

## 🔑 THÔNG TIN QUAN TRỌNG

### Skill System v7.0 — Streamlined Architecture
Phiên bản 7.0 nâng cấp từ v6.2, tuân thủ [Claude Skill Guide](https://dotanminh.github.io/hdsd_claude_skill/):
- **14 → 8 skills**: Loại bỏ 100% overlap, giảm 42% context token
- **Progressive Disclosure**: YAML frontmatter (Tier 1) → SKILL.md body (Tier 2) → references/ (Tier 3)
- **YAML Frontmatter chuẩn**: name (kebab-case) + description (trigger phrases) + metadata
- **Composable modes**: Mỗi skill có modes thay vì tách riêng

### Triết lý v7.0: "Less Skills, More Modes, Zero Overlap"
```
✅ PROGRESSIVE DISCLOSURE: 3-tier loading giảm token 42%
✅ ZERO OVERLAP: 14→8 skills, mỗi knowledge chỉ 1 nơi
✅ COMPOSABLE MODES: /build --mode plan, /craft --mode audit, etc.
✅ YAML FRONTMATTER: Chuẩn Claude guide cho auto-trigger
✅ TDD Iron Law: Viết test TRƯỚC code
✅ Spec-Driven: Mỗi change = 1 folder
✅ SELF-REASONING GATE: 3-Question Self-Check
✅ Graceful fallback: Layer 4-5 không available → bỏ qua
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

## ⚠️ GLOBAL RULES (14 Rules)

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

### Rule 9: ACCESSIBILITY WCAG AA
> Mọi user-facing page PHẢI đạt WCAG 2.2 AA.
> Xem `/craft --mode audit` → axe DevTools 0 violations.

### Rule 10: SECURITY HEADERS
> HSTS, CSP, X-Frame-Options, X-Content-Type-Options là BẮT BUỘC.
> Xem `/ship check` → security headers checklist.

### Rule 11: DESIGN TOKEN MANDATORY
> KHÔNG hardcode colors, fonts, spacing.
> Mọi giá trị visual PHẢI qua semantic design tokens (var(--xxx)).

### Rule 12: PERFORMANCE BUDGET
> Lighthouse Performance (mobile) ≥ 90. LCP < 2.5s. CLS < 0.1. INP < 200ms.
> First Load JS < 150KB gzip.

### Rule 13: SELF-REASONING GATE
> 🛑 Trước MỌI quyết định thực thi, AI PHẢI chạy **3-Question Self-Check**:
>
> **Q1 — "Đây đã là phương án tốt nhất chưa?"**
> → Liệt kê ≥ 2 alternatives. So sánh nhanh pros/cons/effort.
>
> **Q2 — "Có risk hoặc side-effect nào tôi đang bỏ qua không?"**
> → Check: breaking changes? Regression? Performance? Security?
>
> **Q3 — "User có cần approve quyết định này không?"**
> → ≤ 2 files, không breaking → tự thực thi. ≥ 3 files hoặc risky → PHẢI hỏi.

### Rule 14: BKNS REPO STANDARD
> Mỗi repo BKNS **PHẢI** có: README.md, SPEC.md, DEPLOYMENT.md, CHANGELOG.md, PROJECT-META.md, .env.example.
> **Templates**: Xem `templates/` trong skill repo.

---

## 📁 CẤU TRÚC SKILL REPOSITORY v7.0

```
/root/skill/
├── GEMINI.md                  ← File này — brain & rules
├── README.md
│
│── ─── 8 SKILLS (v7.0) ───
├── session/                   ← /start /save /checkpoint (← start+save+memory)
│   ├── SKILL.md
│   └── references/
│       ├── review-checklist.md
│       └── trigger-keywords.md
│
├── build/                     ← /build /plan (← build+plan)
│   ├── SKILL.md
│   └── references/
│       ├── tdd-patterns.md
│       ├── code-standards.md
│       └── spec-templates.md
│
├── fix/                       ← /fix (tinh gọn)
│   ├── SKILL.md
│   └── references/
│       └── lessons-template.md
│
├── craft/                     ← /craft /quality (← craft+quality)
│   ├── SKILL.md
│   └── references/
│       ├── design-tokens.md
│       ├── wcag-checklist.md
│       └── perf-targets.md
│
├── secure/                    ← /security /ship (← web-security+ship)
│   ├── SKILL.md
│   └── references/
│       ├── owasp-checklist.md
│       ├── security-headers.md
│       └── ci-templates.md
│
├── automate/                  ← /n8n /integrate (← n8n-pro+integrate)
│   ├── SKILL.md
│   └── references/
│       ├── n8n-patterns.md
│       └── api-design.md
│
├── content/                   ← /seo /docs user (← seo+docs handoff)
│   ├── SKILL.md
│   └── references/
│       ├── seo-checklist.md
│       └── handoff-template.md
│
├── spec/                      ← /spec /docs init /docs adr (← docs arch+templates)
│   ├── SKILL.md
│   └── references/
│       ├── architecture-template.md
│       ├── api-template.md
│       └── adr-template.md
│
│── ─── BKNS TEMPLATES ───
├── templates/                 ← BKNS repo templates (giữ nguyên)
│
│── ─── INFRASTRUCTURE ───
├── qdrant-memory/SKILL.md     ← Qdrant Layer 4 setup
│
│── ─── ARCHIVED (v6.x → v7.0) ───
├── start/        ← → merged into session
├── save/         ← → merged into session
├── memory/       ← → merged into session
├── plan/         ← → merged into build
├── quality/      ← → merged into craft
├── ship/         ← → merged into secure
├── integrate/    ← → merged into automate
├── n8n-pro/      ← → merged into automate
├── web-security/ ← → merged into secure
├── docs/         ← → split into content + spec
├── seo/          ← → merged into content
├── design/       ← → merged into craft (v4.0)
├── guard/        ← → merged into craft (v5.0)
├── brainstorm/   ← → merged into build (v6.0)
├── review-website/ ← → merged into craft (v6.0)
└── archive/      ← Skills cũ v1+v2
```

---

## 🎯 8 SKILLS — QUICK REFERENCE

| # | Skill | Lệnh | Mục đích | Gộp từ |
|---|---|---|---|---|
| 1 | **session** | `/start` `/save` `/checkpoint` | 🔄 Lifecycle: mở/đóng/checkpoint phiên | start+save+memory |
| 2 | **build** ⭐ | `/build` `/plan` | 🔥 Vibe code: TDD + spec-driven | build+plan |
| 3 | **fix** | `/fix` | � Debug: 4-phase systematic | fix (tinh gọn) |
| 4 | **craft** | `/craft` `/quality` | 🎨 Design web: UI + tokens + a11y + perf | craft+quality |
| 5 | **secure** | `/security` `/ship` | 🔒 Security: OWASP + headers + deploy | web-security+ship |
| 6 | **automate** | `/n8n` `/integrate` | ⚡ Automation: N8N + API integration | n8n-pro+integrate |
| 7 | **content** | `/seo` `/docs user` | 📝 Content: SEO + user docs | seo+docs handoff |
| 8 | **spec** | `/spec` `/docs init` `/docs adr` | 📋 Spec & docs kỹ thuật | docs arch+templates |

---

## 🌐 WEBSITE DEVELOPMENT WORKFLOW v7.0

```
/craft setup   → /craft component → /craft audit  → /ship check  → /security
     │                 │                 │                │              │
  Stack choice     Token system     A11y + Perf     Pre-launch      OWASP audit
  Atomic Design    ARIA attrs       axe 0 err       Lighthouse≥90   Headers check
  TS strict        Animation        Contrast OK     Bundle<150KB    CI/CD pipeline
```

### Typical Sprint:
```
FEATURE MỚI:
/plan [feature]       → Ideation + Change folder + Tasks
/craft component      → Atomic Design + Tokens + ARIA
/build [task]         → TDD implementation
/craft audit          → A11y + Perf verification
/save                 → 2-stage review + commit

PRÉ-LAUNCH:
/security [target]    → OWASP deep audit
/ship check           → Full pre-launch checklist
/ship ci              → CI/CD pipeline setup
/ship monitor         → Monitoring + alerts
```

---

## 🧠 MEMORY ARCHITECTURE (5 Tầng)

| Tầng | Engine/File | Mục đích | Skill |
|---|---|---|---|
| **1 — Working** | `ACTIVE_CONTEXT.md` | Session memory (xóa sau /save) | /checkpoint |
| **2 — Semantic** | `GEMINI.md`, `STATE.md`, `NEXT-TODO.md` | Project brain | /start, /save |
| **3 — Episodic** | `LESSONS.md`, `CHANGE_LOG.md` | Long-term (append-only) | /fix, /save |
| **4 — Vector** | **Qdrant** MCP | Semantic search cross-session | /start, /build, /fix |
| **5 — Task Graph** | **Beads** CLI | Dependency-aware tasks | /start, /build, /save |

### 3-Tier Document Load Order
```
TIER 1 — LUÔN LOAD (/start):  GEMINI.md + LESSONS grep tags
TIER 2 — LOAD KHI CẦN:        docs/architecture.md, docs/business-rules.md
TIER 3 — LOAD KHI /build:     changes/<feature>/specs/ (chỉ spec đang build)
```

---

## 📚 DOCUMENTATION — ZERO OVERLAP

| File | Viết gì | KHÔNG viết gì |
|---|---|---|
| `GEMINI.md` | Tech stack, Rules, Version | Tasks, logs, state |
| `LESSONS.md` | Bug #WARN-XXX + ✅❌ code | Features, changelog |
| `NEXT-TODO.md` | Task backlog (⬜🔄✅) | Tasks đã xong (XÓA) |
| `ACTIVE_CONTEXT.md` | Working memory phiên hiện tại | Persistent info |
| `changes/` | Spec-driven: SPEC.md + tasks + delta-specs | |

---

## 📋 CHEAT SHEET v7.0

```
Bắt đầu phiên?           → /start [task]
Ý tưởng mơ hồ?           → /plan [idea]
Feature phức tạp?         → /plan [feature]       🔥 Ideation + SPEC
Viết code?                → /build [task]         🔥 TDD Iron Law
Gặp bug?                  → /fix [bug]            🔥 4-Phase Systematic
Design UI?                → /craft [task]         � Tokens + WCAG
Audit chất lượng?         → /craft audit          � A11y + Perf
Audit bảo mật?            → /security [target]    🔒 OWASP
Chuẩn bị deploy?          → /ship check           🔒 Pre-launch
Setup CI/CD?              → /ship ci              🔒 GitHub Actions
Monitor production?       → /ship monitor         🔒 Monitoring
API / webhook?            → /integrate [service]  ⚡ REST + webhook
N8N workflow?             → /n8n [task]            ⚡ Automation
Viết bài SEO?             → /seo [topic]           📝 SEO + GEO
Docs hướng dẫn?           → /docs user             📝 Onboarding
Architecture docs?        → /spec [scope]          � Technical docs
Khởi tạo repo BKNS?      → /docs init             📋 BKNS setup
Ghi ADR?                  → /docs adr              📋 Decision record
Lưu context?              → /checkpoint            🔄 Working memory
Kết thúc phiên?           → /save                  🔄 Review + commit
```

---

## 📝 CHANGE LOG

| Date | Change |
|---|---|
| **2026-03-09** | **v7.0 STREAMLINED**: Gộp 14→8 skills theo Claude Skill Guide. Progressive Disclosure (YAML→SKILL.md→references/). +session (start+save+memory), +secure (web-security+ship), +automate (n8n+integrate), +content (seo+docs user), +spec (docs tech+templates). Giảm ~3200→~1850 lines SKILL.md. |
| 2026-03-06 | v6.2 BKNS-ALIGNED: Rule 14 — BKNS Repo Standard. +8 templates. |
| 2026-03-06 | v6.1 SELF-REASONING GATE: Rule 13 — 3-Question Self-Check. |
| 2026-03-05 | v6.0 MEMORY-FIRST: 18→14 skills. +craft, +quality, +ship. |
| 2026-03-05 | v5.0 HYBRID UPGRADE: TDD Iron Law, Spec-Driven, 4-Phase Debug. |
| 2026-03-04 | v4.x: Beads + Qdrant 5-Layer Memory. +docs, +seo, +brainstorm, +n8n-pro. |
| 2026-02-28 | v2.0: Gộp 26 skills → 8 unified skills. |
