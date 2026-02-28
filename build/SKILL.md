---
name: Build — Production Code Workflow (Super Skill)
description: "Quy trình viết code production-grade: từ blueprint → LESSONS check → spec → dependencies → code → test → verify → commit → record. Thay thế vibe-code + gsd + typescript-pro + react + nodejs + frontend-scaffold. Kích hoạt bằng /build [task]."
---

# /build [task description]

> **SUPER SKILL** — Thay thế 7 skills cũ: vibe-code, gsd, typescript-pro, react-modernization, nextjs-app-router-patterns, nodejs-backend-patterns, frontend-scaffold
>
> **Triết lý**: Production code, không phải vibe code. Mọi dòng code phải: tested, verified, documented.

---

## HAI CHẾ ĐỘ HOẠT ĐỘNG

| Chế độ | Khi nào | Bước thực hiện |
|---|---|---|
| **Blueprint Mode** | Có file `.agent/commands/task-*.md` | Theo blueprint chính xác |
| **Direct Mode** | Không có blueprint | 8 bước chuẩn bên dưới |

```
NẾU /start gợi ý blueprint:
  → Đọc .agent/commands/task-NNN-name.md
  → Theo Step 1-4 của blueprint (Read → Implement → Test → Docs)
  → Skip Step 1 SPEC (blueprint đã có)
  → Vẫn chạy Step 0 LOAD + Step 4 TEST + Step 7 RECORD

NẾU không có blueprint:
  → Chạy 8 bước đầy đủ bên dưới
```

---

## QUY TRÌNH 8 BƯỚC (Direct Mode)

### Step 0 — LOAD CONTEXT (KHÔNG SKIP)

```
1. Đọc LESSONS.md → grep entries liên quan [task description]
   🛑 NẾU CHƯA ĐỌC LESSONS → DỪNG. Quy tắc không có ngoại lệ.

2. Đọc GEMINI.md → Architecture Rules áp dụng cho task này

3. Đọc ARCHITECTURE.md/docs → patterns hiện có, không invent pattern mới

4. NẾU có ACTIVE_CONTEXT.md → restore working state
```

**LESSONS áp dụng cho task này:**
```
Liệt kê ở đây (ví dụ):
⚠️ #WARN-002 — case-insensitive string comparison cho DB values
⚠️ #WARN-005 — FK constraint xóa theo thứ tự con → cha
```

---

### Step 1 — SPEC TASK (< 2 phút)

Trả lời 6 câu hỏi TRƯỚC KHI viết bất kỳ dòng code nào:

```
WHY   : Vấn đề đang giải quyết là gì?
WHAT  : Cần tạo/sửa file nào? [liệt kê cụ thể]
HOW   : Dùng pattern/thư viện gì?
DONE  : Acceptance criteria — khi nào biết là xong?
TEST  : Làm sao verify? [curl/test/screenshot — BẮT BUỘC]
DEPS  : Cần gì trước? [migration/service/config — nếu có]
```

> ⭐ **MỚI so với vibe-code cũ**: TEST và DEPS là bắt buộc, không optional.

Với task phức tạp (> 3 files) → dùng Wave format:
```
Wave 1 — Foundation: DB schema, models, migrations
Wave 2 — Integration: Services, controllers, logic
Wave 3 — Polish: UI, validation, edge cases, tests
```

---

### Step 2 — DEPENDENCY CHECK (MỚI — từ bkns-minicrm)

```
1. Files trong WHAT có phụ thuộc vào code chưa tồn tại?
   NẾU CÓ → DỪNG: "⚠️ Dependency missing: [file/service/table]"
   → Gợi ý: implement dependency trước, hoặc viết stub

2. Migration cần chạy trước?
   → php artisan migrate:status / npx prisma migrate status

3. Task trong NEXT-TODO.md có dependency nào chưa hoàn thành?
   NẾU CÓ → cảnh báo và hỏi user có tiếp tục không
```

---

### Step 3 — CODE (Architecture Rules Enforced)

#### PHP/Laravel Rules
```php
// ✅ DOs — BẮT BUỘC
- Type hinting: function calculate(int $month): array
- Null-safe: $value = $data['key'] ?? 'default';
- Service layer: logic trong app/Services/, KHÔNG trong Controller
- DB Transaction: DB::transaction(fn() => ...);
- Bcmath cho tiền: bcadd($a, $b, 4), bcmul($a, $b, 4)
- Batch pre-fetch: whereIn('ma_nv', $list) trước loop → tránh N+1
- Escape wildcard: str_replace(['%','_'], ['\\%','\\_'], $search)

// ❌ DON'Ts — VI PHẠM = BUG
- dd(), dump() trong production code
- Hardcode giá trị salary/config → luôn dùng SystemConfig/PayrollFormula
- passthru() → dùng Symfony\Process
- eval($userInput) chưa sanitize → dùng FormulaService 3-layer
- float cho tiền → luôn bcmath
```

#### TypeScript/JavaScript Rules
```typescript
// ✅ DOs
- Explicit types: const fn = (id: number): Promise<User> => ...
- Optional chaining: user?.profile?.avatar ?? '/default.png'
- Error boundaries: try/catch với typed errors
- Async/await thay vì callback hell
- Prisma parameterized queries → KHÔNG raw SQL

// ❌ DON'Ts
- any type → dùng unknown hoặc generic
- console.log trong production → dùng logger
- eval() / new Function() → dùng safe expression evaluator
- Magic numbers → đặt const/config có tên
```

#### CSS/Blade Rules
```
// ✅ DOs
- Dùng CSS variables: var(--bg-surface), var(--text-primary)
- Responsive: test 1024/768/480px breakpoints

// ❌ DON'Ts
- Hardcode dark colors: rgba(30,41,59,0.8) → dùng var(--bg-card)
- Inline styles cho theme colors → dùng design system
```

---

### Step 4 — TEST (BẮT BUỘC — MỚI)

> ⭐ **Đây là sự khác biệt lớn nhất so với vibe-code cũ.**
> Mỗi task PHẢI có ít nhất 1 verification method.

| Loại thay đổi | Test bắt buộc |
|---|---|
| API endpoint mới | `curl -X POST URL -d data` → verify response |
| Laravel migration | `php artisan migrate:status` |
| Service logic | `php artisan test --filter TestName` hoặc manual verify |
| UI component | Browser test hoặc screenshot |
| Database operation | Query verify: `SELECT COUNT(*) FROM table` |
| Import/Export | File output sample check |
| N8N webhook | Trigger test từ N8N |

```
TEST RESULTS:
✅ [test name] — PASS [evidence]
✅ [test name] — PASS [evidence]
❌ [test name] — FAIL [reason] → FIX trước khi tiếp tục
```

> 🛑 Nếu TEST FAIL → sửa code, chạy lại. KHÔNG commit code chưa pass test.

---

### Step 5 — VERIFY (Empirical Evidence)

Không chấp nhận "nó chạy rồi" — phải có **bằng chứng cụ thể**:

```
EVIDENCE LOG:
┌─────────────────────────────────────┐
│ ✅ curl response 200 + correct JSON │
│ ✅ migrate:status output attached   │
│ ✅ Screenshot: [mô tả]             │
│ ✅ Test output: 5/5 pass            │
└─────────────────────────────────────┘
```

---

### Step 6 — ATOMIC COMMIT

```bash
# Format bắt buộc — conventional commits:
git add <files của task này>
git commit -m "feat(module): mô tả ngắn gọn ≤ 72 ký tự"

# Types:
# feat  — tính năng mới
# fix   — sửa bug
# chore — task kỹ thuật (refactor, config)
# docs  — chỉ cập nhật tài liệu
# perf  — tối ưu performance
# sec   — fix security
# test  — thêm/sửa test
```

> 🔑 **1 task = 1 commit**. Không gom nhiều task.

---

### Step 7 — RECORD (Lessons + State)

```
1. Cập nhật ACTIVE_CONTEXT.md:
   - Mark task vừa commit: [x]
   - Set NEXT IMMEDIATE ACTION = task tiếp theo

2. Phiên có bug/bài học mới?
   NẾU CÓ → ghi LESSONS.md ngay (format: triệu chứng + gốc + fix + ✅❌ code)
   NẾU KHÔNG → tiếp task tiếp theo

3. Cập nhật STATE.md (nếu dùng Wave):
   - Wave hiện tại, task vừa xong, task tiếp theo
```

---

## WAVE EXECUTION (Cho task phức tạp)

```
🌊 Wave 1 — Foundation
   DB schema, models, migrations, core types
   → Commit khi hoàn thành 100%

🌊 Wave 2 — Integration
   Services, controllers, API endpoints, business logic
   → Commit khi hoàn thành 100%

🌊 Wave 3 — Polish
   UI, validation, error handling, edge cases, tests
   → Commit khi hoàn thành 100%
```

> **Rule**: Hoàn thành 100% Wave N TRƯỚC KHI bắt đầu Wave N+1.

---

## BLUEPRINT MODE (Khi có .agent/commands/task-*.md)

```
Blueprint format (tạo bởi /plan):

## Step 1 — Read (Docs cần đọc)
- GEMINI.md (Rules: #3, #5)
- LESSONS.md (grep: payment, formula)
- docs/ARCHITECTURE.md (Section: Services)

## Step 2 — Implementation
### Files to Create/Modify
  [file tree + logic flow + error handling]

## Step 3 — Tests
### Test Cases (Liệt kê cụ thể)
  1. [input] → [expected output]
  2. [edge case] → [expected behavior]

## Step 4 — Documentation
  - Update: CHANGE_LOG, NEXT-TODO
  - Verify: docs/API_REFERENCE (nếu endpoint mới)
```

Khi chạy Blueprint Mode:
1. Đọc blueprint → follow Steps 1-4 chính xác
2. Vẫn apply Architecture Rules từ Step 3
3. Vẫn chạy Test từ Step 4 (BẮT BUỘC)
4. Vẫn chạy Record từ Step 7

---

## CHECKLIST TRƯỚC KHI COMMIT (BẮT BUỘC)

```
[ ] Đã đọc LESSONS.md trước khi code
[ ] Không có dd()/dump()/console.log trong code mới
[ ] Không hardcode magic numbers hoặc config values
[ ] Xử lý null/undefined (null-safe operators)
[ ] DB operations trong transaction nếu multi-step
[ ] Monetary calculations dùng bcmath (PHP) / Decimal (JS)
[ ] Input user đã validate và escape
[ ] CSS dùng variables — không hardcode dark colors
[ ] TEST đã pass (evidence attached)
[ ] Commit message theo conventional commits
[ ] LESSONS entry mới có ✅❌ code examples (nếu applicable)
```
