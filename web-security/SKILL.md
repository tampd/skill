---
name: Web Security — Deep Security Audit & Hardening
description: "Audit bảo mật chuyên sâu: OWASP Top 10 2025, header analysis, dependency scanning, auth audit, API security, server hardening, compliance check. Tạo report + remediation plan. Kích hoạt bằng /security [target/scope]."
---

# /security [target or scope]

> **Security Expert** — CTO-level audit, không phải checklist hời hợt
> Sources: OWASP 2025, CWE Top 25, CVE databases, Trail of Bits patterns
> Mục tiêu: Tìm lỗ hổng THỰC SỰ, không chỉ checkbox

---

## 5 PHASES AUDIT

### Phase 1 — RECONNAISSANCE
```
1. Thu thập thông tin target:
   - Tech stack detection (headers, meta, source)
   - Server info (X-Powered-By, Server header)
   - Framework version (nếu lộ)
   - DNS records, subdomains (nếu có quyền)

2. Attack surface mapping:
   - Public endpoints (API, pages, forms)
   - Authentication mechanisms
   - File upload points
   - Third-party integrations
   - WebSocket connections
```

### Phase 2 — OWASP TOP 10 (2025) DEEP AUDIT
```
A01 — BROKEN ACCESS CONTROL
[ ] Mọi endpoint có auth middleware?
[ ] RBAC enforce ở SERVER, không chỉ UI hide?
[ ] IDOR check: user A truy cập data user B?
[ ] Path traversal: ../../etc/passwd trong file paths?
[ ] CORS: Access-Control-Allow-Origin không phải "*"?
[ ] JWT: verify signature + expiry + issuer?
[ ] Session: httpOnly + secure + SameSite cookies?

A02 — CRYPTOGRAPHIC FAILURES
[ ] HTTPS everywhere? (no mixed content)
[ ] TLS 1.2+ only? (no SSL 3.0, TLS 1.0, 1.1)
[ ] Passwords hashed với bcrypt/argon2 (cost ≥ 12)?
[ ] Sensitive data encrypted at rest?
[ ] API keys/secrets trong .env, KHÔNG trong code/git?
[ ] Backup files (.bak, .sql) không public?

A03 — INJECTION
[ ] SQL: Parameterized queries EVERYWHERE?
[ ] NoSQL: Input validation cho MongoDB queries?
[ ] XSS: Output encoding ({{ }} không {!! !!})?
[ ] Command Injection: no exec()/system() với user input?
[ ] LDAP/XML Injection: input sanitized?
[ ] Template Injection: user input không trong template strings?
[ ] Log Injection: sanitize trước khi log?

A04 — INSECURE DESIGN
[ ] Threat modeling đã làm?
[ ] Rate limiting trên auth endpoints?
[ ] Account lockout sau N failed attempts?
[ ] Password policy (min 8, complexity)?
[ ] Email verification cho signup?
[ ] CAPTCHA cho public forms?

A05 — SECURITY MISCONFIGURATION
[ ] Debug mode OFF trong production?
[ ] Default credentials đã thay đổi?
[ ] Directory listing disabled?
[ ] Stack traces không hiện cho user?
[ ] Unnecessary HTTP methods disabled (TRACE, OPTIONS)?
[ ] Admin panels không public?

A06 — VULNERABLE COMPONENTS
[ ] npm audit / composer audit chạy clean?
[ ] Dependencies cập nhật (không có known CVEs)?
[ ] Lock files (package-lock.json) committed?
[ ] Sub-dependencies cũng checked?
[ ] License compliance (no GPL trong commercial)?

A07 — AUTH FAILURES
[ ] Brute force protection (rate limit + lockout)?
[ ] Session timeout hợp lý (≤ 24h)?
[ ] Logout thực sự invalidate session?
[ ] Password reset flow secure (token + expiry)?
[ ] MFA available cho admin accounts?
[ ] OAuth state parameter validated?

A08 — DATA INTEGRITY FAILURES
[ ] CI/CD pipeline integrity (signed commits)?
[ ] Package integrity (checksums verified)?
[ ] Auto-update mechanisms signed?
[ ] Deserialization safe (no untrusted data)?

A09 — LOGGING & MONITORING
[ ] Authentication events logged?
[ ] Authorization failures logged?
[ ] Input validation failures logged?
[ ] Logs KHÔNG chứa passwords/tokens/PII?
[ ] Log tampering protected?
[ ] Alerting for suspicious patterns?

A10 — SSRF
[ ] Server-side requests validate URLs?
[ ] Internal IP ranges blocked (127.0.0.1, 10.x, 172.x, 192.168.x)?
[ ] DNS rebinding protection?
[ ] URL schema restricted (http/https only)?
```

### Phase 3 — SECURITY HEADERS ANALYSIS
```
Kiểm tra HTTP response headers:

CRITICAL (phải có):
[ ] Strict-Transport-Security: max-age=31536000; includeSubDomains
[ ] Content-Security-Policy: [restrictive policy]
[ ] X-Content-Type-Options: nosniff
[ ] X-Frame-Options: DENY (hoặc SAMEORIGIN)
[ ] Referrer-Policy: strict-origin-when-cross-origin

RECOMMENDED:
[ ] Permissions-Policy: camera=(), microphone=(), geolocation=()
[ ] X-XSS-Protection: 0 (CSP thay thế)
[ ] Cross-Origin-Opener-Policy: same-origin
[ ] Cross-Origin-Resource-Policy: same-origin

PHẢI XÓA:
[ ] X-Powered-By → remove (lộ tech stack)
[ ] Server → remove hoặc generic
```

### Phase 4 — API SECURITY (nếu có API)
```
[ ] Authentication: Bearer token / API key cho mọi endpoint?
[ ] Rate limiting: X-RateLimit headers?
[ ] Input validation: schema validation (Zod/Joi)?
[ ] Output filtering: không leak internal fields?
[ ] Pagination: limit max per page (tránh data dump)?
[ ] Error responses: generic messages, không stack traces?
[ ] CORS: specific origins, không wildcard?
[ ] Request size limit: body parser limits set?
[ ] File upload: type + size validation, scan malware?
[ ] GraphQL: depth limiting, query complexity analysis?
```

### Phase 5 — SERVER & INFRASTRUCTURE
```
[ ] Firewall rules: only necessary ports open?
[ ] SSH: key-based only, no password auth?
[ ] Database: not publicly accessible?
[ ] Backups: encrypted, tested restore?
[ ] Monitoring: uptime + security alerts?
[ ] Secrets management: vault/env, not in code?
[ ] Container security: non-root user, read-only fs?
[ ] DNS: DNSSEC enabled? CAA records?
```

---

## OUTPUT FORMAT

```
🔒 SECURITY AUDIT REPORT — [Target]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
📅 Date: YYYY-MM-DD
🎯 Scope: [Full/Partial — what was tested]

📊 SUMMARY
  🔴 Critical: [N] — Cần fix NGAY
  🟠 High:     [N] — Fix trong 24h
  🟡 Medium:   [N] — Fix trong 1 tuần
  🟢 Low:      [N] — Fix khi có thời gian
  ✅ Pass:     [N] — Đạt chuẩn

🔴 CRITICAL FINDINGS
  [ID] [Title]
  Impact: [mô tả]
  Evidence: [proof]
  Remediation: [cách fix cụ thể]

🟠 HIGH FINDINGS
  ...

📋 REMEDIATION PLAN (ưu tiên)
  1. [Critical fix 1] — ETA: [time]
  2. [Critical fix 2] — ETA: [time]
  3. [High fix 1] — ETA: [time]

📄 Output file: docs/security-audit.md
```

---

## QUY TẮC

- Audit phải EVIDENCE-BASED: chỉ ra lỗi CỤ THỂ, không chung chung
- Remediation phải ACTIONABLE: code snippet hoặc config cụ thể
- KHÔNG scan/attack hệ thống không có quyền
- KHÔNG store credentials/secrets trong report
- Ghi findings vào docs/security-audit.md
- Critical findings → tạo LESSONS.md entry ngay
