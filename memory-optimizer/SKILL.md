---
name: memory-optimizer
description: >
  Hệ thống quản lý bộ nhớ 3 tầng cho Google Antigravity. Đảm bảo AI không bao giờ quên
  context giữa các phiên làm việc. Sử dụng ACTIVE_CONTEXT.md làm working memory real-time.
  Kích hoạt bằng lệnh /checkpoint, /recall, /memory.
---

# HƯỚNG DẪN KỸ NĂNG: MEMORY OPTIMIZER

## Mục tiêu
Đảm bảo AI **không bao giờ quên** mình đang làm gì, đang ở đâu trong dự án, và không
lặp lại sai lầm. Duy trì bộ nhớ liền mạch xuyên suốt toàn bộ quá trình code dự án.

---

## KIẾN TRÚC 3-LAYER MEMORY

```
Layer 3 — EPISODIC (Dài hạn, không xóa)
  LESSONS.md | CHANGE_LOG.md | DECISIONS.md
  → Học từ quá khứ, không bao giờ lặp lại lỗi

Layer 2 — SEMANTIC (Context dự án)
  GEMINI.md | STATE.md | NEXT-TODO.md
  → Biết dự án là gì, đang ở phase nào

Layer 1 — WORKING (Real-time, phiên này)  ← QUAN TRỌNG NHẤT
  ACTIVE_CONTEXT.md  (MỚI)
  → Biết chính xác đang làm gì, đã làm gì, tiếp theo làm gì
```

---

## BA LỆNH CỐT LÕI

### `/checkpoint` — Lưu working memory ngay lúc này

**Khi nào gọi:**
- Sắp nghỉ giữa task (dù chỉ 10 phút)
- Sau mỗi commit quan trọng
- Khi sắp chuyển sang task khác hoàn toàn
- Trước mỗi /save (bắt buộc)
- Khi cảm thấy context window đang đầy

**AI thực hiện:**
1. Tổng hợp toàn bộ context hiện tại
2. Ghi vào `ACTIVE_CONTEXT.md` theo schema chuẩn
3. Xác nhận: "✅ Checkpoint #N — HH:MM. Bạn có thể nghỉ an toàn."

---

### `/recall` — Khôi phục context từ checkpoint

**Khi nào gọi:**
- Vừa quay lại sau khi nghỉ
- AI đang "lạc" không biết đang làm gì
- Sau /start muốn biết chi tiết task đang dở

**AI thực hiện:**
1. Đọc `ACTIVE_CONTEXT.md`
2. Tóm tắt: "📍 Bạn đang ở đây..."
3. Đọc lại các FILES đang chỉnh → load kỹ thuật context
4. Đề xuất ngay: "Tôi sẵn sàng tiếp tục: [NEXT IMMEDIATE ACTION]"

---

### `/memory` — Xem tổng quan toàn bộ memory system

**Khi nào gọi:**
- Muốn check trạng thái 3 layers
- Review tất cả decisions đã ra trong phiên
- Nghi ngờ AI đang mâu thuẫn với quyết định cũ

**AI thực hiện:**
1. Layer 1 status: ACTIVE_CONTEXT.md có không? Cũ bao lâu?
2. Layer 2 status: STATE.md / GEMINI.md relevant info
3. Layer 3 status: Top 3 LESSONS liên quan đến task hiện tại
4. Estimate: "~X% context window đã dùng"
5. Gợi ý nếu > 70%: "Nên checkpoint và bắt đầu phiên mới"

---

## ACTIVE_CONTEXT.MD — SCHEMA CHUẨN

```markdown
# ACTIVE_CONTEXT — [Tên Dự Án]
> Checkpoint #N | Phiên: YYYY-MM-DD HH:MM | Thời gian: Xh Ym

## ⚡ SNAPSHOT HIỆN TẠI
- **Task đang làm:** [Tên task từ NEXT-TODO.md]
- **Skill đang dùng:** [vibe-code / gsd / debugger / ...]
- **Wave/Phase:** Wave N — [Tên]
- **Files đang chỉnh:**
  - `path/to/file.ts` — [đang làm gì cụ thể]
  - `path/to/file.php` — [đang làm gì cụ thể]

## 🔑 QUYẾT ĐỊNH ĐÃ RA (Phiên này)
- [HH:MM] Chọn [pattern X] vì [lý do Y]
- [HH:MM] Bỏ approach [Z] vì [lý do W]

## ✅ ĐÃ HOÀN THÀNH (Phiên này)
- [x] [Task/sub-task] — commit: feat(x): mô tả

## 🔄 ĐANG LÀM (Dở dang)
- [ ] [Sub-task dở dang]
  - Đã làm: [những gì đã làm]
  - Chưa làm: [những gì còn lại]
  - Cần nhớ: [thông tin kỹ thuật quan trọng]

## ⛔ BLOCKERS
- [Issue nếu có / Cần hỏi user về điều gì]

## 🧠 CONTEXT KỸ THUẬT
- [Pattern đang áp dụng, dependency, config đặc biệt]

## ⚠️ LESSONS ĐÃ ÁP DỤNG
- #BUG-XXX: [Đang cẩn thận tránh]

## 🎯 NEXT IMMEDIATE ACTION
[Câu lệnh CỤ THỂ tiếp theo — đủ để AI mới bắt đầu ngay không cần hỏi]
Ví dụ: "Implement validatePayroll() trong PayrollService.php, dùng bcmath, xem #BUG-012"
```

---

## AUTO-CHECKPOINT RULES

| Trigger | Action |
|---|---|
| Sau mỗi commit | Mini checkpoint (cập nhật COMPLETED + NEXT ACTION) |
| Xong 1 wave đầy đủ | Full checkpoint |
| Context > 70% | Cảnh báo + gợi ý checkpoint |
| Gặp blocking error | Auto-capture vào BLOCKERS |
| User nói "nhớ lấy" | Append vào DECISIONS |
| Trước /save | **Mandatory** full checkpoint |

---

## QUY TẮC VÀNG

```
RULE 1: ACTIVE_CONTEXT phải đủ để "AI lạ" tiếp tục ngay
  → "AI lạ" = AI không biết gì về phiên này
  → Đọc xong → tiếp tục được = ĐÚNG. Không thể = THIẾU

RULE 2: NEXT IMMEDIATE ACTION phải actionable cụ thể
  → SAI: "Tiếp tục code"
  → ĐÚNG: "Implement function X trong file Y, dùng pattern Z, ref LESSONS #BUG-012"

RULE 3: Checkpoint sau COMMIT, không phải trước
  → Commit xác nhận task xong → mới checkpoint

RULE 4: KHÔNG ghi credentials vào ACTIVE_CONTEXT
  → Luôn dùng "xem PASSWORD.md" thay vì paste key thật

RULE 5: Xóa ACTIVE_CONTEXT sau /save
  → Phiên mới tạo lại từ đầu từ STATE.md
```

---

## TÍCH HỢP VỚI CÁC SKILL

| Skill | Memory Hook |
|---|---|
| `/start` | Đọc ACTIVE_CONTEXT → alert nếu phiên dở dang |
| `vibe-code` | Load ACTIVE_CONTEXT đầu task, mini-checkpoint sau commit |
| `/save` | Full checkpoint → consolidate → archive ACTIVE_CONTEXT |
| `/supper` | Đọc ACTIVE_CONTEXT để suggest skill phù hợp hơn |
| `bug-memory-journal` | Cross-ref LESSONS với ACTIVE_CONTEXT |
| `gsd` | /pause = /checkpoint, /resume = /recall |

---

## CONTEXT WINDOW MANAGEMENT

```
Dấu hiệu context đang đầy (cần checkpoint ngay):
  ⚠️ AI nói "Xin lỗi, tôi không nhớ/thấy..."
  ⚠️ AI đưa ra quyết định mâu thuẫn với trước đó
  ⚠️ AI yêu cầu bạn nhắc lại thông tin đã nói

Khi > 80% context:
  → Chỉ làm việc với ACTIVE_CONTEXT.md
  → Không đọc thêm file mới
  → Checkpoint ngay và gợi ý bắt đầu phiên mới
```
