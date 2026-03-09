# SPEC Templates Reference

## 3-Tier System

| Tầng | Khi nào | Vị trí |
|---|---|---|
| **Full SPEC** (19 sections) | Dự án mới / Module lớn | `SPEC.md` ở root |
| **Feature SPEC** (10 sections) | Feature ≥ 3 files | `changes/<name>/SPEC.md` |
| **Mini-Spec** (10 dòng) | Task ≤ 2 files | Inline trong /build Step 1 |

## Mini-Spec Template

```
| Field | Value |
|---|---|
| **Task** | [mô tả] |
| **WHY** | [vấn đề giải quyết] |
| **WHAT** | [files tạo/sửa] |
| **HOW** | [approach] |
| **TEST** | [verify bằng gì] |
| **DONE** | [criteria] |
```

## Feature SPEC Template (10 sections)

```markdown
# SPEC — [Feature Name]
> Status: Draft | In Progress | Done

## 1. Tóm tắt (Overview)
## 2. Bối cảnh & Vấn đề (Context)
## 3. Mục tiêu (Goals)
## 4. Phạm vi (Scope — In/Out)
## 6. Yêu cầu chức năng (Functional Requirements)
## 7. Yêu cầu phi chức năng (NFR)
## 10. Thiết kế kỹ thuật (Technical Design)
## 11. Thiết kế API (nếu có)
## 15. Rủi ro (Risks)
## 16. Tiêu chí nghiệm thu (Acceptance Criteria)
```

## Change Folder Structure

```
changes/<feature-name>/
├── SPEC.md           ← Feature SPEC (10 sections)
├── tasks.md          ← Implementation tasks
└── delta-specs.md    ← Spec changes to merge khi done
```

## Tasks.md Template

```markdown
# Tasks — [Feature Name]

| ID | Task | Size | Status | Depends | Notes |
|---|---|---|---|---|---|
| T-001 | [task] | S/M/L | ⬜/🔄/✅ | — | |
| T-002 | [task] | L | ⬜ | T-001 | |
```
