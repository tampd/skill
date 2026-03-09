---
name: secure
description: "Bảo mật web và deploy an toàn: OWASP Top 10 deep audit + security headers + pre-launch checklist + CI/CD pipeline + monitoring + rollback. Kích hoạt khi người dùng nói /security, /ship, 'bảo mật', 'OWASP', 'deploy', 'launch', 'CI/CD', 'monitor', 'rollback', 'pentest'."
metadata:
  author: tampd
  version: 7.0.0
  category: security
---

# Secure — Security Audit & Deploy Safety

> **v7.0** — Gộp web-security + ship
> Mục tiêu: Tìm lỗ hổng THẬT SỰ + deploy an toàn + monitor production.

---

## 4 MODES

### Mode 1 — `/security [target]` (OWASP Deep Audit)

```
PHASE 1 — RECONNAISSANCE:
  1. Tech stack detection (headers, meta, source)
  2. Attack surface mapping:
     - Public endpoints, auth mechanisms
     - File upload, 3rd-party integrations, WebSocket

PHASE 2 — OWASP TOP 10 (2025) DEEP AUDIT:
  A01 Broken Access Control  → RBAC server-side? IDOR? CORS? JWT verify?
  A02 Cryptographic Failures → HTTPS? TLS 1.2+? bcrypt/argon2? Secrets in env?
  A03 Injection              → Parameterized queries? XSS? Command injection?
  A04 Insecure Design        → Rate limiting? Account lockout? CAPTCHA?
  A05 Misconfiguration       → Debug OFF? Default creds? Directory listing?
  A06 Vulnerable Components  → npm audit clean? Lock files committed?
  A07 Auth Failures          → Session timeout? Logout invalidate? MFA?
  A08 Data Integrity         → CI/CD signed? Package checksums?
  A09 Logging & Monitoring   → Auth events logged? No PII in logs?
  A10 SSRF                   → URL validation? Internal IP blocked?
  > Chi tiết checklist đầy đủ: references/owasp-checklist.md

PHASE 3 — SECURITY HEADERS ANALYSIS:
  CRITICAL (phải có):
  [ ] Strict-Transport-Security: max-age=63072000; includeSubDomains; preload
  [ ] Content-Security-Policy: [restrictive — không * cho script-src]
  [ ] X-Content-Type-Options: nosniff
  [ ] X-Frame-Options: DENY (hoặc SAMEORIGIN)
  [ ] Referrer-Policy: strict-origin-when-cross-origin
  > Chi tiết headers: references/security-headers.md

PHASE 4 — API SECURITY (nếu có):
  [ ] Auth: Bearer/API key mọi endpoint?
  [ ] Rate limiting: X-RateLimit headers?
  [ ] Input validation: schema (Zod/Joi)?
  [ ] CORS: specific origins, không wildcard?

PHASE 5 — SERVER & INFRASTRUCTURE:
  [ ] Firewall: only necessary ports?
  [ ] SSH: key-based only?
  [ ] Database: not publicly accessible?
  [ ] Secrets management: vault/env?
```

**Output:**
```
🔒 SECURITY AUDIT REPORT — [Target]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
  🔴 Critical: [N]  🟠 High: [N]  🟡 Medium: [N]  🟢 Low: [N]  ✅ Pass: [N]

📋 REMEDIATION PLAN (ưu tiên)
  1. [Critical fix] — ETA: [time]
```

---

### Mode 2 — `/ship check` (Pre-launch Checklist)

```
✅ CODE QUALITY:
  [ ] TypeScript: tsc --noEmit → 0 errors
  [ ] Lint: 0 errors  |  Tests: all pass, coverage ≥ 80%
  [ ] Build: success  |  No console.log/debugger

✅ SECURITY HEADERS: (xem Phase 3 ở trên)

✅ PERFORMANCE:
  [ ] Lighthouse ≥ 90 (mobile)  |  LCP < 2.5s  |  CLS < 0.1  |  INP < 200ms
  [ ] Images: WebP, lazy (except LCP)  |  Bundle: < 150KB gzip

✅ SEO:
  [ ] title ≤ 60 chars  |  meta ≤ 155 chars  |  OG tags  |  sitemap.xml

✅ ACCESSIBILITY:
  [ ] Keyboard nav  |  ARIA labels  |  Contrast ≥ 4.5:1  |  alt text

✅ ENVIRONMENT:
  [ ] .env.production NOT in git  |  Production API keys
  [ ] CORS: production domain only  |  Rate limiting ON

✅ BKNS RELEASE (nếu BKNS repo):
  [ ] CHANGELOG.md + DEPLOYMENT.md + PROJECT-META.md updated
  [ ] Rollback plan documented  |  Migration tested (up + down)
```

**Output:**
```
🚀 SHIP REPORT — [Project] v[version]
  Code Quality: [✅|🛑]  Security: [✅|🛑]  Performance: [✅|🛑]
  SEO: [✅|🛑]  A11y: [✅|🛑]  Environment: [✅|🛑]
🚦 [✅ READY TO SHIP | 🛑 BLOCKED — fix N issues]
```

---

### Mode 3 — `/ship ci` (CI/CD Pipeline)

> Tạo GitHub Actions pipeline chuẩn: quality → build → lighthouse → deploy.
> Xem template đầy đủ: `references/ci-templates.md`

---

### Mode 4 — `/ship monitor` (Production Monitoring)

```
MONITORING CHECKLIST:
[ ] Uptime: ping /api/health mỗi 5 phút
[ ] Alert: email + Slack khi downtime
[ ] Error tracking: Sentry DSN configured
[ ] Core Web Vitals: Real User Monitoring

ROLLBACK DECISION:
  Error rate > 1%?           → Rollback ngay
  LCP > 4s sau deploy?       → Rollback + investigate
  Core functionality broken?  → Rollback ngay
  Minor UI bug?              → Hotfix (không rollback)
```

---

## QUY TẮC

- Audit phải EVIDENCE-BASED: lỗi CỤ THỂ + remediation ACTIONABLE
- KHÔNG launch mà chưa qua `/ship check`
- Security headers BẮT BUỘC — không có = blocked
- Lighthouse ≥ 90 — không đạt = blocked
- Critical findings → tạo LESSONS.md entry ngay
- KHÔNG scan/attack hệ thống không có quyền
