---
name: Start Workflow
description: "Công cụ khởi động phiên làm việc. Tự động kiểm tra trạng thái các file nhật ký (CHANGE_LOG.md, README.md, NEXT-TODO.md, LESSONS.md, STATE.md) để nắm bắt bối cảnh và gợi ý công việc tiếp theo. Kích hoạt bằng lệnh /start."
---

# HƯỚNG DẪN KỸ NĂNG: START WORKFLOW

## Mục tiêu
Khởi động phiên làm việc với đầy đủ context. Không bao giờ bắt đầu code khi chưa biết: dự án đang ở đâu, còn làm gì, đã học được gì từ bài học cũ. **Tổng thời gian chạy /start: < 60 giây.**

---

## CÁC BƯỚC BẮT BUỘC

### Bước 0 — MEMORY BOOTSTRAP (TRƯỚC TẤT CẢ — KHÔNG SKIP)

Kiểm tra `ACTIVE_CONTEXT.md` trong thư mục dự án:

```
NẾU TỒN TẠI ACTIVE_CONTEXT.md:
  🚨 ALERT: "Phiên làm việc cũ chưa kết thúc đúng cách!"
  Hiển thị ngay:
    📌 Task đang dở: [task từ ACTIVE_CONTEXT]
    📁 Files đang chỉnh: [files từ ACTIVE_CONTEXT]
    🎯 Tiếp theo: [NEXT IMMEDIATE ACTION]
  Hỏi user: "Bạn muốn /recall để tiếp tục, hay bắt đầu task mới?"
  → Nếu tiếp tục: kích hoạt /recall (memory-optimizer skill)
  → Nếu mới: ghi nhận và tiếp tục Bước 1

NẾU KHÔNG CÓ ACTIVE_CONTEXT.md:
  → Tiếp tục Bước 1 như bình thường
```

> 💡 `ACTIVE_CONTEXT.md` là working memory của AI — xem memory-optimizer/SKILL.md

---

### Bước 1 — Load Context Files (Song song)
Đọc tất cả các file sau cùng lúc:

| File | Mục tiêu đọc |
|---|---|
| `GEMINI.md` hoặc `memory.md` | Kiến trúc dự án, tech stack, coding rules |
| `CHANGE_LOG.md` | Những gì đã thay đổi trong phiên gần nhất |
| `NEXT-TODO.md` | Danh sách task chưa hoàn thành |
| `LESSONS.md` | Các lỗi và bài học đã ghi lại |
| `STATE.md` (GSD) | Phase/wave đang làm dở (nếu tồn tại) |
| `PLAN.md` (GSD) | Wave breakdown của phase hiện tại (nếu tồn tại) |

> Nếu file không tồn tại → ghi nhận, không báo lỗi, tiếp tục.

---

### Bước 2 — Phân tích và Tổng hợp

```
Từ GEMINI.md/memory.md:
  → Dự án là gì? Tech stack? Phase hiện tại? Version?

Từ CHANGE_LOG.md:
  → Phiên trước làm gì? Bug nào đã fix? Feature nào đã ship?

Từ NEXT-TODO.md:
  → Task nào đang chờ? Ưu tiên nào cao nhất?

Từ STATE.md (nếu có):
  → Đang ở Phase mấy? Wave mấy? Task nào đang làm dở?
```

---

### Bước 3 — Cảnh báo LESSONS.md (Critical only)

```
1. Đọc LESSONS.md
2. Lọc các entry có mức 🔴 Critical và 🟡 Warning
3. Đối chiếu với task trong NEXT-TODO.md: bài học nào LIÊN QUAN đến task hôm nay?
4. Hiển thị tối đa 5 bài học relevant nhất:
   📌 #BUG-XXX [🔴 Critical] — Mô tả ngắn → Quy tắc cần nhớ
```

> Nếu LESSONS.md chưa tồn tại:
> *"📒 Dự án chưa có LESSONS.md. Gõ `/save` sau phiên đầu tiên để bắt đầu ghi bài học."*

---

### Bước 4 — Báo cáo Trạng thái (Bắt buộc format này)

```
🚀 **Phiên làm việc mới — [Tên dự án] [Version]**
📅 [Ngày giờ hiện tại]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
📋 TRẠNG THÁI DỰ ÁN
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
- Version: vX.Y.Z
- Phase hiện tại: Phase XX — [Tên phase]
- Wave: [Wave N] (nếu có STATE.md/PLAN.md)
- Phiên trước: [Tóm tắt 1-2 dòng từ CHANGE_LOG]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
🔜 TASK CẦN LÀM (từ NEXT-TODO.md)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
🔴 Ưu tiên cao:
  1. [Task ưu tiên cao nhất]
  2. [Task ưu tiên cao thứ 2]

🟡 Trung bình:
  3. [Task trung bình]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
⚠️ BÀI HỌC CẦN LƯU Ý HÔM NAY
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
📌 #BUG-XXX [🔴] — [Mô tả] → [Quy tắc]
...

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
🧭 GỢI Ý TIẾP THEO
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

### Bước 5 — Gợi ý hành động

Ở cuối báo cáo, **bắt buộc** đề xuất:

```
Dựa trên task đang chờ, tôi gợi ý:

Option A (cho task kỹ thuật):
  "/supper [mô tả task cụ thể]" → để chọn skill phù hợp

Option B (cho task có trong PLAN.md/STATE.md):
  Tiếp tục Wave N — Task: [tên task] → kích hoạt vibe-code

Option C (dự án mới):
  "/gsd" → lên kế hoạch với GSD methodology
```

---

## QUY TẮC QUAN TRỌNG

- CHỈ đọc và báo cáo — KHÔNG tự ý sửa file trong /start
- Nếu STATE.md tồn tại và có task đang dở → **ưu tiên hiển thị** ngay đầu báo cáo
- Luôn kết thúc bằng gợi ý `/supper [context]` hoặc action cụ thể
- Tone: đồng nghiệp thân thiện, đã biết dự án từ hôm qua, sẵn sàng làm việc tiếp
