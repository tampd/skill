# Quality Gates — Automated Checks

> Chạy tự động trong `/save` BƯỚC 2, TRƯỚC multi-perspective review.

## Gate Checks (theo ngôn ngữ)

### JavaScript / TypeScript
```bash
# Lint
npx eslint --ext .ts,.tsx,.js,.jsx src/

# Format check
npx prettier --check "src/**/*.{ts,tsx,js,jsx,css,json}"

# TypeScript check
npx tsc --noEmit

# console.log audit
grep -rn "console\.log\|console\.warn\|console\.error" src/ --include="*.ts" --include="*.tsx" | grep -v "// keep" | grep -v "logger\."

# Test
npm test -- --coverage
```

### PHP / Laravel
```bash
# Lint
./vendor/bin/phpstan analyse --level=5

# Format
./vendor/bin/php-cs-fixer fix --dry-run --diff

# dd()/dump() audit
grep -rn "dd(\|dump(\|var_dump(" app/ routes/ resources/ --include="*.php"

# Test
php artisan test --coverage
```

### Python
```bash
# Lint
ruff check .

# Format
black --check .

# Type check
mypy src/

# print() audit
grep -rn "print(" src/ --include="*.py" | grep -v "# keep"

# Test
pytest --cov=src
```

## TODO/FIXME Scan
```bash
grep -rn "TODO\|FIXME\|HACK\|XXX" src/ app/ --include="*.ts" --include="*.tsx" --include="*.php" --include="*.py"
```
→ Mỗi entry: hỏi user "Convert thành ticket hay xóa?"

## Hardcoded Secrets Scan
```bash
# Common patterns
grep -rn "password\s*=\s*['\"]" --include="*.ts" --include="*.php" --include="*.py" .
grep -rn "api_key\s*=\s*['\"]" --include="*.ts" --include="*.php" --include="*.py" .
grep -rn "sk_live_\|pk_live_\|AKIA" --include="*.ts" --include="*.php" --include="*.py" .
```

## Gate Result
```
QUALITY GATE — [Project]
━━━━━━━━━━━━━━━━━━━━━━
✅ Lint:        PASS (0 errors)
✅ Format:      PASS (0 changes)
✅ TypeCheck:   PASS (0 errors)
✅ Debug audit: PASS (0 console.log/dd)
✅ TODO scan:   2 items → converted to tickets
✅ Secrets:     PASS (0 hardcoded)
✅ Tests:       PASS (45/45, 87% coverage)
━━━━━━━━━━━━━━━━━━━━━━
🟢 ALL GATES PASS — Ready to commit
```

## Khi Gate FAIL
```
❌ QUALITY GATE FAILED
  🔴 Lint: 3 errors (src/api.ts:42, src/utils.ts:88, src/index.ts:12)
  🔴 Debug: 2 console.log found (src/service.ts:55, src/handler.ts:23)
  
  → Fix required before commit. Run:
    npx eslint --fix src/
    # Then manually remove console.log statements
```
