---
name: bug-memory-journal
description: >
  Nhật ký lỗi và bài học kinh nghiệm. Đọc file LESSONS.md trước khi viết code
  để tránh lặp lại sai lầm. Ghi lại bài học mới sau mỗi lần fix bug.
  Kích hoạt khi gặp lỗi, debug, fix bug, hoặc bắt đầu task mới.
  Use PROACTIVELY before writing code, after fixing bugs, or when encountering errors.
---

# Bug Memory Journal — Nhật ký lỗi & Bài học kinh nghiệm

## Mục đích
Skill này biến agent thành hệ thống "có bộ nhớ" — học từ sai lầm trong quá khứ
và không bao giờ lặp lại chúng. Agent duy trì một nhật ký lỗi (`LESSONS.md`)
trong mỗi dự án, đọc trước khi code và ghi lại sau khi fix.

## Use this skill when
- Bắt đầu một task mới (đọc LESSONS.md để kiểm tra bài học liên quan)
- Gặp lỗi hoặc bug trong quá trình phát triển
- Sau khi fix xong một bug (ghi lại bài học)
- Người dùng nói "ghi lại bài học", "log lỗi", "nhớ lấy", "đừng lặp lại"
- Người dùng nói "/bug", "/lesson", "/remember", "/recall", "/review"
- Review code và phát hiện pattern lỗi đã biết
- Người dùng hỏi "lỗi thường gặp", "kiểm tra lại", "tôi đã gặp lỗi này chưa"

## Do not use this skill when
- Câu hỏi chung không liên quan đến code hoặc bug
- Người dùng chỉ muốn thảo luận lý thuyết
- Task không liên quan đến dự án hiện tại

## Instructions

### GIAI ĐOẠN 1: ĐỌC — Trước khi bắt đầu task

Mỗi khi nhận task mới liên quan đến code, hãy:

1. **Kiểm tra xem file `LESSONS.md` có tồn tại** trong thư mục dự án
   - Nếu KHÔNG có: thông báo cho người dùng và đề nghị tạo
   - Nếu CÓ: đọc toàn bộ nội dung

2. **Quét bài học liên quan** đến task hiện tại:
   - Tìm theo TỪ KHÓA: framework, thư viện, loại lỗi
   - Tìm theo THẺ: #react, #nextjs, #tailwind, #api, #n8n, v.v.
   - Tìm theo MỨC ĐỘ: 🔴 Critical > 🟡 Warning > 🟢 Info

3. **Cảnh báo người dùng** nếu tìm thấy bài học liên quan:
   - "⚠️ Tôi tìm thấy N bài học liên quan từ LESSONS.md:"
   - Liệt kê ngắn gọn
   - "Tôi sẽ áp dụng chúng trong giải pháp."

4. **Áp dụng bài học** vào code/giải pháp — chủ động tránh lỗi đã biết.

### GIAI ĐOẠN 2: GHI — Sau khi fix bug

Mỗi khi fix xong một bug hoặc phát hiện pattern quan trọng:

1. **Hỏi người dùng**: "📝 Bạn muốn tôi ghi bài học này vào LESSONS.md không?"

2. **Nếu đồng ý**, thêm entry mới vào cuối LESSONS.md theo format:

```
### #BUG-[SỐ] Tiêu đề ngắn gọn mô tả lỗi
- **Ngày:** YYYY-MM-DD
- **Mức độ:** 🔴 Critical | 🟡 Warning | 🟢 Info
- **Thẻ:** #tag1, #tag2, #tag3
- **Triệu chứng:** Mô tả lỗi nhìn thấy
- **Nguyên nhân gốc:** Tại sao lỗi xảy ra
- **Cách fix:** Giải pháp cụ thể với code ví dụ nếu cần
- **Quy tắc rút ra:** Quy tắc LUÔN tuân theo từ nay
- **File liên quan:** path/to/file (nếu có)
```

3. **Đánh số tự động**: đọc entry cuối, tăng mã số lên 1.

4. **Cập nhật thống kê** ở đầu LESSONS.md (tổng, critical, warning, info, ngày).

5. **Xác nhận**: "✅ Đã ghi bài học #BUG-XXX vào LESSONS.md"

### GIAI ĐOẠN 3: NHẮC — Chủ động cảnh báo

Trong quá trình viết code, nếu phát hiện đang làm điều tương tự
một bài học trong LESSONS.md đã cảnh báo:

1. **Dừng lại ngay**
2. **Cảnh báo**: "🛑 CHỜ ĐÃ — Bài học #BUG-XXX cảnh báo về vấn đề này!"
3. **Trích dẫn** quy tắc từ bài học
4. **Áp dụng cách fix đúng** thay vì lặp lại lỗi cũ

### GIAI ĐOẠN 4: TỔNG HỢP — Khi người dùng yêu cầu /review

Khi người dùng nói "review lessons", "tổng hợp bài học", hoặc "/review":

1. Đọc toàn bộ LESSONS.md
2. Phân tích pattern: loại lỗi nào xuất hiện nhiều nhất?
3. Nhóm theo thẻ và mức độ
4. Gợi ý: coding rules, linter configs, hoặc checklist để phòng tránh
5. Đề xuất archive bài học đã lỗi thời (>3 tháng, không tái phát)

## Output Format

Khi cảnh báo bài học liên quan:
```
⚠️ BÀI HỌC LIÊN QUAN từ LESSONS.md:

📌 #BUG-003 [🟡 Warning] — Mô tả ngắn
   Quy tắc: Nội dung quy tắc

→ Tôi đang áp dụng quy tắc này vào giải pháp.
```

Khi chủ động cảnh báo:
```
🛑 CHỜ ĐÃ — Bài học #BUG-XXX cảnh báo về vấn đề này!
   Quy tắc: Nội dung quy tắc
→ Tôi sẽ làm theo cách đúng thay vì lặp lại lỗi.
```

## Safety
- KHÔNG BAO GIỜ xóa nội dung LESSONS.md mà không hỏi người dùng
- KHÔNG sửa đổi bài học cũ — chỉ thêm mới hoặc đánh dấu [ARCHIVED]
- Khi LESSONS.md quá dài (>100 entries), đề xuất archive entries cũ vào LESSONS-ARCHIVE.md
