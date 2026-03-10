# Multi-Perspective Review

> 3-Perspective code review framework.
> Chạy tự động trong `/save` Stage 2 và on-demand qua `/review`.

## Perspective 1: 🔴 Security Reviewer

**Mindset**: "Nếu tôi là attacker, tôi sẽ exploit gì?"

### Checklist
- [ ] **Input Validation**: Tất cả user input đã validated + sanitized?
- [ ] **SQL Injection**: Dùng parameterized queries/ORM? Raw SQL đã escaped?
- [ ] **XSS**: Output đã HTML-encoded? dangerouslySetInnerHTML justified?
- [ ] **CSRF**: State-changing requests có CSRF token?
- [ ] **Auth**: Endpoints protected đúng? Không có bypass?
- [ ] **Authorization**: User chỉ access tài nguyên của mình?
- [ ] **Sensitive Data**: Passwords hashed? API keys in .env? No PII in logs?
- [ ] **File Upload**: File type validated? Size limited? No path traversal?
- [ ] **Rate Limiting**: Public endpoints có rate limit?
- [ ] **Dependencies**: Không có known CVEs? npm audit / pip audit?
- [ ] **Error Messages**: Không leak stack traces/SQL errors cho user?
- [ ] **CORS**: Configured cho allowed origins only?

### Severity
- **CRITICAL**: Direct exploit possible (SQL injection, auth bypass) → MUST FIX
- **HIGH**: Likely exploitable with effort (XSS, CSRF) → MUST FIX
- **MEDIUM**: Potential risk (missing rate limit, verbose errors) → SHOULD FIX

---

## Perspective 2: 🔵 Performance Reviewer

**Mindset**: "Code này sẽ behave thế nào với 10K concurrent users?"

### Checklist
- [ ] **N+1 Queries**: Eager loading cho relationships? No queries inside loops?
- [ ] **Missing Indexes**: Columns used in WHERE/JOIN/ORDER BY có index?
- [ ] **Unnecessary Data**: SELECT * hay chỉ fields cần? Pagination cho lists?
- [ ] **Caching**: Frequently-read, rarely-changed data đã cache?
- [ ] **Bundle Size** (frontend): Tree-shaking? Lazy loading? Code splitting?
- [ ] **Re-renders** (React): Memoization cần thiết? Stable references?
- [ ] **Memory Leaks**: Subscriptions/listeners cleanup? Unclosed connections?
- [ ] **Async Operations**: Long tasks offloaded to queue? Non-blocking?
- [ ] **Image Optimization**: WebP/AVIF? Lazy loading? Responsive sizes?
- [ ] **Font Loading**: preload + display=swap? Subset fonts?

### Severity
- **CRITICAL**: O(n²) or worse in hot path, unbounded memory → MUST FIX
- **HIGH**: N+1 queries, no pagination for large datasets → MUST FIX
- **MEDIUM**: Missing cache, unnecessary re-renders → SHOULD FIX

---

## Perspective 3: 🟢 UX/Accessibility Reviewer

**Mindset**: "Người dùng (bao gồm disabled users) sẽ trải nghiệm thế nào?"

### Checklist
- [ ] **Loading States**: Skeleton/spinner khi fetch data?
- [ ] **Error States**: User-friendly error messages? Retry option?
- [ ] **Empty States**: Meaningful message khi no data? CTA to add?
- [ ] **Keyboard Navigation**: Tab order logical? Focus visible? Enter activates?
- [ ] **Screen Reader**: ARIA labels cho interactive elements? Headings hierarchy?
- [ ] **Color Contrast**: Text ≥ 4.5:1? Large text ≥ 3:1? Icons ≥ 3:1?
- [ ] **Responsive**: Works at 320px, 768px, 1024px, 1280px+?
- [ ] **Touch Targets**: ≥ 44x44px on mobile?
- [ ] **Animations**: Respects prefers-reduced-motion?
- [ ] **Form UX**: Labels visible? Validation inline? Autofocus on first field?
- [ ] **Copy/Text**: Human-readable, no jargon for non-tech users?

### Severity
- **CRITICAL**: Completely inaccessible (no keyboard nav for core flow) → MUST FIX
- **HIGH**: WCAG AA violation (contrast, missing labels) → MUST FIX
- **MEDIUM**: Missing loading/error states → SHOULD FIX
- **LOW**: Minor UX improvements → CREATE TICKET

---

## Report Format

```
🔍 MULTI-PERSPECTIVE REVIEW — [scope]
━━━━━━━━━━━━━━━━━━━━━━

🔴 SECURITY: [N findings]
  • [CRITICAL] Missing input validation — api/users.ts:42
  • [HIGH] XSS via dangerouslySetInnerHTML — components/Bio.tsx:15

🔵 PERFORMANCE: [N findings]
  • [HIGH] N+1 query in user list — services/UserService.ts:88
  • [MEDIUM] Missing cache for config — utils/config.ts:12

🟢 UX/A11Y: [N findings]
  • [HIGH] Missing loading state — pages/Dashboard.tsx:30
  • [MEDIUM] No empty state — components/OrderList.tsx:45

━━━━━━━━━━━━━━━━━━━━━━
Summary: X CRITICAL | Y HIGH | Z MEDIUM
Action: Fix CRITICAL+HIGH before commit
```
