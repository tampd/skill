# GEMINI — Skill Repository Documentation

> **GitHub**: https://github.com/tampd/skill
> **Local path**: /root/skill
> **Cập nhật**: 2026-02-27

---

## 🔑 THÔNG TIN QUAN TRỌNG (ĐỌC TRƯỚC KHI LÀM GÌ)

### GitHub Repository
```
URL:    https://github.com/tampd/skill
Remote: origin → https://github.com/tampd/skill.git
Branch: main
```

> ⚠️ **Mọi thay đổi về skill files phải được push lên GitHub này sau khi hoàn thành.**

### Quy tắc push skill:
```bash
cd /root/skill
git add .
git commit -m "feat(skill): mô tả thay đổi"
git push origin main
```

---

## 📁 CẤU TRÚC SKILL REPOSITORY

```
/root/skill/
├── GEMINI.md              ← File này — rules & context
├── README.md              ← Hướng dẫn sử dụng skill system
├── skill_memory.md        ← Báo cáo kiến trúc memory optimization
│
├── memory-optimizer/      ← Skill thứ 26 (MỚI)
│   └── SKILL.md           → /checkpoint, /recall, /memory
│
├── start/SKILL.md         ← Session management
├── save/SKILL.md
├── review/SKILL.md
├── supper/SKILL.md
├── vibe-code/SKILL.md     ← Core coding skill
├── gsd/SKILL.md
├── bug-memory-journal/    ← Memory & lessons
│   └── SKILL.md
│
└── [22 skill khác]/       ← Code, Design, DevOps skills
```

---

## 🧠 MEMORY SYSTEM

Skill repository này sử dụng **3-Layer Memory Architecture**:

- **Layer 1 (Working)**: `ACTIVE_CONTEXT.md` trong mỗi project
- **Layer 2 (Semantic)**: `GEMINI.md + STATE.md` trong mỗi project
- **Layer 3 (Episodic)**: `LESSONS.md` trong mỗi project

Khi làm việc với skills:
- Dùng `/checkpoint` trước khi nghỉ
- Dùng `/recall` khi quay lại
- Dùng `/memory` để kiểm tra context

---

## 📋 DANH SÁCH 26 SKILLS

### Session Core
| Skill | Lệnh |
|---|---|
| `start` | `/start` |
| `save` | `/save` |
| `review` | `/review` |
| `supper` | `/supper` |
| `memory-optimizer` | `/checkpoint` `/recall` `/memory` |

### Code & Build
`vibe-code` · `gsd` · `typescript-pro` · `nextjs-app-router-patterns`
`react-modernization` · `nodejs-backend-patterns` · `api-design-principles`
`graphql-architect` · `payment-integration` · `frontend-mobile-development-component-scaffold`

### Design
`design-workflow` · `tailwind-design-system` · `ui-visual-validator` · `seo-structure-architect`

### Quality & Integration
`bug-memory-journal` · `debugger` · `security-auditor` · `performance-engineer`
`e2e-testing-patterns` · `git-advanced-workflows` · `n8n-workflow`

---

## 📝 CHANGE LOG

| Date | Change |
|---|---|
| 2026-02-27 | Khởi tạo git repo, thêm memory-optimizer skill (skill #26) |
| 2026-02-27 | Viết skill_memory.md — báo cáo memory optimization |
| 2026-02-27 | Cập nhật start, vibe-code, save, supper, bug-memory-journal với memory hooks |
