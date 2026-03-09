---
name: build
description: "Viết code production-grade theo TDD và lập kế hoạch tính năng. Hỗ trợ 2 mode: /plan (ideation + SPEC + change folder) và /build (TDD RED→GREEN→REFACTOR). Kích hoạt khi người dùng nói /build, /plan, 'viết code', 'tạo feature', 'brainstorm', 'lên spec', hoặc 'TDD'."
metadata:
  author: tampd
  version: 7.0.0
  category: development
---

# Build — Vibe Code & Spec-Driven Development

> **v7.0** — Gộp build + plan
> Mục tiêu: Từ ý tưởng → spec → code TDD → verified trong 1 flow.

---

## 2 MODES

### Mode 1 — `/plan [feature]` (Ideation → Spec)

> Bước này CHỈ chạy khi cần brainstorm hoặc feature ≥ 3 files.
> Task nhỏ ≤ 2 files → dùng Mini-Spec inline ở `/build` Step 1.

```
STEP 0 — IDEATION (cho ý tưởng mơ hồ):
  1. Capture raw idea → hỏi 3 câu mở:
     "Vấn đề gốc?", "Ai là user?", "Thành công = gì?"
  2. Propose ≥ 2 approaches → so sánh pros/cons/effort
  🛑 HARD GATE: KHÔNG tạo spec khi chưa hiểu rõ

STEP 1 — RESEARCH:
  Đọc docs/ → patterns hiện có
  Đọc LESSONS.md → warnings liên quan
  Scan code hiện tại → identify files cần tạo/sửa

STEP 2 — CREATE CHANGE FOLDER:
  changes/<feature-name>/
  ├── SPEC.md         ← Feature SPEC (10 sections BKNS)
  ├── tasks.md        ← Implementation tasks (TDD bite-sized)
  └── delta-specs.md  ← Spec changes to merge vào docs/ khi done

  > Dự án MỚI → SPEC.md ở root (Full SPEC 19 sections)
  > Feature → changes/<name>/SPEC.md (Feature SPEC 10 sections)
  > Task ≤ 2 files → Mini-Spec inline

STEP 3 — PRESENT & CONFIRM:
  Hiển thị change folder → hỏi user approve
  Accepted → "Gõ /build để bắt đầu TDD"
```

---

### Mode 2 — `/build [task]` (TDD Execution)

```
STEP 0 — LOAD CONTEXT:
  1. Đọc LESSONS.md → grep entries liên quan
     🛑 CHƯA ĐỌC LESSONS → DỪNG
  2. Đọc GEMINI.md → Architecture Rules
  3. NẾU có change folder → load SPEC.md + tasks.md
  4. NẾU có ACTIVE_CONTEXT.md → restore state

STEP 0.5 — SPEC CHECK:
  NẾU module mới ≥ 3 files VÀ chưa có SPEC → DỪNG, gợi ý /plan
  NẾU task ≤ 2 files → Mini-Spec inline (10 dòng)

STEP 1 — SPEC TASK (< 2 phút):
  WHY   : Vấn đề giải quyết?
  WHAT  : Files cần tạo/sửa?
  HOW   : Approach kỹ thuật?
  TEST  : Verify bằng gì?
  RISK  : Có phá gì không?
  DONE  : Khi nào xong?

STEP 1.5 — SELF-REASONING GATE:
  Q1: "Phương án tốt nhất chưa?" → ≥ 2 alternatives
  Q2: "Risk/side-effect bỏ qua?" → breaking, regression, perf, security
  Q3: "User cần approve?" → ≤ 2 files → tự làm; ≥ 3 files → hỏi

STEP 2 — DEPENDENCY CHECK:
  Files phụ thuộc code chưa tồn tại? → DỪNG
  Migration cần chạy trước? DB seeder? Package install?

STEP 3 — TDD CYCLE ⭐:
  3.1 RED: Viết test TRƯỚC → chạy → FAIL (expected)
  3.2 GREEN: Viết code TỐI THIỂU → test PASS
  3.3 REFACTOR: Clean up (DRY, YAGNI, naming) → test vẫn PASS
  > TDD exceptions: prototype, config, CSS-only

STEP 4 — VERIFY (Evidence-based):
  Chạy full test suite → screenshot/log/curl output
  NẾU có spec → so sánh: đúng spec? thừa? thiếu?

STEP 5 — SPEC COMPLIANCE:
  Code thừa spec? → xóa hoặc tạo ticket mới
  Behavior khác spec? → sửa lại

STEP 6 — ATOMIC COMMIT:
  git add <files> && git commit -m "feat(scope): mô tả ≤ 72 chars"
  Types: feat | fix | chore | docs | perf | sec | test
```

---

## CHECKLIST TRƯỚC COMMIT

```
[ ] Đã đọc LESSONS.md trước khi code
[ ] Test viết TRƯỚC code (TDD)
[ ] Tất cả tests PASS
[ ] Spec compliance: đúng spec, không thừa
[ ] Không có dd()/dump()/console.log
[ ] Không hardcode magic numbers
[ ] Null-safe operators
[ ] DB transaction cho multi-step writes
[ ] Input validate và escape
[ ] CSS dùng variables (var(--xxx))
[ ] Commit message conventional commits
[ ] Evidence attached
```

---

## QUY TẮC

- 🛑 TDD IRON LAW: Viết test TRƯỚC code. Vi phạm → XÓA code
- 🛑 Module mới ≥ 3 files → PHẢI có SPEC trước
- ✅ Xem code standards chi tiết: `references/code-standards.md`
- ✅ Xem TDD patterns: `references/tdd-patterns.md`
- ✅ Xem SPEC templates: `references/spec-templates.md`
