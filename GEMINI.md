# GEMINI — Skill Repository v3 (Production Code Framework + Extended Arsenal)

> **GitHub**: https://github.com/tampd/skill
> **Local path**: /root/skill
> **Cập nhật**: 2026-03-04 — **v3.0 EXTENDED ARSENAL**

---

## 🔑 THÔNG TIN QUAN TRỌNG

### Skill System v3 — Extended Arsenal
Phiên bản 3.0 giữ nguyên 8 skills core ổn định từ v2 + bổ sung **4 skills mới** chuyên biệt:
- **n8n-pro**: N8N workflow expert (7 sub-skills từ community best practices + MCP integration)
- **brainstorm**: Ideation → Spec → Prototype pipeline (cho planning & sáng tạo)
- **web-security**: OWASP Top 10 deep audit + CVE scanner + hardening + compliance
- **review-website**: Đánh giá website toàn diện (UX/UI/SEO/Performance/Security/A11y)
- Nâng cấp **start** (auto-select nhận diện 12 skills)
- Nâng cấp **guard** (Supply Chain Security audit)

### Triết lý v3: "Less is More, But Sharp"
```
❌ KHÔNG install 900+ skills rải rác → context rot, tool bloat
✅ 12 skills tập trung, mỗi skill là "dao sắc" cho 1 mục đích
✅ Progressive Disclosure: metadata nhẹ, instructions load khi cần
✅ Mỗi skill có quy trình rõ ràng, evidence-based
```

### GitHub Repository
```
URL:    https://github.com/tampd/skill
Remote: origin → git@github.com:tampd/skill.git (SSH)
Branch: main
SSH Key: SHA256:nTSlO07MbIplXX/j2FAHlyuSb+MJxPO1yboDHJJidFs
```

> **RULE**: Mọi dự án đều push qua SSH (`git@github.com:`), KHÔNG dùng HTTPS.
> Nếu remote đang là HTTPS → chạy `git remote set-url origin git@github.com:{user}/{repo}.git`

---

## ⚠️ GLOBAL RULES

### Rule 1: AI PHẢI HỎI KHI CHƯA RÕ
> Nếu yêu cầu chưa rõ nghĩa, thiếu thông tin, hoặc mâu thuẫn:
> 🛑 AI PHẢI HỎI user TRƯỚC KHI lập kế hoạch, viết code, ghi docs.
> ❌ KHÔNG giả định và code khi chưa xác nhận.

### Rule 2: ĐỌC LESSONS TRƯỚC KHI CODE
> Mỗi task PHẢI bắt đầu bằng việc đọc LESSONS.md + grep tags liên quan.
> Không có ngoại lệ. Đây là cách AI "nhớ" để không lặp sai lầm.

### Rule 3: EVIDENCE-BASED VERIFICATION
> Không chấp nhận "nó chạy rồi". Phải có bằng chứng cụ thể:
> curl output, test results, screenshot, hoặc log.

---

## 📁 CẤU TRÚC SKILL REPOSITORY v3

```
/root/skill/
├── GEMINI.md                ← File này — brain & rules
├── README.md                ← Hướng dẫn cài đặt
│
│── ─── 8 CORE SKILLS (từ v2, nâng cấp) ───
├── start/SKILL.md           ← /start [task]
├── build/SKILL.md           ← /build [task] ⭐
├── fix/SKILL.md             ← /fix [bug]
├── save/SKILL.md            ← /save
├── plan/SKILL.md            ← /plan [feature]
├── design/SKILL.md          ← /design [task]
├── guard/SKILL.md           ← /guard [scope]
├── integrate/SKILL.md       ← /integrate [svc]
│
│── ─── 4 NEW SKILLS (v3) ───
├── n8n-pro/SKILL.md         ← /n8n [task] — N8N workflow expert
├── brainstorm/SKILL.md      ← /brainstorm [idea] — Ideation pipeline
├── web-security/SKILL.md    ← /security [target] — Deep security audit
├── review-website/SKILL.md  ← /review-web [url] — Website review
│
│── ─── INFRASTRUCTURE ───
├── qdrant-memory/SKILL.md   ← Qdrant Layer 4 Memory
└── archive/                 ← Skills cũ v1+v2 (preserved)
```

---

## 🎯 12 SKILLS — QUICK REFERENCE

| # | Skill | Lệnh | Mục đích | Từ v |
|---|---|---|---|---|
| 1 | **start** | `/start [task]` | Khởi động phiên + chọn skill | v2 ⬆ |
| 2 | **build** ⭐ | `/build [task]` | Viết code production-grade | v2 |
| 3 | **fix** | `/fix [bug]` | Debug + ghi LESSONS | v2 |
| 4 | **save** | `/save` | Review 7 tiêu chí + save + push | v2 |
| 5 | **plan** | `/plan [feature]` | Tạo task blueprint | v2 |
| 6 | **design** | `/design [task]` | UI/UX + design system | v2 |
| 7 | **guard** | `/guard [scope]` | Test + security + performance | v2 ⬆ |
| 8 | **integrate** | `/integrate [svc]` | API + 3rd-party + webhook | v2 |
| 9 | **n8n-pro** 🆕 | `/n8n [task]` | N8N workflow design + debug + MCP | v3 |
| 10 | **brainstorm** 🆕 | `/brainstorm [idea]` | Ideation → Spec → Prototype | v3 |
| 11 | **web-security** 🆕 | `/security [target]` | OWASP audit + CVE + hardening | v3 |
| 12 | **review-website** 🆕 | `/review-web [url]` | Website review toàn diện 7 chiều | v3 |

---

## 🔄 SESSION LIFECYCLE v3

```
/start [task]  →  [auto-select skill]  →  /save
   │                    │                    │
   ├─ Load context      ├─ /build           ├─ 7-criteria review
   ├─ Check LESSONS     ├─ /fix             ├─ Write LESSONS
   ├─ Qdrant recall     ├─ /design          ├─ Update docs
   ├─ Scan blueprints   ├─ /n8n             ├─ Qdrant store
   └─ Auto-select       ├─ /brainstorm      ├─ Atomic commit
      skill             ├─ /security        └─ Push
                        ├─ /review-web
                        ├─ /guard
                        ├─ /integrate
                        └─ /plan
```

---

## 🧠 MEMORY ARCHITECTURE (5 Tầng)

| Tầng | File/System | Mục đích |
|---|---|---|
| **1 — Session** | `ACTIVE_CONTEXT.md` | Working memory (xóa sau /save) |
| **2 — Project** | `GEMINI.md`, `LESSONS.md`, `CHANGE_LOG.md`, `NEXT-TODO.md` | Não bộ dự án |
| **3 — Knowledge** | `docs/*.md` | Tài liệu kỹ thuật |
| **4 — Vector** | Qdrant (MCP hoặc curl) | Cross-session semantic memory |
| **5 — Skills** | `~/.gemini/antigravity/skills/` + `.agent/commands/` | Skill + Blueprints |

---

## 📚 DOCUMENTATION STANDARDS

### Nguyên tắc viết docs

| File | Viết gì | KHÔNG viết gì |
|---|---|---|
| `GEMINI.md` | Tech stack, Architecture Rules (✅❌ code), Version, Links | Task TODO, logs phiên |
| `LESSONS.md` | Bug #WARN-XXX + ✅❌ code examples + quy tắc rút ra | Feature code, changelog |
| `CHANGE_LOG.md` | Timeline thay đổi: Added/Fixed/Changed/Removed | Task future |
| `NEXT-TODO.md` | Task backlog (bảng tracking: #, Size, Status, Deps) | Task đã xong (xóa ngay) |
| `docs/architecture.md` | System diagrams, module tree, data flow | Task backlog |
| `docs/business-rules.md` | Logic nghiệp vụ thuần (KHÔNG code) | UI details |
| `docs/api-endpoints.md` | REST API: method + URL + auth + request/response | Business logic |
| `docs/setup.md` | Cài đặt môi trường dev/prod | Business rules |
| `docs/security-audit.md` 🆕 | Audit results, CVEs, remediation plan | Code patches |

### Cách viết Architecture Rules (bắt buộc)

```markdown
### Rule N: [Tên rule] (VI PHẠM = BUG)
[Giải thích 1 dòng tại sao rule này tồn tại]

// ✅ Đúng — [giải thích]
[code example đúng]

// ❌ Sai — [giải thích]
[code example sai]
```

### Cách viết LESSONS.md entries (bắt buộc)

```markdown
### 🟡 #WARN-NNN — Tiêu đề (YYYY-MM-DD)
- **Mức độ:** 🟡 Warning
- **Thẻ:** #tag1, #tag2
- **Triệu chứng:** [mô tả quan sát]
- **Nguyên nhân gốc:** [WHY]
- **Cách fix:**
  // ✅ Đúng
  [code]
  // ❌ Sai
  [code]
- **Quy tắc rút ra:** [Rule áp dụng mãi]
- **File liên quan:** `path/to/file`
```

### Cách viết Task Blueprint (.agent/commands/task-NNN.md)

Tạo bởi `/plan`, follow bởi `/build`:

```markdown
> **Complexity: [S|M|L]**

# Task-NNN: [Feature Name]

## Step 1 — Read Rules & Docs
- `GEMINI.md` (Rules: [chỉ rõ rule nào])
- `LESSONS.md` (grep: [keywords])
- `docs/[doc].md` (Section: [chỉ rõ])

## Step 2 — Implementation
[File tree + logic flow + error handling table]

## Step 3 — Tests
[Liệt kê từng test case cụ thể]

## Step 4 — Documentation
[Chỉ rõ files nào cần update]
```

### Cách viết NEXT-TODO.md (bảng tracking)

```markdown
| # | Task | Size | Status | Deps | Notes |
|---|------|------|--------|------|-------|
| T-001 | [name] | M | ⬜ | - | |
| T-002 | [name] | L | 🔄 | T-001 | Đang làm |
| T-003 | [name] | S | ✅ | - | Done 28/02 |
```

Status: ⬜ Not started | 🔄 In progress | ✅ Done | ⏸️ Blocked

---

## 📋 CHEAT SHEET v3

```
Bắt đầu phiên?           → /start [task]
Viết code?                → /build [task]
Gặp bug?                  → /fix [bug]
Feature phức tạp?         → /plan [feature] → /build
Design / UI?              → /design [task]
Test / audit code?        → /guard [scope]
API / webhook?            → /integrate [service]
N8N workflow?             → /n8n [task]           🆕
Cần ý tưởng / spec?      → /brainstorm [idea]     🆕
Audit bảo mật sâu?       → /security [target]     🆕
Review website?           → /review-web [url]      🆕
Kết thúc phiên?           → /save
```

---

## 📝 CHANGE LOG

| Date | Change |
|---|---|
| **2026-03-04** | **v3.0 EXTENDED**: +4 skills mới (brainstorm, n8n-pro, web-security, review-website). Nâng cấp start (auto-select 12 skills) + guard (Supply Chain Security). |
| 2026-02-28 | v2.0: Gộp 26 skills → 8 unified skills. Thêm /plan, /guard. Archive 27 skills cũ. |
| 2026-02-27 | v1.x: 26 skills, memory-optimizer, qdrant integration |
