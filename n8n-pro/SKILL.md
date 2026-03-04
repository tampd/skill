---
name: N8N Pro — Expert Workflow Automation
description: "Chuyên gia N8N toàn diện: thiết kế workflow, debug, tối ưu, bảo mật, tích hợp MCP. 7 chuyên môn: expression syntax, node config, Code nodes, webhook hardening, error handling, template patterns, MCP tools. Kích hoạt bằng /n8n [task]."
---

# /n8n [task description]

> **N8N Expert Agent** — Best practices từ community + CVE-2026-21858 patches
> Mục tiêu: Production-ready n8n workflows, không hallucinate node configs

---

## 7 CHUYÊN MÔN

### 1. EXPRESSION SYNTAX (AI HAY SAI)
```
✅ ĐÚNG:
  {{ $json.body.email }}           ← Webhook data nằm trong .body
  {{ $json["field name"] }}        ← Fields có space
  {{ $('Node Name').item.json }}   ← Tham chiếu node khác
  {{ $input.first().json }}        ← Item đầu tiên
  {{ $now.toISO() }}               ← Timestamp
  {{ $if($json.status === "active", "yes", "no") }}

❌ SAI (THƯỜNG HALLUCINATE):
  {{ $json.email }}                ← THIẾU .body cho webhook
  {{ $node["Name"].json }}         ← Deprecated
  {{ $items }}                     ← Không tồn tại
  {{ $json.body?.email }}          ← Optional chaining KHÔNG work
```

### 2. NODE CONFIGURATION
```
Trước khi config node: LUÔN check essentials trước (MCP: get_node_essentials)
HTTP Request Node Checklist:
[ ] Method, URL (không hardcode)
[ ] Headers: Content-Type
[ ] Auth: credential reference, KHÔNG inline
[ ] Timeout: ≤ 30s
[ ] Retry on Fail: enabled, max 3
[ ] Response: JSON auto-parse
```

### 3. CODE NODES
```javascript
// JS: Return [{json: {...}}], KHÔNG dùng require()/fetch()
const items = $input.all();
return items.map(item => ({
  json: { ...item.json, processed: true, ts: new Date().toISOString() }
}));
```
```python
# Python: Có json/datetime/re/math/hashlib/hmac/base64
# KHÔNG CÓ: pandas, numpy, requests
items = _input.all()
return [{"json": {**item.json, "processed": True}} for item in items]
```

### 4. WEBHOOK HARDENING
```
⚠️ CVE-2026-21858: Content-type confusion → upgrade n8n ≥ 1.121.0

Checklist:
[ ] HMAC Signature verification (SHA256)
[ ] IP Whitelist nếu biết source
[ ] Rate limiting / Auth trên webhook
[ ] Idempotency: check duplicate event ID
[ ] Input validation schema TRƯỚC KHI dùng
[ ] Return 200 NGAY → process async
[ ] Log payload (MASK sensitive data)
```

### 5. ERROR HANDLING (BẮT BUỘC)
```
Mọi production workflow PHẢI có:
1. Error Trigger Node → catch errors
2. Error Workflow → notify admin (Slack/Email/Telegram)
3. Retry logic cho HTTP calls
4. Dead Letter Queue cho failed items
```

### 6. DESIGN PATTERNS
```
A — Simple ETL:    Trigger → Fetch → Transform → Load
B — Fan-out:       Trigger → Split → Process Each → Merge → Output
C — Approval:      Trigger → Check → IF yes → Execute | IF no → Notify
D — Report:        Schedule → Query → Aggregate → Format → Email
E — AI Agent:      Webhook → Validate → AI Model → Format → Return

Anti-patterns:
❌ > 50 nodes → tách sub-workflows
❌ Nested IF > 3 → dùng Switch
❌ Hardcode credentials → Credentials Manager
❌ No error handling → LUÔN có Error Trigger
```

### 7. MCP INTEGRATION
```json
{
  "mcpServers": {
    "n8n-mcp": {
      "command": "npx",
      "args": ["-y", "@leonardsellem/n8n-mcp-server"],
      "env": {
        "N8N_API_URL": "http://localhost:5678/api/v1",
        "N8N_API_KEY": "your-api-key"
      }
    }
  }
}
```

### 8. MCP SERVER (Expose n8n as AI Tool) 🆕

> ⭐ **v4.0** — Biến n8n workflow thành MCP tools cho AI agents

```
Quy trình:
1. Dùng MCP Server Trigger node → define:
   - Tool name: [domain]-[action] (e.g. crm-sync-contacts)
   - Description: MÔ TẢ RÕ RÀNG để AI agent chọn đúng tool
   - Parameters: input schema (JSON)

2. Authentication BẮT BUỘC cho production:
   - Bearer Auth hoặc Header Auth trên MCP Server Trigger
   - Generate API credentials qua n8n credential system
   - Principle of Least Privilege: chỉ expose tools cần thiết

3. Response format:
   - Return structured JSON (KHÔNG trả HTML)
   - Include status, data, errors
   - Return 200 luôn, error info trong body

4. Client config (cho Cursor/Claude/Gemini):
   {
     "mcpServers": {
       "n8n-[project]": {
         "url": "https://n8n.domain.com/mcp",
         "headers": { "Authorization": "Bearer <token>" }
       }
     }
   }

5. Audit:
   - Log TẤT CẢ MCP tool calls qua execution logs
   - Monitor: ai gọi, khi nào, input gì, output gì
```

### 9. SUB-WORKFLOWS (Modular Design) 🆕

```
Rule: Workflow > 15 nodes → PHẢI tách sub-workflow

Design Pattern:
  Main Workflow = Orchestrator (trigger + routing + merge results)
  Sub-Workflow  = Worker (single responsibility)

Naming Convention:
  [domain]-[action]
  Examples:
    crm-sync-contacts
    payroll-calculate-salary
    notification-send-email

Error Handling:
  - Mỗi sub-workflow có Error Trigger riêng
  - Main workflow catch error từ sub-workflow
  - Dead Letter Queue cho failed items

Data Contract:
  - Input/Output schema document rõ ràng
  - Sub-workflow KHÔNG depend vào global state
  - Pass data qua parameters, KHÔNG qua global variables
```

### 10. SCALING PATTERNS 🆕

```
A — Queue Mode (High Throughput):
   - Redis backend cho job queue
   - Main process = trigger + enqueue
   - Worker processes = dequeue + execute
   - Config: EXECUTIONS_MODE=queue, QUEUE_BULL_REDIS_*

B — Webhook Async Processing:
   - Webhook → Validate → Return 200 NGAY
   - Queue → Process async (có retry)
   - Result → Callback / Webhook / DB update

C — Batch Processing:
   - Split items → SplitInBatches node (batch size: 50-100)
   - Process batch → parallel execution
   - Merge results → aggregate
   - Progress tracking: log batch N/total

D — Monitoring:
   - n8n execution logs (built-in)
   - Grafana dashboard cho production
   - Alert: workflow failures > threshold
   - Metrics: execution time, success rate, queue depth
```

---

## QUY TRÌNH

### Step 1 — ANALYZE: Đọc LESSONS #n8n → xác định workflow type → liệt kê nodes
### Step 2 — DESIGN: Vẽ flow → error strategy → credentials needed
### Step 3 — BUILD: Tạo workflow → config nodes → add error handling
### Step 4 — TEST: Mock data → happy path → error path → webhook signature → idempotency
### Step 5 — DOCUMENT: Workflow description → comment Code Nodes → webhook docs → LESSONS
