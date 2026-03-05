---
name: Ship — Pre-launch + CI/CD + Monitoring (v6.0)
description: "Pre-launch checklist toàn diện + CI/CD pipeline + Monitoring + Rollback. Chuẩn bị deploy production với security headers, performance budget, SEO meta, và environment config. Kích hoạt bằng /ship [scope]."
---

# /ship [scope: check | ci | monitor | rollback]

> **Launch Engineer v6.0** — Tất cả cần thiết để go-live an toàn
> Mục tiêu: KHÔNG launch mà chưa qua checklist. KHÔNG deploy mà không monitor.

---

## 4 MODES

### Mode 1 — `/ship check` (Pre-launch Checklist)

#### ✅ CODE QUALITY
```
[ ] TypeScript: npx tsc --noEmit → 0 errors
[ ] Lint: npm run lint → 0 errors
[ ] Tests: npm test → all pass, coverage ≥ 80%
[ ] Build: npm run build → success, no warnings
[ ] No console.log/debugger trong production code
[ ] Error boundaries cho mọi route (error.tsx + loading.tsx)
```

#### ✅ SECURITY HEADERS (BẮT BUỘC)
```
[ ] Strict-Transport-Security: max-age=63072000; includeSubDomains; preload
[ ] Content-Security-Policy: configured (không * cho script-src)
[ ] X-Content-Type-Options: nosniff
[ ] X-Frame-Options: DENY (hoặc SAMEORIGIN)
[ ] Referrer-Policy: strict-origin-when-cross-origin
[ ] Permissions-Policy: camera=(), microphone=(), geolocation=()
[ ] Server header hidden (không lộ tech stack)
```

CSP Builder Template:
```javascript
const cspDirectives = {
  'default-src': ["'self'"],
  'script-src': ["'self'", "'unsafe-inline'"],
  'style-src': ["'self'", "'unsafe-inline'"],
  'img-src': ["'self'", "data:", "blob:", "https:"],
  'font-src': ["'self'", "https://fonts.gstatic.com"],
  'connect-src': ["'self'"],
  'object-src': ["'none'"],
  'frame-ancestors': ["'none'"],
  'base-uri': ["'self'"],
  'upgrade-insecure-requests': [],
}
// Verify: https://csp-evaluator.withgoogle.com/
```

#### ✅ PERFORMANCE
```
[ ] Lighthouse Performance ≥ 90 (mobile)
[ ] Lighthouse Accessibility ≥ 90
[ ] LCP < 2.5s, CLS < 0.1, INP < 200ms
[ ] Images: WebP, explicit dimensions, lazy (except LCP)
[ ] Fonts: preloaded, font-display: swap
[ ] Bundle: First Load JS < 150KB gzip
```

#### ✅ SEO
```
[ ] title tag: unique, ≤ 60 chars per page
[ ] meta description: ≤ 155 chars
[ ] Open Graph: og:title, og:description, og:image (1200x630)
[ ] robots.txt + sitemap.xml present
[ ] Canonical URLs set
[ ] Structured data (Schema.org) cho relevant content
[ ] http → https redirect + www → non-www
```

#### ✅ ACCESSIBILITY
```
[ ] Keyboard navigation: Tab qua tất cả
[ ] Skip to main content link
[ ] ARIA labels cho icons, form fields
[ ] Color contrast ≥ 4.5:1 (axe DevTools)
[ ] Images có alt text
[ ] lang attribute trên <html>
```

#### ✅ ENVIRONMENT
```
[ ] .env.production KHÔNG commit vào git
[ ] Mọi required env vars có trong deployment
[ ] Database: production URL, connection pool
[ ] API keys: production keys (khác development)
[ ] CORS: chỉ allow production domain
[ ] Rate limiting bật
[ ] Error tracking (Sentry) setup
[ ] Analytics setup
```

---

### Mode 2 — `/ship ci` (CI/CD Pipeline)

GitHub Actions pipeline template:

```yaml
# .github/workflows/ci.yml
name: CI/CD
on:
  push:
    branches: [main, develop]
  pull_request:
    branches: [main]

jobs:
  quality:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with: { node-version: '20', cache: 'npm' }
      - run: npm ci
      - run: npx tsc --noEmit          # TypeScript
      - run: npm run lint               # Lint
      - run: npm test -- --coverage --ci # Tests

  build:
    needs: quality
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with: { node-version: '20', cache: 'npm' }
      - run: npm ci && npm run build

  lighthouse:
    needs: build
    if: github.ref == 'refs/heads/main'
    runs-on: ubuntu-latest
    steps:
      - uses: treosh/lighthouse-ci-action@v11
        with:
          budgetPath: './lighthouse-budget.json'
          uploadArtifacts: true

  deploy:
    needs: [build, lighthouse]
    if: github.ref == 'refs/heads/main'
    environment: production
    runs-on: ubuntu-latest
    steps:
      - name: Deploy
        run: echo "Deploy to production"
```

Lighthouse Budget:
```json
[{
  "timings": [
    { "metric": "largest-contentful-paint", "budget": 2500 },
    { "metric": "first-contentful-paint", "budget": 1500 }
  ],
  "categories": [
    { "id": "performance", "minScore": 0.9 },
    { "id": "accessibility", "minScore": 0.9 }
  ]
}]
```

---

### Mode 3 — `/ship monitor` (Production Monitoring)

```typescript
// Error Tracking — Sentry
import * as Sentry from '@sentry/nextjs'
Sentry.init({
  dsn: process.env.NEXT_PUBLIC_SENTRY_DSN,
  tracesSampleRate: 0.1, // 10% in production
})

// Health Check Endpoint
// GET /api/health → { status: 'ok', version, checks: { database } }
```

```
MONITORING CHECKLIST:
[ ] Uptime monitor: ping /api/health mỗi 5 phút
[ ] Alert: email + Slack khi downtime
[ ] Error tracking: Sentry DSN configured
[ ] Analytics: Plausible (privacy) hoặc GA4
[ ] Core Web Vitals: Real User Monitoring bật
```

---

### Mode 4 — `/ship rollback` (Emergency)

```bash
# Vercel instant rollback (< 30s)
vercel rollback [deployment-url]

# Git revert
git revert HEAD --no-edit && git push origin main
```

```
ROLLBACK DECISION:
  Error rate tăng > 1%?         → Rollback ngay
  LCP > 4s sau deploy?          → Rollback + investigate
  Core functionality broken?    → Rollback ngay
  Minor UI bug?                 → Hotfix (không rollback)
```

---

## OUTPUT FORMAT

```
🚀 SHIP REPORT — [Project] v[version]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
📋 PRE-LAUNCH CHECKLIST
  Code Quality:    [✅ PASS | 🛑 FAIL]
  Security:        [✅ PASS | 🛑 FAIL]
  Performance:     [✅ PASS | 🛑 FAIL]
  SEO:             [✅ PASS | 🛑 FAIL]
  Accessibility:   [✅ PASS | 🛑 FAIL]
  Environment:     [✅ PASS | 🛑 FAIL]

🚦 DEPLOY DECISION
  [✅ READY TO SHIP | 🛑 BLOCKED — fix N issues]

⚠️ BLOCKERS (fix trước deploy)
  1. [Issue] — [How to fix]
```

---

## QUY TẮC

- KHÔNG launch mà chưa qua `/ship check`
- KHÔNG deploy mà không setup monitoring
- Security headers BẮT BUỘC — không có = blocked
- Lighthouse ≥ 90 — không đạt = blocked
- Rollback plan PHẢI có trước khi deploy
