---
name: learn
description: Continuous learning, instinct tracking, and pattern extraction. Use for /learn (capture observation), /instinct (view/manage instincts), /evolve (cluster instincts into skills), /review-instincts (validate and prune). Triggers on "học hỏi", "pattern", "instinct", "lesson", "bài học", "ghi nhớ", "learn", "extract pattern".
---

# Learn Skill — Instinct System + Feedback Loop + Pattern Evolution

## INSTINCTS.md Format
```yaml
# INSTINCTS.md
instincts:
  - id: INS-001
    pattern: "Khi dùng useEffect với async, luôn dùng cleanup function"
    context: "React, async data fetching"
    confidence: 0.85
    created: 2025-01-15
    last_applied: 2025-03-10
    apply_count: 12
    success_count: 11
    failure_count: 1
    counter_examples:
      - "Không áp dụng khi effect chỉ chạy một lần và không có cleanup cần thiết"
    source: "Bug INS-001-source: memory leak trong ProductList"
```

---

## /learn [observation]
> Capture bài học mới từ session hiện tại

```
PROCESS:
  1. IDENTIFY: đây là pattern hay one-off?
     Pattern nếu: có thể áp dụng ≥3 tình huống khác → thêm vào INSTINCTS
     One-off nếu: specific quá → chỉ thêm vào LESSONS

  2. CATEGORIZE:
     coding-pattern / architecture / debugging / workflow / security / performance

  3. FORMULATE (nếu là instinct):
     "Khi [trigger context], luôn/nên [action] vì [reason]"
     → Phải actionable, không abstract
     → Phải falsifiable (có thể sai trong một số case)

  4. APPEND (không overwrite):
     INSTINCTS.md: new entry với confidence: 0.5 (unproven)
     LESSONS.md: narrative description + context

  5. CONFIRM:
     □ Pattern rõ ràng, không mơ hồ?
     □ Counter-example nào có thể xảy ra?
     □ Có conflict với instinct nào đã có không?
```

---

## /instinct [action]
> Xem và quản lý instinct library

```
/instinct → list all (sorted by confidence desc)
/instinct apply [INS-ID] → apply và log result
/instinct validate [INS-ID] [correct|wrong|partial] → update confidence
/instinct show [INS-ID] → full detail + history
/instinct prune → list candidates for removal (confidence <0.3)
/instinct export → export sang JSON/YAML
/instinct import [file] → import instincts từ external source
```

**Display format:**
```
INS-001 [0.85] ████████░ "useEffect async cleanup"         React     Applied: 12x
INS-002 [0.72] ███████░░ "Validate input trước DB call"    Backend   Applied: 8x
INS-003 [0.45] ████░░░░░ "CSS Grid cho complex layouts"    Frontend  Applied: 4x
INS-004 [0.28] ██░░░░░░░ [CANDIDATE FOR REMOVAL]           N8N       Applied: 3x
```

---

## INSTINCT VALIDATION CYCLE (tự động trong /save)
```
SAU MỖI TASK — khi /save chạy:

  1. RECALL: instinct nào đã được apply trong session?
     (check log "Applied instinct #X to task Y")

  2. EVALUATE từng applied instinct:
     → Kết quả: correct / wrong / partial?

  3. UPDATE CONFIDENCE:
     correct:  confidence = min(confidence + 0.10, 1.0)
     partial:  confidence = confidence + 0.02 (học được nhưng cần refinement)
     wrong:    confidence = max(confidence - 0.20, 0.0)
               + thêm counter-example vào instinct entry

  4. PRUNE CANDIDATES:
     Nếu confidence < 0.30 sau ≥5 lần apply → flag for review
     Nếu confidence = 0.00 → auto-archive (không xóa, chỉ deactivate)

  5. SURFACE NEW PATTERNS:
     Trong session, có gì lặp lại ≥2 lần mà chưa có instinct?
     → Propose new instinct (user confirm trước khi add)
```

---

## /evolve
> Cluster related instincts thành formal skill section

```
PROCESS:
  1. GROUP: identify instincts có cùng domain/context
  2. THRESHOLD: nhóm ≥3 instincts liên quan → candidate for evolution
  3. DRAFT: viết skill section từ instinct cluster
  4. INTEGRATE: merge vào skill phù hợp (build/fix/craft/...)
  5. RETIRE: instincts đã evolved → đánh dấu "promoted to skill"

VÍ DỤ:
  INS-001 + INS-008 + INS-015 (cả 3 về React performance)
  → Evolve thành section "React Performance Patterns" trong craft/SKILL.md
  → Original instincts vẫn giữ nhưng marked "promoted"
```

---

## /review-instincts
> Periodic review — prune noise, promote high-confidence

```
REVIEW CHECKLIST (chạy monthly):

HIGH CONFIDENCE (≥0.85):
  □ Promote candidates: đưa thành rule hoặc /evolve thành skill section
  □ Document with examples để onboarding mới

MEDIUM CONFIDENCE (0.50-0.84):
  □ Đang learn: tiếp tục apply và validate
  □ Có cần clarify context không?

LOW CONFIDENCE (0.30-0.49):
  □ Còn 5 lần apply nữa → quyết định keep/drop
  □ Context có quá narrow không?

PRUNE CANDIDATES (<0.30, ≥5 lần apply):
  □ Xem lại counter-examples
  □ Archive hoặc rewrite (đổi context/wording)
  □ Không xóa — chỉ deactivate (history preserved)

OUTPUT: INSTINCTS-REVIEW-[date].md với quyết định cho từng instinct
```

---

## LESSONS.md Template
```markdown
# Lessons Learned

## [Date] — [Brief Title]

**Context:** [project/task này liên quan đến gì]

**What happened:** [mô tả tình huống]

**What worked:** [những gì hiệu quả]

**What didn't work:** [những gì không hiệu quả + tại sao]

**Key insight:** [1-2 câu distill bài học cốt lõi]

**Instinct created:** INS-XXX (nếu có)

---
```
