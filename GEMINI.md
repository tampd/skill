# AG-SKILL Global Rules v2.0
> Applied to ALL sessions in this project. Read before every task.

## MEMORY FILES (read at session start)
- `LESSONS.md` — bài học từ quá khứ (append-only)
- `INSTINCTS.md` — patterns đã học + confidence score
- `STATE.md` — project state hiện tại
- `ACTIVE_CONTEXT.md` — working memory phiên này (xóa sau /save)

---

## 20 GLOBAL RULES

### Core Behavior
1. **ASK WHEN UNCLEAR** — Nếu yêu cầu mơ hồ hoặc có ≥2 cách hiểu → hỏi trước, đừng đoán
2. **READ MEMORY FIRST** — Đọc `LESSONS.md` + `INSTINCTS.md` trước bất kỳ task nào
3. **EVIDENCE-BASED** — Mọi quyết định kỹ thuật phải có lý do cụ thể, không phán đoán
4. **SELF-REASONING GATE** — Tự hỏi 3 câu trước khi implement: (a) Đây có phải cách đơn giản nhất? (b) Có test cho cái này không? (c) Bảo mật có ổn không?

### Architecture & Planning
5. **SPEC BEFORE CODE** — Task ≥3 files hoặc ≥2h effort → viết `/spec` trước, không code ngay
6. **PLAN-FIRST (Antigravity)** — Generate task list có checkpoint TRƯỚC khi bắt đầu code
7. **BRAINSTORM HARD GATE** — Feature phức tạp → `/plan` bắt buộc, không nhảy thẳng vào code
8. **ADR FOR DECISIONS** — Mọi architectural decision quan trọng → ghi `/adr` ngay lúc quyết định

### Code Quality
9. **TDD IRON LAW** — Test TRƯỚC code. Không có test = không commit. Coverage ≥80%
10. **SEARCH-FIRST** — Tìm package/solution có sẵn trước khi tự viết. Ưu tiên: official > popular > custom
11. **QUALITY GATE** — Trước mỗi commit: `lint → format → type-check → test → audit`. Tất cả PASS mới commit
12. **CLEANUP PASS** — Sau implement + verify → review riêng pass chỉ để tìm: dead code, TODO cũ, console.log, magic numbers

### Verification
13. **ROOT CAUSE FIRST** — Bug: tìm nguyên nhân gốc trước khi fix. Không patch symptom
14. **VERIFICATION LOOP** — Sau mỗi implementation: run lint→test→diff→regression check. Chỉ báo "done" khi tất cả pass
15. **INSTINCT VALIDATION** — Khi apply instinct → log kết quả. Đúng: +0.1. Sai: −0.2. Dưới 0.3 sau 5 lần → xem xét xóa

### Design & UX
16. **WCAG AA** — Mọi UI component phải đạt WCAG 2.2 AA. Color contrast, keyboard nav, aria-label
17. **DESIGN TOKEN MANDATORY** — Không dùng hardcoded color/spacing/font. Luôn dùng design tokens
18. **PERFORMANCE BUDGET** — LCP ≤2.5s, CLS ≤0.1, FID ≤100ms. Measure before/after feature

### Standards
19. **SECURITY HEADERS** — Mọi HTTP response phải có: CSP, HSTS, X-Frame-Options, X-Content-Type
20. **README SYNC** — Sau mỗi push: kiểm tra README có cần cập nhật không (API changes, new env vars, new commands)

---

## ANTIGRAVITY NATIVE FEATURES — Sử dụng tích cực

```
Browser Sub-Agent    → /craft, /fix UI bugs, /e2e verification
Multi-Agent Parallel → /build large features (split thành sub-tasks)
Mission Control      → Monitor parallel agents, assign /spec + /build + /secure cùng lúc
Artifact Output      → /spec, /handoff, /runbook output dạng structured Artifact
Planning Mode        → Luôn gen task-list có checkpoint trước khi code
Stitch MCP           → /craft setup → generate screens → edit screens → export code
```

---

## 🎯 9 SKILLS — QUICK REFERENCE

| # | Skill | Commands | Purpose |
|---|---|---|---|
| 1 | **session** | `/start` `/save` `/checkpoint` `/review` | 🔄 Lifecycle + Quality Gate + Multi-perspective Review |
| 2 | **build** ⭐ | `/build` `/plan` `/search` | 🔥 Search-First + TDD + Cleanup Pass |
| 3 | **fix** | `/fix` | 🐛 4-Phase Debug + Instinct Match + Common Patterns |
| 4 | **craft** | `/craft` `/audit` `/tokens` `/e2e` | 🎨 UI + Design Tokens + WCAG + Browser Testing |
| 5 | **secure** | `/security` `/harden` `/ship` | 🔒 OWASP + Hardening + Production Readiness |
| 6 | **automate** | `/n8n` `/integrate` `/mcp` | ⚡ N8N + API Design + MCP Tool Design |
| 7 | **content** | `/seo` `/article` `/brief` `/audit-content` | 📝 SEO Pipeline + Content Engine |
| 8 | **spec** ⭐ | `/spec` `/handoff` `/adr` `/runbook` `/apidoc` `/componentdoc` | 📋 Ultra Architecture + Handoff + API/Component Docs |
| 9 | **learn** | `/learn` `/instinct` `/evolve` `/review-instincts` | 🧠 Continuous Learning + Instinct Evolution |

---

## 🧠 MEMORY ARCHITECTURE (5 Layers)

| Layer | Engine/File | Purpose |
|---|---|---|
| **Working** | `ACTIVE_CONTEXT.md` | Session memory (xóa sau /save) |
| **Semantic** | `GEMINI.md`, `STATE.md` | Project brain |
| **Episodic** | `LESSONS.md`, `CHANGELOG.md` | Long-term (append-only) |
| **Instinct** | `INSTINCTS.md` | Learned patterns + confidence |
| **Vector** | Qdrant MCP (optional) | Semantic search cross-session |

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
| Học hỏi, patterns, instincts | `learn` |

---

## 📋 CHEAT SHEET

```
Bắt đầu phiên?           → /start [task]
Ý tưởng mơ hồ?           → /plan [idea]
Feature phức tạp?         → /plan [feature]
Research trước code?      → /search [need]
Viết code?                → /build [task]           🔥 TDD + Search-First
Gặp bug?                  → /fix [bug]              🐛 4-Phase Debug
Design UI?                → /craft [task]           🎨 Tokens + WCAG
Design tokens?            → /tokens [scope]
Browser test?             → /e2e [scope]
Audit chất lượng?         → /audit [scope]
Audit bảo mật?            → /security [target]      🔒 OWASP
Hardening?                → /harden [service]
Chuẩn bị deploy?          → /ship [env]             🔒 Production Readiness
API / webhook?            → /integrate [service]     ⚡
N8N workflow?             → /n8n [task]              ⚡
Thiết kế MCP tool?        → /mcp [tool-name]
Content brief?            → /brief [topic]           📝
Viết bài SEO?             → /seo [topic]             📝
Viết article?             → /article [topic]         📝
Audit content?            → /audit-content [URL]
Architecture docs?        → /spec [scope]            📋 15 sections
Bàn giao dự án?           → /handoff [project]       📋 15 sections
API documentation?        → /apidoc [endpoint]
Component docs?           → /componentdoc [scope]
Ghi ADR?                  → /adr [decision]          📋
Tạo runbook?              → /runbook [service]       📋
Ghi bài học?              → /learn [observation]     🧠
Xem instincts?            → /instinct
Evolve instincts?         → /evolve                  🧠
Review instincts?         → /review-instincts
Review code?              → /review [scope]
Lưu context?              → /checkpoint
Kết thúc phiên?           → /save
```

---

## DOCUMENTATION — ZERO OVERLAP

| File | Viết gì | KHÔNG viết gì |
|---|---|---|
| `GEMINI.md` | Tech stack, Rules, Version | Tasks, logs, state |
| `LESSONS.md` | Bug + bài học + ✅❌ code | Features, changelog |
| `INSTINCTS.md` | Learned patterns + confidence | Bug reports |
| `STATE.md` | Current project state | History |
| `ACTIVE_CONTEXT.md` | Working memory phiên hiện tại | Persistent info |
| `CHANGELOG.md` | Timeline thay đổi theo ngày | Tasks, lessons |

---

## CHANGELOG

| Date | Change |
|---|---|
| **2026-03-10** | **v2.0 FINAL**: Merge v1.0 + v2 draft + research. 20 rules, 9 skills with 32 commands, 30+ reference files. New: /mcp, /tokens, /e2e, /harden, /brief, /audit-content, /apidoc, /componentdoc, /evolve, /review-instincts. Context management, platform design rules. |
| 2026-03-09 | v1.0 LAUNCH: 9 skills, 17 rules, ECC-inspired. |
