# LESSONS — Skill Repository

> **Mục đích**: Ghi nhận bugs, bài học, quy tắc rút ra. KHÔNG BAO GIỜ xóa entries.
> **Format**: #BUG-NNN / #WARN-NNN — APEX format với Importance + Entities + Topics + Connected.
> **⚠️ CRITICAL-ONLY POLICY (v3.0)**: File này chỉ chứa **≤10 entries** có **importance ≥ 0.8**.
> Entries importance < 0.8 → `LESSONS_ARCHIVE.md`. Tất cả đều được embed vào Qdrant.

---

## 📊 Thống kê

| Tổng entries | 🔴 Critical | 🟡 Warning | 🟢 Info |
|---|---|---|---|
| 1 | 0 | 1 | 0 |

---

## Entries

### 🟡 #LEARN-001 — Open-source skill frameworks as upgrade catalyst (2026-03-14)

- **Importance:** 0.8 | 🟡 Warning
- **Type:** mental_model
- **Entities:** [obra/superpowers, skill-upgrade, anti-rationalization, subagent-orchestration]
- **Topics:** #research, #upgrade, #superpowers, #verification
- **Observation:** Studying obra/superpowers (6.9k⭐) revealed 3 critical gaps in APEX v4.0: (1) no subagent prompt templates, (2) no verification-before-completion gate, (3) no anti-rationalization tables. These patterns dramatically improve output quality.
- **Key patterns learned:**
  ```
  1. Verification Gate: "Should pass" ≠ evidence. RUN → READ → THEN claim.
  2. Anti-rationalization: Table mapping common excuses → reality checks
  3. Subagent prompts: 3 specialized templates (implement/spec-review/quality-review)
  4. Status protocol: DONE/DONE_WITH_CONCERNS/NEEDS_CONTEXT/BLOCKED
  5. Model selection: mechanical→cheap, integration→standard, architecture→capable
  ```
- **Quy tắc rút ra:** Periodically audit skill system against top open-source frameworks (superpowers, GSD2, etc.) to discover blind spots. Behavioral engineering (anti-rationalization) is as important as technical workflow.
- **Connected:** v4.1 upgrade, Rule 24, Rule 25
- **File liên quan:** `build/references/subagent-prompts.md`, `session/SKILL.md`, `fix/SKILL.md`

---

> **Archived entries** (importance < 0.8): See `LESSONS_ARCHIVE.md`

