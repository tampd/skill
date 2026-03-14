# Subagent Prompt Templates & Status Protocol

> Load on-demand khi dispatch subagents. Không load vào context mặc định.
> Inspired by: obra/superpowers subagent-driven-development

---

## Model Selection Strategy

Dùng model phù hợp task để tối ưu tốc độ + cost:

| Task Complexity | Model Level | Ví dụ |
|---|---|---|
| Mechanical (1-2 files, clear spec) | Fast/cheap | Implement function theo spec, add test |
| Integration (multi-file, judgment) | Standard | API endpoint + UI component + state |
| Architecture/Design/Review | Most capable | System design, code review, refactoring |

**Signals:**
- Touches 1-2 files + complete spec → cheap model
- Multi-file + integration concerns → standard model
- Design judgment + broad codebase understanding → most capable

---

## Implementer Prompt Template

```markdown
# Task: [Task title from plan]

## Context
[Relevant project context, conventions, patterns]
[File paths and current state]
[Dependencies and constraints]

## Spec
[Exact specification from plan — copy full text]

## Requirements
1. Follow TDD: write failing test → implement → verify
2. Atomic commit khi hoàn thành
3. Self-review trước khi report status
4. KHÔNG sửa code ngoài scope task

## Status Protocol
Report một trong 4 status:

- **DONE**: Task hoàn thành, tests pass, committed
- **DONE_WITH_CONCERNS**: Hoàn thành nhưng có doubts → mô tả concerns
- **NEEDS_CONTEXT**: Cần thêm thông tin → liệt kê câu hỏi cụ thể
- **BLOCKED**: Không thể hoàn thành → mô tả blocker + đề xuất giải pháp
```

---

## Spec Reviewer Prompt Template

```markdown
# Spec Compliance Review

## Original Spec
[Paste full spec from plan]

## Review Checklist
Verify TỪNG requirement trong spec:

1. □ Requirement met? (exact match, không approximate)
2. □ Nothing extra added? (YAGNI — chỉ làm đúng spec)
3. □ Edge cases handled? (từ spec, không tự thêm)
4. □ Tests cover spec requirements?

## Output Format
- ✅ COMPLIANT: Tất cả requirements met, nothing extra
- ❌ ISSUES:
  - MISSING: [requirement thiếu]
  - EXTRA: [feature thêm không trong spec]
  - WRONG: [implementation sai spec]
```

---

## Code Quality Reviewer Prompt Template

```markdown
# Code Quality Review

## Review Dimensions
1. **Correctness**: Logic đúng? Edge cases handled?
2. **Readability**: Code clear? Naming meaningful?
3. **Maintainability**: DRY? Single responsibility? Testable?
4. **Performance**: N+1 queries? Unnecessary re-renders? Memory leaks?
5. **Security**: Input validation? Auth checks? No hardcoded secrets?

## Severity Levels
- 🔴 CRITICAL: Phải fix trước khi merge (bugs, security, data loss)
- 🟡 IMPORTANT: Nên fix (performance, readability, maintainability)
- 🟢 MINOR: Nice-to-have (style, naming suggestions)

## Output Format
Strengths: [điểm tốt]
Issues:
  [severity] [file:line] [description] [suggested fix]
Verdict: ✅ APPROVED | ❌ NEEDS_WORK
```

---

## Handling Implementer Status

```
DONE → Proceed to spec review
DONE_WITH_CONCERNS → Read concerns. Correctness/scope → fix first. Observations → note and proceed.
NEEDS_CONTEXT → Provide missing context → re-dispatch SAME model
BLOCKED →
  1. Context problem → provide context, re-dispatch same model
  2. Reasoning limit → re-dispatch with MORE CAPABLE model
  3. Task too large → break into smaller pieces
  4. Plan wrong → escalate to user

🛑 NEVER ignore escalation. NEVER retry same model without changes.
```

---

## Orchestrator Checklist

```
PRE-DISPATCH:
  □ Read plan once, extract ALL tasks with full text
  □ Create TodoWrite/checklist with all tasks
  □ Note task dependencies (which must be serial vs parallel)

PER TASK:
  □ Get task text + relevant context (already extracted)
  □ Dispatch implementer subagent (fresh context)
  □ Handle status (DONE/CONCERNS/CONTEXT/BLOCKED)
  □ Dispatch spec reviewer subagent
  □ If spec issues → implementer fixes → re-review
  □ Dispatch code quality reviewer subagent
  □ If quality issues → implementer fixes → re-review
  □ Mark task complete

POST ALL TASKS:
  □ Dispatch final code reviewer for entire implementation
  □ Run full test suite
  □ Finishing workflow (merge/PR/keep)
```
