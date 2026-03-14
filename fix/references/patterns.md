# Fix Skill — Reference Patterns

> Load on-demand khi debug. Không load vào context mặc định.

## BUG CLASSIFICATION

```
CRITICAL (fix ngay, không commit khác):
  □ Data loss / corruption
  □ Security vulnerability
  □ Production down / service unavailable
  □ Authentication bypass

HIGH (fix trong sprint này):
  □ Feature không hoạt động đúng spec
  □ Performance degradation >20%
  □ UI broken trên major browser/device

MEDIUM (backlog):
  □ Edge case không handle
  □ Error message không rõ ràng
  □ Minor UX issue

LOW (nice-to-have):
  □ Typo trong UI
  □ Console warning không critical
```

## COMMON PATTERNS & QUICK DIAGNOSIS

```
React / Next.js:
  "Cannot read properties of undefined" → async data chưa load, check loading state
  "Hydration error" → server/client render khác nhau, check conditional rendering
  "Maximum update depth exceeded" → dependency array trong useEffect sai
  "Module not found" → import path sai, check tsconfig paths

Node.js / API:
  "CORS error" → missing/wrong CORS headers, check origin whitelist
  "JWT expired" → token refresh logic, check expiry handling
  "Cannot connect to DB" → connection pool exhausted, check max connections
  "Request timeout" → slow query / external API, check indexes + timeouts

N8N / Automation:
  "Execution failed" → check node input/output mapping
  "Rate limit exceeded" → add delay node, check retry logic
  "Webhook not received" → check URL, authentication, firewall

WordPress / REST API:
  "401 Unauthorized" → check nonce, check user capabilities
  "Cannot modify header information" → output buffer issue, early exit

PHP / Laravel:
  "Class not found" → composer dump-autoload, check namespace
  "SQLSTATE" → check migration, check DB connection
  "419 Page expired" → CSRF token missing, check @csrf
  "Too many redirects" → check middleware, check route groups
```

## ANTIGRAVITY BROWSER AGENT — UI Bug Fixing

```
Dùng cho: visual bugs, layout issues, UI state bugs

WORKFLOW:
  1. Mô tả bug cho Browser Agent
  2. Browser Agent: navigate → screenshot → identify issue
  3. Fix code
  4. Browser Agent: verify fix (before/after screenshots)
  5. Check: responsive (mobile, tablet, desktop)
  6. Check: dark mode nếu có
  7. Confirm với visual diff

Đặc biệt hữu ích cho:
  - CSS specificity issues
  - Z-index layering bugs
  - Animation/transition glitches
  - Form validation visual feedback
```
