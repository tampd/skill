# Deployment Guide Template

```markdown
# 🚀 DEPLOYMENT — [Project Name]
> Updated: YYYY-MM-DD | Strategy: [Rolling/Blue-Green/Canary]

## 1. Prerequisites
  - Server: [OS, specs]
  - Runtime: [Node 20+, PHP 8.2+, Python 3.12+]
  - Database: [type + version]
  - Secrets: [list env vars needed]

## 2. Deployment Steps
  ### Production
  ```bash
  # 1. Pull latest
  git pull origin main

  # 2. Install dependencies
  npm ci --production  # or composer install --no-dev

  # 3. Build
  npm run build

  # 4. Migrate database
  npx prisma migrate deploy  # or php artisan migrate --force

  # 5. Restart services
  pm2 restart app  # or systemctl restart app

  # 6. Verify
  curl -I https://domain.com/health
  ```

  ### Staging
  [Similar but with staging-specific steps]

## 3. Environment Variables
  | Key | Required | Default | Description |
  |-----|----------|---------|-------------|
  | DATABASE_URL | ✅ | — | DB connection string |
  | APP_SECRET | ✅ | — | Session encryption |
  | NODE_ENV | ✅ | production | Environment |

## 4. Rollback Procedure
  ```bash
  git log --oneline -5
  git revert <commit> --no-edit
  git push origin main
  # Verify: curl -I https://domain.com/health
  ```

## 5. CI/CD Pipeline
  Trigger: Push to `main` branch
  Stages: Lint → Test → Build → Deploy staging → Deploy production
  Config: `.github/workflows/deploy.yml`

## 6. Post-Deploy Checklist
  [ ] Health endpoint returns 200
  [ ] Monitoring dashboard shows normal metrics
  [ ] Smoke test critical user flows
  [ ] Check error rate in Sentry/logs
  [ ] Verify database migration succeeded
```
