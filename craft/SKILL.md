---
name: craft
description: "UI/UX implementation and design system. Use for /craft (build UI components), /audit (quality audit), /tokens (design system), /e2e (browser visual testing). Triggers on: UI, component, design, giao diện, responsive, WCAG, accessibility, craft, audit giao diện."
---

# Craft Skill — UI Engineering + Design Tokens + WCAG + Browser Testing (v4.0)

> 📂 Chi tiết bổ sung: `craft/references/patterns.md` (component docs, token structure, audit detail)

---

## /craft [task]
> Build UI components theo Atomic Design, đạt WCAG 2.2 AA

```
PHASE 1 — DESIGN SYSTEM CHECK
  □ Đọc design tokens hiện có (colors, spacing, typography, radius)
  □ Nếu chưa có tokens → tạo tokens trước, không hardcode
  □ Identify component level: Atom / Molecule / Organism / Template?
  □ Check: component tương tự đã tồn tại chưa? (DRY)

PHASE 2 — COMPONENT SPEC
  □ Props interface (TypeScript): required vs optional
  □ Variants: size (sm/md/lg), state (default/hover/active/disabled/error)
  □ Responsive behavior: mobile-first breakpoints
  □ Animation/transition: prefers-reduced-motion check
  □ Dark mode: nếu project hỗ trợ

PHASE 3 — IMPLEMENT
  □ Semantic HTML (đúng element cho đúng role)
  □ Design tokens ONLY (không hardcode)
  □ Keyboard navigation: Tab order, focus visible, Enter/Space/Arrow
  □ ARIA attributes: role, aria-label, aria-describedby, aria-live
  □ Error states + Loading states (skeleton / spinner với aria-busy)

PHASE 4 — WCAG CHECKLIST (bắt buộc)
  □ Color contrast ≥4.5:1 (text), ≥3:1 (large text/UI)
  □ Focus indicator rõ ràng
  □ Touch target ≥44×44px (mobile)
  □ Alt text, heading hierarchy, form labels

PHASE 5 — PERFORMANCE CHECK
  □ Images: WebP/AVIF, lazy loading, srcset
  □ Animation: GPU-accelerated (transform, opacity)
  □ Bundle impact: import cụ thể

PHASE 6 — BROWSER AGENT VERIFY
  □ Screenshots: desktop, tablet (768px), mobile (375px)
  □ Dark mode screenshot nếu applicable
  □ Keyboard navigation test + hover/focus states
```

---

## /audit [scope]
> Multi-dimensional quality audit cho UI

```
4 DIMENSIONS: Visual Consistency 🎨 | Accessibility ♿ | Performance ⚡ | Responsive 📱
OUTPUT: Scored report với CRITICAL / WARNING / INFO

→ Chi tiết audit checklist: references/patterns.md
```

---

## /tokens [scope]
> Tạo hoặc mở rộng design token system

```
TOKEN CATEGORIES:
  Colors:     --color-primary-50 → 900 + semantic tokens
  Spacing:    --space-1 (4px) → --space-16 (64px)
  Typography: --font-size-xs → 4xl, --font-weight-*
  Effects:    --radius-*, --shadow-*, --duration-*

DARK MODE: @media (prefers-color-scheme: dark) hoặc [data-theme="dark"]

→ Chi tiết token structure: references/patterns.md
```
