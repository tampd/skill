# Cleanup Pass — De-sloppify Pattern

> Chạy trong `/build` PHASE 6, SAU verify.
> Rule 17: CLEANUP PASS SAU VERIFY.

## Triết lý

> "AI thường viết code 'good enough' trong lần đầu. Một pass riêng biệt
> chỉ tập trung vào cleanup sẽ bắt được những thứ mà implementation pass bỏ qua."
> — Adapted từ ECC "De-sloppify Pattern"

## Tại sao cần separate pass?

1. Khi implement, focus vào LOGIC → dễ bỏ sót style issues
2. Khi debug, focus vào FIX → quên xóa debug code
3. Review riêng = fresh eyes = không confirmation bias

## Checklist

```
CLEANUP PASS — Đọc LẠI toàn bộ code đã viết:

█ CODE QUALITY
  [ ] Dead code / unused imports removed
  [ ] No debug statements (console.log / dd() / print())
  [ ] No commented-out code blocks
  [ ] No placeholder values ("TODO", "FIXME", "xxx")

█ TYPE SAFETY
  [ ] No `any` type in TypeScript (use specific types)
  [ ] No type assertions (`as any`) unless justified
  [ ] PHPDoc / docstrings on public methods
  [ ] Return types declared

█ VALUES
  [ ] No magic numbers → named constants
  [ ] No hardcoded strings → config/env
  [ ] No hardcoded colors/sizes → CSS variables
  [ ] API URLs in config, not inline

█ DRY
  [ ] No copy-paste blocks > 5 lines → extract function
  [ ] No repeated validation logic → shared validator
  [ ] Similar components → extract base component

█ ERROR HANDLING
  [ ] All async operations have error handling
  [ ] User-facing error messages (not raw errors)
  [ ] Edge cases: null, empty array, undefined, 0

█ NAMING
  [ ] Variables describe WHAT, not HOW (users not arr)
  [ ] Boolean variables use is/has/can/should prefix
  [ ] Functions describe ACTION (calculateTotal not total)
  [ ] Consistent naming across file
```

## Quy trình

```
1. git diff --name-only → list all changed files
2. Đọc từng file top-to-bottom (KHÔNG edit while reading)
3. Ghi chú issues tìm thấy
4. Fix từng issue → micro-commit riêng:
   chore(cleanup): remove console.log from PayrollService
   chore(cleanup): extract magic numbers to constants
   chore(cleanup): add missing TypeScript types
```

## Khi nào SKIP
- Prototype / PoC (sẽ viết lại production code)
- CSS-only changes (no logic)
- Config / env file changes
