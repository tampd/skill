---
name: Vibe Code — Quy trình viết code chuẩn hóa
description: "Quy trình viết code full-stack chuẩn hóa: từ đọc LESSONS.md → plan wave → code → atomic commit → verify. Kích hoạt khi bắt đầu viết feature mới, fix bug, hoặc refactor. Đảm bảo không quên bài học cũ và code đạt chất lượng mỗi lần."
---

# HƯỚNG DẪN KỸ NĂNG: VIBE CODE

## Mục tiêu
Đảm bảo mỗi lần viết code đều: **đúng → sạch → có test → có commit**. Không lan man, không bỏ sót bài học cũ.

---

## QUY TRÌNH 6 BƯỚC BẮT BUỘC

### Bước 0 — LOAD CONTEXT (KHÔNG SKIP)
```
1. Đọc LESSONS.md → tìm bài học liên quan đến task hiện tại
2. Đọc GEMINI.md / ARCHITECTURE.md → kiểm tra pattern hiện có
3. Xác định: task này thuộc Wave nào? (Foundation/Integration/Polish)
```

> 🛑 **STOP**: Nếu chưa đọc LESSONS.md, KHÔNG bắt đầu code. Rule này không có ngoại lệ.

---

### Bước 0.5 — LOAD WORKING MEMORY (MỚI)
```
1. Kiểm tra ACTIVE_CONTEXT.md có tồn tại không?
   → NẾU CÓ: đọc và hiển thị tóm tắt 3 dòng:
       "📍 Đang làm: [task] | Files: [files] | Tiếp theo: [action]"
   → NẾU KHÔNG: bỏ qua, tiếp tục Bước 1

2. Nếu có ACTIVE_CONTEXT → đọc FILES đang chỉnh để restore kỹ thuật context

3. Apply LESSONS đã ghi trong ACTIVE_CONTEXT.LESSONS section
```

> 💡 Nếu ACTIVE_CONTEXT quá cũ (> 24h) → thông báo và hỏi user có muốn reset không

---

### Bước 1 — SPEC TASK (30 giây plan)
Trước khi code, trả lời 4 câu hỏi:
```
WHY  : Tại sao cần làm task này?
WHAT : Cần thay đổi/tạo file nào?
HOW  : Dùng pattern/thư viện gì?
DONE : Khi nào biết là xong?
```

Với task phức tạp → dùng XML format từ GSD skill.

---

### Bước 2 — CODE (Theo chuẩn clean code)

#### PHP/Laravel
```php
// ✅ DOs
- Type hinting: function calculate(int $month, int $year): array
- Null-safe: $value = $data['key'] ?? 'default';
- Service layer: logic trong app/Services/, không ghi trong Controller
- DB Transaction: DB::transaction(fn() => ...);
- Bcmath cho tiền: bcadd($a, $b, 4), bcmul($a, $b, 4)
- Batch pre-fetch: whereIn('ma_nv', $list) trước loop
- Escape wildcard: str_replace(['%','_'], ['\\%','\\_'], $search)

// ❌ DON'Ts
- dd(), dump() trong production code
- Hardcode giá trị salary/config → luôn dùng SystemConfig
- passthru() → dùng Symfony\Process
- eval() user input chưa sanitize
```

#### TypeScript/JavaScript
```typescript
// ✅ DOs
- Explicit types: const fn = (id: number): Promise<User> => ...
- Optional chaining: user?.profile?.avatar ?? '/default.png'
- Error boundaries: try/catch với typed errors
- Async/await thay vì callback hell

// ❌ DON'Ts
- any type → dùng unknown hoặc generic
- console.log trong production
- Magic numbers → đặt const có tên
```

---

### Bước 3 — VERIFY (Không trust, prove it)

| Loại thay đổi | Cách verify |
|---|---|
| PHP API endpoint | `curl -X POST URL -d data` |
| Laravel migration | `php artisan migrate:status` |
| Service logic | `php artisan test --filter TestName` |
| UI component | Chụp screenshot browser |
| N8N webhook | Trigger test từ N8N |
| Import/export | Kiểm tra file output |

---

### Bước 4 — ATOMIC COMMIT

```bash
# Format bắt buộc (conventional commits):
git add <file1> <file2>  # Chỉ add file của task này
git commit -m "feat(module): mô tả ngắn gọn task đã làm"

# Types:
# feat  — tính năng mới
# fix   — sửa bug
# chore — task kỹ thuật (refactor, config, docs)
# perf  — tối ưu performance
# test  — thêm/sửa test
# sec   — fix security

# Ví dụ tốt:
git commit -m "feat(payroll): add bcmath precision for salary calculation"
git commit -m "fix(auth): prevent account lockout bypass via timing"
git commit -m "chore(deps): upgrade dompdf to 3.0"
```

> 🔑 **Quan trọng**: Mỗi task = 1 commit. Không gom nhiều task vào 1 commit lớn.

> 🧠 **Memory Hook**: Sau mỗi commit → cập nhật ngay `ACTIVE_CONTEXT.md`:
> - Mark task vừa commit là `[x]` trong COMPLETED
> - Set NEXT IMMEDIATE ACTION = task tiếp theo
> - Record decision nếu có (dùng `/checkpoint` nếu task lớn)

---

### Bước 5 — LESSONS CHECK (Sau khi xong task)

```
Câu hỏi: Phiên này có gặp bug hoặc rút ra bài học gì không?
- Nếu CÓ → ghi vào LESSONS.md ngay (dùng save/SKILL.md format)
- Nếu KHÔNG → tiếp tục task tiếp theo
```

---

### Bước 6 — UPDATE STATE

```
Cập nhật STATE.md (nếu project có GSD):
- Phase hiện tại: Phase X
- Wave hiện tại: Wave Y
- Task vừa hoàn thành: [tên task]
- Task tiếp theo: [tên task]
- Blockers: [nếu có]
```

---

## COMBO SKILLS THEO TÌNH HUỐNG

| Tình huống | Skills kích hoạt thêm |
|---|---|
| Feature TypeScript mới | `typescript-pro` |
| Feature React/Next.js | `react-modernization` + `nextjs-app-router-patterns` |
| API endpoint mới | `api-design-principles` |
| Gặp bug lạ | `bug-memory-journal` + `debugger` |
| Code chậm/N+1 | `performance-engineer` |
| Có input user→DB | `security-auditor` |
| Feature quan trọng | `e2e-testing-patterns` |

---

## CHECKLIST TRƯỚC KHI COMMIT (BẮT BUỘC)

```
[ ] Đã đọc LESSONS.md trước khi code
[ ] Không có dd(), dump(), console.log trong code mới
[ ] Không hardcode magic numbers hoặc config values
[ ] Có xử lý null/undefined (null-safe operators)
[ ] DB operations có trong transaction nếu multi-step
[ ] Monetary calculations dùng bcmath (PHP) / Decimal (JS)
[ ] Input từ user đã được validate và escape
[ ] Evidence verify đã có (test pass / curl output / screenshot)
[ ] Commit message theo format conventional commits
```
