---
name: Craft — UI/UX + Frontend Production Workflow (v6.0)
description: "Quy trình thiết kế & xây dựng UI toàn diện: Atomic Design + Design Tokens + WCAG A11y + Animation System + Component API Docs. Gộp design + frontend. Modes: setup, component, css, a11y. Kích hoạt bằng /craft [task]."
---

# /craft [task description]

> **UI Lifecycle Skill v6.0** — Từ setup → design → implement → verify
> Gộp: `/design` + `/frontend` (v5.1) → Không overlap, 1 nơi duy nhất
> Mục tiêu: UI đẹp, accessible, responsive, token-based, type-safe

---

## 4 MODES

### Mode 1 — `/craft setup` (Project Structure)

Thiết lập kiến trúc frontend cho project mới:

```
1. Stack Choice:
   - Next.js App Router (default) / Vite + React / Vanilla HTML+CSS
   - TypeScript strict mode BẮT BUỘC

2. File Structure — Atomic Design:
   components/
   ├── atoms/          ← Button, Input, Icon, Badge, Spinner
   │   ├── Button/
   │   │   ├── index.tsx
   │   │   └── Button.test.tsx
   │   └── index.ts    ← Barrel export
   ├── molecules/      ← SearchBar, FormField, Toast (ghép 2+ atoms)
   ├── organisms/      ← Header, DataTable, ProductCard (business logic)
   ├── templates/      ← DashboardLayout, AuthLayout (layout shells)
   └── pages/          ← Actual routes (Next.js: app/)

3. Design System Files:
   design-system/
   ├── tokens.css      ← Primitive tokens (colors, spacing, fonts)
   ├── semantics.css   ← Semantic tokens (light/dark/high-contrast)
   └── animations.css  ← Motion tokens + prefers-reduced-motion

4. TypeScript Config:
   - strict: true
   - noUncheckedIndexedAccess: true
   - Path aliases: @/components, @/lib, @/types

5. Barrel Exports:
   import { Button, Input, FormField } from '@/components'
```

#### Atomic Design Rules:
```
ATOMS — Smallest UI elements:
  → KHÔNG có business logic
  → PHẢI accept className prop
  → PHẢI forward ref
  → PHẢI có aria attributes

MOLECULES — Ghép 2+ atoms:
  → Xử lý local state của nhóm
  → KHÔNG biết về global state

ORGANISMS — Business components:
  → CÓ THỂ connect global state / fetch data
  → PHẢI có Component API Docs

TEMPLATES — Layout shells (chỉ slots/children)
PAGES — Routes (connect templates + organisms + data + SEO)
```

---

### Mode 2 — `/craft component [name]` (Design + Implement)

Toàn bộ lifecycle 1 component: design → implement → verify.

#### Step 1 — BRIEF
```
1. Đọc GEMINI.md → design system hiện có
2. Đọc LESSONS.md → grep #css, #ui, #a11y
3. Xác định:
   - Atomic level? (atom / molecule / organism)
   - States? (default, hover, active, disabled, loading, error)
   - Responsive? (desktop / tablet / mobile)
   - Dark mode support?
```

#### Step 2 — DESIGN TOKEN CHECK
```
1. Scan existing tokens:
   → LUÔN dùng semantic tokens, KHÔNG hardcode colors
   → Ví dụ: var(--bg-surface), KHÔNG #ffffff

2. Nếu cần token mới:
   → Thêm primitive → thêm semantic → dùng semantic

3. Token Naming:
   --[category]-[element]-[property]-[state]
   → --button-primary-bg-hover
   → --input-border-color-focus
```

#### Step 2.5 — SELF-REASONING GATE ⭐ (v6.1)

> Trước khi implement component, AI chạy 3-Question Self-Check:
> Q1: "Design pattern này vs alternatives? (e.g. Compound vs Render Props vs Hooks)"
> Q2: "A11y risks? Performance concerns? Bundle size impact?"
> Q3: "Component scope phức tạp → cần user review design trước?"
>
> Output: ✅ SELF-CHECK PASSED | ⚠️ SELF-CHECK FLAG → đề xuất user

#### Step 3 — IMPLEMENT (ARIA-first)
```
ACCESSIBILITY CHECKLIST mỗi component (WCAG AA BẮT BUỘC):
[ ] Semantic HTML (button vs div, nav vs div)
[ ] Role attribute khi cần (role="alert", role="dialog")
[ ] aria-label cho icons, buttons không text
[ ] aria-describedby cho form errors
[ ] aria-live="polite" cho dynamic content
[ ] aria-expanded cho accordion/menu
[ ] aria-busy cho loading states
[ ] Tabindex: chỉ 0 hoặc -1, KHÔNG positive
[ ] Focus visible: custom focus ring, KHÔNG xóa outline

INTERACTIVITY:
[ ] Keyboard: Enter/Space buttons; arrow keys menus
[ ] Touch target ≥ 44×44px (mobile)
[ ] Hover + Active + Disabled + Loading + Error states

THEMING:
[ ] Light mode contrast OK
[ ] Dark mode contrast OK
[ ] prefers-contrast: high supported
```

> ⚠️ **TDD Exception**: CSS-only = visual verify. Component có logic = BẮT BUỘC TDD.

```typescript
// ✅ Component Pattern (forwardRef + aria + states)
export interface ButtonProps extends React.ButtonHTMLAttributes<HTMLButtonElement> {
  variant?: 'primary' | 'secondary' | 'ghost' | 'destructive'
  size?: 'sm' | 'md' | 'lg'
  isLoading?: boolean
}

export const Button = forwardRef<HTMLButtonElement, ButtonProps>(
  ({ variant = 'primary', size = 'md', isLoading, children, ...props }, ref) => (
    <button
      ref={ref}
      className={cn(buttonVariants({ variant, size }), props.className)}
      aria-busy={isLoading}
      disabled={isLoading || props.disabled}
      {...props}
    >
      {isLoading ? <Spinner size="sm" aria-hidden /> : children}
    </button>
  )
)
Button.displayName = 'Button'
```

#### Step 4 — COMPONENT API DOCS (BẮT BUỘC cho organisms)
```markdown
## [ComponentName]

### Purpose
[Khi nào dùng? Khi nào KHÔNG dùng?]

### Props / API
| Prop | Type | Default | Required | Mô tả |
|------|------|---------|----------|-------|

### States & Visual
| State | Trigger | Visual | ARIA |
|-------|---------|--------|------|

### Accessibility
- Keyboard navigation
- Screen reader behavior
- Focus management
- Contrast verification

### Usage Examples
[Code examples]

### Responsive Behavior
- Desktop / Tablet / Mobile specifics
```

#### Step 5 — VISUAL VERIFY (Evidence bắt buộc)
```
1. Desktop (1440px): Layout + spacing
2. Tablet (768px): Responsive adapt
3. Mobile (375px): Touch targets ≥ 44px
4. Dark mode: Contrast OK? No hardcoded grays?
5. Reduced motion: prefers-reduced-motion respected?
6. axe DevTools: 0 violations
7. Screenshot mỗi state → attach evidence
```

---

### Mode 3 — `/craft css` (CSS Architecture)

#### Design Token System (BẮT BUỘC)

```css
/* design-system/tokens.css — PRIMITIVE TOKENS */
:root {
  /* Colors */
  --color-blue-50: #eff6ff;
  --color-blue-500: #3b82f6;
  --color-blue-600: #2563eb;
  --color-gray-50: #f9fafb;
  --color-gray-900: #111827;

  /* Spacing (4px base) */
  --space-1: 4px; --space-2: 8px; --space-3: 12px;
  --space-4: 16px; --space-6: 24px; --space-8: 32px;

  /* Typography */
  --font-sans: 'Inter', system-ui, sans-serif;
  --font-mono: 'JetBrains Mono', monospace;
  --text-sm: 14px; --text-base: 16px; --text-lg: 18px;

  /* Borders & Shadows */
  --radius-sm: 4px; --radius-md: 8px; --radius-lg: 12px;
  --shadow-sm: 0 1px 2px rgb(0 0 0 / 0.05);
  --shadow-md: 0 4px 6px -1px rgb(0 0 0 / 0.1);
}

/* design-system/semantics.css — SEMANTIC TOKENS */
[data-theme="light"], :root {
  --bg-page: var(--color-gray-50);
  --bg-surface: #ffffff;
  --bg-elevated: #ffffff;
  --text-primary: var(--color-gray-900);
  --text-secondary: var(--color-gray-600);
  --color-brand: var(--color-blue-600);
  --border-color: var(--color-gray-200);
  --focus-ring: 0 0 0 3px rgb(59 130 246 / 0.5);
}

[data-theme="dark"] {
  --bg-page: var(--color-gray-950);
  --bg-surface: var(--color-gray-900);
  --text-primary: var(--color-gray-50);
  --color-brand: var(--color-blue-400);
  --border-color: var(--color-gray-700);
}
```

#### Animation System

```css
/* design-system/animations.css */
:root {
  --duration-fast: 150ms;
  --duration-normal: 250ms;
  --duration-slow: 400ms;
  --ease-in-out: cubic-bezier(0.4, 0, 0.2, 1);
  --ease-spring: cubic-bezier(0.175, 0.885, 0.32, 1.275);
}

/* BẮT BUỘC: Respect reduced motion */
@media (prefers-reduced-motion: reduce) {
  *, *::before, *::after {
    animation-duration: 0.01ms !important;
    transition-duration: 0.01ms !important;
  }
}
```

#### SEO Check (cho pages)
```
[ ] title tag: ≤ 60 chars, unique per page
[ ] meta description: ≤ 155 chars
[ ] Single <h1>, proper hierarchy
[ ] Semantic HTML5
[ ] Image alt text
[ ] Open Graph tags
```

---

### Mode 4 — `/craft a11y [page]` (WCAG Audit)

```
WCAG 2.2 AA CHECKLIST:

PERCEIVABLE:
[ ] Text alternatives: mọi img có alt
[ ] Color contrast: ≥ 4.5:1 text, ≥ 3:1 large text
[ ] Info không phụ thuộc chỉ vào color

OPERABLE:
[ ] Keyboard accessible: Tab order logic
[ ] Focus visible: rõ ràng
[ ] Touch target ≥ 44×44px
[ ] No keyboard traps (trừ modal)

UNDERSTANDABLE:
[ ] lang attr trên <html>
[ ] Error messages specific, actionable
[ ] Labels associated with inputs

ROBUST:
[ ] Valid HTML
[ ] ARIA correct (role, state, property)
[ ] Status messages: role="status" / role="alert"
```

---

## DESIGN TOKEN TABLE (Quick Reference)

| Token | Light | Dark | Usage |
|-------|-------|------|-------|
| --bg-page | #f9fafb | #111827 | Page background |
| --bg-surface | #ffffff | #1f2937 | Card, panel bg |
| --text-primary | #111827 | #f9fafb | Body text |
| --text-secondary | #6b7280 | #9ca3af | Subtext |
| --color-brand | #2563eb | #60a5fa | Primary actions |
| --border-color | #e5e7eb | #374151 | Dividers |
| --focus-ring | blue 3px | blue 3px | Focus state |

---

## QUY TẮC

- ❌ KHÔNG hardcode colors, fonts, spacing — dùng semantic tokens
- ❌ KHÔNG dùng div/span khi có semantic element
- ❌ KHÔNG xóa focus outline mà không thay thế
- ❌ KHÔNG animation mà không có prefers-reduced-motion
- ❌ KHÔNG mix Atomic levels
- ✅ LUÔN test 3 breakpoints + dark mode + axe DevTools
- ✅ LUÔN attach screenshot evidence
- ✅ LUÔN viết Component API docs cho organisms
- ✅ LUÔN type props với explicit interface (no `any`)
