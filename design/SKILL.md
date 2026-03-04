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

### Step 2.5 — DESIGN TOKEN SYSTEM 🆕

> ⭐ **v4.0** — Design tokens = DNA của visual system

```
1. Atomic Tokens (Primitive — Giá trị raw):
   --color-blue-500: #3b82f6
   --color-gray-100: #f1f5f9
   --space-16: 16px
   --font-size-base: 16px
   --radius-md: 8px

2. Semantic Tokens (Contextual — Ý nghĩa):
   --color-primary → maps to --color-blue-500
   --color-danger → maps to --color-red-500
   --button-bg → maps to --color-primary
   --alert-warning-bg → maps to --color-yellow-100
   --text-muted → maps to --color-gray-500

3. Mode Tokens (Dark/Light/High-Contrast):
   [data-theme="light"] {
     --bg-surface: #ffffff;
     --text-primary: #0f172a;
     --border-color: #e2e8f0;
   }
   [data-theme="dark"] {
     --bg-surface: #0f172a;
     --text-primary: #f8fafc;
     --border-color: #334155;
   }

4. Token Documentation Table:
   | Token | Light | Dark | Usage | WCAG |
   |-------|-------|------|-------|------|
   | --bg-surface | #fff | #0f172a | Page background | - |
   | --text-primary | #0f172a | #f8fafc | Body text | AA ✅ |
   | --color-primary | #2563eb | #60a5fa | Buttons, links | AA ✅ |

5. Naming Convention:
   --[category]-[element]-[property]-[state]
   Examples:
     --button-primary-bg-hover
     --input-border-color-focus
     --card-shadow-default
```

> 🛑 **RULE**: KHÔNG hardcode colors — luôn dùng semantic tokens.
> KHÔNG dùng --color-blue-500 trực tiếp — dùng --color-primary.

---

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

### Step 3.5 — COMPONENT API DOCS 🆕

> ⭐ **v4.0** — Mỗi component PHẢI có docs structured cho AI đọc được

```markdown
## [ComponentName]

### Purpose
[Khi nào dùng component này? 1-2 câu]

### Props / API
| Prop | Type | Default | Mô tả |
|------|------|---------|-------|
| variant | 'primary' \| 'secondary' \| 'ghost' | 'primary' | Style variant |
| size | 'sm' \| 'md' \| 'lg' | 'md' | Kích thước |
| disabled | boolean | false | Vô hiệu hóa |

### States
| State | CSS Class | Visual |
|-------|-----------|--------|
| Default | .btn | [mô tả] |
| Hover | .btn:hover | [mô tả] |
| Active | .btn:active | [mô tả] |
| Disabled | .btn:disabled | [mô tả] |
| Loading | .btn.is-loading | Spinner + text mờ |

### Accessibility
- Role: `button`
- Keyboard: Enter/Space để activate
- Screen reader: aria-label bắt buộc nếu icon-only
- Focus: visible focus ring (--color-focus-ring)
- Contrast: text/bg ratio ≥ 4.5:1 (WCAG AA)

### Code Reference
```html
<button class="btn btn--primary btn--md" aria-label="Submit form">
  Submit
</button>
```

### Responsive Behavior
- Desktop: full width hoặc inline
- Mobile: full width, touch target ≥ 44px
```

---

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
