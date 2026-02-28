---
name: Start — Khởi động phiên Production Code
description: "Khởi động phiên làm việc toàn diện: load context, check LESSONS, scan blueprints, chọn skill, sẵn sàng code. Thay thế start + recall + supper cũ. Kích hoạt bằng lệnh /start [mô tả task]."
---

# /start [task description]

> **1 lệnh thay 3** — Gộp start + recall + supper cũ
> Mục tiêu: Từ 0 → sẵn sàng code trong **< 60 giây**

---

## QUY TRÌNH 5 BƯỚC

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

### Bước 4 — BLUEPRINT CHECK (MỚI — từ bkns-minicrm)

```
1. Scan thư mục .agent/commands/ (nếu tồn tại)
2. Tìm file task-*.md match với [task description]
3. NẾU TÌM THẤY:
   📋 "Blueprint found: task-001-payroll-comparison.md"
   → Suggest: "/build sẽ dùng blueprint này"
4. NẾU KHÔNG:
   📋 "Không có blueprint. Bạn muốn /plan trước hay /build trực tiếp?"
```

---

### Bước 5 — SKILL AUTO-SELECT & STATUS REPORT

Dựa vào [task description], tự chọn skill phù hợp:

| Từ khóa trong task | Skill được chọn |
|---|---|
| code, feature, thêm, tạo, viết | `/build` |
| fix, bug, lỗi, sửa, error | `/fix` |
| design, UI, giao diện, logo | `/design` |
| test, security, audit, kiểm tra | `/guard` |
| API, webhook, n8n, payment, integrate | `/integrate` |
| plan, thiết kế, spec, blueprint | `/plan` |

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

📋 BLUEPRINT
  [✅ Có: task-NNN.md | ❌ Chưa có — gợi ý /plan]

🎯 SKILL ĐƯỢC CHỌN
  → /build [task]  (hoặc /fix, /design, /guard, /integrate)

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
💡 Gõ lệnh skill ở trên để bắt đầu, hoặc:
   /plan [feature]  → tạo blueprint trước
   /build [task]    → bắt đầu code ngay
   /fix [bug]       → debug ngay
```

---

## QUY TẮC

- CHỈ đọc và báo cáo — KHÔNG tự sửa file
- Nếu ACTIVE_CONTEXT tồn tại → ưu tiên hiển thị task đang dở
- Nếu có blueprint → gợi ý dùng `/build` với blueprint
- Nếu task phức tạp và chưa có blueprint → gợi ý `/plan` trước
- Tone: đồng nghiệp senior, ngắn gọn, sẵn sàng action
