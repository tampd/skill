---
name: Build — Production Code Workflow (Super Skill)
description: "Quy trình viết code production-grade TDD-first: blueprint → LESSONS check → Qdrant recall → spec → TDD (RED→GREEN→REFACTOR) → 2-stage review → commit → record. v5.0 Hybrid: TDD Iron Law + 5-Layer Memory. Kích hoạt bằng /build [task]."
---

# /build [task description]

> **SUPER SKILL v5.0** — TDD-First Production Code
>
> **Triết lý**: Test-driven, spec-driven, memory-backed.
> Mọi dòng code phải: **tested first**, verified, documented.

> [!CAUTION]
> **TDD IRON LAW**: Viết code trước test? → XÓA code. Bắt đầu lại. Không ngoại lệ.

---

## HAI CHẾ ĐỘ HOẠT ĐỘNG

| Chế độ | Khi nào | Bước thực hiện |
|---|---|---|
| **Blueprint Mode** | Có file `.agent/commands/task-*.md` hoặc `changes/<name>/` | Theo blueprint/spec chính xác |
| **Direct Mode** | Không có blueprint | 8 bước chuẩn bên dưới |

```
NẾU có blueprint hoặc change folder:
  → Đọc spec + tasks → follow chính xác
  → Vẫn BẮT BUỘC TDD cho mỗi task
  → Vẫn chạy Step 0 LOAD + Step 4 TDD + Step 7 RECORD

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

5. ⭐ QDRANT RECALL (Layer 4 — nếu available):
   → qdrant_find("[task description]")
   → Lấy top 3 patterns/lessons liên quan
   → Nếu Qdrant chưa kết nối → bỏ qua, KHÔNG báo lỗi

6. ⭐ BEADS CONTEXT (Layer 5 — nếu available):
   → NẾU task bắt đầu từ Beads issue (bd-XXXX):
     bd show <id> --json → load task details, dependencies, notes
   → NẾU .beads/ tồn tại:
     bd ready --json → hiển thị tasks sẵn sàng
   → Nếu Beads chưa init → bỏ qua, KHÔNG báo lỗi
```

---

### Step 0.5 — ARCHITECTURE SPEC (BẮT BUỘC cho project/module mới ≥ 3 files)

> ⭐ Vibe code CÓ CẤU TRÚC. Không code mà chưa có spec.
> Skip nếu task nhỏ (fix/patch 1-2 files) hoặc project đã có spec.

**TRƯỚC KHI CODE BẤT KỲ FILE NÀO**, tạo file `.spec.md`:

```markdown
# [Project/Module Name] — Architecture Spec

## 1. Site Map (Routes/Pages)
## 2. Component Tree
## 3. Data Flow
## 4. File Structure Convention
## 5. Design Tokens
## 6. Tech Decisions (mini-ADR)
```

> 🛑 **RULE**: Module mới (≥ 3 files) mà CHƯA CÓ `.spec.md` → DỪNG và tạo trước.

---

### Step 1 — SPEC TASK (< 2 phút)

Trả lời 6 câu hỏi TRƯỚC KHI viết bất kỳ dòng code nào:

```
WHY   : Vấn đề đang giải quyết là gì?
WHAT  : Cần tạo/sửa file nào? [liệt kê cụ thể]
HOW   : Dùng pattern/thư viện gì?
DONE  : Acceptance criteria — khi nào biết là xong?
TEST  : Test nào viết TRƯỚC? [TDD — failing test first]
DEPS  : Cần gì trước? [migration/service/config — nếu có]
```

Với task phức tạp (> 3 files) → dùng Wave format:
```
Wave 1 — Foundation: DB schema, models, migrations
Wave 2 — Integration: Services, controllers, logic
Wave 3 — Polish: UI, validation, edge cases
```

---

### Step 2 — DEPENDENCY CHECK (Enhanced v4.1)

```
1. Files trong WHAT có phụ thuộc vào code chưa tồn tại?
   NẾU CÓ → DỪNG: "⚠️ Dependency missing: [file/service/table]"

2. Migration cần chạy trước?

3. Task trong NEXT-TODO.md có dependency nào chưa hoàn thành?

4. ⭐ BEADS DEPENDENCY CHECK (Layer 5 — nếu available):
   → bd dep list <id> --json → check blocking issues
   → bd update <id> --claim → claim task, set status in_progress
   → Nếu Beads chưa init → bỏ qua
```

---

### Step 3 — TDD CODE ⭐ (THAY ĐỔI LỚN v5.0)

> **🔥 TDD IRON LAW**: NO PRODUCTION CODE WITHOUT A FAILING TEST FIRST
> Viết code trước test? **XÓA code. Bắt đầu lại.**

#### 3.1 — RED: Viết Failing Test

```
1. Xác định behavior cần test (1 behavior = 1 test)
2. Viết test TRƯỚC — test phải THẤT BẠI vì code chưa tồn tại
3. Chạy test → XÁC NHẬN test FAIL đúng lý do
   ✅ FAIL vì "function not defined" → đúng
   ❌ FAIL vì syntax error trong test → sửa test
```

#### 3.2 — GREEN: Viết Code Tối Thiểu

```
1. Viết code TỐI THIỂU để test PASS
   - Không optimize, không refactor, không gold-plate
   - Chỉ đủ để test xanh

2. Chạy test → XÁC NHẬN test PASS
   ✅ PASS → tiếp tục REFACTOR
   ❌ FAIL → sửa code (KHÔNG sửa test để pass)
```

#### 3.3 — REFACTOR: Clean Up

```
1. Code đã PASS → giờ clean up:
   - DRY (Don't Repeat Yourself)
   - YAGNI (You Aren't Gonna Need It)
   - Đặt tên rõ ràng, xóa code thừa

2. Chạy test → XÁC NHẬN vẫn PASS
   ✅ Vẫn PASS → commit
   ❌ FAIL → undo refactor, thử lại

3. Chạy TẤT CẢ tests → không break gì
```

#### 3.4 — COMMIT

```bash
git add <test file> <implementation file>
git commit -m "feat(module): add [behavior] with test"
```

> 🔄 **LẶP LẠI** cho mỗi behavior: RED → GREEN → REFACTOR → COMMIT

#### TDD Exceptions (Hỏi user trước)
| Trường hợp | Hành động |
|---|---|
| Throwaway prototype | Hỏi user: "Skip TDD cho prototype?" |
| Config files | Không cần test |
| Generated code | Không cần test |
| UI styling (CSS only) | Visual verify thay test |
| Emergency hotfix | Viết test SAU fix, nhưng PHẢI viết |

#### Architecture Rules (Vẫn áp dụng trong GREEN phase)

**PHP/Laravel:**
```php
// ✅ DOs
- Type hinting, null-safe operators
- Service layer (logic trong Services/, KHÔNG Controller)
- DB::transaction() cho multi-step writes
- bcmath cho tiền: bcadd(), bcmul(), scale=4
- Batch pre-fetch: whereIn() trước loop

// ❌ DON'Ts → VI PHẠM = BUG
- dd(), dump() trong production
- float cho tiền
- Hardcode config values
- eval() chưa sanitize
```

**TypeScript/JavaScript:**
```typescript
// ✅ DOs
- Explicit types, optional chaining
- Async/await, error boundaries
- Prisma parameterized queries

// ❌ DON'Ts
- any type, console.log production
- eval(), magic numbers
```

**CSS/Blade:**
```
// ✅ CSS variables: var(--bg-surface)
// ❌ Hardcode: rgba(30,41,59,0.8)
```

---

### Step 4 — VERIFY (Empirical Evidence)

> Không chấp nhận "nó chạy rồi" — phải có **bằng chứng cụ thể**

| Loại thay đổi | Evidence bắt buộc |
|---|---|
| API endpoint | `curl` response output |
| DB migration | `migrate:status` output |
| Service logic | Test results (pass/fail count) |
| UI component | Browser test hoặc screenshot |
| Database op | Query verify output |

```
EVIDENCE LOG:
┌─────────────────────────────────────┐
│ ✅ All tests PASS: N/N             │
│ ✅ curl response 200 + correct JSON │
│ ✅ Screenshot: [mô tả]             │
└─────────────────────────────────────┘
```

---

### Step 5 — SPEC COMPLIANCE CHECK ⭐ (MỚI v5.0)

> Từ Superpowers: kiểm tra code có đúng spec không TRƯỚC khi review quality.

```
1. So sánh code với SPEC (Step 1) hoặc change proposal:
   - Thiếu requirement nào? → implement
   - Code thừa (không ở spec)? → xóa hoặc tạo ticket mới
   - Behavior khác spec? → sửa lại

2. KẾT QUẢ:
   ✅ Spec compliant — all requirements met, nothing extra
   ❌ Issues: [liệt kê]
```

---

### Step 6 — ATOMIC COMMIT

```bash
# Format bắt buộc — conventional commits:
git add <files của task này>
git commit -m "feat(module): mô tả ngắn gọn ≤ 72 ký tự"

# Types: feat | fix | chore | docs | perf | sec | test
```

> 🔑 **1 TDD cycle = 1 commit**. Không gom nhiều behaviors.

---

### Step 7 — RECORD (Lessons + State + Beads + Qdrant)

```
1. Cập nhật ACTIVE_CONTEXT.md:
   - Mark task vừa commit: [x]
   - Set NEXT IMMEDIATE ACTION = task tiếp theo

2. Phiên có bug/bài học mới?
   NẾU CÓ → ghi LESSONS.md ngay (format: ✅❌ code examples)
   NẾU KHÔNG → tiếp task tiếp theo

3. ⭐ BEADS CLOSE (Layer 5 — nếu available):
   → bd close <id> --reason "Completed: [commit message]" --json
   → Nếu Beads chưa init → bỏ qua

4. ⭐ QDRANT STORE (Layer 4 — nếu available):
   → qdrant_store(content, {project, type, tags, date})
   → Nếu Qdrant chưa kết nối → bỏ qua
```

---

## WAVE EXECUTION (Cho task phức tạp)

```
🌊 Wave 1 — Foundation (TDD cho mỗi model/migration)
🌊 Wave 2 — Integration (TDD cho mỗi service/controller)
🌊 Wave 3 — Polish (TDD cho edge cases, visual verify cho UI)
```

> **Rule**: Hoàn thành 100% Wave N TRƯỚC KHI bắt đầu Wave N+1.

---

## CHECKLIST TRƯỚC KHI COMMIT (BẮT BUỘC)

```
[ ] Đã đọc LESSONS.md trước khi code
[ ] Test viết TRƯỚC code (TDD)
[ ] Tất cả tests PASS
[ ] Spec compliance: đúng spec, không code thừa
[ ] Không có dd()/dump()/console.log
[ ] Không hardcode magic numbers
[ ] Null-safe operators
[ ] DB transaction cho multi-step writes
[ ] Monetary calculations dùng bcmath/Decimal
[ ] Input validate và escape
[ ] CSS dùng variables
[ ] Commit message conventional commits
[ ] Evidence attached
```
