# Production Readiness Checklist — 5 Categories

> Chạy trong `/ship check` trước khi deploy production.
> PHẢI đạt ALL categories trước khi go-live.

## 📱 Application
- [ ] All tests pass (unit, integration, E2E)
- [ ] No hardcoded secrets in code or config files
- [ ] Error handling covers all edge cases
- [ ] Logging is structured (JSON) and does not contain PII
- [ ] Health check endpoint (`/health`) returns meaningful status
- [ ] API versioned (`/api/v1/`) and documented
- [ ] Feature flags for risky features
- [ ] Input validation on ALL user inputs
- [ ] Graceful degradation when dependencies fail

## 🏗️ Infrastructure
- [ ] Docker image builds reproducibly (pinned versions, multi-stage)
- [ ] Environment variables documented in `.env.example`
- [ ] Env vars validated at startup (fail fast on missing required vars)
- [ ] Resource limits set (CPU, memory)
- [ ] Horizontal scaling configured (min/max instances)
- [ ] SSL/TLS enabled on ALL endpoints (no mixed content)
- [ ] Database backups automated + tested restore procedure
- [ ] Static assets CDN-served (Cloudflare, CloudFront)
- [ ] DNS configured with proper TTL

## 📊 Monitoring
- [ ] Application metrics exported (request rate, latency, error rate)
- [ ] Alerts configured for: error rate > 5%, latency > 2s, CPU > 80%
- [ ] Log aggregation set up (structured, searchable)
- [ ] Uptime monitoring on health endpoint (UptimeRobot, Better Stack)
- [ ] Performance monitoring (LCP, TTFB, CLS) — Lighthouse CI or RUM
- [ ] Database query performance tracked (slow query log)
- [ ] Disk space monitoring (alert at 80%)

## 🔒 Security
- [ ] Dependencies scanned for CVEs (`npm audit`, `pip audit`)
- [ ] CORS configured for allowed origins only
- [ ] Rate limiting enabled on public endpoints
- [ ] Authentication + authorization verified
- [ ] Security headers set:
  - [ ] HSTS (Strict-Transport-Security)
  - [ ] CSP (Content-Security-Policy)
  - [ ] X-Frame-Options: DENY
  - [ ] X-Content-Type-Options: nosniff
  - [ ] Referrer-Policy: strict-origin-when-cross-origin
- [ ] SQL injection prevention verified
- [ ] XSS prevention verified
- [ ] CSRF tokens on state-changing requests
- [ ] File upload restrictions (type, size, path)

## ⚙️ Operations
- [ ] Rollback plan documented + tested
- [ ] Database migration tested against production-sized data
- [ ] Runbook for common failure scenarios (`/runbook`)
- [ ] On-call rotation and escalation path defined
- [ ] Incident response process documented
- [ ] Disaster recovery plan documented
- [ ] Maintenance window policy defined
