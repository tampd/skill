---
name: Fix — Systematic Debug & Lessons Workflow
description: "Quy trình debug 4-phase systematic: Qdrant recall → LESSONS check → Root Cause Investigation → Pattern Analysis → Hypothesis Testing → Fix + TDD test → record. v5.0 Iron Law: KHÔNG fix nếu chưa tìm root cause. Kích hoạt bằng /fix [bug description]."
---

# /fix [bug description]

> **v5.0 SYSTEMATIC DEBUG** — Lấy cảm hứng từ Superpowers systematic-debugging
> Mục tiêu: Root cause TRƯỚC, fix SAU. Ghi bài học. KHÔNG BAO GIỜ gặp lại.

> [!CAUTION]
> **IRON LAW**: KHÔNG FIX nếu chưa xong Phase 1 (Root Cause Investigation).
> Quick patch = nợ kỹ thuật. Symptom fix = failure.

---

## QUY TRÌNH 4 PHASE + RECORD

### Phase 0 — MEMORY RECALL (TRƯỚC KHI DEBUG)

```
1. Đọc LESSONS.md → grep entries MATCH với [bug description]
   - Tìm theo tags, file liên quan, triệu chứng tương tự

2. NẾU TÌM THẤY:
   📌 "Bug này GIỐNG #WARN-XXX! Solution đã biết:"
   → Hiển thị cách fix từ LESSONS
   → Apply fix → verify → viết test → XONG (skip Phase 1-3)

3. NẾU KHÔNG TÌM THẤY:
   → "Bug mới. Bắt đầu 4-phase systematic debugging."

4. ⭐ QDRANT RECALL (Layer 4):
   → qdrant_find("bug: [bug description]")
   → Tìm bugs tương tự đã fix (cross-project)
   → Nếu Qdrant chưa kết nối → bỏ qua
```

> 🎯 Mục tiêu: 50% bugs đã gặp rồi → fix trong < 1 phút nhờ LESSONS.

---

### Phase 1 — ROOT CAUSE INVESTIGATION 🔍 (BẮT BUỘC)

> 🛑 **IRON LAW**: Nếu chưa xong Phase 1 → KHÔNG ĐƯỢC propose fix.
> "Nó có vẻ rõ ràng" KHÔNG phải root cause. CHỨNG MINH bằng evidence.

```
1. ĐỌC ERROR MESSAGES — KỸ, TOÀN BỘ
   - Không skip error messages hay warnings
   - Đọc stack traces HOÀN CHỈNH
   - Ghi lại: line numbers, file paths, error codes
   - Error message thường chứa solution → đọc trước khi guess

2. REPRODUCE CONSISTENTLY
   - Trigger lỗi reliably → exact steps (1-2-3)
   - Xảy ra every time? Intermittent?
   - NẾU không reproduce được → thu thập thêm data, KHÔNG guess

3. CHECK RECENT CHANGES
   git log --oneline -10       → commit gần nhất liên quan?
   git diff HEAD~3 [file]      → code gì đã thay đổi?
   - New dependencies? Config changes?

4. TRACE DATA FLOW (cho bugs sâu)
   - Bad value đến từ đâu?
   - Ai gọi function này với bad value?
   - Trace NGƯỢC lên cho đến khi tìm SOURCE
   - Fix tại SOURCE, không fix tại symptom

5. GATHER EVIDENCE (cho multi-component systems)
   - Log ở MỖI component boundary:
     input vào → output ra → state tại mỗi layer
   - Chạy 1 lần để gather evidence
   - Evidence cho thấy WHERE it breaks
   - THEN investigate failing component
```

**Output Phase 1:**
```
🔍 ROOT CAUSE IDENTIFIED:
   Symptom: [mô tả lỗi quan sát]
   Root Cause: [WHY — nguyên nhân gốc chứng minh được]
   Evidence: [data/log/trace chứng minh]
   Location: [exact file:line]
```

---

### Phase 2 — PATTERN ANALYSIS 📊

```
1. TÌM WORKING EXAMPLES
   - Code tương tự đang HOẠT ĐỘNG trong cùng codebase?
   - Pattern reference (docs, framework docs)?

2. SO SÁNH KHÁC BIỆT
   - Working code vs broken code → liệt kê MỌI khác biệt
   - KHÔNG assume "cái này không ảnh hưởng"

3. HIỂU DEPENDENCIES
   - Component này cần gì? (config, env, other services)
   - Assumptions nào đang sai?
```

---

### Phase 3 — HYPOTHESIS & TESTING 🧪

```
1. STATE RÕ RÀNG:
   "Tôi nghĩ [X] là root cause vì [Y]"
   - Viết ra rõ ràng, cụ thể

2. TEST TỐI THIỂU:
   - Thay đổi NHỎ NHẤT có thể để test hypothesis
   - 1 biến tại 1 thời điểm
   - KHÔNG fix nhiều thứ cùng lúc

3. VERIFY:
   ✅ Hypothesis đúng → Phase 4 (Fix)
   ❌ Hypothesis sai → Form NEW hypothesis (KHÔNG stack fixes)

4. KHI KHÔNG BIẾT:
   - Nói rõ: "Tôi chưa hiểu [X]"
   - KHÔNG giả vờ biết
   - Research thêm hoặc hỏi user
```

---

### Phase 4 — IMPLEMENTATION 🔧

```
1. FIX ROOT CAUSE (không fix SYMPTOM):
   - Minimal change → dễ review, dễ revert
   - KHÔNG refactor xung quanh trong fix commit
   - Remove debug logging trước khi commit
   - Architecture Rules vẫn áp dụng

2. TDD TEST CHO BUG ⭐ (MỚI v5.0):
   🔴 RED: Viết test reproduce bug → chạy → FAIL (bug tồn tại)
   🟢 GREEN: Apply fix → chạy test → PASS (bug đã fix)
   🔵 VERIFY: Chạy TẤT CẢ tests → không regression

3. REGRESSION CHECK:
   - Chạy existing tests → không break gì mới
   - Check related functionality → vẫn hoạt động
   - Edge cases: null, empty, zero, negative, concurrent
```

---

### RECORD — LESSONS + COMMIT + STORE

```
1. COMMIT:
   git add [fix files + test files]
   git commit -m "fix(module): [root cause description]"

2. GHI LESSONS.MD — BẮT BUỘC cho MỌI bug fix:

### 🟡 #WARN-NNN — Tiêu đề (YYYY-MM-DD)

- **Mức độ:** 🔴 Critical | 🟡 Warning | 🟢 Info
- **Thẻ:** #tag1, #tag2
- **Triệu chứng:** [mô tả lỗi quan sát]
- **Root Cause:** [WHY — đã chứng minh ở Phase 1]
- **Investigation:** [Tóm tắt trace/evidence]
- **Cách fix:**
  // ✅ Đúng — [giải thích]
  [code snippet đúng]
  // ❌ Sai — [giải thích]
  [code snippet sai]
- **Test:** [Test name that catches this bug]
- **Quy tắc rút ra:** [Rule LUÔN áp dụng]
- **File liên quan:** `path/to/file`

3. ⭐ QDRANT STORE (Layer 4):
   → qdrant_store(bug_fix, {project, type: "bug_fix", tags, severity})
4. ⭐ BEADS CLOSE (Layer 5):
   → bd close <id> --reason "Fixed: [commit message]" --json
```

---

## RED FLAGS — DỪNG VÀ FOLLOW PROCESS

```
🛑 DỪNG nếu:
- Đang viết fix mà chưa reproduce bug
- Đang stack fixes (fix A chưa work → thêm fix B → thêm fix C)
- "Fix nhanh" cho issue phức tạp
- Đã thử > 2 fixes mà chưa tìm root cause
- Đang sửa symptom thay vì source

💡 KHI GẶP RED FLAG:
- Quay lại Phase 1
- Gather thêm evidence
- Trace data flow carefully
- Hỏi user nếu cần context
```

---

## QUY TẮC

- 🛑 Phase 1 PHẢI xong trước khi fix — NO EXCEPTIONS
- ❌ KHÔNG BAO GIỜ bỏ qua ghi LESSONS sau fix
- ✅ LESSONS entry PHẢI có ✅❌ code examples
- ✅ Bug fix PHẢI có test (TDD: RED → GREEN)
- ✅ Remove ALL debug code trước commit
- ✅ Sửa root cause, KHÔNG sửa symptom
- ✅ 1 bug fix = 1 commit = 1 LESSONS entry = 1 test
- ⭐ Beads/Qdrant không available → bỏ qua im lặng
