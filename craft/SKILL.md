---
name: craft
description: "Thiết kế và xây dựng UI toàn diện: Atomic Design + Design Tokens + WCAG Accessibility + Performance audit. Gộp craft + quality. Kích hoạt khi người dùng nói /craft, /quality, 'design UI', 'component', 'CSS', 'responsive', 'accessibility', 'a11y', 'performance', 'lighthouse', 'test', 'audit'."
metadata:
  author: tampd
  version: 7.0.0
  category: frontend
---

# Craft — Design Web & Quality Gate

> **v7.0** — Gộp craft + quality
> Mục tiêu: UI đẹp, accessible, performant, tested.

---

## 4 MODES

### Mode 1 — `/craft setup` (Project Setup)

```
1. Chọn stack: Next.js / Vite / Astro + TypeScript strict
2. Tạo Atomic Design structure:
   components/
   ├── atoms/       ← Button, Input, Badge (no business logic)
   ├── molecules/   ← SearchBar, FormField (combine atoms)
   ├── organisms/   ← Header, DataTable, ProductCard
   ├── templates/   ← DashboardLayout, AuthLayout
   └── pages/       ← Actual routes

3. Setup Design System:
   design-system/
   ├── tokens.css     ← Primitive tokens (colors, spacing, fonts)
   ├── semantics.css  ← Semantic tokens (light/dark/high-contrast)
   ├── animations.css ← Keyframes, transitions, motion-reduce
   └── utilities.css  ← Layout helpers, visually-hidden
```

> Token templates chi tiết: `references/design-tokens.md`

---

### Mode 2 — `/craft component [name]` (Build Component)

```
STEP 1 — ANALYSIS:
  Atomic level? States? Responsive? Dark mode?

STEP 2 — DESIGN TOKEN CHECK:
  LUÔN dùng semantic tokens: var(--bg-surface), KHÔNG #ffffff
  Token mới → thêm vào tokens.css trước

STEP 3 — BUILD (Accessibility-first):
  HTML:
  [ ] Semantic elements (button, nav, article)
  [ ] ARIA: role, aria-label, aria-describedby
  [ ] Focus visible: custom focus ring
  INTERACTIVITY:
  [ ] Keyboard: Enter/Space, arrow keys
  [ ] Touch target ≥ 44×44px
  [ ] All states: hover, active, disabled, loading, error

STEP 4 — COMPONENT API DOCS:
  Props interface, accessibility notes, usage examples

STEP 5 — VISUAL VERIFY (Evidence):
  1. Desktop (1440px) 2. Tablet (768px) 3. Mobile (375px) 4. Dark mode
  > Screenshot evidence BẮT BUỘC
```

---

### Mode 3 — `/craft audit [page]` (Quality Audit)

Gộp a11y + perf + visual verify:

**A11y (WCAG 2.2 AA):**
```
npx jest --testPathPattern=a11y  # hoặc axe-core

KEYBOARD: Tab, Shift+Tab, Enter/Space, Escape, Arrow keys
SCREEN READER: Headings hierarchy, alt text, aria-label, role="status"
CONTRAST: Normal ≥ 4.5:1, Large ≥ 3:1, UI ≥ 3:1, Focus ≥ 3:1
```
> Full WCAG checklist: `references/wcag-checklist.md`

**Performance (Core Web Vitals):**
```bash
npx lighthouse https://yoursite.com --form-factor=mobile \
  --only-categories=performance,accessibility,best-practices,seo
```

| Metric | Target |
|---|---|
| LCP | < 2.5s |
| CLS | < 0.1 |
| INP | < 200ms |
| Lighthouse Performance | ≥ 90 |
| First Load JS | < 150KB gzip |

> Full perf checklist: `references/perf-targets.md`

---

### Mode 4 — `/craft test [module]` (Component Testing)

```
Test pyramid:
  Unit (70%): Service/util functions
  Integration (20%): API endpoints
  E2E (10%): Critical user journeys

React Testing Library best practices:
  - getByRole (semantic queries) > getByTestId
  - userEvent > fireEvent
  - waitFor / findBy cho async
  - jest-axe: toHaveNoViolations()
```

---

## OUTPUT FORMAT

```
🎨 CRAFT REPORT — [scope]
━━━━━━━━━━━━━━━━━━━━━━
🧪 TESTS:        [N/M pass | Coverage X%]
♿ ACCESSIBILITY: [✅ 0 violations | ⚠️ N]
⚡ PERFORMANCE:  [Lighthouse P:XX A:XX]
🔴 CRITICAL: [fix before merge]
🟡 WARNINGS: [fix this sprint]
✅ PASSING: [N checks]
```

## QUY TẮC

- ❌ KHÔNG hardcode colors → var(--xxx)
- ❌ KHÔNG mix Atomic levels
- ✅ LUÔN test 3 breakpoints + dark mode
- ✅ LUÔN attach screenshot evidence
- ✅ Lighthouse mobile ≥ 90 trước khi ship
