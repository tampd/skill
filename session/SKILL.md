---
name: session
description: "Quản lý lifecycle phiên làm việc: mở phiên (load context, auto-select skill), checkpoint giữa phiên, đóng phiên (review, commit, push). Kích hoạt khi người dùng nói /start, /save, /checkpoint, /recall, /memory, hoặc 'bắt đầu phiên', 'kết thúc phiên', 'lưu lại'."
metadata:
  author: tampd
  version: 7.0.0
  category: lifecycle
---

# Session — Lifecycle Management

> **v7.0** — Gộp start + save + memory
> Mục tiêu: Mở/đóng/checkpoint phiên trong 1 skill duy nhất.

---

## 3 MODES

### Mode 1 — `/start [task]` (Mở phiên)

```
BƯỚC 1 — CHECK ACTIVE CONTEXT:
  NẾU ACTIVE_CONTEXT.md tồn tại:
    🚨 "Phiên cũ chưa kết thúc!"
    → Hiển thị: task đang dở + files + next action
    → Hỏi: "Tiếp tục hay bắt đầu mới?"
  NẾU KHÔNG → tiếp tục

BƯỚC 2 — CONTEXT LOAD (song song):
  Đọc đồng thời: GEMINI.md, LESSONS.md, CHANGE_LOG.md,
  NEXT-TODO.md, STATE.md, PROJECT-META.md, SPEC.md, DEPLOYMENT.md
  > File không tồn tại → bỏ qua im lặng

BƯỚC 3 — LESSONS PRE-FLIGHT:
  Grep LESSONS.md → entries match [task description]
  Hiển thị tối đa 3 cảnh báo relevant

BƯỚC 4 — BLUEPRINT CHECK:
  Scan changes/ → SPEC.md + tasks count
  Scan .agent/commands/ → backward compat
  Không tìm thấy → "Chưa có spec. /plan trước hay /build trực tiếp?"

BƯỚC 5 — SKILL AUTO-SELECT:
  Dựa vào [task description] → chọn skill phù hợp
  Xem bảng trigger keywords trong references/trigger-keywords.md
```

**Output:**
```
🚀 START — [Tên dự án] v[X.Y.Z]
📅 [Timestamp]
━━━━━━━━━━━━━━━━━━━━━━
📋 TRẠNG THÁI: v[X.Y.Z] | Phiên trước: [tóm tắt]
🔜 TASK: → [task description]
⚠️ LESSONS: #WARN-XXX — [quy tắc liên quan]
📋 SPEC/BLUEPRINT: [✅ Có | ❌ Chưa có]
🎯 SKILL → /[skill] [task]
━━━━━━━━━━━━━━━━━━━━━━
```

---

### Mode 2 — `/checkpoint` (Lưu giữa phiên)

```
KHI NÀO:
  - Sắp nghỉ giữa task phức tạp
  - Sau sub-task quan trọng hoàn thành
  - Trước khi chuyển task khác
  - Context window cảm giác đầy

AI SẼ LÀM:
  1. Tổng hợp context hiện tại
  2. Ghi ACTIVE_CONTEXT.md:

     # ACTIVE_CONTEXT — [Tên Dự Án]
     > Last checkpoint: YYYY-MM-DD HH:MM

     ## ⚡ SNAPSHOT
     Task: [mô tả]
     Progress: [X/Y bước xong]
     Files đang chỉnh: [list]

     ## 🔜 NEXT IMMEDIATE ACTION
     [Đủ chi tiết để "AI lạ" tiếp tục được]

     ## 🧠 CONTEXT CẦN NHỚ
     [Decisions, constraints, gotchas]
```

**`/recall`** — Đọc ACTIVE_CONTEXT.md → hiển thị tóm tắt → load lại files.

---

### Mode 3 — `/save` (Đóng phiên)

```
BƯỚC 1 — SCOPE REVIEW:
  git status + git diff --stat → liệt kê files thay đổi

BƯỚC 2 — 2-STAGE REVIEW:
  Stage 1 (Spec Compliance):
    Code đúng spec? Thừa? Behavior khớp?
  Stage 2 (Code Quality — 7 tiêu chí):
    ① Bugs ② UI/UX ③ Security ④ Performance
    ⑤ Code Quality ⑥ Test Coverage ⑦ LESSONS Compliance
  🛑 CRITICAL → DỪNG, yêu cầu sửa trước commit

BƯỚC 3 — LESSONS CAPTURE:
  Hỏi: "Phiên này có bug/bài học gì không?"
  NẾU CÓ → ghi LESSONS.md (xem references/lessons-template.md)

BƯỚC 4 — CHANGE ARCHIVE:
  NẾU có change folder → verify Done → merge delta specs → archive

BƯỚC 5 — DOCS UPDATE:
  CHANGE_LOG.md, NEXT-TODO.md, GEMINI.md + BKNS files nếu có

BƯỚC 6 — ATOMIC COMMIT & PUSH:
  git add → git commit (conventional) → git push origin main

BƯỚC 7 — CLEANUP:
  Xóa ACTIVE_CONTEXT.md (consolidated vào docs)
```

**Output:**
```
✅ /save hoàn tất!
📋 REVIEW: Stage 1 [✅|❌] | Stage 2 [✅ N/7]
📝 LESSONS: [N mới | Không đổi]
📦 ARCHIVE: [archived | N/A]
📦 COMMITS: [N commits] | 🔗 PUSHED: main → origin
🔜 NEXT → [task ưu tiên]
```

---

## QUY TẮC

- `/start` — CHỈ đọc và báo cáo, KHÔNG tự sửa file
- `/checkpoint` — KHÔNG BAO GIỜ từ chối. Luôn ghi dù context ít
- `/save` — Stage 1 FAIL → KHÔNG commit. CRITICAL → KHÔNG commit
- ACTIVE_CONTEXT.md bị xóa sau `/save`
- Beads/Qdrant không available → bỏ qua im lặng
