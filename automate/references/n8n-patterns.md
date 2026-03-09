# N8N Patterns & Expert Knowledge

## Expression Syntax (AI HAY SAI)

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

## Code Nodes

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

## Webhook Hardening

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

## MCP Server Integration

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

## Sub-Workflow Design

```
Rule: Workflow > 15 nodes → PHẢI tách sub-workflow

Pattern:
  Main Workflow = Orchestrator (trigger + routing + merge)
  Sub-Workflow  = Worker (single responsibility)

Naming: [domain]-[action] (e.g. crm-sync-contacts)

Data Contract:
  - Input/Output schema document rõ ràng
  - KHÔNG depend vào global state
  - Pass data qua parameters
```

## Scaling Patterns

```
A — Queue Mode:     Redis backend, EXECUTIONS_MODE=queue
B — Webhook Async:  Return 200 → Queue → Process → Callback
C — Batch:          SplitInBatches (50-100) → parallel → Merge
D — Monitoring:     Grafana + alert: failures > threshold
```
