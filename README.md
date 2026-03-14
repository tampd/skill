# APEX Skill System v5.0

> 🧠 **AI không bao giờ quên.** Mỗi phiên đều học hỏi và kết nối với quá khứ.

**APEX**: Adaptive Pattern EXtraction — hệ thống skill cho AI coding assistant.

## ✨ What's New (v5.0)

- **Reflexion Loop** — Self-review sau mỗi /build commit và /fix verification. AI tự hỏi: "Solve đúng problem?", "Edge case bỏ sót?", "Fix root cause hay symptom?"
- **Context Health Monitor v2** — Nâng từ 1 chiều (Usage %) lên 3 chiều: Usage + Relevance (phát hiện context "loãng") + Freshness (phát hiện stale references)
- **Research-based**: [NeoLabHQ/context-engineering-kit](https://github.com/NeoLabHQ/context-engineering-kit), [muratcankoylan/Agent-Skills-for-Context-Engineering](https://github.com/muratcankoylan/Agent-Skills-for-Context-Engineering)

### Previous versions
- **v4.2**: Biomimetic Memory (world/experience/mental_model), 3-Strategy Recall — [vectorize-io/hindsight](https://github.com/vectorize-io/hindsight)
- **v4.1**: Verification Gate, Subagent Orchestration, Anti-Rationalization — [obra/superpowers](https://github.com/obra/superpowers) (6.9k⭐)
- **v4.0**: Progressive Disclosure, /gsd, Ultrathink, Context Health — [GSD2](https://github.com/jlowin/gsd2) (23k⭐)

## 📦 9 Skills — 35+ Commands

| # | Skill | Commands | Purpose |
|---|---|---|---|
| 1 | **session** | `/start` `/save` `/checkpoint` `/review` `/recall` | 4-Layer Bootstrap + Verification Gate |
| 2 | **build** ⭐ | `/build` `/plan` `/search` `/gsd` | TDD + Search-First + Reflexion Loop |
| 3 | **fix** | `/fix` | 4-Phase Debug + Reflexion + Anti-Rationalization |
| 4 | **craft** | `/craft` `/audit` `/tokens` `/e2e` | UI + Design Tokens + WCAG + Browser Testing |
| 5 | **secure** | `/security` `/harden` `/ship` | OWASP + Hardening + Production |
| 6 | **automate** | `/n8n` `/integrate` `/mcp` | N8N + API + MCP Tool Design |
| 7 | **content** | `/seo` `/article` `/brief` `/audit-content` | SEO Pipeline + Content Engine |
| 8 | **spec** ⭐ | `/spec` `/handoff` `/adr` `/runbook` `/apidoc` `/componentdoc` | Architecture + Docs |
| 9 | **learn** ⭐ | `/learn` `/instinct` `/evolve` `/review-instincts` `/consolidate` `/cross-link` | Ingest + Evolution |

## 🚀 Install

```bash
# Clone
git clone git@github.com:tampd/skill.git ~/.gemini/antigravity/skills-repo

# Symlink toàn bộ skills vào Antigravity
for skill in session build fix craft secure automate content spec learn; do
  ln -sfn ~/.gemini/antigravity/skills-repo/$skill ~/.gemini/antigravity/skills/$skill
done

# Verify
ls ~/.gemini/antigravity/skills/
```

## 📋 26 Global Rules

### Core Behavior (1-5)
1. **ASK WHEN UNCLEAR** — ≥2 cách hiểu → hỏi trước
2. **READ MEMORY FIRST** — LESSONS + INSTINCTS + INSIGHTS
3. **EVIDENCE-BASED** — Mọi quyết định phải có lý do
4. **SELF-REASONING GATE** — 3 câu hỏi trước khi implement
5. **SEARCH-FIRST** — Tìm solution có sẵn trước khi tự viết

### Architecture (6-10)
6. **SPEC BEFORE CODE** — ≥3 files → viết spec trước
7. **PLAN-FIRST** — Task list có checkpoints
8. **BRAINSTORM HARD GATE** — ≥2 approaches bắt buộc
9. **ADR FOR DECISIONS** — Ghi lại quyết định lớn
10. **WAVE EXECUTION** — Foundation → Integration → Polish

### Code Quality (11-14)
11. **TDD IRON LAW** — Test trước code. RED → GREEN → REFACTOR
12. **QUALITY GATE** — lint → format → type-check → test → audit
13. **CLEANUP PASS** — Dead code, TODO, console.log
14. **ATOMIC COMMITS** — `feat|fix|chore(scope): desc`

### Verification (15-17)
15. **ROOT CAUSE FIRST** — Không patch symptoms
16. **VERIFICATION LOOP** — Verify → regression check
17. **INSTINCT VALIDATION** — Correct: +0.1 | Wrong: -0.2

### Design (18-20)
18. **WCAG AA** — Color contrast, keyboard nav, ARIA
19. **DESIGN TOKENS** — Không hardcode colors/fonts/spacing
20. **PERFORMANCE BUDGET** — LCP ≤2.5s, CLS ≤0.1, FID ≤100ms

### Fresh Context (21-23)
21. **ULTRATHINK GATE** — Complex tasks → full analysis plan ≥500 words
22. **CONTEXT HEALTH v2** — 3 chiều: Usage 🟢🟡🔴💀 + Relevance + Freshness
23. **GSD CYCLE** — Discuss → Plan → Execute → Verify

### Subagent & Verification (24-26)
24. **VERIFICATION GATE** — Evidence before claims. Anti-rationalization table
25. **SUBAGENT ORCHESTRATION** — 2-stage review + model selection + status protocol
26. **BIOMIMETIC MEMORY** — Memory types: world | experience | mental_model

## 🧠 Memory Architecture (4-Layer)

```
┌─────────────────────────────────────────────────────────────┐
│  LAYER A — RULES (tĩnh, luôn đọc)                           │
│  GEMINI.md + STATE.md                                        │
├─────────────────────────────────────────────────────────────┤
│  LAYER B — CRITICAL LESSONS (≤10 entries, ≥0.8)             │
│  LESSONS.md (critical) + LESSONS_ARCHIVE.md (searchable)     │
├─────────────────────────────────────────────────────────────┤
│  LAYER C — SEMANTIC (Qdrant Vector DB + 3-Strategy Recall)  │
│  semantic + keyword + temporal — cross-project search        │
├─────────────────────────────────────────────────────────────┤
│  LAYER D — AUTO-MEMORY (AI tự ghi)                          │
│  .ai/memory/MEMORY.md (≤200 dòng)                           │
└─────────────────────────────────────────────────────────────┘
```

## 📋 Cheat Sheet

```
Bắt đầu phiên?           → /start [task]
Feature lớn?              → /gsd [feature]          (Discuss→Plan→Execute→Verify)
Viết code?                → /build [task]           🔥 TDD + Reflexion
Gặp bug?                  → /fix [bug]              🐛 4-Phase + Reflexion
Design UI?                → /craft [task]           🎨 Tokens + WCAG + /e2e
Chuẩn bị deploy?          → /ship [env]             🔒 Production Readiness
Ghi bài học?              → /learn [observation]     🧠 Ingest + Tag
Tổng hợp insights?       → /consolidate             🧠 Sleep-Brain Pass
Kết thúc phiên?           → /save                    (Verification Gate + /consolidate)
```

## 📝 Architecture & Sources

- [NeoLabHQ/context-engineering-kit](https://github.com/NeoLabHQ/context-engineering-kit) — Reflexion loops, context degradation (v5.0)
- [muratcankoylan/Agent-Skills-for-Context-Engineering](https://github.com/muratcankoylan/Agent-Skills-for-Context-Engineering) — Context engineering skills (v5.0)
- [vectorize-io/hindsight](https://github.com/vectorize-io/hindsight) — Biomimetic memory (v4.2)
- [obra/superpowers](https://github.com/obra/superpowers) — Verification gate, anti-rationalization (v4.1)
- [GSD2](https://github.com/jlowin/gsd2) — Get Shit Done cycle (v4.0)
- Anthropic SKILL.md patterns, Claude Code docs (v4.0)
- Claude Code MEMORY.md, Cursor context layering, ReMe, MemOS (v3.0)
- Google ADK always-on-memory-agent (v2.0)
- ECC (Extreme Claude Code) methodology (v1.0)

## 📂 Directory Structure

```
skill/
├── GEMINI.md           # 26 Global Rules + Memory Architecture
├── AGENTS.md           # Agent instructions
├── LESSONS.md          # Critical lessons (≤10, ≥0.8)
├── CHANGE_LOG.md       # Timeline changes
├── README.md           # This file
├── session/            # /start /save /checkpoint /review /recall
│   ├── SKILL.md
│   └── references/     # context-management, quality-gates, trigger-keywords
├── build/              # /build /plan /search /gsd
│   ├── SKILL.md        # 8-Step Build + Reflexion + Context Health v2
│   └── references/     # patterns, tdd, parallel-guide, subagent-prompts ⭐
├── fix/                # /fix
│   ├── SKILL.md        # 4-Phase Debug + Reflexion
│   └── references/     # patterns, lessons-template
├── craft/              # /craft /audit /tokens /e2e
│   ├── SKILL.md
│   └── references/     # patterns, design-tokens, wcag, perf-targets
├── secure/             # /security /harden /ship
│   ├── SKILL.md
│   └── references/     # owasp, security-headers, ci-templates
├── automate/           # /n8n /integrate /mcp
│   ├── SKILL.md
│   └── references/     # api-design, n8n-patterns
├── content/            # /seo /article /brief /audit-content
│   ├── SKILL.md
│   └── references/     # seo-checklist, content-engine
├── spec/               # /spec /handoff /adr /runbook /apidoc /componentdoc
│   ├── SKILL.md
│   └── references/     # templates (adr, api, architecture, handoff, etc.)
├── learn/              # /learn /instinct /evolve /consolidate /cross-link
│   ├── SKILL.md
│   └── references/     # instinct-format, review-perspectives
└── templates/          # File templates (ACTIVE_CONTEXT, INSIGHTS, etc.)
```

## 📄 License

MIT
