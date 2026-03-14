---
name: fix
description: "Bug fixing and debugging. Use for /fix (debug any error), error traces, crashes, unexpected behavior, failing tests. Triggers on: lỗi, bug, crash, không chạy được, error, exception, fix, debug, sửa lỗi."
---

# Fix Skill — 4-Phase Debug + Insight Match + Auto-Ingest (v4.0)

> 📂 Chi tiết bổ sung: `fix/references/patterns.md` (bug classification, common patterns, browser agent)

---

## /fix [bug description or error]
> Tìm root cause trước khi fix — không patch symptoms

```
PHASE 0 — MEMORY MATCH (v4.0 Smart Retrieval)
  □ Check INSTINCTS.md: đã gặp error pattern này chưa?
  □ Nếu có instinct match (confidence ≥0.7) → apply và log
  □ Check LESSONS.md: critical entries (importance ≥0.8) liên quan
  □ Qdrant (nếu available): qdrant_find("error: [mô tả lỗi]")
     → "🧠 Qdrant found: đã fix lỗi tương tự ở [project]!"
  □ Check INSIGHTS.md: compound insight nào liên quan?
  □ Nếu không match → tiếp tục 4-phase debug

PHASE 1 — REPRODUCE (không fix mù)
  □ Reproduce bug một cách nhất quán
  □ Xác định: LUÔN hay ĐÔI KHI? Trigger conditions?
  □ Expected vs actual behavior
  □ Screenshot / error trace / log đầy đủ

PHASE 2 — DIAGNOSE (root cause)
  □ Đọc TOÀN BỘ error message (không chỉ dòng đầu)
  □ Trace call stack từ dưới lên
  □ Check: input data, state, network/async timing, env differences
  □ 5-WHY technique → "Root cause is: ___"

  🛑 STOP: Không được bắt đầu PHASE 3 khi chưa xác nhận root cause.

  ULTRATHINK: Nếu bug không reproduce được sau 15 phút
    → Prefix "ultrathink:" → full reasoning plan TRƯỚC khi fix

PHASE 3 — FIX (targeted, minimal)
  □ SELF-REASONING GATE:
      (a) Đây có phải cách fix tốt nhất? (≥2 alternatives)
      (b) Có risk/side-effect nào đang bỏ qua?
      (c) User có cần approve không?

  RATIONALIZATION PREVENTION:
  | Excuse                            | Reality                                    |
  |-----------------------------------|--------------------------------------------|
  | "Issue is simple, skip process"   | Simple bugs have root causes too           |
  | "Emergency, no time"              | Systematic is FASTER than thrashing        |
  | "Just try this first"             | First fix sets the pattern. Do it right.   |
  | "I see the problem, let me fix"   | Seeing symptoms ≠ understanding root cause |
  | "One more fix attempt" (after 2+) | 3+ failures = architectural problem        |
  | "Multiple fixes at once"          | Can't isolate what worked. Causes new bugs |

  RED FLAGS — nếu đang nghĩ bất kỳ câu nào ở trên → STOP, return to PHASE 1

  □ Fix chỉ root cause — không "cải thiện" thêm trong cùng PR
  □ Thêm test case reproduce exact bug (regression guard)
  □ Đảm bảo fix không break existing tests

PHASE 4 — VERIFY + LEARN (APEX Enhanced)
  □ Confirm bug không còn reproduce được
  □ Run full test suite: không có new failures
  □ UI bug → Browser Agent visual verify
  □ Run VERIFICATION LOOP: lint → type-check → test → audit

  AUTO-INGEST:
  □ Ingest tagging: Summary, Entities, Topics, Importance
  □ ≥0.8 → LESSONS.md | <0.8 → LESSONS_ARCHIVE.md
  □ Qdrant store + auto-memory update
  □ "🔗 Connection với #BUG-XXX?" + "/consolidate?"

→ Bug classification & common patterns: references/patterns.md
→ Browser Agent workflow: references/patterns.md
```
