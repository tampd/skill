# Skill System v7.0 — Streamlined + Progressive Disclosure

> 8 Skills Production Code Framework + 5-Layer Memory + Progressive Disclosure
> Cập nhật: 2026-03-09

## Cài đặt

### Global (áp dụng tất cả projects):
```bash
# Clone repo
git clone git@github.com:tampd/skill.git ~/skill

# Symlink 8 skills vào Antigravity global
for skill in session build fix craft secure automate content spec; do
  rm -rf ~/.gemini/antigravity/skills/$skill
  ln -s ~/skill/$skill ~/.gemini/antigravity/skills/$skill
done
```

## Có gì mới?

### v7.0 — Streamlined + Progressive Disclosure 🆕

- **14 → 8 skills**: Giảm 43% số skills, loại bỏ 100% overlap
- **Progressive Disclosure**: YAML frontmatter → SKILL.md body → `references/`
- **~3200 → 993 lines SKILL.md**: Giảm 69% context token
- **YAML frontmatter chuẩn**: `name` + `description` (trigger phrases) + `metadata`
- **Composable modes**: Mỗi skill có modes thay vì tách riêng

#### Bảng gộp skills:

| Skill mới | Gộp từ | Lệnh |
|---|---|---|
| **session** | start + save + memory | `/start` `/save` `/checkpoint` |
| **build** ⭐ | build + plan | `/build` `/plan` |
| **fix** | fix (tinh gọn) | `/fix` |
| **craft** | craft + quality | `/craft` `/quality` |
| **secure** | web-security + ship | `/security` `/ship` |
| **automate** | n8n-pro + integrate | `/n8n` `/integrate` |
| **content** | seo + docs (user-facing) | `/seo` `/docs user` |
| **spec** | docs (technical) + templates | `/spec` `/docs init` `/docs adr` |

### v6.x — Memory-First + Self-Reasoning Gate

- **v6.2**: Rule 14 — BKNS Repo Standard + 8 templates
- **v6.1**: Rule 13 — 3-Question Self-Check (Self-Reasoning Gate)
- **v6.0**: 18→14 skills, +craft/quality/ship, Context Compression, 3-Tier Load

### v5.0 — TDD + Spec-Driven

- TDD Iron Law (RED→GREEN→REFACTOR)
- 4-Phase Systematic Debug
- Change Folder (SPEC + tasks + delta-specs)
- 2-Stage Review (Spec + Quality 7 tiêu chí)

## 🧠 5-Layer Memory

| Layer | Engine | Mục đích |
|---|---|---|
| 1 — Working | `ACTIVE_CONTEXT.md` | Working memory (xóa sau /save) |
| 2 — Semantic | `GEMINI.md`, `STATE.md` | Não bộ dự án |
| 3 — Episodic | `LESSONS.md`, `CHANGE_LOG.md` | Bộ nhớ dài hạn (append-only) |
| 4 — Vector | Qdrant (MCP) | Semantic search cross-session |
| 5 — Task Graph | Beads (CLI) | Dependency-aware task tracking |

## Quick Reference — 8 Skills

```
── Lifecycle ──
/start [task]        → Mở phiên (context load + skill auto-select)
/checkpoint          → Lưu giữa phiên
/save                → 2-Stage Review + Commit + Push                  🔥

── Development ──
/build [task]        → TDD: RED→GREEN→REFACTOR + Self-Reasoning        🔥
/plan [feature]      → Ideation + SPEC + Change Folder                 🔥
/fix [bug]           → 4-Phase Systematic Debug + Root Cause            🔥

── Frontend ──
/craft [task]        → UI/UX + Atomic Design + Tokens + WCAG           🎨
/craft audit         → A11y + Performance verification                  🎨

── Security & Deploy ──
/security [target]   → OWASP deep audit + CVE + hardening              🔒
/ship check          → Pre-launch checklist                             🔒
/ship ci             → CI/CD pipeline (GitHub Actions)                  🔒
/ship monitor        → Production monitoring + rollback                 🔒

── Automation ──
/integrate [svc]     → API + webhook + 3rd-party                       ⚡
/n8n [task]          → N8N workflow + MCP Server                        ⚡

── Content ──
/seo [topic]         → SEO + GEO content writer                        📝
/docs user           → User guide + onboarding handoff                  📝

── Technical Docs ──
/spec [scope]        → Architecture + API reference + Data flow         📋
/docs init           → Khởi tạo repo BKNS chuẩn                        📋
/docs adr            → Architecture Decision Records                    📋
```

## Cấu trúc thư mục

```
/root/skill/
├── GEMINI.md              ← Brain & rules
├── README.md              ← File này
│
├── session/               ├── SKILL.md + references/
├── build/                 ├── SKILL.md + references/
├── fix/                   ├── SKILL.md + references/
├── craft/                 ├── SKILL.md + references/
├── secure/                ├── SKILL.md + references/
├── automate/              ├── SKILL.md + references/
├── content/               ├── SKILL.md + references/
├── spec/                  ├── SKILL.md + references/
│
├── templates/             ← BKNS repo templates
├── qdrant-memory/         ← Qdrant Layer 4 setup
└── archive/               ← Skills cũ v1+v2
```

## Version History

| Version | Date | Skills | Highlights |
|---|---|---|---|
| **v7.0** 🆕 | 2026-03-09 | 8 skills | **Streamlined** — 14→8 skills, Progressive Disclosure, -69% token |
| v6.2 | 2026-03-06 | 14 skills | BKNS Repo Standard, +8 templates |
| v6.1 | 2026-03-06 | 14 skills | Self-Reasoning Gate — 3-Question Self-Check |
| v6.0 | 2026-03-05 | 14 skills | Memory-First, 18→14 skills, craft/quality/ship |
| v5.0 | 2026-03-05 | 15 skills | TDD Iron Law, 4-Phase Debug, Change Folder |
| v4.x | 2026-03-04 | 14-15 | +memory, Qdrant, Beads, +docs, +seo |
| v3.0 | 2026-03-04 | 12 skills | +brainstorm, +n8n-pro, +web-security |
| v2.0 | 2026-02-28 | 8 skills | Gộp 26 → 8 unified skills |
| v1.x | 2026-02-27 | 26 skills | Original (archived) |
