---
name: spec
description: Architecture documentation, project handoff, ADR, runbooks, API docs, component docs. Use for /spec (system architecture), /handoff (project handoff doc), /adr (architecture decision record), /runbook (operations guide), /apidoc (API documentation), /componentdoc (component library docs). Triggers on "architecture", "spec", "handoff", "bàn giao", "ADR", "runbook", "API docs", "tài liệu", "documentation".
---

# Spec Skill — Ultra Architecture + Handoff + Runbook + API/Component Docs

## /spec [scope]
> 15-section architecture document — bắt buộc trước khi code feature lớn

```
OUTPUT: Structured Artifact (Antigravity) — lưu vào docs/SPEC-[feature].md

SECTION 1 — EXECUTIVE SUMMARY
  Mô tả ngắn: feature làm gì, tại sao cần, ai dùng, expected outcome

SECTION 2 — PROBLEM STATEMENT
  Current state: hiện tại có vấn đề gì?
  Root cause: tại sao vấn đề tồn tại?
  Impact: ảnh hưởng gì nếu không giải quyết?

SECTION 3 — GOALS & NON-GOALS
  Goals: những gì PHẢI đạt được (measurable)
  Non-goals: những gì KHÔNG nằm trong scope (explicitly)
  Success metrics: đo bằng gì?

SECTION 4 — USER STORIES & ACCEPTANCE CRITERIA
  As a [user], I want [action], so that [benefit]
  Acceptance criteria: Given/When/Then format
  Edge cases: những scenario đặc biệt

SECTION 5 — SYSTEM ARCHITECTURE
  High-level diagram (ASCII hoặc Mermaid)
  Component interaction flow
  Data flow diagram
  External dependencies

SECTION 6 — DATA MODEL
  Entities và relationships
  Database schema (nếu có changes)
  Migration plan
  Data retention / deletion policy

SECTION 7 — API CONTRACT
  Endpoints: method, path, description
  Request schema (JSON Schema)
  Response schema (success + error)
  Authentication/authorization requirements
  Rate limits
  Breaking vs non-breaking changes

SECTION 8 — COMPONENT BREAKDOWN (Frontend)
  Atomic Design mapping
  Component tree
  State management strategy
  Props interface cho key components

SECTION 9 — SECURITY CONSIDERATIONS
  Threat model: ai có thể abuse this?
  Authentication: who can access?
  Authorization: what can they do?
  Data sensitivity: PII? Encryption?
  OWASP risks specific to this feature

SECTION 10 — PERFORMANCE REQUIREMENTS
  Response time targets (p50, p95, p99)
  Expected load (requests/second, users)
  Scalability: horizontal? vertical?
  Caching strategy
  Core Web Vitals impact (nếu frontend)

SECTION 11 — ERROR HANDLING & RESILIENCE
  Failure modes: gì có thể fail?
  Error recovery strategy
  Retry logic
  Circuit breaker nếu applicable
  Graceful degradation

SECTION 12 — TESTING STRATEGY
  Unit tests: what to cover, target coverage
  Integration tests: critical paths
  E2E tests: user journeys
  Performance tests: load scenarios
  Security tests: OWASP checklist items

SECTION 13 — DEPLOYMENT PLAN
  Dependencies: gì cần deploy trước?
  Migration steps (nếu có)
  Feature flag strategy
  Rollback plan
  Zero-downtime requirements?

SECTION 14 — MONITORING & OBSERVABILITY
  Key metrics to track
  Alerts to set up
  Dashboards needed
  Log format / structured logging
  SLA targets

SECTION 15 — OPEN QUESTIONS & RISKS
  Unresolved questions (owner + deadline)
  Technical risks + mitigation
  Dependencies on external teams
  Timeline concerns
```

---

## /handoff [project]
> 15-section handoff document — developer mới tiếp nhận 100%

```
OUTPUT: Artifact lưu vào docs/HANDOFF-[project]-[date].md

SECTION 1 — PROJECT OVERVIEW
  Business context: tại sao project này tồn tại?
  Current status: stable / active development / legacy / sunset
  Primary contacts: PO, tech lead, stakeholders

SECTION 2 — QUICK START (new dev productive trong 1 giờ)
  Prerequisites list (tools, access, accounts)
  Step-by-step local setup (copy-paste ready commands)
  Common gotchas: những gì hay bị sai lúc setup
  First thing to check: how to verify setup works

SECTION 3 — ARCHITECTURE OVERVIEW
  Stack: frontend / backend / database / infra
  Diagram nếu có
  Key design decisions (và tại sao)
  Third-party services và dependencies

SECTION 4 — REPOSITORY STRUCTURE
  Directory map với descriptions
  Naming conventions
  Where does [X] live? (config, components, utils, tests)
  Branch strategy: main/develop/feature/hotfix

SECTION 5 — DEVELOPMENT WORKFLOW
  Local development workflow
  Code review process
  PR / merge requirements
  CI/CD pipeline description
  How to run tests

SECTION 6 — ENVIRONMENT & CONFIGURATION
  Environment files: what each does
  Required env vars table (name, description, example, required?)
  Where to find secrets (password manager / secret service)
  Staging vs production differences

SECTION 7 — DATABASE
  Schema overview + ER diagram nếu có
  How to run migrations
  Seed data: where, how to apply
  Common queries for debugging
  Backup / restore procedure

SECTION 8 — API REFERENCE
  Base URLs (dev/staging/prod)
  Authentication method + how to get token
  Key endpoints list với examples
  Error code reference
  Postman/Bruno collection location

SECTION 9 — EXTERNAL INTEGRATIONS
  List tất cả third-party services
  Per service: purpose, auth method, rate limits, admin URL
  What breaks if service goes down?
  How to test integrations locally (mocking)

SECTION 10 — DEPLOYMENT
  Deploy process: step by step
  Who can deploy? (permissions)
  Pre-deploy checklist
  Post-deploy verification
  Rollback procedure

SECTION 11 — KNOWN ISSUES & TECH DEBT
  Known bugs / limitations (với severity)
  Tech debt items (với effort estimate)
  Shortcuts taken và why
  DO NOT touch without talking to [person]

SECTION 12 — MONITORING & ALERTS
  Monitoring tools: URLs, how to access
  Critical alerts: what they mean, first response
  Performance baselines: what's "normal"
  On-call rotation (if applicable)

SECTION 13 — BUSINESS LOGIC
  Core business rules (không obvious từ code)
  Edge cases đặc biệt của domain
  Regulatory / compliance requirements
  "Unwritten rules" quan trọng

SECTION 14 — CONTACTS & ESCALATION
  Team structure
  Who owns what? (ownership matrix)
  External vendor contacts
  Escalation path cho critical issues

SECTION 15 — CHANGELOG & HISTORY
  Major versions + what changed
  Breaking changes history
  Migration guides nếu có
  Future roadmap items
```

---

## /adr [decision]
> Architecture Decision Record — ghi lại quyết định quan trọng

```
OUTPUT: docs/adr/ADR-[NNN]-[title].md

# ADR-[NNN]: [Title]

**Status:** Proposed | Accepted | Deprecated | Superseded by ADR-NNN
**Date:** YYYY-MM-DD
**Deciders:** [names]

## Context
Tình huống và constraints tại thời điểm quyết định.

## Decision Drivers
- [driver 1]: tại sao điều này quan trọng
- [driver 2]

## Options Considered
### Option A: [name]
- Pros: ...
- Cons: ...

### Option B: [name]
- Pros: ...
- Cons: ...

## Decision
Chọn **Option [X]** vì [lý do cụ thể].

## Consequences
**Positive:** ...
**Negative:** ...
**Risks:** ...
```

---

## /runbook [service]
> 7-section operations runbook

```
OUTPUT: docs/runbooks/RUNBOOK-[service].md

SECTION 1 — SERVICE OVERVIEW
  Purpose, criticality (P0/P1/P2), SLA

SECTION 2 — HEALTH CHECKS
  How to verify service is healthy
  URLs, commands, expected responses
  Dashboard links

SECTION 3 — COMMON OPERATIONS
  Per operation: restart / scale / clear cache / rollback
  Command templates (copy-paste ready)
  Expected output

SECTION 4 — INCIDENT RESPONSE
  Severity levels + response times
  Step-by-step triage for each alert type
  Escalation matrix

SECTION 5 — TROUBLESHOOTING
  Symptom → Diagnosis → Fix
  Common errors + solutions
  Log locations + key search queries

SECTION 6 — MAINTENANCE
  Scheduled tasks + schedules
  Backup verification procedure
  Certificate renewal
  Dependency updates

SECTION 7 — DISASTER RECOVERY
  RTO / RPO targets
  Backup locations + how to restore
  Failover procedure
  Post-incident review checklist
```

---

## /apidoc [endpoint or service]
> OpenAPI-compatible API documentation

```
OUTPUT: docs/api/[service]-api.md hoặc openapi.yaml

PER ENDPOINT:
  ## POST /api/v1/[resource]
  **Description:** [what it does]
  **Authentication:** Bearer token / API Key / None
  **Rate limit:** [X] requests/minute

  ### Request
  ```json
  {
    "field": "type — description (required/optional)"
  }
  ```

  ### Response 200
  ```json
  {
    "success": true,
    "data": { ... }
  }
  ```

  ### Error Responses
  | Code | Meaning | How to fix |
  |------|---------|------------|
  | 400  | Validation error | Check request body |
  | 401  | Unauthorized | Refresh token |
  | 429  | Rate limited | Wait + retry |

  ### Example
  ```bash
  curl -X POST https://api.example.com/v1/resource \
    -H "Authorization: Bearer TOKEN" \
    -d '{"field": "value"}'
  ```
```

---

## /componentdoc [scope]
> Component library reference documentation

```
OUTPUT: docs/components/[name].md

PER COMPONENT:
  ## [ComponentName]
  **Category:** Atom / Molecule / Organism
  **Status:** Stable | Beta | Deprecated

  **Purpose:** [1 câu]

  ### Props
  | Prop | Type | Default | Required | Description |
  |------|------|---------|----------|-------------|
  | variant | 'default'\|'primary'\|'destructive' | 'default' | No | Visual style |

  ### Usage
  ```tsx
  <Button variant="primary" size="md" onClick={handleClick}>
    Click me
  </Button>
  ```

  ### Variants Gallery
  [Screenshot hoặc Storybook link]

  ### Accessibility
  - Keyboard: Enter/Space to activate
  - ARIA: role="button", aria-disabled

  ### Do / Don't
  ✅ Do: ...
  ❌ Don't: ...

  ### Known Limitations
  - [limitation 1]
```
