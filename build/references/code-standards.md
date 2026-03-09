# Code Standards Reference

## PHP/Laravel
```php
// ✅ DOs
- Service layer cho business logic
- FormRequest validation
- Eloquent: whereIn, chunkById
- bcmath cho monetary calculations
- Null-safe operators: $user?->profile?->avatar

// ❌ DON'Ts
- dd()/dump() trong production
- float cho tiền
- Hardcode config values
- eval() chưa sanitize
```

## TypeScript/JavaScript
```typescript
// ✅ DOs
- Explicit types, optional chaining
- Async/await, error boundaries
- Prisma parameterized queries

// ❌ DON'Ts
- any type, console.log production
- eval(), magic numbers
```

## CSS
```css
/* ✅ CSS variables: var(--bg-surface) */
/* ❌ Hardcode: rgba(30,41,59,0.8) */
```

## Commit Message Convention
```bash
# Format: type(scope): mô tả ≤ 72 chars
# Types: feat | fix | chore | docs | perf | sec | test

# Examples:
git commit -m "feat(auth): add JWT refresh token flow"
git commit -m "fix(payroll): correct overtime calculation rounding"
git commit -m "docs: update changelog v2.1.0"
```
