# 🧠 HỆ THỐNG "BỘ NHỚ LỖI" CHO GOOGLE ANTIGRAVITY
### Bug Memory System — Học từ sai lầm, không lặp lại

---

## MỤC LỤC

1. [Tại sao cần hệ thống này?](#-1-tại-sao-cần-hệ-thống-này)
2. [3 phương án và phương án tối ưu](#-2-ba-phương-án--phương-án-tối-ưu)
3. [Hướng dẫn cài đặt từng bước](#-3-hướng-dẫn-cài-đặt-từng-bước)
4. [Nội dung file SKILL.md (copy & paste)](#-4-nội-dung-file-skillmd)
5. [Nội dung file LESSONS.md mẫu (copy & paste)](#-5-nội-dung-file-lessonsmd-mẫu)
6. [Cách sử dụng hàng ngày](#-6-cách-sử-dụng-hàng-ngày)
7. [Ví dụ thực tế từ A→Z](#-7-ví-dụ-thực-tế)
8. [Mẹo nâng cao](#-8-mẹo-nâng-cao)

---

# 🔍 1. Tại sao cần hệ thống này?

Antigravity (và mọi AI agent) có một hạn chế cốt lõi: **không có bộ nhớ dài hạn giữa các phiên**. Mỗi lần bạn mở phiên mới, agent quên sạch mọi thứ đã xảy ra trước đó — kể cả bug đã fix, sai lầm đã mắc, hay bài học đã rút ra.

**Hệ quả:**
- Agent lặp lại cùng một lỗi mà bạn đã sửa tuần trước
- Bạn phải giải thích lại context mỗi lần
- Những pattern xấu cứ xuất hiện đi xuất hiện lại
- Mất thời gian debug cùng một vấn đề nhiều lần

**Giải pháp:** Tạo một "bộ nhớ ngoài" bằng file — agent đọc file này mỗi khi bắt đầu và tự kiểm tra trước khi viết code.

---

# 🎯 2. Ba phương án & Phương án tối ưu

## So sánh 3 cách tiếp cận

```
┌─────────────────────────────────────────────────────────────────────┐
│                    3 PHƯƠNG ÁN SO SÁNH                             │
├──────────────────┬──────────────┬──────────────┬───────────────────┤
│                  │ A. Chỉ file  │ B. Chỉ skill │ C. Skill + File  │
│                  │ LESSONS.md   │ SKILL.md     │ (KẾT HỢP)        │
├──────────────────┼──────────────┼──────────────┼───────────────────┤
│ Agent tự đọc?    │ ❌ Phải nhắc │ ✅ Tự kích   │ ✅ Tự kích        │
│ Ghi bài học mới? │ ✅ Dễ edit   │ ❌ Phức tạp  │ ✅ Dễ edit        │
│ Tìm kiếm nhanh? │ ⚠️ Thủ công  │ ❌ Không có  │ ✅ Có cấu trúc    │
│ Tự động check?   │ ❌ Không     │ ✅ Có        │ ✅ Có             │
│ Cập nhật liên tục│ ✅ Dễ        │ ⚠️ Restart   │ ✅ Dễ             │
│ Token cost       │ 🟢 Thấp     │ 🟡 Trung     │ 🟡 Trung          │
├──────────────────┼──────────────┼──────────────┼───────────────────┤
│ ĐÁNH GIÁ        │ 5/10         │ 6/10         │ ✅ 9/10           │
└──────────────────┴──────────────┴──────────────┴───────────────────┘
```

## ✅ Phương án tối ưu: C — Kết hợp Skill + File

Cách hoạt động:

```
┌───────────────────────────────────────────────────────┐
│  SKILL: bug-memory-journal (SKILL.md)                │
│  → Dạy agent QUY TRÌNH: khi nào đọc, khi nào ghi    │
│  → Tự kích hoạt khi bạn gặp bug hoặc fix xong bug   │
│  → Hướng dẫn agent format ghi chép chuẩn             │
└────────────────────┬──────────────────────────────────┘
                     │ đọc & ghi vào
                     ↓
┌───────────────────────────────────────────────────────┐
│  FILE: LESSONS.md (trong thư mục dự án)              │
│  → Kho dữ liệu: danh sách bug, nguyên nhân, cách fix│
│  → Cập nhật liên tục, không cần restart agent        │
│  → Agent đọc mỗi khi skill kích hoạt                 │
└───────────────────────────────────────────────────────┘
```

**Tại sao đây là phương án tốt nhất:**

1. **Skill** dạy agent *quy trình* — LUÔN đọc LESSONS.md trước khi code, LUÔN ghi lại sau khi fix bug
2. **File** lưu *dữ liệu* — dễ edit bằng tay, dễ cập nhật, không cần restart agent
3. Khi skill kích hoạt, agent đọc cả SKILL.md (quy trình) + LESSONS.md (dữ liệu lịch sử)
4. Bạn cũng có thể tự thêm bài học vào LESSONS.md bằng text editor bất kỳ

---

# 🛠️ 3. Hướng dẫn cài đặt từng bước

## Bước 1: Tạo thư mục skill

```bash
# Cài GLOBAL (dùng cho mọi dự án)
mkdir -p ~/.gemini/antigravity/skills/bug-memory-journal

# HOẶC cài WORKSPACE (dự án cụ thể)
mkdir -p .agent/skills/bug-memory-journal
```

## Bước 2: Tạo file SKILL.md

```bash
# Tạo file (global)
nano ~/.gemini/antigravity/skills/bug-memory-journal/SKILL.md

# HOẶC (workspace)
nano .agent/skills/bug-memory-journal/SKILL.md
```

Copy toàn bộ nội dung từ **Phần 4** bên dưới vào file này.

## Bước 3: Tạo file LESSONS.md trong dự án

```bash
# Trong THƯ MỤC GỐC của dự án
touch LESSONS.md
```

Copy nội dung mẫu từ **Phần 5** bên dưới vào file này.

## Bước 4: Khởi động lại Antigravity

Thoát phiên hiện tại và mở phiên mới để agent nhận diện skill.

## Bước 5: Kiểm tra

Gõ trong Antigravity:
> "Đọc file LESSONS.md và cho tôi biết có bao nhiêu bài học đã ghi"

Nếu agent phản hồi đúng → skill hoạt động.

---

# 📄 4. Nội dung file SKILL.md

**Copy TOÀN BỘ nội dung dưới đây** vào file `SKILL.md`:

---

```markdown
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
- Người dùng nói "/bug", "/lesson", "/remember", "/recall"
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
   - "⚠️ Tôi tìm thấy 2 bài học liên quan từ LESSONS.md:"
   - Liệt kê ngắn gọn
   - "Tôi sẽ áp dụng chúng trong giải pháp."

4. **Áp dụng bài học** vào code/giải pháp — chủ động tránh lỗi đã biết.

### GIAI ĐOẠN 2: GHI — Sau khi fix bug

Mỗi khi fix xong một bug hoặc phát hiện pattern quan trọng:

1. **Hỏi người dùng**: "Bạn muốn tôi ghi bài học này vào LESSONS.md không?"

2. **Nếu đồng ý**, thêm entry mới theo format chuẩn:

```
### [MÃ-SỐ] Tiêu đề ngắn gọn mô tả lỗi
- **Ngày:** YYYY-MM-DD
- **Mức độ:** 🔴 Critical | 🟡 Warning | 🟢 Info
- **Thẻ:** #tag1, #tag2, #tag3
- **Triệu chứng:** Mô tả lỗi bạn thấy
- **Nguyên nhân gốc:** Tại sao lỗi xảy ra
- **Cách fix:** Giải pháp cụ thể
- **Quy tắc rút ra:** Quy tắc LUÔN tuân theo từ nay
- **File liên quan:** path/to/file.ts (nếu có)
```

3. **Đánh số tự động**: đọc entry cuối cùng, tăng mã số lên 1.

4. **Xác nhận**: "✅ Đã ghi bài học #BUG-XXX vào LESSONS.md"

### GIAI ĐOẠN 3: NHẮC — Chủ động cảnh báo

Trong quá trình viết code, nếu phát hiện đang làm điều tương tự
một bài học trong LESSONS.md đã cảnh báo:

1. **Dừng lại ngay**
2. **Cảnh báo**: "🛑 CHỜ ĐÃ — Bài học #BUG-XXX cảnh báo về vấn đề này!"
3. **Trích dẫn** quy tắc từ bài học
4. **Áp dụng cách fix đúng** thay vì lặp lại lỗi cũ

### GIAI ĐOẠN 4: TỔNG HỢP — Định kỳ review

Khi người dùng nói "review lessons", "tổng hợp bài học", hoặc "/review":

1. Đọc toàn bộ LESSONS.md
2. Phân tích pattern: loại lỗi nào xuất hiện nhiều nhất?
3. Gợi ý: coding rules nên thêm vào dự án để phòng tránh
4. Đề xuất xóa/archive bài học đã lỗi thời

## Output Format

Khi cảnh báo bài học liên quan, dùng format:

```
⚠️ BÀI HỌC LIÊN QUAN từ LESSONS.md:

📌 #BUG-003 [🟡 Warning] — Hydration mismatch khi dùng Date
   Quy tắc: Luôn wrap Date-dependent content trong <ClientOnly>

📌 #BUG-007 [🔴 Critical] — API route thiếu error handling
   Quy tắc: Mọi API route PHẢI có try-catch + chuẩn error response

→ Tôi đang áp dụng cả 2 quy tắc vào giải pháp.
```

## Safety
- KHÔNG BAO GIỜ xóa nội dung LESSONS.md mà không hỏi người dùng
- KHÔNG sửa đổi bài học cũ — chỉ thêm mới hoặc đánh dấu [ARCHIVED]
- Khi LESSONS.md quá dài (>100 entries), đề xuất archive entries cũ
```

---

# 📝 5. Nội dung file LESSONS.md mẫu

**Copy toàn bộ nội dung dưới đây** vào file `LESSONS.md` ở thư mục gốc dự án:

---

```markdown
# 📒 LESSONS.md — Nhật ký Lỗi & Bài học Kinh nghiệm

> File này được đọc tự động bởi Antigravity skill `bug-memory-journal`.
> Agent sẽ kiểm tra file này trước khi viết code và ghi bài học mới sau khi fix bug.

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
- **Cách fix:** Dùng complete class names, không concatenate. Ví dụ: `bg-red-500` thay vì `` `bg-${color}-500` ``
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
- **Nguyên nhân gốc:** N8N có default timeout 60s, server xử lý task nặng (scraping 50 URLs) mất hơn 60s
- **Cách fix:** Chuyển sang async pattern: server nhận request → trả về 202 + job_id ngay → N8N poll kết quả sau
- **Quy tắc rút ra:** Với task xử lý >10s, LUÔN dùng async pattern (accept + poll) thay vì sync (wait for response). Thiết kế API endpoint: POST /jobs → 202 {jobId}, GET /jobs/:id → {status, result}
- **File liên quan:** `api/webhooks/scraper.ts`

---

<!-- THÊM BÀI HỌC MỚI BÊN DƯỚI DÒNG NÀY -->
```

---

# 🎮 6. Cách sử dụng hàng ngày

## Workflow 1: Bắt đầu task mới → Agent tự đọc

Bạn chỉ cần gõ task bình thường. Skill sẽ tự kích hoạt:

```
Bạn: "Tạo component hiển thị thời gian đặt hàng cho trang order history"

Antigravity (có skill):
⚠️ BÀI HỌC LIÊN QUAN từ LESSONS.md:

📌 #BUG-001 [🔴 Critical] — Hydration mismatch khi hiển thị thời gian
   Quy tắc: Luôn dùng Client Component cho Date-dependent content

→ Tôi sẽ tạo component dưới dạng Client Component ('use client')
  và dùng useEffect + useState để tránh hydration mismatch.

[... code component đúng cách ...]
```

## Workflow 2: Fix bug xong → Agent hỏi ghi lại

```
Bạn: "Lỗi: TypeError map is not a function ở ProductList"

Antigravity: [phân tích, tìm nguyên nhân, fix xong]
"Lỗi do API trả về object thay vì array khi không có sản phẩm.
Fix: thêm Array.isArray() check trước .map()

📝 Bạn muốn tôi ghi bài học này vào LESSONS.md không?"

Bạn: "Ghi đi"

Antigravity: "✅ Đã ghi bài học #BUG-004 vào LESSONS.md:
  #BUG-004 — API response không phải array gây TypeError
  Quy tắc: LUÔN validate Array.isArray(data) trước .map()"
```

## Workflow 3: Chủ động yêu cầu đọc/ghi

```
# Đọc bài học
Bạn: "Kiểm tra LESSONS.md xem có bài học gì liên quan đến Tailwind không"
Bạn: "/recall tailwind"

# Ghi bài học thủ công
Bạn: "Ghi vào LESSONS.md: khi dùng next/image, luôn set width+height
      tránh layout shift. Thẻ: #nextjs #image #cls. Mức: Warning"
Bạn: "/lesson Next/Image cần width+height tránh CLS"

# Review tổng hợp
Bạn: "Review toàn bộ LESSONS.md, pattern nào xuất hiện nhiều nhất?"
Bạn: "/review"
```

## Workflow 4: Agent chủ động cảnh báo

```
Bạn: "Tạo dynamic badge component, color dựa trên status prop"

Antigravity:
🛑 CHỜ ĐÃ — Bài học #BUG-002 cảnh báo về vấn đề này!

📌 #BUG-002 [🟡 Warning] — Tailwind class bị purge trong production
   Quy tắc: LUÔN viết class name đầy đủ, dùng object mapping

→ Tôi sẽ KHÔNG dùng `bg-${color}-500`.
  Thay vào đó, dùng mapping:

const statusColors = {
  pending: 'bg-yellow-100 text-yellow-800',
  active: 'bg-green-100 text-green-800',
  error: 'bg-red-100 text-red-800',
};
```

---

# 📚 7. Ví dụ thực tế

## Ví dụ đầy đủ: Từ gặp bug → Ghi → Tránh lặp lại

### Ngày 1: Gặp bug

```
Bạn: "Trang product bị trắng trên Safari mobile"

Antigravity: [debug, tìm ra nguyên nhân]
"Nguyên nhân: dùng gap trong flexbox, Safari iOS 14 không hỗ trợ.
Fix: thay gap bằng margin trên children.

📝 Bạn muốn tôi ghi bài học này vào LESSONS.md không?"

Bạn: "Ghi đi"

Antigravity: "✅ Đã ghi:
### #BUG-004 Flexbox gap không hoạt động trên Safari iOS 14
- Mức độ: 🔴 Critical
- Thẻ: #css, #safari, #mobile, #flexbox
- Quy tắc: KHÔNG dùng `gap` trong flex container cho mobile.
  Dùng `space-x-*` hoặc margin trên children thay thế."
```

### Ngày 7: Tránh lặp lại

```
Bạn: "Tạo horizontal scroll menu cho categories"

Antigravity:
⚠️ BÀI HỌC LIÊN QUAN từ LESSONS.md:

📌 #BUG-004 [🔴 Critical] — Flexbox gap trên Safari iOS 14
   Quy tắc: Không dùng gap, dùng space-x-* thay thế

→ Tôi sẽ dùng `flex` + `space-x-3` thay vì `flex` + `gap-3`
  để đảm bảo hoạt động trên Safari iOS.

[... code đúng cách ...]
```

---

## Ví dụ: Review tổng hợp

```
Bạn: "/review"

Antigravity:
📊 TỔNG HỢP LESSONS.MD — 12 bài học

Pattern xuất hiện nhiều nhất:
1. SSR/Hydration issues (4 lần) — #react #nextjs
2. CSS compatibility (3 lần) — #tailwind #safari #css
3. API error handling (3 lần) — #api #n8n
4. Type safety (2 lần) — #typescript

Gợi ý coding rules:
1. Thêm ESLint rule: no-direct-date-render
2. Tạo SafariCSS checklist vào PR template
3. Thêm Zod validation cho MỌI API endpoint
4. Bật TypeScript strict mode

Bài học có thể archive (>3 tháng, không tái phát):
- #BUG-002 (đã config safelist trong tailwind.config.ts)
```

---

# 🚀 8. Mẹo nâng cao

## Mẹo 1: LESSONS.md theo dự án vs Global

```
# LESSONS.md ở workspace → bài học riêng cho dự án
my-project/LESSONS.md

# LESSONS.md ở global → bài học chung cho mọi dự án
~/.gemini/LESSONS-GLOBAL.md
```

Cập nhật SKILL.md để agent đọc cả 2 file:
```markdown
1. Đọc `LESSONS.md` trong thư mục dự án hiện tại
2. Đọc `~/.gemini/LESSONS-GLOBAL.md` nếu tồn tại
3. Kết hợp cả 2 nguồn khi kiểm tra
```

## Mẹo 2: Phân loại bằng thẻ thông minh

Dùng hệ thống thẻ nhất quán:

```
Theo công nghệ:  #react #nextjs #tailwind #n8n #stripe #prisma
Theo loại lỗi:   #hydration #css-compat #type-error #api-error #build
Theo môi trường:  #production #safari #mobile #docker
Theo mức độ:      🔴 Critical (gây crash/trắng trang)
                  🟡 Warning (gây bug nhưng có workaround)
                  🟢 Info (best practice, tip tối ưu)
```

## Mẹo 3: Kết hợp với AGENTS.md hoặc Rules

Nếu dự án có file `AGENTS.md` hoặc `.antigravity/rules`:

```markdown
# Trong AGENTS.md hoặc rules file, thêm:

## Quy tắc bắt buộc
- Trước khi viết code, LUÔN đọc file LESSONS.md
- Sau khi fix bug, LUÔN hỏi có ghi vào LESSONS.md không
- Khi viết code liên quan đến thẻ trong LESSONS.md, cảnh báo trước
```

Điều này đảm bảo agent tuân thủ ngay cả khi skill chưa kích hoạt.

## Mẹo 4: Tích hợp Git

Thêm LESSONS.md vào version control:

```bash
git add LESSONS.md
git commit -m "docs: add bug lesson #BUG-004 flexbox gap safari"
```

Lợi ích: toàn team cùng học, Git history cho thấy lỗi nào được ghi khi nào.

## Mẹo 5: Template bài học nhanh (1 dòng)

Khi muốn ghi nhanh không cần detail:

```
Bạn: "/lesson 🟡 #tailwind #dark-mode — dark: prefix không hoạt
      động khi thiếu class dark trên html tag"

→ Agent tự format và thêm vào LESSONS.md
```

---

# 📋 TỔNG KẾT: CHECKLIST CÀI ĐẶT

```
☐ Tạo thư mục: ~/.gemini/antigravity/skills/bug-memory-journal/
☐ Tạo file SKILL.md (copy từ Phần 4)
☐ Tạo file LESSONS.md trong dự án (copy từ Phần 5)
☐ Khởi động lại Antigravity
☐ Test: "Đọc LESSONS.md cho tôi"
☐ Test: Fix 1 bug → xem agent có hỏi ghi lại không
☐ (Tùy chọn) Thêm rule vào AGENTS.md
☐ (Tùy chọn) Git commit LESSONS.md
```

---

*Tài liệu này giúp bạn biến Antigravity từ "agent hay quên" thành "agent có bộ nhớ dài hạn".
Mỗi lần fix bug là một lần agent trở nên thông minh hơn.*

*Tạo ngày: 24/02/2026*
