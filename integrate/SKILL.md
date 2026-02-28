---
name: Integrate — API & Third-Party Integration Workflow
description: "Quy trình tích hợp API và dịch vụ bên ngoài: design → implement → security → test. Thay thế api-design-principles + nodejs-backend-patterns + n8n-workflow + payment-integration + graphql-architect. Kích hoạt bằng /integrate [service]."
---

# /integrate [service name]

> **5-in-1** — Gộp api-design + nodejs + n8n + payment + graphql
> Mục tiêu: API chuẩn, secure, documented, tested

---

## QUY TRÌNH 5 BƯỚC

### Step 1 — API DESIGN

```
1. Xác định integration type:
   - REST API endpoint mới
   - Webhook receiver (N8N, HostBill, etc.)
   - Third-party API consumer
   - Payment gateway
   - GraphQL schema

2. Design specification:
   Endpoint: [METHOD] [URL]
   Auth: [JWT | API Key | Webhook Secret | Public]
   Input: [DTO / payload schema]
   Output: [Response format: { data, meta?, errors? }]
   Errors: [Error codes table]

3. RBAC check:
   | Action | Admin | Manager | Sales | Accountant |
   |--------|-------|---------|-------|------------|
   | [action] | ✅/❌ | ... | ... | ... |
```

### Step 2 — IMPLEMENT

```
REST API:
- Controller: validation → service → response envelope
- Service: business logic, DB transactions
- DTO: class-validator decorators (NestJS) / FormRequest (Laravel)
- Response: { data, meta?, errors? } format

Webhook:
- Signature verification (HMAC-SHA256)
- Idempotency (duplicate check before processing)
- Async processing (queue nếu heavy)
- Logging: raw payload + result + errors
- Return 200 luôn (tránh retry flood)

N8N Integration:
- Webhook URL: /api/webhooks/[source]
- Secret: stored in config/env, KHÔNG hardcode
- Test mode: mock data cho development
- Error notification: cho admin khi webhook fail

Payment:
- PCI compliance: KHÔNG store raw card data
- Webhook verification: signature check
- Idempotent operations: unique transaction ID
- Audit logging: mọi financial operations
```

### Step 3 — SECURITY

```
Mandatory checks:
[ ] Input validation: DTO decorators / FormRequest rules
[ ] Auth: @Roles() decorator / middleware
[ ] Rate limiting: throttle appropriate level
[ ] Secrets: env variables, KHÔNG hardcode
[ ] SQL injection: parameterized queries only
[ ] Webhook: signature verification
[ ] CORS: restrict to known origins
[ ] Audit log: log userId, action, before/after, IP
```

### Step 4 — TEST

```
API endpoint tests:
1. Happy path: valid input → 200/201 + correct response
2. Validation: invalid input → 400/422 + error details
3. Auth: no token → 401, wrong role → 403
4. Not found: invalid ID → 404
5. Duplicate: existing record → 409
6. Edge cases: empty body, max length, special characters

Webhook tests:
1. Valid signature → processed
2. Invalid signature → 401
3. Duplicate event → skipped (idempotent)
4. Missing fields → logged, not rejected

Evidence: curl commands + expected responses
```

### Step 5 — DOCUMENT

```
1. Update docs/API_REFERENCE.md:
   - Endpoint, method, auth
   - Request/response examples
   - Error codes

2. If webhook: update integration guide
3. If new service: update docs/ARCHITECTURE.md
```

---

## QUY TẮC

- Mọi API endpoint PHẢI có validation + auth + rate limiting
- Webhook PHẢI verify signature + idempotent
- Response format NHẤT QUÁN: `{ data, meta?, errors? }`
- Financial operations PHẢI có audit log
- Secrets KHÔNG BAO GIỜ hardcode → env/config
