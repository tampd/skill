# Handoff Document Template — 15 Sections

> Sử dụng khi bàn giao dự án cho dev/team mới.
> Mục tiêu: Dev mới tiếp quản 100% mà KHÔNG cần người cũ giải thích.

```markdown
# 📋 HANDOFF — [Project Name]
> Bàn giao từ: [tên] | Cho: [tên/team] | Ngày: YYYY-MM-DD | Version: vX.Y.Z

## 1. Project Overview
  [Mục đích hệ thống. Users/stakeholders. Business context]

## 2. Current State Dashboard
  | Metric | Value |
  |--------|-------|
  | Version | vX.Y.Z |
  | Last deploy | YYYY-MM-DD |
  | Open issues | N (N critical) |
  | Test coverage | N% |

## 3. Quick Start Guide (< 5 phút)
  [git clone → install → env → run]

## 4. Architecture Summary
  → Link ARCHITECTURE.md
  3 Core Concepts dev mới PHẢI biết

## 5. Codebase Map
  [Cây thư mục + files quan trọng nhất]

## 6. Common Tasks (How-to)
  | Task | Steps | Files |
  |------|-------|-------|

## 7. Environment & Secrets
  | Env Var | Purpose | Source |
  |---------|---------|--------|

## 8. Known Gotchas ⚠️
  [Trích LESSONS.md — top 5 gotchas]

## 9. Active TODOs
  → Source: NEXT-TODO.md + context

## 10. Team & Contacts
  | Topic | Contact | Channel |
  |-------|---------|---------|

## 11. Credentials Transfer Checklist
  [ ] GitHub / Server / DB / Monitoring / Domain / CDN

## 12. Monitoring & Observability
  | Service | URL | Purpose |
  |---------|-----|---------|

## 13. Operations Runbook (Top Incidents)
  → Link RUNBOOK.md hoặc inline top 3

## 14. Rollback Procedure
  [Step-by-step rollback commands]

## 15. Acceptance Checklist
  [ ] Clone + run local ✅
  [ ] Read architecture docs ✅
  [ ] Deploy staging ✅
  [ ] Complete 1 small task ✅
  Date signed: _____ | Dev mới: _____
```
