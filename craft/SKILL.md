---
name: craft
description: UI/UX implementation and design system. Use for /craft (build UI components), /audit (quality audit), /tokens (design system), /e2e (browser visual testing). Triggers on "UI", "component", "design", "giao diện", "responsive", "WCAG", "accessibility", "craft", "audit giao diện".
---

# Craft Skill — UI Engineering + Design Tokens + WCAG + Browser Testing

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
  □ Animation/transition: nếu có, dùng prefers-reduced-motion check
  □ Dark mode: nếu project hỗ trợ

PHASE 3 — IMPLEMENT
  □ Semantic HTML (đúng element cho đúng role)
  □ Design tokens ONLY (không hardcode bất kỳ giá trị nào)
  □ Keyboard navigation: Tab order, focus visible, Enter/Space/Arrow keys
  □ ARIA attributes: role, aria-label, aria-describedby, aria-live
  □ Error states: accessible error messages
  □ Loading states: skeleton / spinner với aria-busy

PHASE 4 — WCAG CHECKLIST (bắt buộc)
  □ Color contrast ≥4.5:1 (text), ≥3:1 (large text/UI)
  □ Không dùng màu là cách DUY NHẤT truyền thông tin
  □ Focus indicator rõ ràng (không remove outline mà không thay thế)
  □ Touch target ≥44×44px (mobile)
  □ Alt text cho mọi ảnh có nghĩa
  □ Heading hierarchy đúng (h1→h2→h3, không skip)
  □ Form labels kết nối với input (for/id hoặc aria-labelledby)

PHASE 5 — PERFORMANCE CHECK
  □ Images: WebP/AVIF, lazy loading, srcset
  □ Icons: SVG inline (không dùng icon font nếu có thể)
  □ Animation: GPU-accelerated (transform, opacity — không layout thrash)
  □ Fonts: font-display: swap, preload critical fonts
  □ Bundle impact: import cụ thể, không import toàn bộ library

PHASE 6 — BROWSER AGENT VERIFY (Antigravity)
  □ Spawn Browser Sub-Agent với task: "Verify component renders correctly"
  □ Screenshots: desktop, tablet (768px), mobile (375px)
  □ Dark mode screenshot nếu applicable
  □ Keyboard navigation test (Tab through all interactive elements)
  □ Hover/focus states visual check
  □ Fix issues discovered → re-verify
```

---

## /audit [scope]
> Multi-dimensional quality audit cho UI

```
AUDIT DIMENSIONS:

VISUAL CONSISTENCY 🎨
  □ Spacing theo grid system? (4px / 8px base)
  □ Typography scale nhất quán?
  □ Color palette chỉ dùng design tokens?
  □ Border radius nhất quán?
  □ Shadow/elevation consistent?

ACCESSIBILITY ♿
  □ Run axe-core / Lighthouse accessibility
  □ Screen reader test (NVDA/VoiceOver mentally)
  □ Color contrast ratio check
  □ Focus management đúng không?
  □ Error messages accessible?

PERFORMANCE ⚡
  □ Lighthouse score (target: Performance ≥90)
  □ LCP, CLS, FID trong budget?
  □ Unused CSS/JS?
  □ Image optimization?
  □ Third-party scripts impact?

RESPONSIVE 📱
  □ 320px (small mobile) → không bị overflow
  □ 768px (tablet) → layout đúng
  □ 1280px (desktop) → full layout
  □ 1920px (wide) → không bị stretched

OUTPUT: Scored report với CRITICAL / WARNING / INFO
```

---

## /tokens [scope]
> Tạo hoặc mở rộng design token system

```
TOKEN STRUCTURE (CSS Custom Properties):
  --color-primary-50 → --color-primary-900  (scale)
  --color-semantic-bg-default
  --color-semantic-text-primary
  --color-semantic-border-focus
  
  --space-1 (4px) → --space-16 (64px)
  --font-size-xs → --font-size-4xl
  --font-weight-regular, --font-weight-medium, --font-weight-bold
  --radius-sm → --radius-full
  --shadow-sm → --shadow-2xl
  --duration-fast (100ms), --duration-normal (200ms), --duration-slow (300ms)

DARK MODE:
  @media (prefers-color-scheme: dark) { ... }
  hoặc [data-theme="dark"] { ... }
  Semantic tokens tự động flip — không hardcode dark values trong components
```

---

## COMPONENT DOCUMENTATION TEMPLATE
> Dùng sau khi build component để /handoff chuyên nghiệp

```markdown
## ComponentName

**Purpose:** [1 câu mô tả purpose]

**Props:**
| Prop | Type | Default | Required | Description |
|------|------|---------|----------|-------------|
| ... | ... | ... | ... | ... |

**Variants:** default | primary | secondary | destructive

**Usage:**
```tsx
<ComponentName variant="primary" size="md" />
```

**Accessibility:** [keyboard shortcuts, ARIA notes]
**Known Limitations:** [edge cases, browser compat]
```
