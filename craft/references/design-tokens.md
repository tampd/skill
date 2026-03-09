# Design Tokens Reference

## Primitive Tokens (tokens.css)
```css
:root {
  /* Colors */
  --color-blue-50: #eff6ff; --color-blue-600: #2563eb; --color-blue-700: #1d4ed8;
  --color-green-50: #f0fdf4; --color-green-600: #16a34a;
  --color-red-50: #fef2f2; --color-red-600: #dc2626;
  --color-gray-50: #f8fafc; --color-gray-100: #f1f5f9;
  --color-gray-600: #475569; --color-gray-900: #0f172a;

  /* Spacing */
  --space-1: 0.25rem; --space-2: 0.5rem; --space-3: 0.75rem;
  --space-4: 1rem; --space-6: 1.5rem; --space-8: 2rem;

  /* Typography */
  --font-sans: 'Inter', system-ui, sans-serif;
  --font-mono: 'JetBrains Mono', monospace;
  --text-xs: 0.75rem; --text-sm: 0.875rem; --text-base: 1rem;

  /* Borders & Shadows */
  --radius-sm: 4px; --radius-md: 8px; --radius-lg: 12px;
  --shadow-sm: 0 1px 2px rgb(0 0 0 / 0.05);
  --shadow-md: 0 4px 6px -1px rgb(0 0 0 / 0.1);
}
```

## Semantic Tokens (semantics.css)
```css
[data-theme="light"], :root {
  --bg-page: var(--color-gray-50);
  --bg-surface: #ffffff;
  --bg-elevated: #ffffff;
  --text-primary: var(--color-gray-900);
  --text-secondary: var(--color-gray-600);
  --color-brand: var(--color-blue-600);
}

[data-theme="dark"] {
  --bg-page: var(--color-gray-900);
  --bg-surface: #1e293b;
  --bg-elevated: #334155;
  --text-primary: var(--color-gray-50);
  --text-secondary: var(--color-gray-100);
  --color-brand: var(--color-blue-600);
}
```

## Animation System (animations.css)
```css
@media (prefers-reduced-motion: no-preference) {
  :root {
    --transition-fast: 150ms ease;
    --transition-base: 250ms ease;
    --transition-slow: 350ms ease;
  }
}
@media (prefers-reduced-motion: reduce) {
  :root {
    --transition-fast: 0ms;
    --transition-base: 0ms;
    --transition-slow: 0ms;
  }
}
```
