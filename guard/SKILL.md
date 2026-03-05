---
name: Guard — Quality Assurance Workflow
description: "Quality gate toàn diện: viết tests + security audit + performance check. Thay thế e2e-testing-patterns + security-auditor + performance-engineer. Kích hoạt bằng /guard [scope]."
---

# /guard [scope]

> **3-in-1** — Gộp e2e-testing + security-auditor + performance-engineer
> Mục tiêu: Code an toàn, nhanh, có test coverage

---

## 3 MODES

### Mode 1 — `/guard test [module]`

> ⚠️ **TDD Rule 6**: Tests phải viết TRƯỚC code nếu chưa tồn tại (RED→GREEN→REFACTOR).
> Nếu module đã có code nhưng chưa có test → viết test trước, verify test FAIL vì behavior chưa đúng, rồi refactor.

Viết tests cho module chỉ định:

```
1. Identify testable units:
   - Service methods với business logic
   - Controller endpoints
   - Edge cases từ LESSONS.md

2. Write test specs:
   Unit tests (*.test.php / *.spec.ts):
   - Happy path: input hợp lệ → output đúng
   - Edge cases: null, empty, zero, negative
   - Error cases: invalid input → proper error

   Integration tests:
   - API endpoint → response format + status code
   - DB operation → data consistency
   - Auth/RBAC → proper 401/403

3. Run tests:
   PHP:   php artisan test --filter [TestName]
   Node:  npm test -- --testPathPattern=[module]

4. Report coverage:
   ✅ [N/M] test cases passing
   ❌ [failures with details]
```

### Mode 2 — `/guard security`

OWASP Top 10 + Supply Chain audit:

```
OWASP Checklist:
[ ] SQL Injection: parameterized queries? Escape wildcards?
[ ] XSS: {{ }} in Blade? React auto-escape? No {!! $userInput !!}?
[ ] CSRF: @csrf in forms? SameSite cookies?
[ ] Auth: middleware auth/role on ALL routes?
[ ] Secrets: no hardcoded keys? .env in .gitignore?
[ ] File upload: MIME type validation (not just extension)?
[ ] Rate limiting: throttle on auth + API routes?
[ ] Headers: HSTS, CSP, Referrer-Policy?
[ ] Password: min 8 chars + mixed case + numbers?
[ ] Audit: sensitive actions logged?

Supply Chain Security (🆕 v3):
[ ] npm audit / composer audit → 0 critical?
[ ] Lock files committed (package-lock.json / composer.lock)?
[ ] No typosquatting packages (verify package names)?
[ ] Sub-dependencies checked (npm ls --all)?
[ ] GitHub Dependabot / Snyk enabled?
[ ] No postinstall scripts từ unknown packages?
[ ] License compliance (no GPL in commercial)?
```

### Mode 3 — `/guard performance`

Performance audit:

```
Checklist:
[ ] N+1 queries: with() eager loading?
[ ] Batch pre-fetch: whereIn() thay vì query trong loop?
[ ] Missing indexes: WHERE/JOIN columns indexed?
[ ] Heavy computation: queue cho tác vụ > 3 giây?
[ ] Cache: frequently-read data cached? Cache invalidation?
[ ] Pagination: large datasets paginated?
[ ] Lazy loading: images/components lazy loaded?
```

---

## OUTPUT FORMAT

```
🛡️ GUARD REPORT — [module/scope]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
🧪 TESTS:      [N pass / M total | ❌ N failures]
🔒 SECURITY:   [✅ Clean | ⚠️ N warnings | 🛑 N critical]
⚡ PERFORMANCE: [✅ Clean | ⚠️ N warnings]

[Chi tiết issues nếu có]
```
