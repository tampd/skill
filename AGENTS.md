# Agent Instructions

This project uses **bd** (beads) for issue tracking. Run `bd onboard` to get started.

## Quick Reference

```bash
bd ready              # Find available work
bd show <id>          # View issue details
bd update <id> --claim  # Claim work atomically
bd close <id>         # Complete work
bd sync               # Sync with git
```

## Non-Interactive Shell Commands

**ALWAYS use non-interactive flags** with file operations to avoid hanging on confirmation prompts.

```bash
cp -f source dest           # NOT: cp source dest
mv -f source dest           # NOT: mv source dest
rm -f file                  # NOT: rm file
rm -rf directory            # NOT: rm -r directory
```

## Skill System v6.0 Core Rules

**MANDATORY** for all AI agents working on this project:

1. **TDD Iron Law (Rule 6)**: Write tests BEFORE code. RED → GREEN → REFACTOR → COMMIT.
2. **Brainstorming Hard Gate (Rule 7)**: `/plan` MUST ask + propose ≥ 2 approaches BEFORE creating specs.
3. **Root Cause First (Rule 8)**: `/fix` MUST complete Phase 1 (Root Cause) BEFORE fixing.
4. **Spec-Driven Changes**: Each change = 1 folder (`changes/<name>/`) with proposal.md, specs/, design.md, tasks.md.
5. **2-Stage Review**: Stage 1: Spec compliance. Stage 2: Code quality.
6. **WCAG AA (Rule 9)**: All user-facing pages MUST meet WCAG 2.2 AA.
7. **Security Headers (Rule 10)**: HSTS, CSP, X-Frame-Options mandatory.
8. **Design Tokens (Rule 11)**: No hardcoded colors/fonts/spacing — use semantic tokens.
9. **Performance Budget (Rule 12)**: Lighthouse ≥ 90, LCP < 2.5s, CLS < 0.1, JS < 150KB gzip.
10. **Self-Reasoning Gate (Rule 13)**: 3-Question Self-Check before EVERY execution decision — best approach? hidden risks? user approval needed?

### 14 Skills (v6.0 — Memory-First Architecture)

| Category | Skills |
|---|---|
| **Core** | start, build ⭐, fix, save, plan, memory |
| **Web** | craft 🆕 (design+frontend), quality 🆕 (guard+perf+review), ship 🆕 |
| **Specialist** | integrate, n8n-pro, web-security, docs, seo |

See `GEMINI.md` for full rules and individual `*/SKILL.md` for workflows.

<!-- BEGIN BEADS INTEGRATION -->
## Issue Tracking with bd (beads)

- ✅ Use bd for ALL task tracking
- ✅ Use `--json` flag for programmatic use
- ✅ Link discovered work with `discovered-from` dependencies
- ❌ Do NOT create markdown TODO lists
- ❌ Do NOT duplicate tracking systems

## Landing the Plane (Session Completion)

**MANDATORY WORKFLOW:**
1. File issues for remaining work
2. Run quality gates (if code changed)
3. Update issue status
4. **PUSH TO REMOTE** — MANDATORY: `git push`
5. Verify — all changes committed AND pushed

**CRITICAL**: Work is NOT complete until `git push` succeeds.
<!-- END BEADS INTEGRATION -->
