# Runbook Template — 7 Sections

> Operations runbook cho production services.
> Mục tiêu: Anyone on-call có thể resolve incidents theo hướng dẫn.

```markdown
# 📖 RUNBOOK — [Service Name]
> Last updated: YYYY-MM-DD | On-call: [team]

## 1. Service Overview
  Purpose: [1-2 sentences]
  Dependencies: [DB, Redis, External APIs]
  SLA: [99.9% uptime]
  Business impact: [Revenue / User flow affected]

## 2. Health Checks
  | Check | Command | Expected | Interval |
  |-------|---------|----------|----------|
  | HTTP | curl /health | 200 | 1min |
  | DB | pg_isready | accepting | 30s |

## 3. Common Incidents
  ### 🔴 [Incident Type]
  Symptoms: [what observer sees]
  Diagnosis: [commands to run]
  Resolution: [step-by-step fix]
  Prevention: [long-term fix]
  Est. time: [X minutes]

## 4. Scaling Procedures
  Horizontal: [add instances]
  Vertical: [resize]
  DB: [read replicas]

## 5. Backup & Restore
  | What | Schedule | Location | Retention | Restore |
  |------|----------|----------|-----------|---------|
  | DB | Daily 3AM | S3 | 30d | pg_restore |

## 6. Log Locations
  | Log | Path | Format | Useful for |
  |-----|------|--------|------------|
  | App | /var/log/app/ | JSON | Errors |

## 7. Emergency Contacts
  | Severity | Contact | Channel | SLA |
  |----------|---------|---------|-----|
  | P0 | Lead | Phone | 15min |
```
