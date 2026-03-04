# Skill System v3 — Extended Arsenal

> Production Code Framework + 4 New Expert Skills
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

## Có gì mới trong v3?

### 4 Skills mới:
| Skill | Lệnh | Mục đích |
|---|---|---|
| **n8n-pro** | `/n8n [task]` | N8N expert: 7 chuyên môn + MCP + CVE patches |
| **brainstorm** | `/brainstorm [idea]` | Ideation → Spec → Prototype pipeline |
| **web-security** | `/security [target]` | OWASP Top 10 deep audit + hardening |
| **review-website** | `/review-web [url]` | Website review 7 chiều (100 điểm) |

### Nâng cấp skills hiện có:
- **start**: Nhận diện 12 skills (thêm 4 skills mới vào auto-select)
- **guard**: Thêm Supply Chain Security audit

### MCP Integration:
- n8n-MCP cho workflow automation
- Qdrant MCP cho persistent memory
- Context7/Docfork cho documentation grounding

## Quick Reference
```
/start [task]        → Khởi động phiên
/build [task]        → Viết code
/fix [bug]           → Debug + LESSONS
/save                → Review + push
/plan [feature]      → Blueprint
/design [task]       → UI/UX
/guard [scope]       → Test + Security + Perf
/integrate [svc]     → API + webhook
/n8n [task]          → N8N workflow     🆕
/brainstorm [idea]   → Ideation         🆕
/security [target]   → Security audit   🆕
/review-web [url]    → Website review   🆕
```
