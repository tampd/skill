# 📒 LESSONS.md — Nhật ký Lỗi & Bài học Kinh nghiệm

> File này được đọc tự động bởi Antigravity skill `bug-memory-journal`.
> Agent sẽ kiểm tra file này trước khi viết code và ghi bài học mới sau khi fix bug.
> Lệnh nhanh: `/recall <keyword>` để tìm, `/lesson <mô tả>` để ghi, `/review` để tổng hợp.

---

## Thống kê nhanh
- Tổng bài học: 3
- 🔴 Critical: 1
- 🟡 Warning: 1
- 🟢 Info: 1
- Cập nhật lần cuối: 2026-02-24

---

## Danh sách bài học

### #BUG-001 Hydration mismatch khi hiển thị thời gian
- **Ngày:** 2026-02-24
- **Mức độ:** 🔴 Critical
- **Thẻ:** #react, #nextjs, #hydration, #ssr
- **Triệu chứng:** Lỗi "Text content does not match server-rendered HTML" khi hiển thị ngày/giờ
- **Nguyên nhân gốc:** Server render thời gian UTC, client render local timezone → mismatch
- **Cách fix:** Wrap date-dependent content trong component có `useEffect` + `useState`, chỉ render trên client
- **Quy tắc rút ra:** KHÔNG BAO GIỜ render `new Date()`, `Date.now()`, hay `toLocaleDateString()` trực tiếp trong Server Component. Luôn dùng Client Component với `'use client'` cho mọi nội dung phụ thuộc thời gian.
- **File liên quan:** `components/DateDisplay.tsx`

---

### #BUG-002 Tailwind class bị purge trong production
- **Ngày:** 2026-02-24
- **Mức độ:** 🟡 Warning
- **Thẻ:** #tailwind, #css, #production, #build
- **Triệu chứng:** Một số styles hoạt động ở dev nhưng biến mất khi build production
- **Nguyên nhân gốc:** Dynamic class names (string concatenation) không được Tailwind scanner detect
- **Cách fix:** Dùng complete class names, không concatenate. Ví dụ: `bg-red-500` thay vì `bg-${color}-500`
- **Quy tắc rút ra:** LUÔN viết Tailwind class name đầy đủ. Nếu cần dynamic, dùng object mapping:
  ```ts
  const colorMap = { red: 'bg-red-500', blue: 'bg-blue-500' };
  ```
- **File liên quan:** `tailwind.config.ts`, `components/Badge.tsx`

---

### #BUG-003 N8N webhook timeout khi xử lý lâu
- **Ngày:** 2026-02-24
- **Mức độ:** 🟢 Info
- **Thẻ:** #n8n, #webhook, #api, #async
- **Triệu chứng:** N8N HTTP Request node báo timeout sau 60s dù server vẫn đang xử lý
- **Nguyên nhân gốc:** N8N có default timeout 60s, server xử lý task nặng mất hơn 60s
- **Cách fix:** Chuyển sang async pattern: server nhận request → trả về 202 + job_id ngay → N8N poll kết quả sau
- **Quy tắc rút ra:** Với task xử lý >10s, LUÔN dùng async pattern (accept + poll) thay vì sync (wait for response). Thiết kế API endpoint: POST /jobs → 202 {jobId}, GET /jobs/:id → {status, result}
- **File liên quan:** `api/webhooks/scraper.ts`

---

<!-- ═══════════════════════════════════════════ -->
<!-- THÊM BÀI HỌC MỚI BÊN DƯỚI DÒNG NÀY      -->
<!-- ═══════════════════════════════════════════ -->
