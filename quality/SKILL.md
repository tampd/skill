---
name: Quality — Testing + A11y + Performance Gate (v6.0)
description: "Quality gate toàn diện: TDD tests + WCAG axe-core audit + Core Web Vitals + Lighthouse CI + Security check. Gộp guard + perf + review-website. 4 modes: test, a11y, perf, all. Kích hoạt bằng /quality [scope]."
---

# /quality [scope]

> **4-in-1 Quality Gate v6.0** — Tests + A11y + Performance + Security
> Gộp: `/guard` + `/perf` + `/review-website`
> Mục tiêu: Code an toàn, accessible, performant, đạt chuẩn ship

---

## 4 MODES

### Mode 1 — `/quality test [module]`

> ⚠️ **TDD Rule 6**: Tests phải viết TRƯỚC code (RED→GREEN→REFACTOR).

```
1. Identify testable units:
   - Service methods với business logic
   - Controller endpoints
   - Hooks với side effects
   - Edge cases từ LESSONS.md

2. Test pyramid:
   Unit (70%):    Service/util functions, isolated
   Integration (20%): API endpoints, DB operations
   E2E (10%):    Critical user journeys

3. Write test specs:
   Unit tests (*.test.ts / *.spec.ts):
   - Happy path: input hợp lệ → output đúng
   - Edge cases: null, empty, zero, negative, max length
   - Error cases: invalid input → proper error thrown

   API tests:
   - 200: valid request → correct response
   - 400/422: invalid input → error details
   - 401/403: auth failures
   - 404: not found

   Component tests (React Testing Library):
   - Render: getByRole (semantic queries)
   - Interaction: userEvent.click/type
   - Accessibility: getByRole, getByLabelText
   - Async: waitFor, findBy

4. Run:
   PHP:   php artisan test --filter [TestName]
   Node:  npm test -- --coverage --testPathPattern=[module]
```

```typescript
// ✅ Component test semantic queries (best practice)
describe('Button', () => {
  it('renders with accessible label', () => {
    render(<Button>Submit form</Button>)
    expect(screen.getByRole('button', { name: /submit/i })).toBeInTheDocument()
  })

  it('shows loading state accessibly', () => {
    render(<Button isLoading>Submit</Button>)
    expect(screen.getByRole('button')).toHaveAttribute('aria-busy', 'true')
  })
})
```

---

### Mode 2 — `/quality a11y [page/component]`

> WCAG 2.2 AA — BẮT BUỘC cho mọi user-facing page.

```bash
# Automated axe-core test
npm install --save-dev jest-axe @axe-core/playwright
```

```typescript
// ✅ Automated axe test
import { axe, toHaveNoViolations } from 'jest-axe'
expect.extend(toHaveNoViolations)

it('has no axe violations', async () => {
  const { container } = render(<HomePage />)
  expect(await axe(container)).toHaveNoViolations()
})
```

```
KEYBOARD NAVIGATION:
[ ] Tab qua tất cả interactive elements
[ ] Shift+Tab ngược lại
[ ] Enter/Space kích hoạt buttons
[ ] Escape đóng modals/dropdowns
[ ] Arrow keys navigate menu/listbox
[ ] Modal: focus trap + return focus on close

SCREEN READER:
[ ] Headings hierarchy: H1 > H2 > H3 (không skip)
[ ] Images có alt text meaningful
[ ] Icon buttons có aria-label
[ ] Form errors được announce (aria-describedby)
[ ] Dynamic content: role="status" / role="alert"

CONTRAST (Chrome DevTools):
[ ] Normal text: ≥ 4.5:1
[ ] Large text (≥18px bold / ≥24px): ≥ 3:1
[ ] UI components: ≥ 3:1
[ ] Focus indicator: ≥ 3:1
[ ] Dark mode: verify lại tất cả
```

---

### Mode 3 — `/quality perf [target: page|bundle|images|all]`

> **RULE**: KHÔNG optimize premature. Profile trước, optimize sau.

#### Performance Targets

| Metric | Target | Công cụ |
|---|---|---|
| **LCP** | < 2.5s | Lighthouse |
| **CLS** | < 0.1 | Lighthouse |
| **INP** | < 200ms | Web Vitals |
| **FCP** | < 1.8s | Lighthouse |
| **Lighthouse Performance** | ≥ 90 | `npx lighthouse` |
| **Lighthouse Accessibility** | ≥ 90 | Lighthouse |
| **First Load JS** | < 150KB gzip | bundle-analyzer |

#### Audit Steps

```bash
# Lighthouse CLI
npx lighthouse https://yoursite.com \
  --output=json --form-factor=mobile \
  --only-categories=performance,accessibility,best-practices,seo
```

```
IMAGE OPTIMIZATION:
[ ] WebP/AVIF format?
[ ] Responsive srcset (480w, 768w, 1200w)?
[ ] Explicit width + height (tránh CLS)?
[ ] LCP image: priority={true}, KHÔNG lazy load?
[ ] Other images: lazy loading?

BUNDLE OPTIMIZATION:
[ ] First Load JS < 150KB gzip?
[ ] Dynamic imports cho heavy components?
[ ] import { specific } thay import *?
[ ] No duplicate deps?
[ ] Third-party scripts async/defer?

RENDER OPTIMIZATION:
[ ] LCP image preloaded?
[ ] Fonts: preload + font-display: swap?
[ ] No render-blocking resources?
[ ] SSR/SSG cho above-fold content?

CACHING:
  Static assets: Cache-Control: public, max-age=31536000, immutable
  HTML pages: Cache-Control: public, s-maxage=3600, stale-while-revalidate=86400
  API private: Cache-Control: private, no-cache
```

---

### Mode 4 — `/quality all [url]` (Full Website Audit)

Đánh giá toàn diện 7 chiều (100 điểm):

```
Scoring:
  🎨 UX/UI:          25 điểm
  ⚡ Performance:     20 điểm
  🔍 SEO:            15 điểm
  🔒 Security:       15 điểm
  ♿ Accessibility:   10 điểm
  💻 Code Quality:   10 điểm
  📱 Mobile:          5 điểm
  ──────────────────────────
  TOTAL:            100 điểm

Rating: 90+ ⭐⭐⭐⭐⭐ | 75-89 ⭐⭐⭐⭐ | 60-74 ⭐⭐⭐ | <60 ⭐⭐

SECURITY QUICK CHECK (cho /quality, deep audit → /security):
[ ] HTTPS + HSTS + CSP headers?
[ ] No mixed content? Cookie flags set?
[ ] npm audit clean? No exposed secrets?

SEO CHECK:
[ ] title unique ≤ 60 chars? meta description ≤ 155?
[ ] Sitemap.xml + robots.txt present?
[ ] Structured data (Schema.org)?
[ ] Canonical URLs + Open Graph tags?

MOBILE CHECK:
[ ] Responsive 3 breakpoints?
[ ] Touch targets ≥ 44x44px?
[ ] No horizontal scroll?
[ ] Font size ≥ 16px?
```

---

## OUTPUT FORMAT

```
🛡️ QUALITY REPORT — [module/scope]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
🧪 TESTS:        [N/M pass | Coverage: X%]
♿ ACCESSIBILITY: [✅ 0 violations | ⚠️ N warnings | 🛑 N critical]
⚡ PERFORMANCE:  [Lighthouse: P:XX A:XX BP:XX SEO:XX]
🔒 SECURITY:     [✅ Clean | ⚠️ N warnings]

🔴 CRITICAL — Fix before merge:
  1. [Type]: [Issue] → [Location] → [Fix]

🟡 WARNINGS — Fix this sprint:
  1. [Type]: [Issue] → [Fix]

✅ PASSING: [N checks]
```

---

## QUY TẮC

- Evidence-based: mỗi issue phải có proof (metric, screenshot, code)
- KHÔNG optimize premature — profile trước
- Quick Wins phải thực sự quick (< 30 phút effort)
- axe DevTools: target 0 violations
- Lighthouse mobile ≥ 90 trước khi ship
- Cần security audit sâu → gợi ý `/security`
