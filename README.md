# Skill System v5.0 — TDD + Spec-Driven + Systematic Debug

> 15 Skills Production Code Framework + 5-Layer Memory
> Cập nhật: 2026-03-05

## Cài đặt

### Global (áp dụng tất cả projects):
```bash
# Clone repo
git clone git@github.com:tampd/skill.git ~/skill

# Symlink tất cả skills vào Antigravity global
for skill in start build fix save plan design guard integrate \
  memory brainstorm n8n-pro web-security review-website docs seo; do
  rm -rf ~/.gemini/antigravity/skills/$skill
  ln -s ~/skill/$skill ~/.gemini/antigravity/skills/$skill
done
```

## Có gì mới trong v5.0?

### 🔥 4 Core Skills nâng cấp (từ OpenSpec + Superpowers):

| Skill | Thay đổi v5.0 |
|---|---|
| **build** ⭐ | **TDD Iron Law**: RED→GREEN→REFACTOR. Viết code trước test? → XÓA. + Spec Compliance Check |
| **fix** | **4-Phase Systematic Debug**: Root Cause IRON LAW (không fix khi chưa tìm nguyên nhân gốc) |
| **plan** | **Change Folder** (proposal+specs+design+tasks+delta-specs) + Brainstorming Hard Gate |
| **save** | **2-Stage Review** (Spec Compliance + Code Quality 7 tiêu chí) + Change Archive + Delta Merge |

### 🧠 5-Layer Memory (v4.1+):

| Layer | Engine | Mục đích |
|---|---|---|
| 1 — Working | `ACTIVE_CONTEXT.md` | Working memory (xóa sau /save) |
| 2 — Semantic | `GEMINI.md`, `STATE.md` | Não bộ dự án |
| 3 — Episodic | `LESSONS.md`, `CHANGE_LOG.md` | Bộ nhớ dài hạn (append-only) |
| 4 — Vector | Qdrant (MCP) | Semantic search cross-session |
| 5 — Task Graph | Beads/Dolt (CLI) | Dependency-aware task tracking |

## Quick Reference — 15 Skills

```
── Session ──
/start [task]        → Khởi động phiên (15-rule auto-select + Qdrant recall)
/save                → 2-Stage Review + Change Archive + Push    🔥

── Build ──
/build [task]        → TDD: RED→GREEN→REFACTOR + Spec Check     🔥
/fix [bug]           → 4-Phase Systematic Debug + Root Cause     🔥
/plan [feature]      → Change Folder + Brainstorming Hard Gate   🔥

── Design ──
/design [task]       → UI/UX + Token System + Component API

── Quality ──
/guard [scope]       → Test + Security + Perf

── Integration ──
/integrate [svc]     → API + webhook
/n8n [task]          → N8N + MCP Server + Sub-workflows

── Analysis ──
/brainstorm [idea]   → Ideation → Spec → Prototype
/security [target]   → OWASP audit + CVE + hardening
/review-web [url]    → Website review toàn diện 7 chiều

── Content ──
/docs [scope]        → Documentation + ADR + handoff
/seo [topic]         → SEO + GEO writer

── Memory ──
/memory              → 5-Layer Memory management                 🆕
```

## Version History

| Version | Date | Skills | Highlights |
|---|---|---|---|
| **v5.0** 🔥 | 2026-03-05 | 15 skills | TDD Iron Law, 4-Phase Debug, Change Folder, 2-Stage Review |
| v4.1 | 2026-03-04 | 15 skills | +/memory, Qdrant Layer 4, Beads Layer 5 |
| v4.0 | 2026-03-04 | 14 skills | +/docs, +/seo, Architecture Spec Phase |
| v3.0 | 2026-03-04 | 12 skills | +brainstorm, +n8n-pro, +web-security, +review-website |
| v2.0 | 2026-02-28 | 8 skills | Gộp 26 → 8 unified skills |
| v1.x | 2026-02-27 | 26 skills | Original (archived) |
