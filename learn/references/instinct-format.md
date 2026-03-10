# Instinct Format

> Format chuẩn cho INSTINCTS.md (project-level) và GLOBAL_INSTINCTS.md.

## File Structure

```markdown
# 🧠 INSTINCTS — [Project Name]
> Last updated: YYYY-MM-DD | Total: N instincts

---

### prefer-css-variables
- trigger: "when writing CSS styles"
- confidence: 0.8
- domain: frontend
- scope: project
- evidence:
    - "2026-03-09: User corrected hardcoded color to var(--color-primary)"
    - "2026-03-10: Same pattern observed in header component"
    - "Matches LESSONS #WARN-011"
- action: "Luôn dùng CSS Variables (var(--xxx)), không hardcode colors/fonts/spacing"
- created: 2026-03-09
- last_seen: 2026-03-10

---

### test-before-code
- trigger: "when starting new feature implementation"
- confidence: 0.9
- domain: testing
- scope: global
- evidence:
    - "Observed in 5+ projects consistently"
    - "TDD Iron Law (Rule 5)"
- action: "Viết test TRƯỚC code. RED → GREEN → REFACTOR."
- created: 2026-02-28
- last_seen: 2026-03-09

---
```

## Confidence Scale

| Score | Label | Behavior | Visual |
|-------|-------|----------|--------|
| 0.3 | Tentative | Suggested but not enforced | 🟠 |
| 0.5 | Moderate | Applied when clearly relevant | 🟡 |
| 0.7 | Strong | Auto-applied, mention to user | 🟢 |
| 0.9 | Near-certain | Core behavior, always follow | 🟢 |
| 0.0 | Retired | Not applied, kept for history | ⚫ |

## Confidence Evolution

### Increase (+0.1 each)
- Pattern repeatedly observed in sessions
- User doesn't correct the behavior
- Matches existing LESSONS entry
- Similar instinct from another project

### Decrease
- User explicitly corrects behavior → -0.2
- Contradicting evidence appears → -0.1
- Pattern not seen for 30+ days → -0.05

### Promotion Criteria (Project → Global)
- Same instinct ID in ≥ 2 projects
- Average confidence ≥ 0.8
- No recent contradiction (< 7 days)

## Domains

| Domain | When | Examples |
|--------|------|---------|
| `code-style` | Code formatting, patterns | "Prefer functional", "Use const" |
| `testing` | Test practices | "Test edge cases", "Mock external" |
| `git` | Git workflow | "Small commits", "Conventional format" |
| `security` | Security patterns | "Validate input", "Sanitize output" |
| `frontend` | UI/CSS/design | "Mobile-first", "Use tokens" |
| `backend` | Server/API/DB | "Service layer", "N+1 prevention" |
| `workflow` | Dev process | "Read before write", "Plan before code" |
| `ops` | Operations | "Health check endpoints", "Structured logs" |
