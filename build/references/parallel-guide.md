# Parallel Build & Sub-Agent Orchestration Guide

> Load on-demand khi feature lớn (>3h). Không load vào context mặc định.

## Khi Nào Dùng Parallel Build

```
Feature size > 3h estimated → xem xét parallel
Feature size > 8h estimated → BẮT BUỘC parallel
Điều kiện: tasks có thể tách rời (low coupling)
```

## Orchestrator Pattern

```
MAIN AGENT (Orchestrator):
  1. Nhận task → phân tích dependencies
  2. Split thành independent sub-tasks
  3. Assign cho sub-agents với isolated context
  4. Collect summaries (KHÔNG full output)
  5. Verify tính nhất quán + integration
  6. Merge và chạy final test suite
```

## Sub-Agent Roles

```
Agent A: Backend / API layer
  - Database schema, migrations
  - API endpoints, middleware
  - Business logic, services

Agent B: Frontend / UI components
  - Components, pages, layouts
  - State management
  - Styling, responsiveness

Agent C: Tests / documentation
  - Unit tests, integration tests
  - API documentation
  - Component storybook
```

## Rules

```
✅ Mỗi sub-agent có context riêng (fresh start)
✅ Sub-agent chỉ return summary, không full output
✅ Main agent verify tính nhất quán trước khi merge
✅ Mỗi sub-agent atomic commit riêng
❌ Sub-agents KHÔNG share context với nhau
❌ KHÔNG merge mà không chạy integration tests
```

## Sync Points

```
Checkpoint 1: Sau foundation (schema, interfaces) — verify contracts
Checkpoint 2: Sau integration (API + UI connected) — verify flow
Checkpoint 3: Sau polish (tests, docs) — final review
```

## Model Selection Strategy

```
Dùng model phù hợp task để tối ưu tốc độ + cost:

  Mechanical (1-2 files, clear spec)  → fast/cheap model
  Integration (multi-file, judgment)  → standard model
  Architecture/Design/Review          → most capable model

Signals:
  □ 1-2 files + complete spec → cheap
  □ Multi-file + integration → standard
  □ Design judgment + broad knowledge → most capable
```

## Subagent-Driven Development

```
Fresh subagent per task + 2-stage review = high quality, fast iteration

  → Chi tiết prompts: references/subagent-prompts.md
  → 3 templates: Implementer, Spec Reviewer, Code Quality Reviewer
  → 4 status levels: DONE, DONE_WITH_CONCERNS, NEEDS_CONTEXT, BLOCKED
```

