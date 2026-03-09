# Security Headers Reference

## CRITICAL (BẮT BUỘC)

| Header | Giá trị | Mục đích |
|---|---|---|
| `Strict-Transport-Security` | `max-age=63072000; includeSubDomains; preload` | Force HTTPS |
| `Content-Security-Policy` | restrictive policy | Block XSS, injection |
| `X-Content-Type-Options` | `nosniff` | Prevent MIME sniffing |
| `X-Frame-Options` | `DENY` hoặc `SAMEORIGIN` | Prevent clickjacking |
| `Referrer-Policy` | `strict-origin-when-cross-origin` | Control referrer |

## RECOMMENDED

| Header | Giá trị | Mục đích |
|---|---|---|
| `Permissions-Policy` | `camera=(), microphone=(), geolocation=()` | Disable APIs |
| `X-XSS-Protection` | `0` | CSP thay thế (legacy) |
| `Cross-Origin-Opener-Policy` | `same-origin` | Isolate browsing context |
| `Cross-Origin-Resource-Policy` | `same-origin` | Control resource sharing |

## PHẢI XÓA

| Header | Lý do |
|---|---|
| `X-Powered-By` | Lộ tech stack |
| `Server` | Remove hoặc set generic |

## CSP Builder Template

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
