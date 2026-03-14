# APEX SKILL SYSTEM v4.0 — Global Rules
> Applied to ALL sessions in this project. Read before every task.
> Triết lý: AI không bao giờ quên. Mỗi phiên đều học hỏi và kết nối với quá khứ. Fresh context = High quality.

## MEMORY FILES (read at session start)
- `LESSONS.md` — **critical-only** bài học (≤10 entries, importance ≥0.8). Luôn đọc.
- `LESSONS_ARCHIVE.md` — archived lessons (importance <0.8 hoặc entries cũ). Chỉ đọc khi cần.
- `INSTINCTS.md` — patterns đã học + confidence score (0.0–1.0)
- `STATE.md` — project state hiện tại (phase, wave, blockers)
- `ACTIVE_CONTEXT.md` — working memory phiên này (xóa sau /save)
- `INSIGHTS.md` — compound insights từ /consolidate (auto-generated)
- `.ai/memory/MEMORY.md` — **auto-memory**: AI tự ghi notes giữa phiên (≤200 dòng index)

---

## 23 GLOBAL RULES

### Core Behavior
1. **ASK WHEN UNCLEAR** — Nếu yêu cầu mơ hồ hoặc có ≥2 cách hiểu → hỏi trước, đừng đoán
2. **READ MEMORY FIRST** — Đọc `LESSONS.md` + `INSTINCTS.md` + `INSIGHTS.md` trước bất kỳ task nào
3. **EVIDENCE-BASED** — Mọi quyết định kỹ thuật phải có lý do cụ thể, không phán đoán
4. **SELF-REASONING GATE** — Tự hỏi 3 câu trước khi implement: (a) Đây có phải cách tốt nhất? (≥2 alternatives) (b) Có risk/side-effect nào đang bỏ qua? (c) User có cần approve không?
5. **SEARCH-FIRST** — Tìm code/solution có sẵn trước khi tự viết. Ưu tiên: official > popular > custom

### Architecture & Planning
6. **SPEC BEFORE CODE** — Task ≥3 files hoặc ≥2h effort → viết `/spec` trước, không code ngay
7. **PLAN-FIRST** — Generate task list có checkpoint TRƯỚC khi bắt đầu code
8. **BRAINSTORM HARD GATE** — Feature phức tạp → `/plan` bắt buộc, đề xuất ≥2 approaches
9. **ADR FOR DECISIONS** — Mọi architectural decision quan trọng → ghi `/adr` ngay lúc quyết định
10. **WAVE EXECUTION** — Foundation → Integration → Polish

### Code Quality
11. **TDD IRON LAW** — Test TRƯỚC code (feature phức tạp). Coverage ≥80%
12. **QUALITY GATE** — Trước mỗi commit: `lint → format → type-check → test → audit`. Tất cả PASS mới commit
13. **CLEANUP PASS** — Sau implement + verify → review pass: dead code, TODO cũ, console.log, magic numbers
14. **ATOMIC COMMITS** — 1 task = 1 commit. Conventional format: `feat|fix|chore|perf|test|sec(scope): mô tả`

### Verification
15. **ROOT CAUSE FIRST** — Bug: tìm nguyên nhân gốc trước khi fix. Không patch symptom
16. **VERIFICATION LOOP** — Sau mỗi implementation: verify → regression check. Chỉ báo "done" khi tất cả pass
17. **INSTINCT VALIDATION** — Khi apply instinct → log kết quả. Đúng: +0.1. Sai: −0.2. Dưới 0.3 sau 5 lần → xem xét xóa

### Design & UX
18. **WCAG AA** — Mọi UI component phải đạt WCAG 2.2 AA. Color contrast, keyboard nav, aria-label
19. **DESIGN TOKEN MANDATORY** — Không dùng hardcoded color/spacing/font. Luôn dùng design tokens
20. **PERFORMANCE BUDGET** — LCP ≤2.5s, CLS ≤0.1, FID ≤100ms

### Fresh Context & Deep Thinking (v4.0)
21. **ULTRATHINK GATE** — Architecture decisions, complex bugs, refactoring >5 files → prefix "ultrathink:" → full analysis plan (≥500 words) TRƯỜC khi code. List ≥3 alternatives.
22. **CONTEXT HEALTH MONITOR** — Track context window health: 🟢 Fresh (0-30%) | 🟡 Loaded (30-60%) | 🔴 Heavy (60-80%) | 💀 Critical (>80%). Khi Heavy/Critical → commit, /save, fresh session.
23. **GSD CYCLE** — Features lớn dùng /gsd: Discuss → Plan → Execute → Verify. Mỗi phase có checkpoint context. KHÔNG skip Phase V (Verify).

---

## ANTIGRAVITY NATIVE FEATURES — Sử dụng tích cực

```
Browser Sub-Agent    → /craft, /fix UI bugs, /e2e verification
Multi-Agent Parallel → /build large features (split thành sub-tasks)
Mission Control      → Monitor parallel agents, assign /spec + /build + /secure cùng lúc
Artifact Output      → /spec, /handoff, /runbook output dạng structured Artifact
Planning Mode        → Luôn gen task-list có checkpoint trước khi code
Stitch MCP           → /craft setup → generate screens → edit screens → export code
Ultrathink           → Deep reasoning cho architecture/complex bugs (v4.0)
Context Health       → Monitor context window, auto-suggest fresh session (v4.0)
```

---

## 🎯 9 SKILLS — 35 COMMANDS

| # | Skill | Commands | Purpose |
|---|---|---|---|
| 1 | **session** | `/start` `/save` `/checkpoint` `/review` `/recall` | 🔄 6-Layer Bootstrap + Consolidation Save + Quality Gate |
| 2 | **build** ⭐ | `/build` `/plan` `/search` `/gsd` | 🔥 Search-First + TDD + GSD Cycle + Ultrathink |
| 3 | **fix** | `/fix` | 🐛 4-Phase Debug + Insight Match + Auto-Ingest |
| 4 | **craft** | `/craft` `/audit` `/tokens` `/e2e` | 🎨 UI + Design Tokens + WCAG + Browser Testing |
| 5 | **secure** | `/security` `/harden` `/ship` | 🔒 OWASP + Hardening + Production Readiness |
| 6 | **automate** | `/n8n` `/integrate` `/mcp` | ⚡ N8N + API Design + MCP Tool Design |
| 7 | **content** | `/seo` `/article` `/brief` `/audit-content` | 📝 SEO Pipeline + Content Engine |
| 8 | **spec** ⭐ | `/spec` `/handoff` `/adr` `/runbook` `/apidoc` `/componentdoc` | 📋 Architecture + Handoff + API/Component Docs |
| 9 | **learn** ⭐ | `/learn` `/instinct` `/evolve` `/review-instincts` `/consolidate` `/cross-link` | 🧠 Ingest + Consolidation + Instinct Evolution |

---

## 🧠 MEMORY ARCHITECTURE v4.0 — 4-Layer Smart Retrieval + Context Health

> Triết lý: Không đọc hết, chỉ đọc đúng. Tối ưu context window.

```
┌─────────────────────────────────────────────────────────────┐
│  LAYER A — RULES (tĩnh, user kiểm soát, luôn đọc)          │
│  Files: GEMINI.md, STATE.md                                  │
│  Nội dung: Tech stack, conventions, rules, project state     │
│  Đặc điểm: Git-tracked, human-editable                       │
├─────────────────────────────────────────────────────────────┤
│  LAYER B — CRITICAL LESSONS (tĩnh, ≤10 entries, luôn đọc)   │
│  File: LESSONS.md (chỉ importance ≥ 0.8)                     │
│  Archive: LESSONS_ARCHIVE.md (importance < 0.8 hoặc cũ)      │
│  Đặc điểm: Luôn đọc mỗi /start, ngắn gọn, git-tracked      │
├─────────────────────────────────────────────────────────────┤
│  LAYER C — SEMANTIC MEMORY (Qdrant, search theo nghĩa)      │
│  Engine: Qdrant Vector DB (MCP: qdrant_find / qdrant_store)  │
│  Nội dung: TẤT CẢ lessons + insights + patterns              │
│  Truy xuất: /start search top-5 relevant theo task context   │
│  Đặc điểm: Cross-project, scale vô hạn, semantic search     │
├─────────────────────────────────────────────────────────────┤
│  LAYER D — AUTO-MEMORY (AI tự viết, local)                  │
│  File: .ai/memory/MEMORY.md (≤200 dòng) + topic files        │
│  Nội dung: Debugging insights, gotchas, project-specific      │
│  Đặc điểm: AI tự ghi, machine-local, không git               │
│  200-line rule: quá dài → tách thành topic files              │
└─────────────────────────────────────────────────────────────┘
```

### /start Smart Load Flow
```
1. RULES:    Đọc GEMINI.md + STATE.md                    ← Layer A
2. CRITICAL: Đọc LESSONS.md (≤10 entries, ≥0.8)          ← Layer B
3. SEMANTIC: qdrant_find(task context) → top 5 relevant  ← Layer C
4. AUTO:     Đọc .ai/memory/MEMORY.md (≤200 dòng)        ← Layer D
5. INSTINCT: INSTINCTS.md (top-3, confidence ≥0.7)       
6. INSIGHT:  INSIGHTS.md (nếu có liên quan)
→ Tổng: ~15-20 entries MAX thay vì ALL entries
```

### Importance Score (áp dụng cho LESSONS entries)

```
≥0.8 — Giữ trong LESSONS.md (critical, luôn đọc)
<0.8 — Archive sang LESSONS_ARCHIVE.md (Qdrant search khi cần)

1.0 — Security vulnerability, data loss risk
0.8 — 🔴 Critical: breaks production, major UX failure
0.6 — 🟡 Warning: blocks feature, performance hit
0.4 — 🟢 Info: best practice, optimization tip
0.2 — 💡 Note: minor improvement, style
```

---

## SKILL MAP

| Trigger | Skill |
|---------|-------|
| Bắt đầu / kết thúc phiên | `session` |
| Viết code mới, feature | `build` |
| Debug, lỗi, crash | `fix` |
| UI, design, component | `craft` |
| Security, deploy, production | `secure` |
| N8N, API, automation, MCP | `automate` |
| Bài viết, SEO, content | `content` |
| Architecture, docs, handoff | `spec` |
| Học hỏi, patterns, instincts, consolidate | `learn` |

---

## 📋 CHEAT SHEET

```
Bắt đầu phiên?           → /start [task]
Resume phiên cũ?          → /recall
Ý tưởng mơ hồ?           → /plan [idea]
Feature phức tạp?         → /plan [feature]
Feature lớn (GSD)?       → /gsd [feature]          🆕 v4.0
Research trước code?      → /search [need]
Viết code?                → /build [task]           🔥 TDD + Search-First
Deep thinking?            → ultrathink: [task]      🆕 v4.0
Gặp bug?                  → /fix [bug]              🐛 4-Phase Debug
Design UI?                → /craft [task]           🎨 Tokens + WCAG
Design tokens?            → /tokens [scope]
Browser test?             → /e2e [scope]
Audit chất lượng?         → /audit [scope]
Audit bảo mật?            → /security [target]      🔒 OWASP
Hardening?                → /harden [service]
Chuẩn bị deploy?          → /ship [env]
API / webhook?            → /integrate [service]     ⚡
N8N workflow?             → /n8n [task]              ⚡
Thiết kế MCP tool?        → /mcp [tool-name]
Content brief?            → /brief [topic]           📝
Viết bài SEO?             → /seo [topic]
Viết article?             → /article [topic]
Audit content?            → /audit-content [URL]
Architecture docs?        → /spec [scope]            📋
Bàn giao dự án?           → /handoff [project]
API documentation?        → /apidoc [endpoint]
Component docs?           → /componentdoc [scope]
Ghi ADR?                  → /adr [decision]
Tạo runbook?              → /runbook [service]
Ghi bài học?              → /learn [observation]     🧠 Ingest + Tag
Xem instincts?            → /instinct
Evolve instincts?         → /evolve
Review instincts?         → /review-instincts
Tổng hợp insights?       → /consolidate             🧠 Sleep-Brain Pass
Cross-link memories?      → /cross-link
Review code?              → /review [scope]
Lưu context?              → /checkpoint
Kết thúc phiên?           → /save                    (auto /consolidate)
```

---

## 📊 COMBO SKILLS

| Tình huống | Combo |
|---|---|
| Feature code mới | `/start` → check INSIGHTS → `/build` → `/save` |
| Feature lớn (GSD) | `/start` → `/gsd` (D→P→E→V) → `/save` |
| Fix bug | `/fix` (4-phase) → `/learn` (ingest) → `/consolidate` (nếu ≥3 lessons) |
| API endpoint mới | `/search` → `/build` → `/security` → `/review` |
| UI/Component mới | `/craft` → `/tokens` → `/audit` → `/e2e` |
| Dự án mới | `/plan` (GSD) → `/spec` → `/build` (wave-by-wave) |
| Deploy/Release | `/audit` → `/security` → `/harden` → `/ship` |
| Sau nhiều phiên | `/consolidate` → check INSIGHTS → `/evolve` |

---

## DOCUMENTATION — ZERO OVERLAP

| File | Layer | Viết gì | KHÔNG viết gì |
|---|---|---|---|
| `GEMINI.md` | A — Rules | Tech stack, Rules, Skill map | Tasks, logs, state |
| `STATE.md` | A — Rules | Phase, wave, blockers, next milestones | History |
| `LESSONS.md` | B — Critical | **Chỉ** lessons importance ≥0.8, ≤10 entries | Low-priority lessons |
| `LESSONS_ARCHIVE.md` | B → C | Lessons importance <0.8 hoặc cũ (Qdrant searchable) | Critical lessons |
| `CHANGELOG.md` | A — Rules | Timeline thay đổi theo ngày | Tasks, lessons |
| `INSTINCTS.md` | A — Rules | Learned patterns + confidence 0.0–1.0 | Bug reports |
| `INSIGHTS.md` | C — Semantic | Cross-memory connections, compound insights | Raw data |
| `.ai/memory/MEMORY.md` | D — Auto | AI notes: debugging, gotchas, ≤200 dòng | User rules |
| `ACTIVE_CONTEXT.md` | D — Auto | Working memory phiên hiện tại | Persistent info |

---

## CHANGELOG

| Date | Change |
|---|---|
| **2026-03-14** | **APEX v4.0**: Progressive Disclosure (references/ folders), /gsd command (GSD workflow), Ultrathink Mode, Context Health Monitor. 3 rules mới (21-23). Skills: build/fix/craft được refactor với references/. Research-based: GSD2 (23k⭐), Anthropic SKILL.md patterns, Claude Code docs, sub-agent patterns, context rot research. |
| 2026-03-10 | **APEX v3.0 Memory**: 4-Layer Smart Retrieval. LESSONS.md critical-only (≤10, ≥0.8). LESSONS_ARCHIVE.md. Auto-Memory. Qdrant search. |
| 2026-03-10 | **APEX v2.0**: 6-Layer Memory, 35 commands, Importance scoring, INSIGHTS.md, consolidation. |
| 2026-03-10 | v2.0: 20 rules, 9 skills, 32 commands. Research-based: ECC + 12 GitHub repos. |
| 2026-03-09 | v1.0 LAUNCH: 9 skills, 17 rules, ECC-inspired. |
