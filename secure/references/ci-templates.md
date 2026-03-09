# CI/CD Pipeline Templates

## GitHub Actions — Standard Pipeline

```yaml
# .github/workflows/ci.yml
name: CI/CD
on:
  push:
    branches: [main, develop]
  pull_request:
    branches: [main]

jobs:
  quality:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with: { node-version: '20', cache: 'npm' }
      - run: npm ci
      - run: npx tsc --noEmit          # TypeScript
      - run: npm run lint               # Lint
      - run: npm test -- --coverage --ci # Tests

  build:
    needs: quality
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with: { node-version: '20', cache: 'npm' }
      - run: npm ci && npm run build

  lighthouse:
    needs: build
    if: github.ref == 'refs/heads/main'
    runs-on: ubuntu-latest
    steps:
      - uses: treosh/lighthouse-ci-action@v11
        with:
          budgetPath: './lighthouse-budget.json'
          uploadArtifacts: true

  deploy:
    needs: [build, lighthouse]
    if: github.ref == 'refs/heads/main'
    environment: production
    runs-on: ubuntu-latest
    steps:
      - name: Deploy
        run: echo "Deploy to production"
```

## Lighthouse Budget

```json
[{
  "timings": [
    { "metric": "largest-contentful-paint", "budget": 2500 },
    { "metric": "first-contentful-paint", "budget": 1500 }
  ],
  "categories": [
    { "id": "performance", "minScore": 0.9 },
    { "id": "accessibility", "minScore": 0.9 }
  ]
}]
```

## Rollback Commands

```bash
# Vercel instant rollback (< 30s)
vercel rollback [deployment-url]

# Git revert
git revert HEAD --no-edit && git push origin main
```
