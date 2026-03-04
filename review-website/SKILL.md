---
name: Review Website — Comprehensive Website Audit
description: "Đánh giá website toàn diện 7 chiều: UX/UI, Performance, SEO, Security, Accessibility, Code Quality, Mobile. Tạo report chi tiết với điểm số và action items. Kích hoạt bằng /review-web [url hoặc project path]."
---

# /review-web [url or project path]

> **Website Inspector** — Đánh giá 360° như một senior consultant
> Mục tiêu: Report chuyên nghiệp + actionable improvements

---

## 7 CHIỀU ĐÁNH GIÁ

### 1. UX/UI DESIGN (25 điểm)
```
Đánh giá theo 5 tiêu chí:

VISUAL HIERARCHY (5đ):
[ ] Heading hierarchy rõ ràng (H1 → H2 → H3)?
[ ] CTA nổi bật (contrast, size, positioning)?
[ ] Whitespace đủ (không quá chật)?
[ ] Color scheme consistent?
[ ] Typography readable (font-size ≥ 16px body)?

NAVIGATION (5đ):
[ ] Menu rõ ràng, dễ tìm?
[ ] Breadcrumbs (cho sites > 3 levels)?
[ ] Search functionality (cho sites > 20 pages)?
[ ] Footer navigation đầy đủ?
[ ] 404 page helpful (gợi ý links)?

INTERACTION (5đ):
[ ] Loading states cho async operations?
[ ] Error messages rõ ràng, actionable?
[ ] Success feedback cho form submissions?
[ ] Hover/focus states cho interactive elements?
[ ] Smooth transitions (không janky)?

CONSISTENCY (5đ):
[ ] Design system nhất quán (colors, fonts, spacing)?
[ ] Button styles consistent?
[ ] Form field styles consistent?
[ ] Icon style consistent (outline/filled)?
[ ] Spacing system (4px/8px grid)?

CONTENT (5đ):
[ ] Copy rõ ràng, không jargon?
[ ] CTAs action-oriented ("Đăng ký ngay" > "Submit")?
[ ] Empty states informative?
[ ] Microcopy helpful (tooltips, placeholders)?
[ ] Error messages human-friendly?
```

### 2. PERFORMANCE (20 điểm)
```
Core Web Vitals (12đ):
[ ] LCP (Largest Contentful Paint) < 2.5s? (4đ)
[ ] INP (Interaction to Next Paint) < 200ms? (4đ)
[ ] CLS (Cumulative Layout Shift) < 0.1? (4đ)

Loading Optimization (8đ):
[ ] Images optimized (WebP/AVIF, lazy load)?
[ ] CSS/JS minified + bundled?
[ ] Fonts: preload, font-display: swap?
[ ] Critical CSS inlined?
[ ] Gzip/Brotli compression enabled?
[ ] CDN for static assets?
[ ] Caching headers đúng (Cache-Control, ETag)?
[ ] No render-blocking resources in above-fold?

Kiểm tra bằng:
  - Lighthouse (Chrome DevTools)
  - PageSpeed Insights API
  - WebPageTest
```

### 3. SEO (15 điểm)
```
On-Page (10đ):
[ ] Title tag unique, descriptive, ≤ 60 chars?
[ ] Meta description compelling, ≤ 155 chars?
[ ] Single H1 per page?
[ ] Heading hierarchy đúng (H1 → H2 → H3)?
[ ] Image alt text descriptive?
[ ] Internal linking structure hợp lý?
[ ] URL structure clean (/products/shoes, không /p?id=123)?
[ ] Canonical tags đúng?
[ ] Hreflang (nếu multilingual)?
[ ] Structured data (JSON-LD Schema)?

Technical (5đ):
[ ] Sitemap.xml tồn tại và cập nhật?
[ ] Robots.txt hợp lý (không block CSS/JS)?
[ ] SSL certificate valid?
[ ] Mobile-friendly (responsive)?
[ ] Page speed đạt (xem mục Performance)?
```

### 4. SECURITY (15 điểm)
```
Quick Security Check (không cần deep audit — dùng /security cho đó):
[ ] HTTPS everywhere?
[ ] Security headers (HSTS, CSP, X-Frame-Options)?
[ ] No mixed content warnings?
[ ] Forms có CSRF protection?
[ ] No exposed sensitive info (API keys, debug info)?
[ ] Cookie flags (httpOnly, Secure, SameSite)?
[ ] Admin panel protected (không /admin public)?
[ ] Dependencies không có critical CVEs?
```

### 5. ACCESSIBILITY — A11y (10 điểm)
```
WCAG 2.1 AA Checklist:
[ ] Color contrast ratio ≥ 4.5:1 (text)?
[ ] Color contrast ratio ≥ 3:1 (large text, UI)?
[ ] Không dùng color alone để convey info?
[ ] Keyboard navigation hoạt động (Tab order)?
[ ] Focus indicators visible?
[ ] ARIA labels cho interactive elements?
[ ] Alt text cho images (decorative = alt="")?
[ ] Form labels associated with inputs?
[ ] Skip navigation link?
[ ] Screen reader compatible (semantic HTML)?
```

### 6. CODE QUALITY (10 điểm)
```
HTML:
[ ] Semantic HTML5 (header, main, nav, footer)?
[ ] No deprecated elements (center, font)?
[ ] Valid HTML (W3C validator)?

CSS:
[ ] No inline styles (ngoại trừ dynamic)?
[ ] CSS custom properties cho theming?
[ ] No !important abuse?
[ ] Responsive (media queries hoặc container queries)?

JavaScript:
[ ] No console.log trong production?
[ ] Error handling cho async operations?
[ ] No memory leaks (event listeners cleaned up)?
[ ] Bundle size reasonable (< 300KB gzipped)?
```

### 7. MOBILE EXPERIENCE (5 điểm)
```
[ ] Responsive across 3 breakpoints (mobile/tablet/desktop)?
[ ] Touch targets ≥ 44x44px?
[ ] No horizontal scroll?
[ ] Font size ≥ 16px (no zoom on iOS input focus)?
[ ] Viewport meta tag correct?
```

---

## SCORING SYSTEM

```
Mỗi chiều có điểm tối đa:
  UX/UI:          25 điểm
  Performance:    20 điểm
  SEO:            15 điểm
  Security:       15 điểm
  Accessibility:  10 điểm
  Code Quality:   10 điểm
  Mobile:          5 điểm
  ─────────────────────────
  TOTAL:         100 điểm

Rating:
  90-100: ⭐⭐⭐⭐⭐ Excellent
  75-89:  ⭐⭐⭐⭐  Good
  60-74:  ⭐⭐⭐    Average — cần cải thiện
  40-59:  ⭐⭐      Below Average — nhiều vấn đề
  0-39:   ⭐        Poor — cần rebuild
```

---

## OUTPUT FORMAT

```
🌐 WEBSITE REVIEW — [URL/Project]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
📅 Date: YYYY-MM-DD

📊 OVERALL SCORE: [XX/100] ⭐⭐⭐⭐

  🎨 UX/UI:         [XX/25]
  ⚡ Performance:    [XX/20]
  🔍 SEO:           [XX/15]
  🔒 Security:      [XX/15]
  ♿ Accessibility:  [XX/10]
  💻 Code Quality:  [XX/10]
  📱 Mobile:        [XX/5]

🔴 TOP ISSUES (ưu tiên fix)
  1. [Issue] — Impact: [High] — Fix: [action]
  2. [Issue] — Impact: [High] — Fix: [action]
  3. [Issue] — Impact: [Med]  — Fix: [action]

💡 QUICK WINS (dễ fix, impact cao)
  1. [Action] — Effort: [5 min] — Impact: [+N điểm]
  2. [Action] — Effort: [15 min] — Impact: [+N điểm]

📋 FULL REPORT: [chi tiết từng mục]
```

---

## QUY TẮC

- Đánh giá KHÁCH QUAN — dựa trên tiêu chuẩn, không ý kiến cá nhân
- Mỗi issue phải có EVIDENCE (screenshot description, metric, code)
- Quick Wins phải THỰC SỰ quick (< 30 phút effort)
- Nếu review project code → /guard sẽ deep hơn
- Nếu cần security audit sâu → gợi ý /security
