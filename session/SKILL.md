---
name: session
description: Session lifecycle management. Use for /start (begin session), /save (end session), /checkpoint (save mid-session), /review (multi-perspective code review), /recall (quick resume). Triggers on "bắt đầu phiên", "kết thúc phiên", "lưu context", "review code", "start session", "save session", "recall", "resume".
---

# Session Skill — 4-Layer Smart Retrieval + Consolidation Save + Quality Gate (v4.2)

## /start [task]
> Khởi động phiên làm việc, load 4-Layer smart memory, set context

```
APEX v4.2 Bootstrap Protocol:

LAYER 0 — CHECK ACTIVE_CONTEXT.md:
  → Có: hiển thị "📍 Đang làm: [task] | Files: [X] | Tiếp theo: [Y]"
         → /recall ngay, không cần đọc thêm
  → Không: tiếp tục bình thường

LAYER A — RULES (luôn đọc):
  □ Đọc GEMINI.md: tech stack, rules hiện tại
  □ Đọc STATE.md: phase, wave, blockers

LAYER B — CRITICAL LESSONS (luôn đọc, ≤10 entries):
  □ Đọc LESSONS.md: chỉ entries importance ≥ 0.8
  □ Nếu cần sâu hơn → đọc LESSONS_ARCHIVE.md (on-demand)

LAYER C — SEMANTIC SEARCH (Qdrant + 3-Strategy Recall v4.2):
  □ Nếu Qdrant available — 3-STRATEGY RECALL:
     Strategy 1 — Semantic: qdrant_find("[task context]")
     Strategy 2 — Keyword: grep LESSONS.md + LESSONS_ARCHIVE.md (tags + entities)
     Strategy 3 — Temporal: lessons từ 7 ngày gần nhất (recent context)
     → Merge top-5 từ cả 3 strategies (deduplicate by entry ID)
     → Ưu tiên: match ≥2 strategies > match 1 strategy
     → "🧠 Recall: [N] memories (semantic: X, keyword: Y, temporal: Z)"
  □ Nếu Qdrant chưa setup: skip (fallback Layer B)

LAYER D — AUTO-MEMORY:
  □ Đọc .ai/memory/MEMORY.md (chỉ 200 dòng đầu)
  □ Nếu có topic files liên quan → đọc on-demand

INSTINCTS + INSIGHTS:
  □ INSTINCTS.md: top-3 instincts liên quan (confidence ≥ 0.7)
  □ INSIGHTS.md: có compound insight nào liên quan task không?
  □ Nếu có: alert ngay "💡 INSIGHT liên quan: [tóm tắt]"

SET TASK:
  □ Parse [task] input
  □ Áp dụng SELF-REASONING GATE:
      (a) Đây có phải cách tốt nhất? (≥2 alternatives)
      (b) Có risk/side-effect nào đang bỏ qua?
      (c) User có cần approve không?
  □ Tạo ACTIVE_CONTEXT.md với: task, approach, risks, checklist

ANTIGRAVITY PLAN:
  □ Generate task-list có numbered checkpoints
  □ Nếu task lớn → đề xuất spawn parallel sub-agents
  □ Output: plan rõ ràng trước khi bắt đầu code

OUTPUT FORMAT:
═══════════════════════════════════════
🚀 APEX SESSION START (v4.2)
═══════════════════════════════════════
📍 Task: [task nếu có]
🏗️ Phase: [X] | Wave: [Y]
⚠️ Critical Lessons (Layer B):
  - #BUG-007 [0.8] — [tóm tắt 1 dòng]
🧠 Qdrant Relevant (Layer C):
  - [lesson/pattern liên quan từ Qdrant]
🧠 Instincts active:
  - INS-003 [0.85] — [pattern]
💡 Insights: [tóm tắt nếu có]
📊 Context Health: 🟢 Fresh | 🟡 Loaded | 🔴 Heavy | 💀 Critical
🎯 Ready. Gõ lệnh tiếp theo.
═══════════════════════════════════════
```

**Output:** `ACTIVE_CONTEXT.md` + task plan với checkpoints

---

## /recall
> Quick resume từ ACTIVE_CONTEXT.md — không cần load lại toàn bộ

```
□ Đọc ACTIVE_CONTEXT.md
□ Hiển thị: snapshot hiện tại + NEXT IMMEDIATE ACTION
□ Tiếp tục từ đúng vị trí đã checkpoint
```

---

## /checkpoint
> Save mid-session state, không kết thúc phiên

```
□ Update ACTIVE_CONTEXT.md (theo template chuẩn):
    - Task snapshot: đang làm gì, đã xong gì
    - Files: path đầy đủ + đang làm gì với file đó
    - Decisions: quyết định quan trọng đã ra phiên này
    - Blockers: nếu có
    - Lessons applied: #BUG-XXX đang cẩn thận tránh
    - NEXT IMMEDIATE ACTION: đủ cụ thể để AI tiếp tục ngay
□ Chạy VERIFICATION LOOP nhanh:
    lint → type-check → test (nếu applicable)
□ AUTO-MEMORY: ghi insights phiên này vào .ai/memory/MEMORY.md
□ Ghi timestamp
□ Output: "✅ Checkpoint #N lưu thành công lúc HH:MM"
```

---

## /save
> Kết thúc phiên, consolidate memory, push code

```
STEP 1 — MEMORY AUDIT
  □ Đọc ACTIVE_CONTEXT.md → liệt kê task xong/còn dở
  □ Task dở → cảnh báo: "⚠️ [N] task chưa xong. Tiếp tục hay lưu tạm?"

STEP 2 — VERIFICATION GATE (Evidence Before Claims)
  🛑 Iron Law: NO COMPLETION CLAIMS WITHOUT FRESH VERIFICATION EVIDENCE
  □ Run: lint → format → type-check → test (FRESH, COMPLETE)
  □ READ full output, check exit code, count failures
  □ Nếu FAIL → FIX trước, không save với broken state
  □ Run CLEANUP PASS: tìm console.log, TODO, hardcoded values
  □ ONLY claim "done" khi có evidence output

  ANTI-RATIONALIZATION TABLE:
  | Excuse                    | Reality                            |
  |---------------------------|------------------------------------|
  | "Should pass now"         | RUN verification. Evidence > hope  |
  | "I'm confident"           | Confidence ≠ evidence              |
  | "Just this once"          | No exceptions                      |
  | "Linter passed"           | Linter ≠ type-check ≠ tests        |
  | "Agent said success"      | Verify independently               |
  | "Partial check is enough" | Partial proves nothing             |

  RED FLAGS — nếu đang nghĩ từ "should", "probably", "seems to" → STOP, RUN verification

  □ Commit nếu có changes (conventional commit format)

STEP 3 — EXTRACT LEARNINGS + INGEST
  □ Trong phiên này, gặp vấn đề gì?
  □ Giải pháp nào hiệu quả? Cái nào không?
  □ Classify memory type (Rule 26):
     world = fact/rule mới phát hiện ("SvelteKit phải export load() từ +page.server")
     experience = trải nghiệm debug/build cụ thể ("Fix bug XSS bằng sanitize-html")
     mental_model = pattern tổng hợp từ ≥2 experiences ("API errors 99% do missing validation")
  □ Đánh giá Importance Score:
     ≥ 0.8 → Append vào LESSONS.md (APEX format)
     < 0.8 → Append vào LESSONS_ARCHIVE.md
  □ Never overwrite existing entries
  □ ARCHIVE CHECK: nếu LESSONS.md > 10 entries →
     di chuyển entries thấp nhất (importance < 0.8) sang LESSONS_ARCHIVE.md

STEP 4 — QDRANT EMBED (nếu available)
  □ qdrant_store: embed TẤT CẢ lessons mới (cả critical và archived)
  □ Metadata: { project, date, tags, memory_type, importance, reference,
               timestamp, related_entries }
  □ Nếu Qdrant chưa setup: skip, ghi vào LESSONS_ARCHIVE.md làm fallback

STEP 5 — CONSOLIDATION CHECK
  □ Đếm LESSONS entries kể từ /consolidate cuối
  □ Nếu ≥3: tự động chạy /consolidate trước khi save
  □ INSIGHTS.md được update

STEP 6 — UPDATE INSTINCTS
  □ Pattern nào được confirm? → confidence += 0.1
  □ Pattern nào sai? → confidence -= 0.2 + thêm counter-example
  □ Pattern mới nào xuất hiện? → thêm vào INSTINCTS.md (confidence: 0.5)

STEP 7 — AUTO-MEMORY UPDATE
  □ Ghi debugging insights, gotchas vào .ai/memory/MEMORY.md
  □ Nếu MEMORY.md > 200 dòng → tách topic files
  □ AI tự quyết nội dung (user không cần review)

STEP 8 — UPDATE STATE + CLEANUP
  □ Merge ACTIVE_CONTEXT quan trọng → STATE.md
  □ Cập nhật CHANGELOG.md: những gì đã thay đổi
  □ Archive ACTIVE_CONTEXT.md (timestamp) → Xóa ACTIVE_CONTEXT.md
  □ README sync check (có cần update không?)
  □ git push (MANDATORY — không xong = không "done")
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
