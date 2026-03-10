---
name: automate
description: N8N workflow automation and API integration. Use for /n8n (build N8N workflows), /integrate (API/webhook integration), /mcp (design MCP tools). Triggers on "N8N", "automation", "webhook", "API integration", "workflow", "tự động hóa", "integrate", "MCP tool".
---

# Automate Skill — N8N + API Design + MCP Tools

## /n8n [task]
> Thiết kế và implement N8N workflow automation

```
WORKFLOW DESIGN PROTOCOL:

STEP 1 — DEFINE (trước khi mở N8N)
  □ Trigger: gì kích hoạt workflow? (webhook/schedule/event/manual)
  □ Input: data gì vào? Format? Source?
  □ Output: data gì ra? Destination?
  □ Error scenarios: những gì có thể fail?
  □ Idempotency: chạy 2 lần có safe không?

STEP 2 — ARCHITECTURE
  □ Single workflow vs modular (sub-workflows)?
  □ State management: cần track state không? (DB/file/variable)
  □ Error handling: retry logic, dead letter queue
  □ Rate limiting: external API có limits không?
  □ Parallel execution safe không?

STEP 3 — IMPLEMENT

  TRIGGER NODES:
    Webhook → set respond: "Last Node" hoặc "Immediately"
    Schedule → định nghĩa timezone rõ ràng
    
  DATA VALIDATION (luôn có sau trigger):
    □ Validate required fields
    □ Type coercion nếu cần
    □ Sanitize strings (XSS prevention)
    
  CORE LOGIC NODES:
    □ HTTP Request: set timeout (30s default), error handling
    □ Code Node: document rõ input/output, handle exceptions
    □ Set Node: tên variables rõ ràng, không abbreviate
    
  ERROR HANDLING (bắt buộc):
    □ Try/Catch wrapping cho risky operations
    □ Error workflow (n8n Error Trigger)
    □ Slack/email alert khi critical failure
    □ Log errors với đủ context để debug
    
  RESPONSE/OUTPUT:
    □ Consistent response format
    □ Status codes đúng (200/201/400/500)
    □ Log successful execution với key metrics

STEP 4 — TEST
  □ Test với sample data trước production
  □ Test error paths (không chỉ happy path)
  □ Test rate limit behavior
  □ Test với null/empty inputs

STEP 5 — DOCUMENT
  □ Workflow description rõ ràng
  □ Node names mô tả action (không phải default "HTTP Request1")
  □ Sticky notes cho complex logic
  □ Env variables documented
```

---

## ANTI-PATTERNS (không được làm)
```
❌ Hardcode API keys trong workflow (dùng n8n credentials)
❌ Chaining 20+ nodes mà không có error handling
❌ Dùng Google Sheets làm database (race conditions)
❌ Monolithic prompt trong 1 AI node (modularize)
❌ No timeout trên HTTP Request nodes
❌ No retry logic cho external API calls
❌ Log sensitive data (PII, tokens, passwords)
```

---

## /integrate [service]
> API integration design và implementation

```
PRE-INTEGRATION CHECKLIST:
  □ Read official API docs (không guess)
  □ Authentication type: API Key / OAuth2 / JWT / Basic?
  □ Rate limits: requests/minute, requests/day?
  □ Pagination: cursor / page / offset?
  □ Webhooks available? (prefer over polling)
  □ SDK có sẵn? (prefer over raw HTTP)

INTEGRATION ARCHITECTURE:
  □ Abstraction layer: không gọi thẳng API ở business logic
  □ Retry với exponential backoff (3 retries, 1s/2s/4s)
  □ Circuit breaker: fail fast nếu service down
  □ Response caching nếu data ít thay đổi
  □ Webhook signature verification
  □ Idempotency keys cho write operations

ERROR HANDLING:
  □ 400: log request, return clear error message
  □ 401/403: refresh token / notify user
  □ 429: respect Retry-After header, pause + retry
  □ 500: retry với backoff, alert on persistent failure
  □ Timeout: set reasonable timeout (10-30s), handle gracefully

WEBHOOK SECURITY:
  □ Verify signature (HMAC-SHA256)
  □ Idempotency: store processed event IDs
  □ Respond 200 quickly, process async
  □ HTTPS only, no redirect following
```

---

## /mcp [tool-name]
> Design và implement MCP Tool cho N8N hoặc Antigravity

```
MCP TOOL DESIGN PRINCIPLES:
  □ Single responsibility: một tool = một action rõ ràng
  □ Name: verb_noun format (get_post_list, create_draft, publish_post)
  □ Description: đủ rõ để AI biết khi nào dùng
  □ Input schema: JSON Schema, validate ngay tại entry point
  □ Output: consistent structure { success, data, error }
  □ Error messages: actionable (AI có thể hiểu và retry)

MCP TOOL TEMPLATE:
  name: "action_resource"
  description: "Clear description of what this tool does and when to use it"
  inputSchema:
    type: object
    required: [field1, field2]
    properties:
      field1:
        type: string
        description: "What this field is for"
  
IDEMPOTENCY:
  □ GET operations: always idempotent
  □ POST/create: check for duplicates, return existing nếu đã tồn tại
  □ Use idempotency key cho critical operations

8-TOOL ARCHITECTURE (SEO Pipeline reference):
  tool_1_keyword_research   → input: topic → output: keyword list
  tool_2_content_gen        → input: keyword, brief → output: article draft
  tool_3_image_gen          → input: topic, style → output: image URL
  tool_4_media_upload       → input: image URL → output: wp media ID
  tool_5_taxonomy           → input: category name → output: term ID
  tool_6_publish_draft      → input: full post data → output: post ID
  tool_7_seo_meta           → input: post ID, meta → output: updated post
  tool_8_verify             → input: post ID → output: verification report
```
