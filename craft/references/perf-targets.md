# Performance Targets & Optimization

## Core Web Vitals Targets
| Metric | Target | Tool |
|---|---|---|
| LCP | < 2.5s | Lighthouse |
| CLS | < 0.1 | Lighthouse |
| INP | < 200ms | Web Vitals |
| FCP | < 1.8s | Lighthouse |
| Lighthouse Performance | ≥ 90 | `npx lighthouse` |
| First Load JS | < 150KB gzip | bundle-analyzer |

## Image Optimization
- [ ] WebP/AVIF format
- [ ] Responsive srcset (480w, 768w, 1200w)
- [ ] Explicit width + height (avoid CLS)
- [ ] LCP image: priority, NO lazy load
- [ ] Other images: lazy loading

## Bundle Optimization
- [ ] First Load JS < 150KB gzip
- [ ] Dynamic imports cho heavy components
- [ ] `import { specific }` thay `import *`
- [ ] Third-party scripts async/defer

## Caching Strategy
```
Static assets:  Cache-Control: public, max-age=31536000, immutable
HTML pages:     Cache-Control: public, s-maxage=3600, stale-while-revalidate=86400
API private:    Cache-Control: private, no-cache
```
