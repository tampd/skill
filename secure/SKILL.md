---
name: secure
description: Security audit and production deployment. Use for /security (OWASP audit), /ship (pre-deploy checklist), /harden (security hardening). Triggers on "security", "bảo mật", "deploy", "production", "OWASP", "ship", "hardening", "vulnerability", "lỗ hổng".
---

# Secure Skill — OWASP + Production Readiness + Deploy

## /security [target]
> OWASP Top 10 audit + security hardening

```
OWASP TOP 10 CHECKLIST:

A01 — BROKEN ACCESS CONTROL
  □ Authentication required cho mọi protected route
  □ Authorization check: user chỉ access data của mình
  □ Admin endpoints protected (không chỉ hide ở UI)
  □ IDOR prevention: không expose sequential IDs trực tiếp
  □ JWT/session validation đúng

A02 — CRYPTOGRAPHIC FAILURES
  □ Passwords hashed với bcrypt/argon2 (min cost 12)
  □ Sensitive data encrypted at rest (PII, tokens)
  □ HTTPS enforced (no HTTP fallback)
  □ TLS 1.2+ only
  □ Secrets trong env vars (không trong code)

A03 — INJECTION
  □ SQL: parameterized queries / ORM only (không string concat)
  □ NoSQL: input validation trước khi query
  □ XSS: output encoding, Content-Security-Policy header
  □ Command injection: không exec() với user input
  □ LDAP/XML injection check nếu applicable

A04 — INSECURE DESIGN
  □ Rate limiting: login, API, webhook endpoints
  □ Account lockout sau N failed attempts
  □ Security design review cho new features

A05 — SECURITY MISCONFIGURATION
  □ Debug mode OFF trong production
  □ Default credentials đã đổi
  □ Unnecessary features/ports disabled
  □ Error messages không expose stack traces
  □ Directory listing disabled

A06 — VULNERABLE COMPONENTS
  □ npm audit / pip-audit: không có critical CVEs
  □ Dependencies update strategy (Dependabot?)
  □ Docker base images: không dùng :latest

A07 — AUTH & SESSION FAILURES
  □ Session invalidated sau logout
  □ Session timeout reasonable (idle + absolute)
  □ MFA available cho admin accounts
  □ Password reset flow secure (token expires)
  □ Cookie flags: HttpOnly, Secure, SameSite=Strict

A08 — INTEGRITY FAILURES
  □ Subresource Integrity (SRI) cho CDN assets
  □ Package lock file committed
  □ CI/CD pipeline không nhận untrusted input

A09 — LOGGING FAILURES
  □ Auth events logged (login, logout, failed attempts)
  □ Access control failures logged
  □ Logs không chứa PII / sensitive data
  □ Log integrity (append-only, offsite backup)

A10 — SSRF
  □ Outbound requests: validate URL destination
  □ Whitelist external domains nếu có thể
  □ Internal network inaccessible từ user input
```

---

## /harden [service/app]
> Security headers + hardening configuration

```
HTTP SECURITY HEADERS (bắt buộc):
  Content-Security-Policy: default-src 'self'; script-src 'self' 'unsafe-inline'...
  Strict-Transport-Security: max-age=31536000; includeSubDomains; preload
  X-Frame-Options: DENY
  X-Content-Type-Options: nosniff
  Referrer-Policy: strict-origin-when-cross-origin
  Permissions-Policy: camera=(), microphone=(), geolocation=()

CORS CONFIGURATION:
  □ Whitelist specific origins (không dùng *)
  □ Credentials: only nếu thực sự cần
  □ Methods: chỉ những gì dùng

NGINX / REVERSE PROXY:
  □ Hide server version (server_tokens off)
  □ Rate limiting (limit_req_zone)
  □ Max body size phù hợp
  □ Request timeout reasonable

DOCKER / CONTAINER:
  □ Non-root user trong container
  □ Read-only filesystem nếu có thể
  □ No --privileged flag
  □ Secrets qua Docker secrets / env, không COPY vào image
```

---

## /ship [environment]
> Pre-deployment production readiness checklist

```
CODE QUALITY
  □ All tests passing (CI green)
  □ No TODO/FIXME critical
  □ No debug code (console.log, breakpoints)
  □ Dependencies: no unresolved peer deps
  □ Bundle size within budget

ENVIRONMENT & CONFIG
  □ Environment variables documented (.env.example updated)
  □ Production config khác với dev (debug off, strict mode on)
  □ Database: migrations ready + rollback tested
  □ Feature flags: correct state for prod

PERFORMANCE
  □ Lighthouse score ≥90 (Performance, Accessibility, Best Practices)
  □ Core Web Vitals trong budget (LCP, CLS, FID)
  □ Images optimized
  □ CDN configured cho static assets

MONITORING & OBSERVABILITY
  □ Error tracking configured (Sentry / similar)
  □ Uptime monitoring active
  □ Key metrics dashboarded (response time, error rate)
  □ Alerts set up cho critical thresholds
  □ Log aggregation working

SECURITY (quick final check)
  □ npm audit / pip-audit: no criticals
  □ Security headers present
  □ No secrets in code / git history
  □ API keys rotated nếu cần

ROLLBACK PLAN
  □ Previous version tagged in git
  □ Database rollback migration ready
  □ Rollback procedure documented + tested
  □ Who to notify if rollback triggered?

DEPLOYMENT
  □ Deploy to staging first → smoke test
  □ Zero-downtime deploy? (blue-green / canary)
  □ Health check endpoint responds
  □ Post-deploy smoke test list ready

SIGN-OFF: Code ✓  Security ✓  Performance ✓  Monitoring ✓  Rollback ✓
```
