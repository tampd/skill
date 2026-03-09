---
name: fix
description: "Debug 4-phase systematic: Memory recall → Root Cause Investigation → Pattern Analysis → Hypothesis Testing → Fix + TDD test → record LESSONS. KHÔNG fix nếu chưa tìm root cause. Kích hoạt khi người dùng nói /fix, 'bug', 'lỗi', 'error', 'debug', 'crash', 'sửa lỗi'."
metadata:
  author: tampd
  version: 7.0.0
  category: debugging
---

# Fix — Systematic Debug

> **v7.0** — 4-Phase Systematic + Root Cause Iron Law
> Mục tiêu: Root cause TRƯỚC, fix SAU. Ghi bài học. KHÔNG BAO GIỜ gặp lại.

> [!CAUTION]
> **IRON LAW**: KHÔNG FIX nếu chưa xong Phase 1 (Root Cause Investigation).

---

## Phase 0 — MEMORY RECALL

```
1. Đọc LESSONS.md → grep entries MATCH với [bug description]
   NẾU TÌM THẤY:
   📌 "Bug này GIỐNG #WARN-XXX! Solution đã biết:"
   → Apply fix → verify → viết test → XONG (skip Phase 1-3)

2. NẾU KHÔNG → "Bug mới. Bắt đầu systematic debugging."
```

## Phase 1 — ROOT CAUSE INVESTIGATION 🔍 (BẮT BUỘC)

> 🛑 "Nó có vẻ rõ ràng" KHÔNG phải root cause. CHỨNG MINH bằng evidence.

```
1. ĐỌC ERROR MESSAGES — KỸ, TOÀN BỘ
   Stack traces HOÀN CHỈNH → line numbers, file paths, error codes

2. REPRODUCE CONSISTENTLY
   Trigger lỗi reliably → exact steps (1-2-3)
   Không reproduce được → thu thập thêm data, KHÔNG guess

3. CHECK RECENT CHANGES
   git log --oneline -10 → commit gần nhất liên quan?
   git diff HEAD~3 [file] → code gì đã thay đổi?

4. TRACE DATA FLOW (cho bugs sâu)
   Bad value đến từ đâu? → trace NGƯỢC → fix tại SOURCE

5. GATHER EVIDENCE (multi-component)
   Log input → output → state tại mỗi component boundary
```

**Output Phase 1:**
```
🔍 ROOT CAUSE: [WHY — chứng minh bằng evidence]
   Evidence: [data/log/trace]
   Location: [file:line]
```

## Phase 2 — PATTERN ANALYSIS 📊

```
1. Tìm WORKING EXAMPLES trong cùng codebase
2. So sánh khác biệt: working vs broken
3. Hiểu dependencies: config, env, services
```

## Phase 3 — HYPOTHESIS & TESTING 🧪

```
SELF-REASONING GATE:
  Q1: "Fix ROOT CAUSE hay chỉ symptom?"
  Q2: "Side-effect? Regression risk?"
  Q3: "Fix scope lớn → cần hỏi user trước?"

TEST: Thay đổi NHỎ NHẤT → 1 biến → verify
  ✅ Đúng → Phase 4
  ❌ Sai → NEW hypothesis (KHÔNG stack fixes)
```

## Phase 4 — IMPLEMENTATION 🔧

```
1. FIX ROOT CAUSE (minimal change, dễ revert)
2. TDD TEST: 🔴 RED → 🟢 GREEN → 🔵 VERIFY all tests
3. REGRESSION CHECK: existing tests + edge cases
4. COMMIT: git commit -m "fix(module): [root cause]"
5. GHI LESSONS.MD (xem references/lessons-template.md)
```

---

## RED FLAGS — DỪNG VÀ FOLLOW PROCESS

```
🛑 DỪNG nếu:
- Fix mà chưa reproduce bug
- Stack fixes (A chưa work → thêm B → thêm C)
- "Fix nhanh" cho issue phức tạp
- Đã thử > 2 fixes mà chưa tìm root cause
- Sửa symptom thay vì source
```

## QUY TẮC

- 🛑 Phase 1 PHẢI xong trước khi fix — NO EXCEPTIONS
- ✅ 1 bug fix = 1 commit = 1 LESSONS entry = 1 test
- ✅ Remove ALL debug code trước commit
- ✅ Sửa root cause, KHÔNG sửa symptom
