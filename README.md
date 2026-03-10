# AG-SKILL — APEX Skill System v3.0

> 🧠 **AI không bao giờ quên.** Mỗi phiên đều học hỏi và kết nối với quá khứ.

**APEX**: Adaptive Pattern EXtraction — hệ thống skill tối ưu cho AI coding assistant.

## ✨ What's New (APEX v3.0)

- **4-Layer Smart Retrieval** — Không đọc hết, chỉ đọc đúng. Tối ưu context window.
  - **Layer A (Rules)**: `GEMINI.md` + `STATE.md` — luôn đọc
  - **Layer B (Critical Lessons)**: `LESSONS.md` ≤10 entries, importance ≥0.8
  - **Layer C (Semantic)**: Qdrant Vector DB — semantic search cross-project
  - **Layer D (Auto-Memory)**: `.ai/memory/MEMORY.md` — AI tự ghi (200-line rule)
- **LESSONS_ARCHIVE.md** — entries importance < 0.8 (Qdrant searchable)
- **Smart `/start` Load** — chỉ load ~15-20 entries thay vì tất cả
- **Qdrant integration** — `/start`, `/build`, `/fix`, `/save` đều dùng semantic search
- Synthesized from: Claude Code MEMORY.md, Cursor context layering, ReMe, MemOS patterns

## 📦 9 Skills — 35 Commands

| # | Skill | Commands | Purpose |
|---|---|---|---|
| 1 | **session** | `/start` `/save` `/checkpoint` `/review` `/recall` | 4-Layer Bootstrap + Consolidation Save |
| 2 | **build** ⭐ | `/build` `/plan` `/search` | Search-First + TDD + Memory-Enhanced |
| 3 | **fix** | `/fix` | 4-Phase Debug + Qdrant Match + Auto-Ingest |
| 4 | **craft** | `/craft` `/audit` `/tokens` `/e2e` | UI + Design Tokens + WCAG + Browser |
| 5 | **secure** | `/security` `/harden` `/ship` | OWASP + Hardening + Production |
| 6 | **automate** | `/n8n` `/integrate` `/mcp` | N8N + API + MCP Tool Design |
| 7 | **content** | `/seo` `/article` `/brief` `/audit-content` | SEO Pipeline + Content Engine |
| 8 | **spec** ⭐ | `/spec` `/handoff` `/adr` `/runbook` `/apidoc` `/componentdoc` | Architecture + Docs |
| 9 | **learn** ⭐ | `/learn` `/instinct` `/evolve` `/review-instincts` `/consolidate` `/cross-link` | Ingest + Consolidation + Evolution |

## 🚀 Install

```bash
# Clone
git clone git@github.com:tampd/skill.git /root/skill

# Symlink vào Antigravity skills directory
for skill in session build fix craft secure automate content spec learn; do
  ln -sfn /root/skill/$skill /root/.gemini/antigravity/skills/$skill
done

# Verify
ls /root/.gemini/antigravity/skills/
```

## 📋 Cheat Sheet

```
Bắt đầu phiên?           → /start [task]
Resume phiên cũ?          → /recall
Viết code?                → /build [task]           🔥 TDD + Search-First
Gặp bug?                  → /fix [bug]              🐛 4-Phase + Qdrant Match
Design UI?                → /craft [task]           🎨 Tokens + WCAG
Chuẩn bị deploy?          → /ship [env]             🔒 Production Readiness
Ghi bài học?              → /learn [observation]     🧠 Ingest + Tag
Tổng hợp insights?       → /consolidate             🧠 Sleep-Brain Pass
Kết thúc phiên?           → /save                    (auto /consolidate)
```

## 🧠 Memory Architecture v3.0 (4-Layer Smart Retrieval)

```
┌─────────────────────────────────────────────────────────────┐
│  LAYER A — RULES (tĩnh, user kiểm soát, luôn đọc)          │
│  Files: GEMINI.md, STATE.md                                  │
│  Nội dung: Tech stack, conventions, rules, project state     │
├─────────────────────────────────────────────────────────────┤
│  LAYER B — CRITICAL LESSONS (≤10 entries, importance ≥0.8)  │
│  File: LESSONS.md (critical-only, luôn đọc)                  │
│  Archive: LESSONS_ARCHIVE.md (Qdrant searchable)             │
├─────────────────────────────────────────────────────────────┤
│  LAYER C — SEMANTIC (Qdrant Vector DB)                      │
│  Engine: qdrant_find / qdrant_store (MCP)                    │
│  Cross-project, scale vô hạn, semantic search               │
├─────────────────────────────────────────────────────────────┤
│  LAYER D — AUTO-MEMORY (AI tự ghi, local)                  │
│  File: .ai/memory/MEMORY.md (≤200 dòng)                     │
│  Nội dung: Debugging insights, gotchas, project-specific     │
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

## 📜 20 Global Rules

| # | Category | Rule |
|---|---|---|
| 1 | Core | Ask when unclear |
| 2 | Core | Read memory first (LESSONS + INSTINCTS + INSIGHTS) |
| 3 | Core | Evidence-based decisions |
| 4 | Core | Self-Reasoning Gate (3-question check) |
| 5 | Core | Search-first |
| 6 | Arch | Spec before code |
| 7 | Arch | Plan-first with task list |
| 8 | Arch | Brainstorm hard gate (≥2 approaches) |
| 9 | Arch | ADR for big decisions |
| 10 | Arch | Wave execution |
| 11 | Code | TDD Iron Law |
| 12 | Code | Quality gate before commit |
| 13 | Code | Cleanup pass |
| 14 | Code | Atomic commits |
| 15 | Verify | Root cause first |
| 16 | Verify | Verification loop |
| 17 | Verify | Instinct validation |
| 18 | Design | WCAG AA |
| 19 | Design | Design tokens mandatory |
| 20 | Design | Performance budget |

## 📝 Sources

- Claude Code MEMORY.md pattern (context layering)
- Cursor context window optimization
- ReMe (Retrieval-Enhanced Memory) research
- MemOS (Memory Operating System) architecture
- Google ADK `always-on-memory-agent` (Ingest → Consolidate → Cross-Link → Query)
- AG-SKILL v2.0 (9 Skills, 32 Commands, 20 Rules)
- Research: awesome-claude-code, awesome-agent-skills + 10 more GitHub repos
- ECC (Extreme Claude Code) methodology

## 📄 License

MIT
