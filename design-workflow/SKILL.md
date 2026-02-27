---
name: Design Workflow — Quy trình Design Website & Logo
description: "Quy trình chuẩn hóa cho design website, UI components và logo. Từ brief → mockup → implement → verify. Đảm bảo consistency design system, accessibility và SEO. Kích hoạt khi làm trang web mới, redesign UI, tạo logo/brand assets."
---

# HƯỚNG DẪN KỸ NĂNG: DESIGN WORKFLOW

## Mục tiêu
Mỗi output design phải: **đẹp → nhất quán → accessible → đã verify**. Không design rồi implement thiếu, không bỏ qua SEO.

---

## QUY TRÌNH 5 BƯỚC CHUẨN HÓA

### Bước 1 — DESIGN BRIEF (Thu thập yêu cầu)

Trả lời các câu hỏi trước khi mở bất kỳ editor nào:

```
🎯 Mục tiêu: Trang/component này phục vụ ai? Mục đích gì?
🎨 Phong cách: Tone (professional/playful/minimal)?
               Màu chủ đạo? Dark/Light mode?
               Tham khảo design nào (URL)?
📐 Kích thước: Desktop-first hay Mobile-first?
               Breakpoints: 1024px / 768px / 480px
📦 Assets: Cần logo? Icon? Image? Font?
```

---

### Bước 2 — MOCKUP (Generate hình ảnh tham khảo)

Với UI/landing page mới → dùng `generate_image`:
```
Prompt format tốt:
"[Loại trang] với [màu sắc] color scheme, [phong cách] design,
 [elements mô tả], clean layout, modern typography, [dark/light mode]"

Ví dụ:
"SaaS payroll dashboard với navy blue và teal accent,
 glassmorphism cards, sidebar navigation, data tables,
 dark mode, professional corporate design"
```

> 💡 Tạo 2-3 variation → chọn 1 → implement

---

### Bước 3 — IMPLEMENT (Với tailwind-design-system)

#### Design Token Checklist
```css
/* Phải định nghĩa trước khi code component */
--color-primary: ...     /* Brand color chính */
--color-secondary: ...   /* Màu phụ */
--color-bg: ...          /* Background */
--color-surface: ...     /* Card/panel background */
--color-text: ...        /* Text chính */
--color-text-muted: ...  /* Text phụ */

--spacing-xs: 4px;
--spacing-sm: 8px;
--spacing-md: 16px;
--spacing-lg: 24px;
--spacing-xl: 32px;

--radius-sm: 4px;
--radius-md: 8px;
--radius-lg: 12px;
```

#### Nguyên tắc viết CSS
```css
/* ✅ DOs */
- Mobile-first: @media (min-width: 768px) { ... }
- CSS custom properties cho theme tokens
- transition: 0.2s ease cho interactive elements
- :hover, :focus-visible cho accessibility

/* ❌ DON'Ts */
- !important (trừ override critical)
- px cứng cho font-size → dùng rem
- Màu hardcode: #fff → dùng var(--color-bg)
- z-index random → dùng scale: 10, 20, 30...
```

#### Dark/Light Mode
```css
/* Pattern chuẩn */
[data-theme="dark"] { --color-bg: #0f1117; }
[data-theme="light"] { --color-bg: #ffffff; }

/* Toggle script */
document.documentElement.setAttribute('data-theme', 
  localStorage.getItem('theme') || 'dark');
```

---

### Bước 4 — VERIFY (Kiểm tra visual)

Dùng `ui-visual-validator`:

#### Checklist kiểm tra bắt buộc
```
[ ] Desktop (1440px): Layout không bị vỡ
[ ] Tablet (768px): Sidebar collapse đúng
[ ] Mobile (375px): Stack vertical, text readable
[ ] Dark mode: Contrast ratio ≥ 4.5:1 (WCAG AA)
[ ] Light mode: Đủ tương phản
[ ] Focus states: Tab navigation visible
[ ] Hover effects: Có feedback rõ ràng
[ ] Loading states: Skeleton/spinner đầy đủ
[ ] Empty states: Message khi không có data
[ ] Error states: Error message rõ ràng
```

#### Screenshot evidence
```
Chụp screenshot ít nhất:
1. Desktop view (full page)
2. Mobile view (375px)
3. Dark + Light mode comparison
```

---

### Bước 5 — SEO & PERFORMANCE

Dùng `seo-structure-architect`:

```html
<!-- Checklist SEO bắt buộc cho mỗi trang -->
<title>Từ khóa chính | Tên thương hiệu</title>
<meta name="description" content="150-160 ký tự mô tả trang">
<meta property="og:image" content="1200x630 social image">

<!-- Heading hierarchy -->
<h1>Chỉ 1 H1 mỗi trang — từ khóa chính</h1>
<h2>Sections</h2>
<h3>Subsections</h3>

<!-- Semantic HTML -->
<header>, <nav>, <main>, <section>, <article>, <footer>
<button type="button"> thay vì <div onclick="">
<img alt="Mô tả cụ thể"> — không để alt=""
```

---

## LOGO & BRAND ASSETS

### Quy trình tạo logo
```
1. Brief: Tên thương hiệu, ngành, value, style tham khảo

2. generate_image với prompt:
   "Minimalist logo for [brand], [industry], 
    [color palette], vector style, clean lines, 
    modern design, [tagline if any]"

3. Tạo 3-5 variations → chọn concept

4. Implement CSS version (nếu text-based):
   - Không dùng ảnh cho logo text → dùng CSS/SVG
   - Đảm bảo sharp ở mọi DPI
   - Cả dark và light version

5. Export assets:
   - SVG (vector, scalable)
   - PNG 512px (favicon)
   - PNG 1200px (social media)
```

---

## COMBO SKILLS

| Tình huống | Combo |
|---|---|
| Landing page mới | `design-workflow` → `tailwind-design-system` → `seo-structure-architect` |
| Component redesign | `design-workflow` → `ui-visual-validator` |
| Logo + brand | `design-workflow` (generate_image) → `tailwind-design-system` (implement) |
| Full website | `design-workflow` → `tailwind-design-system` → `seo-structure-architect` → `performance-engineer` |

---

## DESIGN SYSTEM CONSISTENCY RULES

```
Mỗi dự án chỉ dùng 1 design system file (thường là app.css hoặc index.css).
Không tạo CSS inline trong component — luôn dùng class.
Không dùng 2 font family khác nhau trừ heading vs body.
Spacing luôn là bội số của 4px (4, 8, 12, 16, 20, 24, 32, 48, 64).
Màu sắc: tối đa 3 màu chính (primary, secondary, accent) + neutrals.
```
