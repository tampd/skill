# AG-SKILL — Antigravity Skill System v2.0

> **The best of [everything-claude-code](https://github.com/affaan-m/everything-claude-code) + Skill v7.0 + 12 top repos, optimized for Google Antigravity.**

```
"FEWER SKILLS. DEEPER MODES. SMARTER LEARNING. ZERO COMPROMISE."
```

## ✨ What's New (v2.0)

- **32 commands** across 9 skills (up from 22)
- **20 Global Rules** grouped by category (Core, Architecture, Code Quality, Verification, Design, Standards)
- **Context Engineering** — strategic loading, compression, optimization
- **Platform Design Rules** — 300+ design values (spacing, typography, color, animation)
- **MCP Tool Design** — `/mcp` mode for building N8N + Antigravity tools
- **Content Pipeline** — `/brief` → `/article` → `/audit-content` complete workflow
- **Component Docs** — `/apidoc` + `/componentdoc` specialized documentation
- **Instinct Evolution** — `/evolve` cluster instincts → skill sections
- **Browser Testing** — `/e2e` Antigravity browser sub-agent verification
- **Security Hardening** — `/harden` separate from audit
- **Bug Classification** — CRITICAL/HIGH/MEDIUM/LOW with action guide
- **Common Debug Patterns** — React/Node/N8N/WordPress quick diagnosis

## 📦 9 Skills — 32 Commands

| # | Skill | Commands | Purpose |
|---|---|---|---|
| 1 | **session** | `/start` `/save` `/checkpoint` `/review` | 🔄 Lifecycle + Quality Gate + Multi-perspective Review |
| 2 | **build** ⭐ | `/build` `/plan` `/search` | 🔥 Search-First + TDD + Cleanup Pass |
| 3 | **fix** | `/fix` | 🐛 4-Phase Debug + Instinct Matching |
| 4 | **craft** | `/craft` `/audit` `/tokens` `/e2e` | 🎨 UI + Design Tokens + WCAG + Browser Testing |
| 5 | **secure** | `/security` `/harden` `/ship` | 🔒 OWASP + Hardening + Production Readiness |
| 6 | **automate** | `/n8n` `/integrate` `/mcp` | ⚡ N8N + API Design + MCP Tool Design |
| 7 | **content** | `/seo` `/article` `/brief` `/audit-content` | 📝 SEO Pipeline + Content Engine |
| 8 | **spec** ⭐ | `/spec` `/handoff` `/adr` `/runbook` `/apidoc` `/componentdoc` | 📋 Ultra Docs (15+15+7 sections) |
| 9 | **learn** | `/learn` `/instinct` `/evolve` `/review-instincts` | 🧠 Continuous Learning + Instincts |

## 🚀 Install

```bash
# Clone
git clone git@github.com:tampd/AG-SKILL.git /root/skill/ag-skill

# Symlink vào Antigravity skills directory
for skill in session build fix craft secure automate content spec learn; do
  ln -sfn /root/skill/ag-skill/$skill /root/.gemini/antigravity/skills/$skill
done

# Verify
ls /root/.gemini/antigravity/skills/
```

## 📋 Cheat Sheet

```
Bắt đầu phiên?           → /start [task]
Ý tưởng mơ hồ?           → /plan [idea]
Feature phức tạp?         → /plan [feature]
Research trước code?      → /search [need]
Viết code?                → /build [task]           🔥 TDD + Search-First
Gặp bug?                  → /fix [bug]              🐛 4-Phase + Instinct
Design UI?                → /craft [task]           🎨 Tokens + WCAG
Design tokens?            → /tokens [scope]
Browser test?             → /e2e [scope]
Audit chất lượng?         → /audit [scope]
Audit bảo mật?            → /security [target]      🔒 OWASP
Hardening?                → /harden [service]
Chuẩn bị deploy?          → /ship [env]             🔒 Production Ready
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

## 🧠 Memory Architecture (5 Layers)

| Layer | Engine | Purpose |
|---|---|---|
| Working | `ACTIVE_CONTEXT.md` | Session memory (deleted after /save) |
| Semantic | `GEMINI.md`, `STATE.md` | Project brain |
| Episodic | `LESSONS.md`, `CHANGELOG.md` | Long-term (append-only) |
| Instinct | `INSTINCTS.md` | Learned patterns + confidence |
| Vector | Qdrant MCP (optional) | Semantic search |

## 📜 20 Global Rules

**Core**: Ask when unclear · Read memory first · Evidence-based · Self-reasoning gate
**Architecture**: Spec before code · Plan-first · Brainstorm gate · ADR for decisions
**Code Quality**: TDD Iron Law · Search-first · Quality gate · Cleanup pass
**Verification**: Root cause first · Verification loop · Instinct validation
**Design**: WCAG AA · Design tokens · Performance budget
**Standards**: Security headers · README sync

## 📝 Inspiration

- [everything-claude-code](https://github.com/affaan-m/everything-claude-code) — Search-First, De-sloppify, Instincts
- [awesome-agent-skills](https://github.com/VoltAgent/awesome-agent-skills) — 500+ cross-platform skills
- [context-engineering-kit](https://github.com/NeoLabHQ/context-engineering-kit) — Context management
- [compound-engineering-plugin](https://github.com/EveryInc/compound-engineering-plugin) — Learning loop
- [platform-design-skills](https://github.com/ehmo/platform-design-skills) — 300+ design rules
- Skill System v7.0 — Zero Overlap, Self-Reasoning Gate, 5-Layer Memory

## 📄 License

MIT
