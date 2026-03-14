# Craft Skill — Reference Patterns

> Load on-demand khi build UI. Không load vào context mặc định.

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
\`\`\`tsx
<ComponentName variant="primary" size="md" />
\`\`\`

**Accessibility:** [keyboard shortcuts, ARIA notes]
**Known Limitations:** [edge cases, browser compat]
```

## TOKEN STRUCTURE (CSS Custom Properties)

```
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
```

## DARK MODE PATTERNS

```
@media (prefers-color-scheme: dark) { ... }
hoặc [data-theme="dark"] { ... }
Semantic tokens tự động flip — không hardcode dark values trong components
```

## AUDIT DIMENSIONS DETAIL

```
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
```
