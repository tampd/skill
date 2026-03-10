# Platform Design Rules — Quick Reference

> Adapted từ ehmo/platform-design-skills (300+ rules)
> Curated cho Web + Mobile development thực tế

## Web (Material Design 3 + Custom)

### Spacing & Layout
```
Base unit: 4px → tất cả spacing là bội số 4
  --space-1:   4px   (icon padding)
  --space-2:   8px   (inline elements)
  --space-3:  12px   (compact list items)
  --space-4:  16px   (standard padding)
  --space-6:  24px   (section spacing)
  --space-8:  32px   (card padding)
  --space-12: 48px   (section gaps)
  --space-16: 64px   (page sections)

Grid: 12-column cho desktop, 4-column cho mobile
Max content width: 1200px (readable), 1440px (full width)
```

### Typography
```
Type scale (modular 1.25):
  --font-size-xs:    0.75rem  (12px)  — labels, captions
  --font-size-sm:    0.875rem (14px)  — secondary text
  --font-size-base:  1rem     (16px)  — body text (min)
  --font-size-lg:    1.125rem (18px)  — large body
  --font-size-xl:    1.25rem  (20px)  — subheadings
  --font-size-2xl:   1.5rem   (24px)  — section titles
  --font-size-3xl:   1.875rem (30px)  — page titles
  --font-size-4xl:   2.25rem  (36px)  — hero text

Line height:
  Headings: 1.2 - 1.3
  Body text: 1.5 - 1.75
  
Paragraph width: 45-75 characters (optimal readability)
```

### Interactive Elements
```
Touch target: ≥44×44px (WCAG) — ≥48×48px (recommended)
Button heights: sm=32px, md=40px, lg=48px, xl=56px
Icon sizes: 16px (inline), 20px (button), 24px (navigation), 32px (feature)
Border radius: none=0, sm=4px, md=8px, lg=12px, xl=16px, full=9999px
Focus ring: 2px solid, offset 2px, high contrast color
```

### Color System
```
Primary:   Brand color + 10 shades (50-900)
Secondary: Complement + 10 shades
Neutral:   Gray for text/borders/backgrounds
Semantic:
  --color-success:  green tones (confirm, complete)
  --color-warning:  amber tones (caution, attention)
  --color-error:    red tones (danger, delete, fail)
  --color-info:     blue tones (information, help)

Dark mode: swap semantic tokens, NOT individual colors
Contrast: ≥4.5:1 text, ≥3:1 large text/UI elements
```

### Elevation & Shadows
```
Level 0: No shadow (flat, inline with surface)
Level 1: 0 1px 2px rgba(0,0,0,0.05)  — cards, dropdowns
Level 2: 0 2px 4px rgba(0,0,0,0.1)   — floating elements
Level 3: 0 4px 8px rgba(0,0,0,0.12)  — popovers, tooltips
Level 4: 0 8px 16px rgba(0,0,0,0.15) — modals, dialogs
Level 5: 0 16px 32px rgba(0,0,0,0.2) — full-screen overlays
```

### Animation
```
Duration:
  Instant:    50ms  (color change, opacity)
  Fast:      100ms  (button feedback, toggle)
  Normal:    200ms  (transitions, expand/collapse)
  Slow:      300ms  (page transitions, complex)
  Deliberate: 500ms (attention-grabbing)

Easing:
  Standard:   cubic-bezier(0.4, 0, 0.2, 1)  — most interactions
  Enter:      cubic-bezier(0, 0, 0.2, 1)     — elements appearing
  Exit:       cubic-bezier(0.4, 0, 1, 1)     — elements leaving
  Bounce:     cubic-bezier(0.34, 1.56, 0.64, 1) — playful

GPU-accelerated properties ONLY:
  ✅ transform, opacity, filter
  ❌ width, height, top, left, margin, padding (layout thrash)

@media (prefers-reduced-motion: reduce) {
  * { animation-duration: 0.01ms !important; transition-duration: 0.01ms !important; }
}
```

## Responsive Breakpoints
```
--bp-mobile:    320px   (small phone)
--bp-mobile-lg: 375px   (standard phone)
--bp-tablet:    768px   (tablet portrait)
--bp-desktop:  1024px   (tablet landscape / small desktop)
--bp-wide:     1280px   (standard desktop)
--bp-ultra:    1920px   (wide desktop)

Strategy: Mobile-first → @media (min-width: ...)
```

## Component Sizing Matrix
```
| Component | sm | md (default) | lg |
|-----------|-----|-------|-----|
| Button    | 32px h, 12px px | 40px h, 16px px | 48px h, 24px px |
| Input     | 32px h | 40px h | 48px h |
| Badge     | 20px h | 24px h | 28px h |
| Avatar    | 32px | 40px | 48px |
| Icon btn  | 32px | 40px | 48px |
```
