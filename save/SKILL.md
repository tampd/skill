---
name: Save — Đóng gói & Xuất bản Production Code
description: "Kết thúc phiên v5.0: 2-stage review (spec compliance + code quality), ghi LESSONS, archive change folders + delta-spec merge, Beads close + compact, Qdrant store, atomic commit, push. Kích hoạt bằng /save."
---

# /save

> **v5.0** — 2-Stage Review + Change Archive
> Code đã review (spec + quality) → docs cập nhật → archived → committed → pushed → memory consolidated.

---

## QUY TRÌNH 7 BƯỚC

### Bước 1 — SCOPE REVIEW

```bash
git status          # Files đã thay đổi
git diff --stat     # Tổng quan changes
```

Liệt kê files thay đổi, nhóm theo module.

---

### Bước 2 — 2-STAGE REVIEW ⭐ (MỚI v5.0)

> Từ Superpowers: kiểm tra spec compliance TRƯỚC, rồi mới code quality.

#### Stage 1: SPEC COMPLIANCE (từ Superpowers)

```
Kiểm tra code với spec/proposal (nếu có change folder):

1. CODE ĐÚng SPEC?
   - Tất cả requirements trong specs/spec.md đã implement?
   - Scenarios trong spec đã handle?

2. CODE THỪA?
   - Có code/feature nào KHÔNG ở trong spec?
   - Nếu có → xóa hoặc tạo change mới

3. BEHAVIOR KHỚP?
   - Behavior thực tế matches expected behavior?
   - Edge cases handled?

KẾT QUẢ:
✅ Spec compliant — all requirements met, nothing extra
❌ Issues: [liệt kê] → sửa trước khi tiếp tục Stage 2
```

> Nếu không có change folder → skip Stage 1, chỉ chạy Stage 2.

#### Stage 2: CODE QUALITY (7 tiêu chí — giữ nguyên + enhanced)

| # | Tiêu chí | Kiểm tra |
|---|---|---|
| ① | **Bugs & Logic** | Null pointer, edge cases, infinite loop, logic đúng? |
| ② | **UI/UX** | Responsive, contrast, ARIA, loading/empty/error states? |
| ③ | **Security** | SQL injection, XSS, CSRF, hardcoded secrets? |
| ④ | **Performance** | N+1 query, missing index, heavy computation? |
| ⑤ | **Code Quality** | Magic numbers, DRY, YAGNI, naming, types? |
| ⑥ | **Test Coverage** | Tests viết trước code (TDD)? All pass? Evidence? |
| ⑦ | **LESSONS Compliance** | Code mới có vi phạm bài học cũ? |

**Output format:**
```
📋 STAGE 1 — SPEC COMPLIANCE:
  [✅ All requirements met | ❌ Issues found]

📋 STAGE 2 — CODE QUALITY:
  ① BUGS:          [✅ | ⚠️ | 🛑]
  ② UI/UX:         [✅ | ⚠️ | N/A]
  ③ SECURITY:      [✅ | ⚠️ | 🛑]
  ④ PERFORMANCE:   [✅ | ⚠️ | 🛑]
  ⑤ CODE QUALITY:  [✅ | ⚠️ | 🛑]
  ⑥ TEST COVERAGE: [✅ TDD verified | ❌ Tests missing]
  ⑦ LESSONS:       [✅ Tuân thủ | 🛑 Vi phạm #WARN-XXX]
```

> 🛑 CRITICAL issues → **DỪNG**, yêu cầu sửa trước khi commit.

---

### Bước 3 — LESSONS CAPTURE

```
Hỏi: "Phiên này có gặp bug hoặc rút ra bài học gì không?"

NẾU CÓ → Ghi vào LESSONS.md:

### [🔴|🟡|🟢] #WARN-NNN — Tiêu đề (YYYY-MM-DD)
- **Mức độ:** 🔴 Critical | 🟡 Warning | 🟢 Info
- **Thẻ:** #tag1, #tag2
- **Triệu chứng:** [description]
- **Nguyên nhân gốc:** [WHY]
- **Cách fix:**
  // ✅ Đúng: [code]
  // ❌ Sai: [code]
- **Quy tắc rút ra:** [Rule]
- **File liên quan:** `path/to/file`
```

---

### Bước 4 — CHANGE ARCHIVE ⭐ (MỚI v5.0)

> Từ OpenSpec: Archive change folders + merge delta specs.

```
1. NẾU có change folder (changes/<feature>/):
   a. Verify: tất cả tasks trong tasks.md đã ✅?
      ✅ → tiếp tục archive
      ❌ → cảnh báo: "Còn N tasks chưa done. Archive anyway?"

   b. MERGE DELTA SPECS:
      → Đọc delta-specs.md
      → Apply changes vào docs/ tương ứng:
        - docs/architecture.md ← thêm sections mới
        - docs/api-endpoints.md ← thêm endpoints mới
        - GEMINI.md ← thêm rules mới (nếu có)
      → Specs giờ phản ánh trạng thái MỚI

   c. ARCHIVE:
      → Move: changes/<feature>/ → archive/YYYY-MM-DD-<feature>/
      → "✅ Change archived, specs updated"

2. NẾU không có change folder → bỏ qua bước này
```

---

### Bước 5 — DOCS UPDATE

| File | Hành động |
|---|---|
| `CHANGE_LOG.md` | Thêm entry `## [vX.Y.Z] - YYYY-MM-DD` |
| `NEXT-TODO.md` | XÓA task đã hoàn thành |
| `GEMINI.md` | Cập nhật version, status |
| `STATE.md` | Cập nhật phase/wave (nếu có) |

---

### Bước 6 — ATOMIC COMMIT & PUSH

```bash
# Feature/fix commit (nếu chưa commit trong TDD cycle):
git add [files cùng feature]
git commit -m "feat(scope): mô tả 72 ký tự max"

# Docs commit:
git add [docs files]
git commit -m "docs: update changelog vX.Y.Z"

# Archive commit (nếu có):
git add [archive/* delta changes]
git commit -m "chore: archive change <feature-name>"

# Push
git push origin main
```

---

### Bước 7 — MEMORY CONSOLIDATION & END REPORT

```
1. NẾU có ACTIVE_CONTEXT.md → xóa (persisted vào docs)

2. ⭐ BEADS CONSOLIDATION (Layer 5):
   → Close issues hoàn thành:
     bd close <id1> <id2> --reason "Completed in session"
   → Tạo follow-up nếu có:
     bd create "Follow-up: [mô tả]" -p 2
   → Compact task cũ:
     bd admin compact --days 90 --dry-run

3. ⭐ QDRANT STORE (Layer 4):
   → Store session highlights:
     - Lessons mới → type: "lesson"
     - Patterns → type: "pattern"
     - Decisions → type: "decision"
```

**End Report:**
```
✅ /save hoàn tất!

📋 REVIEW:
  Stage 1 (Spec): [✅ Compliant | N/A]
  Stage 2 (Quality): [✅ 7/7 pass | ⚠️ N warnings]
📝 LESSONS: [N bài học mới | Không thay đổi]
📦 ARCHIVE: [changes/<name>/ → archived | N/A]
📋 DOCS: CHANGE_LOG ✅ | NEXT-TODO ✅ | GEMINI ✅
📦 COMMITS: [N commits]
🔗 PUSHED: main → origin

🎯 BEADS: [N issues closed | N follow-ups | Chưa init]
🧠 QDRANT: [N memories stored | Chưa kết nối]

🔜 TASK TIẾP THEO:
  → [Task ưu tiên từ Beads bd ready / NEXT-TODO]

Gõ /start [task] để bắt đầu phiên mới!
```

---

## QUY TẮC QUAN TRỌNG

- 🛑 Stage 1 FAIL → KHÔNG cho phép commit (sửa spec compliance trước)
- 🛑 CRITICAL trong Stage 2 → KHÔNG cho phép commit
- ❌ Thiếu evidence verify → KHÔNG commit
- ❌ Vi phạm LESSONS → phải sửa trước
- ✅ Delta specs PHẢI merge khi archive
- ✅ Commit message rõ WHY, không chỉ WHAT
- ✅ LESSONS entry mới PHẢI có ✅❌ code examples
