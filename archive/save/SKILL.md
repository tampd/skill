---
name: Save Workflow
description: "Công cụ tự động hóa quy trình lưu trữ, ghi chú, cập nhật LESSONS.md và đẩy mã nguồn lên GitHub với atomic commit. Kích hoạt bằng lệnh /save."
---

# HƯỚNG DẪN KỸ NĂNG: SAVE WORKFLOW

## Mục tiêu
Kết thúc phiên làm việc một cách toàn vẹn: docs cập nhật, bài học được ghi, code được push với commit message chuẩn. Không bao giờ kết thúc phiên mà không có commit.

---

## CÁC BƯỚC BẮT BUỘC

### Bước -1 — MEMORY AUDIT (MỚI — TRƯỚC TẤT CẢ)
```
1. Đọc ACTIVE_CONTEXT.md (nếu có):
   - Liệt kê: task nào đã xong, task nào còn dở
   - NẾU còn task dở → cảnh báo:
     "⚠️ Còn [task] chưa hoàn thành. Lưu tạm hay tiếp tục?"
   - Nếu lưu tạm → chạy /checkpoint ngay (memory-optimizer skill)

2. Merge thông tin quan trọng từ ACTIVE_CONTEXT vào STATE.md
```

---

### Bước 0 — Nhắc Review (KHÔNG SKIP)
```
⚠️ Bạn đã chạy lệnh /review chưa?
- Đã review → gõ "tiếp tục"
- Muốn review ngay → tôi sẽ chạy /review trước
- Muốn bỏ qua → gõ "bỏ qua" (chỉ nên dùng khi chỉ sửa docs)
```
**DỪNG** và chờ phản hồi. CHỈ tiếp tục khi được xác nhận.

---

### Bước 1 — Kiểm tra Task Đang Dở (GSD Awareness)
```
1. Đọc STATE.md (nếu tồn tại):
   - Có task nào đang dở trong Wave hiện tại không?
   - Nếu CÓ → cảnh báo: "⚠️ Còn [N] task chưa xong trong Wave X"
   - Hỏi: "Bạn muốn tiếp tục sau hay lưu trạng thái tạm?"

2. Đọc git status:
   - Liệt kê các file đã thay đổi
   - Nhóm theo feature/module để commit hợp lý
```

---

### Bước 2 — Cập nhật CHANGE_LOG.md
```
Format chuẩn:
## [vX.Y.Z] - YYYY-MM-DD

### Added (Tính năng mới)
- Mô tả tính năng đã thêm

### Fixed (Bug đã sửa)
- #BUG-XXX: Mô tả bug và cách fix

### Changed (Thay đổi)
- Mô tả thay đổi existing behavior

### Security
- Mô tả fix security (nếu có)
```

---

### Bước 3 — Cập nhật NEXT-TODO.md
```
Quy tắc:
- XÓA task đã hoàn thành trong phiên này
- THÊM task mới phát sinh
- Sắp xếp theo độ ưu tiên: 🔴 Cao → 🟡 Trung bình → 🟢 Thấp
- Ghi rõ: Task chưa xong do hết thời gian (carried over)
```

---

### Bước 4 — Cập nhật GEMINI.md / README.md
```
GEMINI.md: Cập nhật section Implementation Status + Development History Log
README.md: Cập nhật version, tính năng mới (nếu đáng kể)
```

---

### Bước 5 — Cập nhật LESSONS.md (Critical Step)

```
1. Nhìn lại phiên làm việc: Có bug nào fix? Pattern nào học được?

2. Nếu CÓ bài học mới:
   Hỏi: "📝 Phiên này có [N] bài học cần ghi. Ghi vào LESSONS.md không?"

3. Nếu đồng ý, ghi theo format:
```

```markdown
### #BUG-XXX Tiêu đề ngắn gọn (YYYY-MM-DD)

- **Mức độ:** 🔴 Critical | 🟡 Warning | 🟢 Info
- **Thẻ:** #laravel, #auth, #n8n (tối đa 3 tags)
- **Triệu chứng:** Mô tả lỗi quan sát được
- **Nguyên nhân gốc:** Tại sao lỗi xảy ra
- **Cách fix:** Giải pháp cụ thể (kèm code snippet nếu cần)
- **Quy tắc rút ra:** Quy tắc LUÔN áp dụng từ nay
- **File liên quan:** `path/to/file.php`
```

```
4. Cập nhật phần Thống kê nhanh đầu LESSONS.md:
   - Tổng số bug: +1
   - Lần cuối cập nhật: YYYY-MM-DD
```

> ❌ KHÔNG BAO GIỜ xóa nội dung LESSONS.md — chỉ thêm mới

---

### Bước 6 — Cập nhật STATE.md (GSD Integration)
```
Nếu dự án có PLAN.md/STATE.md:
- Cập nhật: phase hiện tại, wave hiện tại, task hoàn thành, task tiếp theo
- Mark task đã xong: [x] trong PLAN.md
```

---

### Bước 7 — Atomic Commit & Push

```bash
# Format commit message (conventional commits):
# feat(scope): mô tả ngắn gọn — 72 ký tự tối đa
# fix(scope): mô tả bug đã sửa
# chore(scope): task kỹ thuật, refactor, config
# docs(scope): chỉ cập nhật tài liệu
# sec(scope): security fix
# perf(scope): cải thiện performance

# Nếu nhiều thay đổi khác nhau → nhiều commit:
git add app/Services/PayrollService.php
git commit -m "feat(payroll): add bcmath precision for salary"

git add app/Http/Controllers/AuthController.php
git commit -m "fix(auth): prevent timing attack on login"

git add GEMINI.md README.md CHANGE_LOG.md
git commit -m "docs: update changelog and version to vX.Y.Z"

# Push lên GitHub
git push origin main
```

---

### Bước 7.5 — MEMORY CONSOLIDATION (MỚI — SAU PUSH)
```
1. Đã push thành công → archive ACTIVE_CONTEXT.md:
   - Rename: ACTIVE_CONTEXT.md → .archived/ACTIVE_CONTEXT_[date].md
   - Hoặc xóa nếu không cần lưu lịch sử

2. Xác nhận: "✅ Working memory đã được consolidate và archive"
3. Phiên mới sẽ tạo ACTIVE_CONTEXT.md mới khi cần
```

---

### Bước 8 — Kết thúc phiên

```
✅ Phiên /save hoàn thành!

📝 Docs đã cập nhật: CHANGE_LOG.md | NEXT-TODO.md | GEMINI.md
🧠 LESSONS.md: [N bài học mới / Không có thay đổi]
📦 Committed: [N commits]
🔗 Pushed: [branch] → [remote] (hash: xxxxxxx)

Task tiếp theo (phiên sau):
  → [Task ưu tiên cao nhất từ NEXT-TODO.md]

Gõ /start để bắt đầu phiên mới!
```

---

## QUY TẮC QUAN TRỌNG
- Luôn dùng tool edit file (không chỉ in ra màn hình)
- Commit message phải rõ WHY, không chỉ WHAT
- Nếu phiên chỉ sửa docs → commit type là `docs:`
- Số BUG trong LESSONS.md: đọc entry cuối cùng rồi tăng +1
