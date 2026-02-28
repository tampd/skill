---
name: Design — UI/UX Production Workflow
description: "Quy trình design toàn diện: brief → mockup → implement → visual verify → SEO. Thay thế design-workflow + tailwind-design-system + ui-visual-validator + seo-structure-architect. Kích hoạt bằng /design [task]."
---

# /design [task description]

> **4-in-1** — Gộp design-workflow + tailwind + ui-validator + seo
> Mục tiêu: UI đẹp, accessible, responsive, SEO-ready

---

## QUY TRÌNH 5 BƯỚC

### Step 1 — BRIEF
```
1. Đọc GEMINI.md → design system hiện có, CSS variables
2. Đọc LESSONS.md → grep #css, #blade, #theme, #ui
3. Xác định:
   - Screen nào? (page/component/modal)
   - Target users? (admin/employee/public)
   - Responsive requirements? (desktop/tablet/mobile)
   - Dark mode? Light mode? Both?
```

### Step 2 — DESIGN SYSTEM CHECK
```
1. CSS Variables hiện có:
   --bg-surface, --bg-card, --text-primary, --border-color
   → LUÔN dùng variables, KHÔNG hardcode colors

2. Component patterns đã có:
   → Reuse existing patterns trước khi tạo mới
   → Check layout/sidebar/card templates

3. Typography & spacing:
   → Font system, spacing scale, border radius
```

### Step 3 — IMPLEMENT
```
Coding rules:
- CSS variables cho MỌI color (support light + dark)
- Responsive breakpoints: 1024px / 768px / 480px
- ARIA labels cho interactive elements
- Loading/empty/error states đầy đủ
- Form validation messages rõ ràng
- Glassmorphism, gradients → dùng CSS variables cho tones
```

### Step 4 — VISUAL VERIFY
```
1. Desktop (1440px+): Layout đúng? Spacing?
2. Tablet (768-1024px): Sidebar collapse? Grid adapt?
3. Mobile (< 768px): Touch targets ≥ 44px? Text readable?
4. Dark mode: Colors adequate contrast? No hardcoded grays?
5. Screenshot mỗi breakpoint → attach evidence
```

### Step 5 — SEO CHECK (cho pages)
```
- Title tag: descriptive, ≤ 60 chars
- Meta description: compelling, ≤ 155 chars
- Single <h1> per page, proper heading hierarchy
- Semantic HTML5 elements
- Unique IDs for interactive elements
- Image alt text
```

---

## QUY TẮC
- ❌ KHÔNG hardcode dark colors → dùng var(--bg-surface)
- ❌ KHÔNG inline styles cho theme colors
- ✅ LUÔN test 3 breakpoints + dark mode
- ✅ LUÔN attach screenshots evidence
