---
name: Plan — Task Blueprint Generator
description: "Tạo blueprint chi tiết cho task phức tạp: Read → Implement → Test → Docs. Học từ bkns-minicrm PR commands pattern. Output: .agent/commands/task-NNN.md. Kích hoạt bằng /plan [feature name]."
---

# /plan [feature name]

> **Hoàn toàn MỚI** — Lấy cảm hứng từ bkns-minicrm PR Commands
> Mục tiêu: Tạo "recipe" hoàn chỉnh để AI follow chính xác khi `/build`

---

## KHI NÀO DÙNG /plan?

| Tình huống | Dùng /plan? |
|---|---|
| Fix bug nhỏ / sửa UI | ❌ Không — `/fix` hoặc `/build` trực tiếp |
| Feature mới 1-2 files | ❌ Không — `/build` trực tiếp |
| Feature phức tạp (3+ files) | ✅ `/plan` trước, `/build` sau |
| Feature mới cần design | ✅ `/plan` + `/design` |
| Dự án mới toàn bộ | ✅ `/plan` cho từng phase |

---

## QUY TRÌNH 4 BƯỚC

### Step 1 — RESEARCH

```
1. Đọc GEMINI.md → tech stack, Architecture Rules áp dụng
2. Đọc ARCHITECTURE.md / docs → patterns hiện có
3. Đọc LESSONS.md → warnings liên quan
4. Đọc NEXT-TODO.md → dependencies với task khác
5. Scan code hiện tại → identify files cần tạo/sửa
```

---

### Step 2 — SPEC

Trả lời trong 5 phút:

```
WHY     : Vấn đề/nhu cầu business
WHO     : Ai dùng? (ADMIN / EMPLOYEE / API / SYSTEM)
WHAT    : 3-5 bullet tính năng cụ thể
DONE    : Acceptance criteria đo lường được
FILES   : List đầy đủ files cần tạo/sửa
DEPS    : Prerequisites (task/migration/service nào phải xong trước)
SIZE    : S (< 1h) | M (1-4h) | L (4h+)
```

---

### Step 3 — GENERATE BLUEPRINT

Tạo file `.agent/commands/task-NNN-[short-name].md`:

```markdown
> **Complexity: [S|M|L]**

# Task-NNN: [Feature Name]

## Step 1 — Read Rules & Docs
Đọc kỹ các file sau trước khi code:
- `GEMINI.md` (Rules: [list relevant rules])
- `LESSONS.md` (grep: [relevant keywords])
- `docs/[specific-doc].md` (Section: [specific section])

## Step 2 — Implementation

### Files to Create
[file tree với mô tả mỗi file]

### Files to Modify
[file tree + chỉ rõ thay đổi gì]

### Logic Flow
[Mô tả flow chi tiết — diagram nếu phức tạp]

### Error Handling
| Scenario | HTTP | Behavior |
|---|---|---|
| [case] | [code] | [behavior] |

## Step 3 — Tests

### Verification Commands
[Chỉ rõ từng command cần chạy]

### Test Cases
1. [Input] → [Expected Output] → [Verify How]
2. [Edge Case] → [Expected Behavior]
3. [Error Case] → [Expected Error Response]

### Acceptance Criteria
- [ ] [Criterion 1 — đo lường được]
- [ ] [Criterion 2]
- [ ] Tests pass
- [ ] No LESSONS violations

## Step 4 — Documentation
After completing:
1. Update `CHANGE_LOG.md`: version + Added/Fixed/Changed
2. Update `NEXT-TODO.md`: mark done, add new if any
3. Verify `GEMINI.md` Implementation Status
4. If new API → update `docs/API_REFERENCE.md`
```

---

### Step 4 — PROGRESS TRACKING

Cập nhật NEXT-TODO.md thành dạng bảng (học từ bkns-minicrm progress.md):

```markdown
| # | Task | Size | Status | Deps | Notes |
|---|------|------|--------|------|-------|
| T-001 | [task name] | M | ⬜ | - | |
| T-002 | [task name] | L | ⬜ | T-001 | Cần T-001 xong trước |
| T-003 | [task name] | S | ⬜ | - | |
```

Nếu có nhiều tasks → vẽ dependency graph:
```
T-001 (Foundation)
  ├── T-002 (Logic) ← depends on T-001
  │     └── T-004 (UI) ← depends on T-002
  └── T-003 (API) ← depends on T-001
```

---

## OUTPUT

```
📋 BLUEPRINT đã tạo:
  .agent/commands/task-NNN-[name].md

📊 SPEC:
  Size: [S|M|L] | Files: [N] | Tests: [N cases]
  Dependencies: [list hoặc "none"]

🔜 TIẾP THEO:
  /build [task name] → AI sẽ tự đọc blueprint và follow
```

---

## QUY TẮC

- Blueprint phải ĐỦ chi tiết để AI follow mà KHÔNG cần hỏi thêm
- Mỗi Step 3 Tests phải có ít nhất 3 test cases
- Luôn include Error Handling table cho API endpoints
- NNN tăng dần từ task trước đó (scan .agent/commands/ để biết next number)
