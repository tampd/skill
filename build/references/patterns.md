# Build Skill — Reference Patterns

> Load on-demand khi cần. Không load vào context mặc định.

## TDD HARD GATE — Không được bypass

```
❌ KHÔNG được viết implementation trước test
❌ KHÔNG được commit khi coverage <80%
❌ KHÔNG được skip verification loop
❌ KHÔNG được bỏ qua cleanup pass
✅ Exceptions phải được ghi rõ lý do vào LESSONS.md
```

## CHECKLIST TRƯỚC COMMIT

```
[ ] Đã search trước khi code
[ ] Không có debug statements
[ ] Null-safe operators đầy đủ
[ ] DB transactions nếu multi-step
[ ] Evidence verify đã có
[ ] Commit message theo conventional commits
[ ] INSTINCT đã apply: log kết quả
```

## SEARCH PROTOCOL — Decision Matrix

```
official lib exists         → dùng official
no official + >1M downloads → dùng popular
niche need + well-maintained → dùng specialized
nothing good                → build minimal custom (document why)
```

## Package Evaluation Criteria

```
- Weekly downloads / stars (popularity = maturity)
- Last commit (maintenance active?)
- Bundle size / performance overhead
- Security: known CVEs?
- TypeScript support?
```
