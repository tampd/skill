---
name: Review Workflow
description: "Công cụ tự động rà soát mã nguồn toàn diện trước khi lưu. Kiểm tra 7 tiêu chí: bugs, UI/UX, security, performance, code quality, empirical verification và đối chiếu LESSONS.md. Kích hoạt bằng lệnh /review."
---

# HƯỚNG DẪN KỸ NĂNG: REVIEW WORKFLOW

## Mục tiêu
Là code reviewer khắt khe nhất — phát hiện mọi vấn đề **trước** khi commit. Không trust "có vẻ ổn" — phải có evidence.

---

## BƯỚC THỰC HIỆN

### 1. Xác định scope review
```bash
git status         # Files đã thay đổi
git diff --stat    # Tổng quan thay đổi
```

### 2. Áp dụng 7 tiêu chí cho mỗi file thay đổi

### 3. Đối chiếu LESSONS.md

### 4. Xuất báo cáo theo format chuẩn

### 5. Đề xuất hành động

---

## 7 TIÊU CHÍ KIỂM TRA

### ① Bug & Logic Errors
```
❓ Kiểm tra:
- Biến chưa khởi tạo hoặc null pointer không xử lý?
- Điều kiện edge case (array rỗng, value âm, division by zero)?
- Vòng lặp có thể infinite loop không?
- Business logic có đúng yêu cầu không?
- Các thay đổi có break existing functionality không?
```

### ② UI/UX & Responsive
```
❓ Kiểm tra:
- CSS responsive: 1024px / 768px / 480px breakpoints?
- Color contrast WCAG AA: ≥ 4.5:1 cho text?
- ARIA labels cho interactive elements?
- Loading states / Empty states / Error states đầy đủ?
- Form validation messages rõ ràng?
```

### ③ Security (OWASP Top 10)
```
❓ Kiểm tra:
- SQL Injection: Dùng query builder/prepared statement? Escape wildcard %_?
- XSS: Escape output trong Blade ({{ }})? Không dùng {!! !!} với user input?
- CSRF: Có @csrf trong form POST?
- Authentication: Route có middleware auth/role không?
- Hardcoded secrets: API key, password trong code?
- File upload: Validate MIME type thực (không chỉ extension)?
```

### ④ Performance
```
❓ Kiểm tra:
- N+1 query: Có eager load với with()?
- Batch pre-fetch: Dùng whereIn() thay vì query trong loop?
- Missing index: Column dùng trong WHERE có index không?
- Heavy computation trong request: Có nên queue không?
- Cache stale: Cache có bị invalidate đúng không?
```

### ⑤ Code Quality
```
❓ Kiểm tra:
- Magic numbers: Có hằng số hoặc config thay vì số cứng?
- DRY: Logic bị duplicate ở 2+ nơi?
- Naming: Tên biến/hàm mô tả rõ ý nghĩa?
- Function too long: > 50 lines nên refactor?
- Dead code: Code không dùng đến (commented out > 1 tuần)?
- Strict types: Có type hinting và null-safe operators?
```

### ⑥ Empirical Verification
```
❓ Kiểm tra: Task này có evidence verify chưa?

| Loại thay đổi       | Evidence cần có               |
|---------------------|-------------------------------|
| API endpoint mới    | curl response 200 + schema    |
| DB migration        | migrate:status output         |
| Service logic       | php artisan test passed       |
| UI component        | Screenshot desktop + mobile   |
| N8N webhook         | N8N test trigger log          |
| Import/Export       | Sample file output            |

Nếu KHÔNG có evidence → yêu cầu tạo trước khi commit.
```

### ⑦ LESSONS.md Compliance
```
1. Đọc LESSONS.md
2. Với mỗi bài học 🔴 Critical / 🟡 Warning:
   - So sánh quy tắc với code mới
   - Nếu vi phạm → báo: 🛑 VI PHẠM #BUG-XXX: [dòng code vi phạm]
3. Nếu phát hiện bug mới trong review:
   → Đề xuất ghi vào LESSONS.md khi /save
```

---

## FORMAT BÁO CÁO

```
🔍 KẾT QUẢ REVIEW — [Tên dự án] [Version]
📅 [Timestamp]
📁 Files đã review: [danh sách]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
① BUGS & LOGIC:      [✅ Sạch | ⚠️ N cảnh báo | 🛑 N lỗi]
② UI/UX:             [✅ / ⚠️ / 🛑]
③ SECURITY:          [✅ / ⚠️ / 🛑]
④ PERFORMANCE:       [✅ / ⚠️ / 🛑]
⑤ CODE QUALITY:      [✅ / ⚠️ / 🛑]
⑥ VERIFICATION:      [✅ Evidence đủ | ❌ Thiếu evidence]
⑦ LESSONS CHECK:     [✅ Tuân thủ | 🛑 N vi phạm]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

📋 CHI TIẾT VẤN ĐỀ:

🛑 CRITICAL (phải sửa trước commit):
  - [File:Line] Mô tả vấn đề → Cách sửa

⚠️ WARNING (khuyến nghị sửa):
  - [File:Line] Mô tả vấn đề → Gợi ý

💡 SUGGESTIONS (tùy chọn):
  - [File:Line] Đề xuất cải thiện

🧠 LESSONS.MD:
  - ✅ Tuân thủ #BUG-XXX: [quy tắc]
  - 🛑 VI PHẠM #BUG-XXX: [File:Line] [mô tả vi phạm]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
📊 KẾT LUẬN:
  [✅ Mã nguồn sạch — sẵn sàng commit!]
  [⚠️ Có N warning — bạn có muốn sửa trước không?]
  [🛑 Có N lỗi nghiêm trọng — BẮT BUỘC sửa trước /save]
```

---

## QUY TẮC QUAN TRỌNG
- Nếu có 🛑 CRITICAL → KHÔNG cho phép /save cho đến khi sửa
- Nếu thiếu evidence verify → nhắc tạo evidence trước
- Bug mới phát hiện → ghi nhận để đưa vào LESSONS.md khi /save
- Nếu không có vấn đề gì: "✅ Mã nguồn sạch, đạt chuẩn chất lượng!"
