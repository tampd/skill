---
name: Plan — Ideation + Spec-Driven Task Planner (v6.0)
description: "Brainstorm → Spec-driven planning: Ideation pipeline → Qdrant recall → Research → Proposal → Specs → Design → Tasks → Beads epic. v6.0 gộp brainstorm + plan. Change Folder Structure + Delta Specs. Kích hoạt bằng /plan [feature name] hoặc /brainstorm [idea]."
---

# /plan [feature name] — cũng nhận `/brainstorm [idea]`

> **v6.0 IDEATION + SPEC-DRIVEN** — Gộp brainstorm + plan
> Mục tiêu: Từ ý tưởng mơ hồ → spec actionable → TDD tasks
> Mỗi change = 1 folder với proposal + specs + design + tasks.

---

## KHI NÀO DÙNG /plan?

| Tình huống | Dùng /plan? |
|---|---|
| Fix bug nhỏ / sửa UI | ❌ Không — `/fix` hoặc `/build` trực tiếp |
| Feature mới 1-2 files | ❌ Không — `/build` trực tiếp |
| Feature phức tạp (3+ files) | ✅ `/plan` trước, `/build` sau |
| Feature cần design decision | ✅ `/plan` + `/craft` |
| Ý tưởng mơ hồ, cần phân tích | ✅ `/plan` (= brainstorm + spec) |
| So sánh nhiều approaches | ✅ `/plan` |
| Dự án mới toàn bộ | ✅ `/plan` cho từng phase |

---

## QUY TRÌNH 6 BƯỚC

### Step 0 — IDEATION (Brainstorm — cho ý tưởng mơ hồ) 🆕 v6.0

> Bước này CHỈ chạy khi user có ý tưởng chưa rõ hoặc gọi `/brainstorm`.
> Nếu feature đã rõ ràng → nhảy sang Step 1.

```
1. CAPTURE raw idea:
   - Ghi lại CHÍNH XÁC lời user (không interpret sai)
   - Hỏi 3 câu hỏi mở:
     • "Vấn đề gốc mà bạn muốn giải quyết là gì?"
     • "Ai là người dùng chính?"
     • "Kết quả lý tưởng nhất trông như thế nào?"

2. EXPAND — Brainstorm ≥ 3 approaches:
   | # | Approach | Ưu điểm | Nhược điểm | Effort |
   |---|---|---|---|---|
   | A | [approach 1] | [pros] | [cons] | S/M/L |
   | B | [approach 2] | [pros] | [cons] | S/M/L |
   | C | [approach 3] | [pros] | [cons] | S/M/L |

3. ANALYZE approach được chọn:
   - User Stories: "Là [role], tôi muốn [action], để [benefit]"
   - Technical Feasibility: stack hiện tại support?
   - Risk Assessment: xác suất × impact
   - Dependencies: API, data, approval cần?

4. Output → Feature Spec sẵn sàng cho Step 1-5
```

### Step 1 — RESEARCH ⭐ (Brainstorming hard gate)

```
1. EXPLORE context trước khi plan:
   - Đọc GEMINI.md → tech stack, Architecture Rules
   - Đọc docs/ → patterns hiện có
   - Đọc LESSONS.md → warnings liên quan
   - Scan code hiện tại → identify files cần tạo/sửa
   - Đọc existing specs (changes/*/specs/) → context từ changes trước

2. HỎI CLARIFYING QUESTIONS:
   🛑 HARD GATE: KHÔNG tạo spec khi chưa hiểu rõ
   - Hỏi user TỪ 1 CÂU MỘT THỜI ĐIỂM
   - Multiple choice preferred, open-ended khi cần
   - Hiểu rõ: purpose, constraints, success criteria

3. PROPOSE 2-3 APPROACHES:
   - Trình bày trade-offs mỗi approach
   - Dẫn đầu bằng recommended option + lý do
   - Hỏi user chọn trước khi tạo spec

4. ⭐ QDRANT RECALL (Layer 4):
   → qdrant_find("plan: [feature name]") → patterns tương tự
5. ⭐ BEADS CONTEXT (Layer 5):
   → bd ready --json → tasks liên quan đã có
```

> 🛑 **HARD GATE** (từ Superpowers): Phải present approach VÀ được user approve TRƯỚC KHI tạo change folder.

---

### Step 2 — CREATE CHANGE FOLDER ⭐ (MỚI v5.0)

Tạo folder `changes/<feature-name>/` với 4 artifacts:

```
changes/<feature-name>/
├── proposal.md      ← WHY: Business context, impact, scope
├── specs/
│   └── spec.md      ← WHAT: Requirements & scenarios
├── design.md        ← HOW: Technical approach, architecture
├── tasks.md         ← Implementation checklist (TDD bite-sized)
└── delta-specs.md   ← Spec changes to merge into docs/ when done
```

#### proposal.md
```markdown
# [Feature Name] — Proposal

## Problem / Opportunity
[Mô tả vấn đề hoặc cơ hội business]

## Proposed Solution
[Giải pháp đã chọn (từ Step 1 approach selection)]

## Impact & Scope
- Files affected: [N] files
- Size: S | M | L
- Breaking changes: [yes/no — nếu yes, mô tả chi tiết]

## Dependencies
- [Prerequisite tasks / services / migrations]

## Success Criteria
- [ ] [Measurable criterion 1]
- [ ] [Measurable criterion 2]
```

#### specs/spec.md
```markdown
# [Feature Name] — Requirements

## Functional Requirements
1. [User CAN do X]
2. [System MUST handle Y]
3. [When Z happens, THEN W]

## Scenarios
### Happy Path
1. [Step 1] → [Expected result]
2. [Step 2] → [Expected result]

### Edge Cases
1. [Null input] → [Expected behavior]
2. [Concurrent requests] → [Expected behavior]

### Error Cases
1. [Invalid data] → [Error response]
2. [Permission denied] → [Error response]
```

#### design.md
```markdown
# [Feature Name] — Technical Design

## Architecture
[Component diagram / data flow]

## Files to Create/Modify
[File tree với mô tả mỗi file]

## Data Model
[DB schema changes nếu có]

## API Endpoints
| Method | URL | Auth | Request | Response |
|---|---|---|---|---|

## Error Handling
| Scenario | HTTP | Behavior |
|---|---|---|
```

#### tasks.md (TDD bite-sized — từ Superpowers)
```markdown
# [Feature Name] — Implementation Tasks

> **REQUIRED**: Use /build with TDD (RED→GREEN→REFACTOR) for each task.

### Task 1: [Component Name] (2-5 min)

**Files:**
- Create: `path/to/file.py`
- Test: `tests/path/test_file.py`

**Step 1: Write failing test**
[Exact test code]

**Step 2: Run test — expect FAIL**
Command: `pytest tests/path/test_file.py -v`
Expected: FAIL "function not defined"

**Step 3: Write minimal implementation**
[Exact implementation code]

**Step 4: Run test — expect PASS**
Command: `pytest tests/path/test_file.py -v`
Expected: PASS

**Step 5: Commit**
`git commit -m "feat: add [behavior] with test"`

### Task 2: [Next Component] (2-5 min)
[Same structure...]
```

#### delta-specs.md (từ OpenSpec)
```markdown
# [Feature Name] — Delta Specs

## Changes to existing specs
> These deltas will be merged into docs/ when this change is archived.

### docs/architecture.md
- ADD: [New component/service description]
- MODIFY: [Updated data flow]

### docs/api-endpoints.md  
- ADD: [New endpoint documentation]

### GEMINI.md
- ADD: [New Architecture Rule if any]
```

---

### Step 3 — PROGRESS TRACKING

#### NEXT-TODO.md (bảng tracking)
```markdown
| # | Task | Size | Status | Deps | Notes |
|---|------|------|--------|------|-------|
| T-001 | [task] | M | ⬜ | - | |
| T-002 | [task] | L | ⬜ | T-001 | |
```

#### ⭐ BEADS TASK CREATION (Layer 5 — nếu available)
```bash
bd create "[Feature Name]" -t epic -p 1 --json
bd create "[Task 1]" --parent <epic-id> -p 1 --json
bd dep add <task-2-id> <task-1-id>
```

---

### Step 4 — PRESENT & CONFIRM

```
📋 CHANGE FOLDER đã tạo:
  changes/<feature-name>/
  ├── proposal.md   (WHY)
  ├── specs/spec.md (WHAT + Scenarios)
  ├── design.md     (HOW)
  ├── tasks.md      (N tasks, TDD bite-sized)
  └── delta-specs.md (Merge when done)

📊 SPEC:
  Size: [S|M|L] | Files: [N] | Tasks: [N] | Tests: [N cases]

🔜 TIẾP THEO:
  /build [feature name] → TDD từng task trong tasks.md
```

---

### Step 5 — ARCHIVE (Khi feature DONE — gọi từ /save)

```
1. Verify: tất cả tasks trong tasks.md đã ✅
2. Merge delta-specs.md vào docs/ tương ứng
3. Move folder: changes/<name>/ → archive/YYYY-MM-DD-<name>/
4. Specs giờ phản ánh trạng thái MỚI

→ Virtuous cycle: Specs → Change → Implement → Archive → Updated Specs
```

---

## QUY TẮC

- 🛑 HARD GATE: Phải hỏi + được approve TRƯỚC KHI tạo change folder
- Tasks phải TDD bite-sized (2-5 phút mỗi task)
- Mỗi task có exact file paths, code, test commands
- Delta specs PHẢI merge sau khi done
- NNN tăng dần (scan .agent/commands/ hoặc changes/)
- Beads/Qdrant không available → bỏ qua im lặng
