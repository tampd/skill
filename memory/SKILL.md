---
name: Memory — 5-Layer Memory Management (v6.0)
description: "Quản lý 5-Layer Memory System: checkpoint working memory, recall context từ Beads + Qdrant, xem trạng thái 5 tầng, context compression. Lệnh: /checkpoint, /recall, /memory. Kích hoạt bằng /memory hoặc /checkpoint hoặc /recall."
---

# /memory | /checkpoint | /recall

> **v6.0** — Quản lý Hybrid 5-Layer Memory System
> Mục tiêu: AI không bao giờ quên context, không lặp lại sai lầm, làm việc xuyên suốt cross-session

---

## KIẾN TRÚC 5-LAYER MEMORY

```
╔════════════════════════════════════════════════════════╗
║  Layer 5 — TASK GRAPH MEMORY (Beads/Dolt SQL)         ║
║  Tasks, dependencies, epics, blocking graph            ║
║  Tool: bd CLI (bd ready, bd create, bd close)          ║
╠════════════════════════════════════════════════════════╣
║  Layer 4 — VECTOR MEMORY (Qdrant)                     ║
║  Lessons, patterns, decisions (semantic search)        ║
║  Tool: MCP qdrant_store / qdrant_find                  ║
╠════════════════════════════════════════════════════════╣
║  Layer 3 — EPISODIC MEMORY (Files — append-only)      ║
║  LESSONS.md, CHANGE_LOG.md, DECISIONS.md               ║
╠════════════════════════════════════════════════════════╣
║  Layer 2 — SEMANTIC MEMORY (Files — project context)  ║
║  GEMINI.md, STATE.md, NEXT-TODO.md                     ║
╠════════════════════════════════════════════════════════╣
║  Layer 1 — WORKING MEMORY (File — session only)       ║
║  ACTIVE_CONTEXT.md                                     ║
╚════════════════════════════════════════════════════════╝
```

---

## BA LỆNH

### `/checkpoint` — Lưu Trạng Thái Working Memory

```
Khi nào gọi:
  - Sắp nghỉ giữa task phức tạp (ngay cả 10 phút)
  - Sau mỗi sub-task quan trọng hoàn thành
  - Khi sắp chuyển sang task khác
  - Khi context window cảm giác đầy (nhiều file đã đọc)
  - Trước mỗi /save

AI sẽ làm:
  1. Tổng hợp toàn bộ context hiện tại
  2. Ghi/Cập nhật ACTIVE_CONTEXT.md theo schema:

     # ACTIVE_CONTEXT — [Tên Dự Án]
     > Last checkpoint: YYYY-MM-DD HH:MM

     ## ⚡ SNAPSHOT HIỆN TẠI
     - Task đang làm: [tên task]
     - Skill đang dùng: [build/fix/design...]
     - Files đang mở:
       - `path/to/file1` — [đang làm gì]
       - `path/to/file2` — [đang làm gì]

     ## 🔑 QUYẾT ĐỊNH ĐÃ RA
     - [HH:MM] [quyết định + lý do]

     ## ✅ ĐÃ HOÀN THÀNH
     - [x] [sub-task xong]

     ## 🔄 ĐANG LÀM
     - [ ] [sub-task đang dở]
       - Đã làm: [...]
       - Chưa làm: [...]

     ## 🎯 NEXT IMMEDIATE ACTION
     [Câu lệnh/hành động CỤ THỂ tiếp theo]

  3. ⭐ Beads snapshot (nếu available):
     → bd ready --json → ghi vào ACTIVE_CONTEXT
  4. Xác nhận: "✅ Checkpoint lưu thành công lúc HH:MM"
```

---

### `/recall` — Khôi Phục Context

```
Khi nào gọi:
  - Vừa mở lại sau khi nghỉ giữa chừng
  - Cảm thấy AI đang "lạc" không biết đang làm gì
  - Sau khi /start nhưng muốn detail hơn về task đang dở

AI sẽ làm:
  1. Đọc ACTIVE_CONTEXT.md → hiển thị tóm tắt:
     "📌 Bạn đang: [task] | Files: [list] | Next: [action]"

  2. Load lại FILES đang chỉnh → restore context kỹ thuật

  3. ⭐ Qdrant Recall (Layer 4 — nếu available):
     → qdrant_find("task đang làm: [task description]")
     → Hiển thị top 3 patterns liên quan

  4. ⭐ Beads Status (Layer 5 — nếu available):
     → bd ready --json → tasks sẵn sàng
     → bd show <current-id> --json → context của task hiện tại

  5. Đề xuất: "Tôi sẵn sàng tiếp tục: [NEXT IMMEDIATE ACTION]"
```

---

### `/memory` — Xem Tổng Quan 5-Layer Memory

```
Khi nào gọi:
  - Muốn check trạng thái toàn bộ memory system
  - Muốn biết layer nào đang active, layer nào chưa setup
  - Review tất cả decisions đã ra trong phiên

AI sẽ làm:

🧠 MEMORY STATUS — [Tên dự án]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Layer 1 — WORKING MEMORY:
  [✅ ACTIVE_CONTEXT.md: checkpoint lúc HH:MM | ❌ Chưa tạo]

Layer 2 — SEMANTIC MEMORY:
  GEMINI.md:     [✅ v5.0 | ❌ Không tìm thấy]
  STATE.md:      [✅ Phase X | ❌ Không có]
  NEXT-TODO.md:  [✅ N tasks | ❌ Không có]

Layer 3 — EPISODIC MEMORY:
  LESSONS.md:    [✅ N entries | ❌ Không có]
  CHANGE_LOG.md: [✅ Cập nhật DD/MM | ❌ Không có]

Layer 4 — VECTOR MEMORY (Qdrant):
  [✅ Connected: N memories in collection | ❌ Chưa kết nối]
  Setup: xem /root/skill/qdrant-memory/SKILL.md

Layer 5 — TASK GRAPH (Beads):
  [✅ Initialized: N open issues, N ready | ❌ Chưa init]
  Setup: chạy `bd init` trong thư mục dự án

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
💡 Tips:
  - Layer 1-3: luôn sẵn sàng (file-based)
  - Layer 4: cần Qdrant trên VPS → xem qdrant-memory/SKILL.md
  - Layer 5: cần `bd init` → npm install -g @beads/bd
```

---

## AUTO-CHECKPOINT RULES

```
TRIGGER                              ACTION
─────────────────────────────────────────────────────
Sau mỗi commit          ────────────→ Mini checkpoint (3 dòng)
Xong 1 wave đầy đủ      ────────────→ Full checkpoint
Context > 70% (estimate) ───────────→ Cảnh báo + gợi ý checkpoint
Gặp blocking error       ───────────→ Auto-capture vào BLOCKERS
User nói "nhớ lấy"       ───────────→ Append vào DECISIONS
Trước /save              ────────────→ Mandatory full checkpoint
```

---

## CONTEXT COMPRESSION PROTOCOL 🆕 v6.0

> Khi phiên dài, context window đầy → AI bắt đầu "quên" files đọc đầu phiên.

```
COMPRESSION LEVEL 1 — Tag-based LESSONS (nhẹ):
  Thay vì đọc full LESSONS.md → chỉ grep tags liên quan task
  Tiết kiệm: ~70% context của LESSONS.md

COMPRESSION LEVEL 2 — Progressive file loading (trung bình):
  Chỉ đọc file đang làm, đọc file khác khi CẦN THẬT SỰ
  Tiết kiệm: ~60% context code files

COMPRESSION LEVEL 3 — Summary mode (nặng):
  Khi context > 80%: chỉ làm việc với ACTIVE_CONTEXT.md
  Checkpoint ngay → gợi ý user bắt đầu phiên mới
```

#### Dấu hiệu Context Overflow:
```
🔴 AI nói "Xin lỗi, tôi không nhớ..." → Checkpoint ngay
🔴 AI đưa quyết định mâu thuẫn → Context bị truncate
🔴 AI hỏi lại info đã nói → Cần phiên mới
```

#### Document Load Order (3-Tier):
```
TIER 1 — LUÔN LOAD (mỗi /start):
  GEMINI.md → rules, stack (~100 tokens)
  LESSONS.md → grep relevant tags (~50 tokens)

TIER 2 — LOAD KHI CẦN:
  docs/architecture.md, docs/business-rules.md
  docs/api-endpoints.md, docs/setup.md

TIER 3 — LOAD KHI /build:
  changes/<feature>/specs/ → chỉ spec đang build
```

---

## QUY TẮC

- `/checkpoint` = KHÔNG BAO GIỜ từ chối. Luôn ghi dù context ít.
- `/recall` = Phải actionable. Kết thúc bằng "Tôi sẵn sàng: [action cụ thể]"
- `/memory` = Chỉ report, KHÔNG tự sửa file
- ACTIVE_CONTEXT.md bị xóa sau mỗi `/save` (memory consolidated)
- Layer 4-5 không available → **bỏ qua im lặng**, KHÔNG gây lỗi
- NEXT IMMEDIATE ACTION phải đủ chi tiết để "AI lạ" tiếp tục được
- Context > 70% → cảnh báo user, gợi ý checkpoint

