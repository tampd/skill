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

---

## QUY TRÌNH

### Step 1 — ANALYZE: Đọc LESSONS #n8n → xác định workflow type → liệt kê nodes
### Step 2 — DESIGN: Vẽ flow → error strategy → credentials needed
### Step 3 — BUILD: Tạo workflow → config nodes → add error handling
### Step 4 — TEST: Mock data → happy path → error path → webhook signature → idempotency
### Step 5 — DOCUMENT: Workflow description → comment Code Nodes → webhook docs → LESSONS
