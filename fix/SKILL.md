---
name: Fix — Debug & Lessons Workflow
description: "Quy trình debug và ghi bài học toàn diện: check LESSONS cũ → reproduce → isolate → fix → test → record. Thay thế debugger + bug-memory-journal. Kích hoạt bằng /fix [bug description]."
---

# /fix [bug description]

> **2-in-1** — Gộp `debugger` + `bug-memory-journal`
> Mục tiêu: Fix bug + ghi bài học + KHÔNG BAO GIỜ gặp lại cùng bug

---

## QUY TRÌNH 6 BƯỚC

### Step 1 — LESSONS SEARCH (TRƯỚC KHI DEBUG)

```
1. Đọc LESSONS.md
2. Grep: entry nào MATCH với [bug description]?
   - Tìm theo tags, file liên quan, triệu chứng tương tự
3. NẾU TÌM THẤY:
   📌 "Bug này GIỐNG #WARN-XXX! Solution đã biết:"
   → Hiển thị cách fix từ LESSONS
   → Apply fix → verify → XONG (skip step 2-4)
4. NẾU KHÔNG TÌM THẤY:
   → "Bug mới. Bắt đầu debug systematic."
   → Tiếp tục Step 2
```

> 🎯 **Mục tiêu**: 50% bugs đã gặp rồi → fix trong < 1 phút nhờ LESSONS.

---

### Step 2 — REPRODUCE

```
1. Capture chính xác:
   - Error message / stack trace
   - URL / endpoint / page bị lỗi
   - Steps to reproduce (1-2-3)
   - Expected behavior vs actual behavior

2. Reproduce error:
   - Chạy lại exact steps → confirm error tái hiện
   - NẾU không tái hiện → thu thập thêm context
```

---

### Step 3 — ISOLATE (Root Cause Analysis)

```
Systematic approach:

1. Check recent changes:
   git log --oneline -10       → commit nào gần nhất liên quan?
   git diff HEAD~3 [file]      → code gì đã thay đổi?

2. Form hypothesis:
   "Bug xảy ra vì [hypothesis]"

3. Test hypothesis:
   - Add strategic debug logging (tạm thời)
   - Inspect variable states tại failure point
   - Binary search: comment out code blocks → tìm dòng gây lỗi

4. Confirm root cause:
   ✅ "Root cause: [giải thích cụ thể]"
   ❌ "Hypothesis sai → thử hypothesis khác"
```

---

### Step 4 — FIX (Minimal Change)

```
Nguyên tắc:
- Sửa ROOT CAUSE, không sửa SYMPTOM
- Minimal change → dễ review, dễ revert
- KHÔNG refactor code xung quanh trong fix commit
- Remove debug logging trước khi commit

Architecture Rules vẫn áp dụng:
- Bcmath cho tiền
- Transaction cho multi-step DB
- Null-safe operators
- No hardcode values
```

---

### Step 5 — TEST (Prove fix works)

```
1. Verify fix:
   - Chạy lại exact reproduction steps → error KHÔNG còn
   - Evidence: [curl output / test pass / screenshot]

2. Regression check:
   - Chạy existing tests → không break gì mới
   - Check related functionality → vẫn hoạt động

3. Edge cases:
   - Null input? Empty array? Zero value? Negative number?
   - Concurrent requests? Race condition?
```

---

### Step 6 — RECORD (LESSONS + COMMIT)

```
1. COMMIT:
   git add [fix files only]
   git commit -m "fix(module): mô tả bug đã sửa"

2. GHI LESSONS.MD NGAY — BẮT BUỘC cho mọi bug fix:

### 🟡 #WARN-NNN — Tiêu đề ngắn gọn (YYYY-MM-DD)

- **Mức độ:** 🔴 Critical | 🟡 Warning | 🟢 Info
- **Thẻ:** #tag1, #tag2
- **Triệu chứng:** [mô tả lỗi quan sát được]
- **Nguyên nhân gốc:** [WHY — root cause]
- **Cách fix:**
  ```
  // ✅ Đúng — [giải thích]
  [code snippet đúng]

  // ❌ Sai — [giải thích]
  [code snippet sai]
  ```
- **Quy tắc rút ra:** [Rule LUÔN áp dụng từ nay]
- **File liên quan:** `path/to/file`

3. Cập nhật thống kê LESSONS.md (tổng bug +1)
```

> ❌ KHÔNG ĐƯỢC bỏ qua ghi LESSONS sau fix bug. Đây là cách AI "học" để không lặp lại.

---

## QUY TẮC

- Fix bug → **BẮT BUỘC** ghi LESSONS.md (không optional)
- LESSONS entry PHẢI có ✅❌ code examples
- Remove ALL debug code trước commit
- Sửa root cause, KHÔNG sửa symptom
- 1 bug fix = 1 commit = 1 LESSONS entry
