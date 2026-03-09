# OWASP Top 10 (2025) — Full Checklist

## A01 — BROKEN ACCESS CONTROL
- [ ] Mọi endpoint có auth middleware?
- [ ] RBAC enforce ở SERVER, không chỉ UI hide?
- [ ] IDOR check: user A truy cập data user B?
- [ ] Path traversal: ../../etc/passwd trong file paths?
- [ ] CORS: Access-Control-Allow-Origin không phải "*"?
- [ ] JWT: verify signature + expiry + issuer?
- [ ] Session: httpOnly + secure + SameSite cookies?

## A02 — CRYPTOGRAPHIC FAILURES
- [ ] HTTPS everywhere? (no mixed content)
- [ ] TLS 1.2+ only? (no SSL 3.0, TLS 1.0, 1.1)
- [ ] Passwords hashed với bcrypt/argon2 (cost ≥ 12)?
- [ ] Sensitive data encrypted at rest?
- [ ] API keys/secrets trong .env, KHÔNG trong code/git?
- [ ] Backup files (.bak, .sql) không public?

## A03 — INJECTION
- [ ] SQL: Parameterized queries EVERYWHERE?
- [ ] NoSQL: Input validation cho MongoDB queries?
- [ ] XSS: Output encoding ({{ }} không {!! !!})?
- [ ] Command Injection: no exec()/system() với user input?
- [ ] LDAP/XML Injection: input sanitized?
- [ ] Template Injection: user input không trong template strings?
- [ ] Log Injection: sanitize trước khi log?

## A04 — INSECURE DESIGN
- [ ] Threat modeling đã làm?
- [ ] Rate limiting trên auth endpoints?
- [ ] Account lockout sau N failed attempts?
- [ ] Password policy (min 8, complexity)?
- [ ] Email verification cho signup?
- [ ] CAPTCHA cho public forms?

## A05 — SECURITY MISCONFIGURATION
- [ ] Debug mode OFF trong production?
- [ ] Default credentials đã thay đổi?
- [ ] Directory listing disabled?
- [ ] Stack traces không hiện cho user?
- [ ] Unnecessary HTTP methods disabled (TRACE, OPTIONS)?
- [ ] Admin panels không public?

## A06 — VULNERABLE COMPONENTS
- [ ] npm audit / composer audit chạy clean?
- [ ] Dependencies cập nhật (không có known CVEs)?
- [ ] Lock files committed?
- [ ] Sub-dependencies cũng checked?
- [ ] License compliance (no GPL trong commercial)?

## A07 — AUTH FAILURES
- [ ] Brute force protection (rate limit + lockout)?
- [ ] Session timeout ≤ 24h?
- [ ] Logout invalidate session?
- [ ] Password reset: token + expiry?
- [ ] MFA available cho admin?
- [ ] OAuth state parameter validated?

## A08 — DATA INTEGRITY FAILURES
- [ ] CI/CD pipeline integrity (signed commits)?
- [ ] Package integrity (checksums verified)?
- [ ] Deserialization safe (no untrusted data)?

## A09 — LOGGING & MONITORING
- [ ] Authentication events logged?
- [ ] Authorization failures logged?
- [ ] Logs KHÔNG chứa passwords/tokens/PII?
- [ ] Log tampering protected?
- [ ] Alerting for suspicious patterns?

## A10 — SSRF
- [ ] Server-side requests validate URLs?
- [ ] Internal IP ranges blocked (127.0.0.1, 10.x, 172.x, 192.168.x)?
- [ ] DNS rebinding protection?
- [ ] URL schema restricted (http/https only)?
