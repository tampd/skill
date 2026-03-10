---
name: build
description: Feature implementation and code writing. Use for /build (implement feature with TDD), /plan (plan before code), /search (research packages/solutions). Triggers on "viết code", "implement", "tạo feature", "build", "tdd", "xây dựng".
---

# Build Skill — Search-First + TDD Hard Gate + Cleanup Pass

## /plan [idea or feature]
> Brainstorm và lập kế hoạch TRƯỚC khi viết bất kỳ dòng code nào

```
STEP 1 — UNDERSTAND
  □ Parse requirement: what exactly needs to be built?
  □ Clarifying questions (hỏi nếu mơ hồ, không đoán)
  □ Define success criteria rõ ràng

STEP 2 — RESEARCH (Search-First)
  □ Tìm kiếm: có package/library nào giải quyết được không?
  □ Đánh giá top 3 options: pros/cons/bundle size/maintenance
  □ Decision: build vs buy vs adapt

STEP 3 — ARCHITECTURE
  □ Component breakdown (nếu UI)
  □ Data flow diagram (nếu có state/API)
  □ File structure đề xuất
  □ Interfaces / types cần định nghĩa
  □ Edge cases + error scenarios

STEP 4 — ANTIGRAVITY TASK LIST
  □ Break down thành numbered tasks có checkpoint
  □ Identify tasks có thể parallel (spawn sub-agents?)
  □ Estimate: small (<1h) / medium (1-3h) / large (>3h)
  □ Nếu large → đề xuất /spec trước

OUTPUT: Structured plan, task list, file list — TRƯỚC khi bất kỳ code nào được viết
```

---

## /search [need]
> Research packages và solutions có sẵn

```
SEARCH PROTOCOL:
  1. Define: chính xác cần gì (input/output/constraints)?
  2. Search: npm/pip/github + official docs
  3. Evaluate top 3 candidates:
     - Weekly downloads / stars (popularity = maturity)
     - Last commit (maintenance active?)
     - Bundle size / performance overhead
     - Security: known CVEs?
     - TypeScript support?
  4. Recommend với lý do cụ thể
  5. Check compatibility: version conflicts?

DECISION MATRIX:
  official lib exists → dùng official
  no official + >1M weekly downloads → dùng popular
  niche need + small + well-maintained → dùng specialized
  nothing good → build minimal custom (document why)
```

---

## /build [task]
> Implement feature với TDD Hard Gate

```
PHASE 0 — PRE-BUILD CHECK
  □ Đọc INSTINCTS.md → có pattern liên quan?
  □ Đọc LESSONS.md → đã gặp vấn đề tương tự chưa?
  □ /search đã chạy chưa? (nếu chưa → chạy trước)
  □ Task ≥3 files? → /spec hoặc /plan bắt buộc trước

PHASE 1 — DEFINE CONTRACTS
  □ Viết TypeScript interfaces / type definitions TRƯỚC
  □ Viết function signatures (không có implementation)
  □ Viết API request/response types
  □ Review: types có đủ không?

PHASE 2 — TDD: RED
  □ Viết test file TRƯỚC khi có implementation
  □ Test cases phải cover: happy path, error cases, edge cases
  □ Run tests → confirm chúng FAIL (nếu pass → test sai)
  □ Không tiếp tục nếu test không fail

PHASE 3 — TDD: GREEN
  □ Implement code MINIMAL để tests pass
  □ Không add logic mà test chưa cover
  □ Run tests → confirm chúng PASS
  □ Coverage check: nếu <80% → thêm tests, không tiếp tục

PHASE 4 — TDD: REFACTOR
  □ Clean code: rename, extract, simplify
  □ Không thêm logic mới trong bước này
  □ Run tests → confirm vẫn PASS sau refactor

PHASE 5 — VERIFICATION LOOP
  □ lint → format → type-check → test → audit
  □ Tất cả PASS? Nếu không → fix ngay
  □ Diff review: list từng file đã thay đổi + lý do
  □ Regression check: những gì có thể bị ảnh hưởng?

PHASE 6 — CLEANUP PASS (separate review pass)
  □ Scan: console.log, debugger, TODO cũ
  □ Scan: magic numbers → extract vào constants
  □ Scan: hardcoded strings → extract vào config/i18n
  □ Scan: dead code, unused imports
  □ Scan: error handling đầy đủ (no silent failures)
  □ Đây là PASS RIÊNG — không làm cùng lúc với implementation

PHASE 7 — COMMIT
  □ Conventional commit: feat/fix/refactor/test/docs
  □ Format: type(scope): description
  □ Ví dụ: feat(auth): add JWT refresh token rotation
```

### TDD HARD GATE — Không được bypass
```
❌ KHÔNG được viết implementation trước test
❌ KHÔNG được commit khi coverage <80%
❌ KHÔNG được skip verification loop
❌ KHÔNG được bỏ qua cleanup pass
✅ Exceptions phải được ghi rõ lý do vào LESSONS.md
```

### ANTIGRAVITY PARALLEL BUILD
```
Khi feature lớn (>3h), xem xét split cho parallel agents:
  Agent A: Backend / API layer
  Agent B: Frontend / UI components
  Agent C: Tests / documentation
  
Sync point: Trước khi merge, chạy /review trên combined output
```
