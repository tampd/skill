---
name: build
description: "Feature implementation and code writing. Use for /build (implement feature with TDD), /plan (plan before code), /search (research), /gsd (full GSD cycle). Triggers on: viết code, implement, tạo feature, build, tdd, xây dựng, gsd."
---

# Build Skill — Search-First + TDD + Memory-Enhanced Build (v4.2)

> 📂 Chi tiết bổ sung: `build/references/patterns.md` | `build/references/parallel-guide.md`

---

## /plan [idea or feature]
> Brainstorm và lập kế hoạch TRƯỚC khi viết bất kỳ dòng code nào

```
STEP 1 — UNDERSTAND
  □ Parse requirement: what exactly needs to be built?
  □ Clarifying questions (hỏi nếu mơ hồ, không đoán)
  □ Define success criteria rõ ràng

STEP 2 — BRAINSTORM GATE (Bắt buộc)
  □ Đề xuất ≥2 approaches KHÁC NHAU:
      "Approach A: [mô tả] — Pros/Cons"
      "Approach B: [mô tả] — Pros/Cons"
  □ Hỏi user chọn TRƯỚC KHI tiếp tục
  □ Check INSIGHTS.md: có compound insight liên quan?

STEP 3 — SPEC (GSD Methodology)
  WHY:  [vấn đề đang giải quyết]
  WHAT: [2-3 bullet tính năng cụ thể]
  HOW:  [pattern/library nào]
  DONE: [acceptance criteria]
  FILES: [list file cần tạo/sửa]

STEP 4 — WAVE PLAN
  Wave 1: Foundation (data, schema, base structure)
  Wave 2: Integration (logic, API, connections)
  Wave 3: Polish (UI, UX, edge cases, tests)

STEP 5 — ANTIGRAVITY TASK LIST
  □ Break down thành numbered tasks có checkpoint
  □ Identify tasks có thể parallel (→ references/parallel-guide.md)
  □ Estimate: small (<1h) / medium (1-3h) / large (>3h)
  □ Nếu large → đề xuất /spec trước

OUTPUT: Structured plan, task list, file list — TRƯỚC khi bất kỳ code nào được viết
```

---

## /gsd [feature]
> Full GSD (Get Shit Done) cycle — Discuss → Plan → Execute → Verify
> Dùng cho features lớn, cần fresh context giữa các phases

```
PHASE D — DISCUSS
  □ Thảo luận UI/UX, behavior decisions với user
  □ Clarify edge cases, error handling strategy
  □ Define: what does "done" look like?
  □ Output: Decisions document (ghi vào ACTIVE_CONTEXT.md)

PHASE P — PLAN
  □ Research: /search relevant packages/patterns
  □ Viết SPEC (WHY/WHAT/HOW/DONE/FILES)
  □ Break thành tasks bằng /plan
  □ Output: Task list + file list + dependencies

  ⚡ CONTEXT CHECKPOINT: Nếu context >50% → /save + fresh session

PHASE E — EXECUTE
  □ Implement theo /build protocol (7-Step)
  □ Mỗi task = 1 atomic commit
  □ Context health check mỗi 3-5 tasks
  □ Output: Working code + passing tests

  ⚡ CONTEXT CHECKPOINT: Nếu context >60% → commit, /save, resume fresh

PHASE V — VERIFY
  □ Run full test suite
  □ Diff review: mọi file đã thay đổi
  □ Regression check: impact analysis
  □ Browser verify nếu UI changes
  □ Confirm spec met: checklist từ Phase D
  □ Output: Verification evidence

🛑 KHÔNG skip Phase V — đây là gate chất lượng cuối cùng
```

---

## /search [need]
> Research packages, solutions, và existing patterns

```
SEARCH PROTOCOL — 4 Steps:

  1. INTERNAL SEARCH (codebase + memory):
     □ grep -r "[keyword]" --include="*.ts" .
     □ Check LESSONS.md + INSTINCTS.md + INSIGHTS.md
     □ Qdrant (nếu available): qdrant_find("[search context]")

  2. EXTERNAL SEARCH (packages/docs):
     □ npm/pip/github + official docs
     □ Evaluate top 3 candidates (→ references/patterns.md)

  3. DECISION MATRIX:
     official → popular → specialized → custom (document why)

  4. OUTPUT:
     "🔍 Found: [kết quả] → Sẽ dùng pattern [X]"
     "🔍 Not found: → Sẽ implement từ đầu theo pattern [Y]"
```

---

## /build [task]
> Implement feature với APEX 7-Step Build Protocol

```
BƯỚC 0 — MEMORY LOAD (v4.2 Biomimetic Retrieval)
  □ Đọc ACTIVE_CONTEXT.md → snapshot hiện tại
  □ Check theo memory type ưu tiên (Rule 26):
     Building feature → world (facts/rules) + mental_model (patterns)
     Fixing bug → experience (past fixes) + mental_model (patterns)
     Design/UI → world (design tokens) + mental_model (UX patterns)
  □ Qdrant (nếu available): qdrant_find("[task keywords]")
    → Filter by relevant memory_type for task context
  □ Auto-memory: check .ai/memory/MEMORY.md
  🛑 STOP: Nếu chưa hoàn thành Bước 0, KHÔNG code.

BƯỚC 1 — SEARCH FIRST
  □ /search đã chạy chưa? (nếu chưa → chạy trước)
  □ Task ≥3 files? → /spec hoặc /plan bắt buộc trước

BƯỚC 2 — SPEC (30 giây)
  WHY / WHAT / HOW / DONE

BƯỚC 3 — CODE (Clean + Typed)
  □ TypeScript interfaces TRƯỚC → function signatures → implement

BƯỚC 4 — TDD (Red → Green → Refactor)
  □ Test TRƯỚC code nếu feature phức tạp
  □ RED → GREEN → REFACTOR → Coverage ≥80%

BƯỚC 5 — VERIFY
  □ lint → format → type-check → test → audit
  □ Diff review + regression check

BƯỚC 6 — CLEANUP PASS (separate pass)
  □ Scan: console.log, debugger, TODO cũ, magic numbers, dead code

BƯỚC 7 — ATOMIC COMMIT
  □ git commit -m "feat(module): mô tả"
  □ Update ACTIVE_CONTEXT.md

  → Chi tiết checklists: references/patterns.md
```

---

## ULTRATHINK MODE 🧠
> Kích hoạt deep reasoning cho tasks phức tạp

```
TRIGGER: Prefix task với "ultrathink:" hoặc khi:
  - Architecture decisions ảnh hưởng >5 files
  - Complex bugs không reproduce được sau 15 phút
  - Refactoring ảnh hưởng core abstractions
  - Trade-off decisions không có đáp án rõ ràng

PROTOCOL:
  1. Allocate maximum reasoning tokens
  2. Present FULL analysis plan (≥500 words) TRƯỚC khi code
  3. List ≥3 alternatives với pros/cons/risks
  4. Wait for user approval trước khi execute
  5. Document decision trong ACTIVE_CONTEXT.md

OUTPUT: Detailed plan → User approve → Execute
```

---

## CONTEXT HEALTH MONITOR 📊

```
Giám sát context window health trong suốt build session:

  🟢 Fresh  (0-30%):  Full capability, all tasks OK
  🟡 Loaded (30-60%): Monitor, tạo checkpoints thường xuyên
  🔴 Heavy  (60-80%): Commit, /save + fresh session cho complex tasks
  💀 Critical (>80%): MUST save + restart TRƯỚC khi tiếp tục

ACTION khi context Heavy/Critical:
  1. Commit tất cả work hiện tại
  2. Update ACTIVE_CONTEXT.md (checkpoint)
  3. Gợi ý user /save + fresh session
  4. Resume từ ACTIVE_CONTEXT.md trong session mới
```

---

## PARALLEL BUILD (Feature lớn >3h)

```
→ Chi tiết: references/parallel-guide.md

  Agent A: Backend / API layer
  Agent B: Frontend / UI components
  Agent C: Tests / documentation

Sync point: Trước khi merge, chạy /review trên combined output
```
