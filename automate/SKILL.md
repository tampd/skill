---
name: automate
description: "Tự động hóa workflow và tích hợp API: N8N expert (expression, Code nodes, webhook, MCP Server) + REST/GraphQL API design + 3rd-party integration. Kích hoạt khi người dùng nói /n8n, /integrate, 'workflow', 'webhook', 'API', 'automation', 'payment', '3rd-party'."
metadata:
  author: tampd
  version: 7.0.0
  category: automation
---

# Automate — Workflow & API Integration

> **v7.0** — Gộp n8n-pro + integrate
> Mục tiêu: Production-ready automation + API chuẩn, secure, tested.

---

## 2 MODES

### Mode 1 — `/integrate [service]` (API & 3rd-Party)

```
STEP 1 — API DESIGN:
  1. Xác định type: REST endpoint | Webhook receiver | API consumer | Payment | GraphQL
  2. Design spec:
     Endpoint: [METHOD] [URL]
     Auth: [JWT | API Key | Webhook Secret]
     Input: [DTO / payload schema]
     Output: [{ data, meta?, errors? }]
  3. RBAC check: roles + permissions

STEP 2 — IMPLEMENT:
  REST: Controller → Service → DTO → Response envelope
  Webhook: Signature verify (HMAC-SHA256) → Idempotency → Async queue → Return 200 ngay
  Payment: PCI compliance + unique transaction ID + audit log

STEP 3 — SECURITY:
  [ ] Input validation: DTO decorators
  [ ] Auth: middleware + RBAC
  [ ] Rate limiting + CORS restrict
  [ ] Secrets: env only, KHÔNG hardcode
  [ ] Webhook: signature verification
  [ ] Audit log: userId, action, IP

STEP 4 — TEST:
  API: happy path (200) → validation (422) → auth (401/403) → not found (404)
  Webhook: valid sig → invalid sig → duplicate → missing fields
  Evidence: curl commands + responses

STEP 5 — DOCUMENT:
  Update docs/API_REFERENCE.md + docs/ARCHITECTURE.md nếu cần
```

---

### Mode 2 — `/n8n [task]` (N8N Workflow Expert)

```
STEP 1 — ANALYZE:
  Đọc LESSONS #n8n → workflow type → liệt kê nodes

STEP 2 — DESIGN:
  Chọn pattern:
    A — Simple ETL:  Trigger → Fetch → Transform → Load
    B — Fan-out:     Trigger → Split → Process Each → Merge
    C — Approval:    Trigger → Check → IF yes → Execute | IF no → Notify
    D — Report:      Schedule → Query → Aggregate → Format → Email
    E — AI Agent:    Webhook → Validate → AI Model → Format → Return

  Anti-patterns:
    ❌ > 50 nodes → tách sub-workflows
    ❌ Nested IF > 3 → dùng Switch
    ❌ Hardcode credentials
    ❌ No error handling

STEP 3 — BUILD:
  Tạo workflow → config nodes → add error handling
  Error Handling BẮT BUỘC: Error Trigger → Notify admin → Retry → Dead Letter Queue

STEP 4 — TEST:
  Mock data → happy path → error path → webhook signature → idempotency

STEP 5 — DOCUMENT:
  Workflow description → comment Code Nodes → webhook docs → LESSONS
```

> N8N expression syntax, Code nodes, MCP Server patterns → xem `references/n8n-patterns.md`
> API design principles, response format → xem `references/api-design.md`

---

## QUY TẮC

- API endpoint PHẢI có validation + auth + rate limiting
- Webhook PHẢI verify signature + idempotent
- Response format NHẤT QUÁN: `{ data, meta?, errors? }`
- Financial operations PHẢI có audit log
- Workflow > 15 nodes → PHẢI tách sub-workflow
- Error handling BẮT BUỘC cho mọi production workflow
- Secrets KHÔNG BAO GIỜ hardcode
