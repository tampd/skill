---
name: Start — Khởi động phiên Production Code
description: "Khởi động phiên làm việc toàn diện: load context, check LESSONS, Beads ready, Qdrant recall, scan blueprints, chọn skill, sẵn sàng code. Thay thế start + recall + supper cũ. Kích hoạt bằng lệnh /start [mô tả task]."
---

# /start [task description]

> **1 lệnh thay 3** — Gộp start + recall + supper cũ
> Mục tiêu: Từ 0 → sẵn sàng code trong **< 60 giây**
> **v5.0** — TDD Iron Law + Spec-Driven + 5-Layer Memory

---

## QUY TRÌNH 7 BƯỚC

### Bước 1 — MEMORY BOOTSTRAP

```
1. Kiểm tra ACTIVE_CONTEXT.md trong thư mục dự án:

   NẾU TỒN TẠI:
     🚨 "Phiên cũ chưa kết thúc!"
     📌 Task đang dở: [task]
     📁 Files: [files]
     🎯 Tiếp theo: [action]
     → Hỏi: "Tiếp tục task cũ hay bắt đầu task mới?"

   NẾU KHÔNG:
     → Tiếp tục Bước 2
```

---

### Bước 2 — CONTEXT LOAD (Song song, < 10 giây)

Đọc **đồng thời** tất cả file context:

| File | Trích xuất |
|---|---|
| `GEMINI.md` | Version, tech stack, Architecture Rules |
| `LESSONS.md` | ⚠️ Warnings liên quan task |
| `CHANGE_LOG.md` | Phiên trước: tóm tắt 1 dòng |
| `NEXT-TODO.md` | Top 3 task ưu tiên |
| `STATE.md` / `.gsd/STATE.md` | Phase/wave đang dở (nếu có) |

> Nếu file không tồn tại → bỏ qua, không báo lỗi.

---

### Bước 2.5 — BEADS READY (Layer 5 — Task Graph)

```
1. Kiểm tra thư mục .beads/ tồn tại trong dự án:

   NẾU TỒN TẠI:
     → Chạy: bd ready --json
     → Lấy danh sách tasks không bị block (sẵn sàng làm)
     → Hiển thị top 3 tasks:
       🎯 bd-a1b2 [P0] "Implement login API" (ready)
       🎯 bd-c3d4 [P1] "Add error handling" (ready)
       🎯 bd-e5f6 [P2] "Write unit tests" (ready)

   NẾU KHÔNG TỒN TẠI:
     → Bỏ qua, KHÔNG báo lỗi
     → Gợi ý nhỏ: "💡 Gõ `bd init` để bật task tracking với Beads"

   NẾU bd CHƯA CÀI:
     → Bỏ qua hoàn toàn, KHÔNG báo lỗi
```

> **Fallback**: Nếu Beads không available → dùng NEXT-TODO.md như bình thường.

---

### Bước 2.6 — QDRANT RECALL (Layer 4 — Vector Memory)

```
1. Kiểm tra Qdrant MCP tools có available không (qdrant_find, qdrant_store):

   NẾU AVAILABLE VÀ CÓ [task description]:
     → Chạy: qdrant_find("[task description]")
     → Lấy top 5 memories liên quan nhất
     → Hiển thị:
       🧠 QDRANT: 5 patterns liên quan
         1. "Luôn dùng bcmath cho tính tiền" (0.92 similarity)
         2. "N+1 query fix: dùng whereIn + groupBy" (0.87)
         3. ...

   NẾU KHÔNG AVAILABLE:
     → Bỏ qua hoàn toàn, KHÔNG báo lỗi
     → Hiển thị: "🧠 QDRANT: Chưa kết nối (xem /memory để setup)"
```

> **Fallback**: Nếu Qdrant chưa deploy → chỉ dùng LESSONS.md grep tags.

---

### Bước 3 — LESSONS PRE-FLIGHT

```
1. Đọc LESSONS.md
2. Grep: entry nào LIÊN QUAN tới task mô tả trong [task description]?
3. Match theo #tags hoặc file liên quan
4. Hiển thị tối đa 3 cảnh báo relevant:
   ⚠️ #WARN-003 — Thiếu @endsection gây vỡ layout Blade
   ⚠️ #WARN-005 — FK constraint phải xóa theo thứ tự
```

> 🛑 **RULE**: Nếu có LESSONS liên quan → AI PHẢI nhắc lại trước khi code. Không exception.

---

### Bước 4 — BLUEPRINT / CHANGE FOLDER CHECK ⭐ v5.0

```
1. Scan thư mục changes/ (v5.0 — ưu tiên)
   NẾU TÌM THẤY changes/<name>/ folder:
   📋 "Change folder found: changes/<name>/"
   → Hiển thị: proposal.md summary + tasks count
   → Suggest: "/build sẽ dùng change folder này"

2. Scan thư mục .agent/commands/ (backward compat v4.x)
   NẾU TÌM THẤY task-*.md:
   📋 "Blueprint found: task-NNN-name.md"
   → Suggest: "/build sẽ dùng blueprint này"

3. Scan thư mục archive/ (context từ past changes)
   NẾU CÓ archived changes:
   📦 "Archive: N completed changes found"

4. NẾU KHÔNG TÌM THẤY GÌ:
   📋 "Không có spec/blueprint. Bạn muốn /plan trước hay /build trực tiếp?"
```

---

### Bước 5 — SKILL AUTO-SELECT & STATUS REPORT

Dựa vào [task description], tự chọn skill phù hợp:

| Từ khóa trong task | Skill được chọn |
|---|---|
| code, feature, thêm, tạo, viết, build, module | `/build` |
| TDD, test-driven, red-green, viết test | `/build` |
| fix, bug, lỗi, sửa, error, debug, crash, root cause | `/fix` |
| design, UI, giao diện, logo, CSS, theme, layout | `/design` |
| test, kiểm tra code, coverage, lint, audit | `/guard` |
| API, webhook, payment, integrate, 3rd-party | `/integrate` |
| plan, thiết kế, spec, blueprint, roadmap, change folder, proposal | `/plan` |
| n8n, workflow, automation, trigger, queue | `/n8n` |
| ý tưởng, brainstorm, idea, compare, đánh giá | `/brainstorm` |
| bảo mật, security, OWASP, CVE, hack, pentest | `/security` |
| review web, đánh giá website, audit site, lighthouse | `/review-web` |
| docs, tài liệu, document, ADR, handoff, onboard | `/docs` |
| SEO, viết bài, content, keyword, GEO, article, blog | `/seo` |
| memory, checkpoint, recall, bộ nhớ, nhớ lại | `/memory` |
| save, kết thúc, đóng phiên, commit, push | `/save` |
| start, bắt đầu, khởi động, mở phiên | `/start` |

> **Fallback**: Nếu không match pattern nào → hỏi user chọn skill.

Xuất báo cáo:

```
🚀 START — [Tên dự án] v[X.Y.Z]
📅 [Timestamp]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

📋 TRẠNG THÁI
  Version: vX.Y.Z | Phase: [X] | Phiên trước: [tóm tắt]

🔜 TASK HÔM NAY
  → [task description]

⚠️ LESSONS CẦN LƯU Ý
  #WARN-XXX — [quy tắc liên quan]

🎯 BEADS READY (Layer 5)
  bd-XXXX [P0] "[task title]" — ready
  bd-YYYY [P1] "[task title]" — ready
  [Hoặc: "Chưa khởi tạo — gõ bd init"]

🧠 QDRANT RECALL (Layer 4)
  N patterns liên quan tìm thấy
  1. "[pattern mô tả]" (similarity: 0.XX)
  [Hoặc: "Chưa kết nối — xem /memory để setup"]

📋 CHANGE FOLDER / BLUEPRINT
  [✅ Có: changes/<name>/ | ✅ Có: task-NNN.md | ❌ Chưa có — gợi ý /plan]

🎯 SKILL ĐƯỢC CHỌN
  → /build [task]  (hoặc /fix, /design, /guard, /integrate, /memory)

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
💡 Gõ lệnh skill ở trên để bắt đầu, hoặc:
   /plan [feature]  → tạo blueprint trước
   /build [task]    → bắt đầu code ngay
   /fix [bug]       → debug ngay
   /memory          → quản lý 5-Layer Memory
```

---

## QUY TẮC

- CHỈ đọc và báo cáo — KHÔNG tự sửa file
- Nếu ACTIVE_CONTEXT tồn tại → ưu tiên hiển thị task đang dở
- Nếu có change folder hoặc blueprint → gợi ý dùng `/build`
- Nếu task phức tạp và chưa có spec → gợi ý `/plan` trước
- Beads/Qdrant không available → **bỏ qua im lặng**, KHÔNG gây lỗi
- Tone: đồng nghiệp senior, ngắn gọn, sẵn sàng action
