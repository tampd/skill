---
name: session
description: Session lifecycle management. Use for /start (begin session), /save (end session), /checkpoint (save mid-session), /review (multi-perspective code review). Triggers on "bắt đầu phiên", "kết thúc phiên", "lưu context", "review code", "start session", "save session".
---

# Session Skill — Lifecycle + Quality Gate + Review

## /start [task]
> Khởi động phiên làm việc, load memory, set context

```
STEP 1 — LOAD MEMORY
  □ Đọc LESSONS.md (bài học cũ)
  □ Đọc INSTINCTS.md (patterns + confidence)
  □ Đọc STATE.md (project state)
  □ Nếu ACTIVE_CONTEXT.md tồn tại → đọc (phiên chưa lưu)

STEP 2 — ORIENT
  □ Tóm tắt: project hiện tại là gì?
  □ Tóm tắt: last session làm gì, kết quả ra sao?
  □ Liệt kê: top 3 open issues / blockers

STEP 3 — SET TASK
  □ Parse [task] input
  □ Áp dụng SELF-REASONING GATE:
      (a) Đây có phải cách tiếp cận đơn giản nhất không?
      (b) Có cần /spec trước không? (≥3 files → YES)
      (c) Có security risk nào không?
  □ Tạo ACTIVE_CONTEXT.md với: task, approach, risks, checklist

STEP 4 — ANTIGRAVITY PLAN
  □ Generate task-list có numbered checkpoints
  □ Nếu task lớn → đề xuất spawn parallel sub-agents
  □ Output: plan rõ ràng trước khi bắt đầu code
```

**Output:** `ACTIVE_CONTEXT.md` + task plan với checkpoints

---

## /checkpoint
> Save mid-session state, không kết thúc phiên

```
□ Update ACTIVE_CONTEXT.md:
    - Đã làm gì (completed steps)
    - Đang ở đâu (current step)
    - Còn lại gì (remaining steps)
    - Blockers / notes
□ Chạy VERIFICATION LOOP nhanh:
    lint → type-check → test (nếu applicable)
□ Ghi timestamp
```

---

## /save
> Kết thúc phiên, lưu tất cả vào persistent memory

```
STEP 1 — VERIFICATION LOOP (bắt buộc trước khi save)
  □ Run: lint → format → type-check → test
  □ Nếu FAIL → FIX trước, không save với broken state
  □ Run CLEANUP PASS: tìm console.log, TODO, hardcoded values
  □ Commit nếu có changes (conventional commit format)

STEP 2 — EXTRACT LEARNINGS
  □ Trong phiên này, gặp vấn đề gì?
  □ Giải pháp nào hiệu quả? Cái nào không?
  □ Append vào LESSONS.md (never overwrite)

STEP 3 — UPDATE INSTINCTS
  □ Pattern nào được confirm? → confidence += 0.1
  □ Pattern nào sai? → confidence -= 0.2 + thêm counter-example
  □ Pattern mới nào xuất hiện? → thêm vào INSTINCTS.md (confidence: 0.5)

STEP 4 — UPDATE STATE
  □ Cập nhật STATE.md: current status, next steps, blockers
  □ Cập nhật CHANGELOG.md: những gì đã thay đổi

STEP 5 — CLEANUP
  □ Xóa ACTIVE_CONTEXT.md
  □ README sync check (có cần update không?)
```

---

## /review [scope]
> Multi-perspective code review — 3 góc nhìn độc lập

```
INPUT: [scope] = file path, PR, hoặc "last changes"

PERSPECTIVE 1 — SECURITY 🔴
  □ Injection vulnerabilities (SQL, XSS, SSRF)
  □ Authentication / authorization gaps
  □ Sensitive data exposure (logs, errors, responses)
  □ Dependency vulnerabilities (outdated packages)
  □ Security headers present?
  □ Secrets hardcoded?

PERSPECTIVE 2 — PERFORMANCE 🔵
  □ Unnecessary re-renders (React) / N+1 queries
  □ Missing indexes, unoptimized queries
  □ Bundle size impact (new imports?)
  □ Memory leaks (event listeners, subscriptions)
  □ Core Web Vitals impact (LCP, CLS, FID)
  □ Caching opportunities missed?

PERSPECTIVE 3 — MAINTAINABILITY 🟢
  □ Functions >30 lines → đề xuất split
  □ Duplicated logic (DRY violations)
  □ Missing/outdated comments trên complex logic
  □ Type safety (any, unknown không có guard)
  □ Error handling đầy đủ không?
  □ Test coverage adequate?

OUTPUT FORMAT:
  🔴 SECURITY ISSUES (phải fix trước khi merge):
  🔵 PERFORMANCE ISSUES (nên fix):
  🟢 MAINTAINABILITY (có thể fix sau):
  ✅ APPROVED nếu không có critical issues
```

---

## VERIFICATION LOOP (dùng trong tất cả workflows)
```bash
# Thứ tự bắt buộc — dừng ngay khi có lỗi
1. lint (eslint/biome/ruff)
2. format check (prettier/black)
3. type-check (tsc/mypy)
4. test (jest/pytest/vitest) — coverage check
5. security audit (npm audit/pip-audit)

# Chỉ sau khi tất cả PASS mới tiếp tục
```
