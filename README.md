# Skill System v4 — Structured Vibe

> 14 Skills Production Code Framework + Documentation + SEO
> Cập nhật: 2026-03-04

## Cài đặt

### Global (áp dụng tất cả projects):
```bash
# Clone repo
git clone git@github.com:tampd/skill.git ~/skill

# Symlink vào Antigravity global skills
ln -sf ~/skill ~/.gemini/antigravity/skills/skill-system
```

### Per-project:
```bash
# Copy skills vào project
cp -r ~/skill/.agent/skills/ .agent/skills/
```

## Có gì mới trong v4?

### 2 Skills mới:
| Skill | Lệnh | Mục đích |
|---|---|---|
| **docs** 🆕 | `/docs [scope]` | Documentation generation + ADR + handoff (9 template files) |
| **seo** 🆕 | `/seo [topic]` | SEO + GEO content writer (keyword → draft → optimize → publish) |

### Nâng cấp skills hiện có:
| Skill | Thay đổi |
|---|---|
| **build** ⭐ | Thêm Architecture Spec Phase — `.spec.md` BẮT BUỘC cho project mới |
| **design** | Thêm Design Token System (Atomic → Semantic → Mode) + Component API Docs |
| **n8n-pro** | Thêm MCP Server + Sub-workflows + Scaling Patterns |
| **start** | Auto-select nâng từ 10 → 14 rules + fallback logic |

### MCP Integration:
- n8n MCP Server cho workflow-as-tool
- Qdrant MCP cho persistent memory
- Context7/Docfork cho documentation grounding

## Quick Reference — 14 Skills
```
── Session ──
/start [task]        → Khởi động phiên (14-rule auto-select)
/save                → Review + push

── Build ──
/build [task]        → Code + Architecture Spec      ⭐
/fix [bug]           → Debug + LESSONS
/plan [feature]      → Blueprint

── Design ──
/design [task]       → UI/UX + Token System          ⬆

── Quality ──
/guard [scope]       → Test + Security + Perf

── Integration ──
/integrate [svc]     → API + webhook
/n8n [task]          → N8N + MCP Server               ⬆

── Analysis ──
/brainstorm [idea]   → Ideation
/security [target]   → OWASP audit
/review-web [url]    → Website review

── Content ──
/docs [scope]        → Documentation + ADR            🆕
/seo [topic]         → SEO + GEO writer               🆕
```

## Version History
| Version | Date | Skills |
|---|---|---|
| **v4.0** | 2026-03-04 | 14 skills (+/docs, +/seo, ⬆build, ⬆design, ⬆n8n, ⬆start) |
| v3.0 | 2026-03-04 | 12 skills (+brainstorm, +n8n-pro, +web-security, +review-website) |
| v2.0 | 2026-02-28 | 8 unified skills |
| v1.x | 2026-02-27 | 26 skills (archived) |
