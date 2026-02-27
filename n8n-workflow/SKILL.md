---
name: N8N Workflow — Quy trình tích hợp N8N
description: "Quy trình chuẩn hóa cho thiết kế và tích hợp N8N workflows: từ thiết kế API → hardening security → implement handler → test. Kích hoạt khi cần kết nối N8N với hệ thống, tạo webhook, automation flows."
---

# HƯỚNG DẪN KỸ NĂNG: N8N WORKFLOW

## Mục tiêu
Mỗi N8N integration phải: **đúng schema → bảo mật → có test → có fallback**. Không expose endpoint không auth, không bỏ qua validation.

---

## KIẾN TRÚC N8N INTEGRATION

```
N8N Trigger
    ↓
[Webhook URL] → Rate Limit → Auth Header Check → Payload Validation
    ↓
Backend Handler (Laravel/Node.js)
    ↓
Business Logic → DB → Response
    ↓
N8N Continue Flow / Error Branch
```

---

## QUY TRÌNH 5 BƯỚC

### Bước 1 — THIẾT KẾ API ENDPOINT (api-design-principles)

```
Xác định trước khi implement:

METHOD: POST (nhận data) | GET (query) | PUT/PATCH (update)
URL: /api/n8n/{action} hoặc /webhooks/{event-name}
AUTH: Bearer token | HMAC signature | Basic auth
PAYLOAD: JSON schema rõ ràng
RESPONSE: 200 success | 400 validation | 401 auth | 500 error
IDEMPOTENCY: Có cần xử lý duplicate webhook không?
```

#### REST Schema mẫu
```json
// Request
{
  "event": "leave.approved",
  "timestamp": "2026-02-25T10:00:00Z",
  "data": {
    "employee_id": "NV001",
    "leave_type": "annual",
    "days": 2
  }
}

// Response thành công
{
  "status": "success",
  "message": "Leave notification processed",
  "processed_at": "2026-02-25T10:00:01Z"
}

// Response lỗi
{
  "status": "error",
  "code": "VALIDATION_ERROR",
  "message": "employee_id is required"
}
```

---

### Bước 2 — HARDENING SECURITY (security-auditor)

#### Authentication
```php
// Laravel: Middleware kiểm tra N8N token
public function handle(Request $request, Closure $next)
{
    $token = $request->header('X-N8N-Token');
    $expected = config('services.n8n.webhook_secret');

    if (!$token || !hash_equals($expected, $token)) {
        return response()->json(['error' => 'Unauthorized'], 401);
    }
    return $next($request);
}
```

#### Checklist bảo mật N8N
```
[ ] Endpoint có Authentication (token/HMAC/IP whitelist)
[ ] Rate limiting: throttle:30,1 (30 req/phút)
[ ] Payload validation: validate tất cả required fields
[ ] Input sanitization: escape trước khi vào DB
[ ] Không log sensitive data (password, token)
[ ] SSL/TLS: chỉ nhận HTTPS
[ ] IP whitelist N8N server nếu biết IP cố định
[ ] Idempotency key nếu webhook có thể duplicate
```

#### HMAC Signature Verification (nâng cao)
```php
$payload = $request->getContent();
$signature = $request->header('X-Webhook-Signature');
$expected = 'sha256=' . hash_hmac('sha256', $payload, config('services.n8n.secret'));

if (!hash_equals($expected, $signature)) {
    abort(401, 'Invalid signature');
}
```

---

### Bước 3 — IMPLEMENT HANDLER

#### Laravel Handler Pattern
```php
// routes/api.php
Route::post('/webhooks/n8n/{action}', [N8nWebhookController::class, 'handle'])
    ->middleware(['n8n.auth', 'throttle:30,1']);

// app/Http/Controllers/Api/N8nWebhookController.php
class N8nWebhookController extends Controller
{
    public function handle(Request $request, string $action): JsonResponse
    {
        $validated = $request->validate([
            'event'    => 'required|string',
            'data'     => 'required|array',
            'timestamp'=> 'required|date',
        ]);

        return match($action) {
            'leave'    => $this->processLeave($validated['data']),
            'payroll'  => $this->processPayroll($validated['data']),
            default    => response()->json(['error' => 'Unknown action'], 400),
        };
    }

    private function processLeave(array $data): JsonResponse
    {
        // Business logic trong Service layer
        $result = app(LeaveService::class)->processN8nApproval($data);

        AuditLog::create([
            'action' => 'n8n.leave.processed',
            'data'   => $data,
        ]);

        return response()->json(['status' => 'success', 'result' => $result]);
    }
}
```

#### Error Handling & Retry
```php
// Luôn có try-catch với structured error response
try {
    $result = $service->process($data);
    return response()->json(['status' => 'success'], 200);
} catch (ValidationException $e) {
    return response()->json(['status' => 'error', 'errors' => $e->errors()], 422);
} catch (\Exception $e) {
    Log::error('N8N webhook failed', ['error' => $e->getMessage(), 'data' => $data]);
    return response()->json(['status' => 'error', 'message' => 'Internal error'], 500);
}
```

---

### Bước 4 — TEST WEBHOOK

#### Test bằng curl
```bash
# Test happy path
curl -X POST https://your-site.com/api/webhooks/n8n/leave \
  -H "Content-Type: application/json" \
  -H "X-N8N-Token: your-secret-token" \
  -d '{"event":"leave.created","data":{"employee_id":"NV001"},"timestamp":"2026-02-25T10:00:00Z"}'

# Test missing auth
curl -X POST https://your-site.com/api/webhooks/n8n/leave \
  -H "Content-Type: application/json" \
  -d '{"event":"test"}' # Phải return 401

# Test rate limit (gửi 31 requests)
for i in $(seq 1 31); do curl -s -o /dev/null -w "%{http_code}\n" ...; done
# Request 31 phải return 429
```

#### N8N Test Flow
```
1. Trong N8N: Set webhook URL → Test trigger
2. Kiểm tra backend nhận request (Laravel log)
3. Kiểm tra data được xử lý đúng
4. Kiểm tra N8N nhận response và continue flow
5. Test error branch: gửi invalid data → N8N có xử lý không?
```

---

### Bước 5 — MONITORING & FALLBACK

```php
// Ghi log mọi N8N webhook
Log::channel('n8n')->info('Webhook received', [
    'action'    => $action,
    'event'     => $request->input('event'),
    'timestamp' => now()->toISOString(),
    'ip'        => $request->ip(),
]);

// Queue nếu processing lâu
dispatch(new ProcessN8nWebhookJob($action, $validated));
return response()->json(['status' => 'queued'], 202);
```

---

## N8N WORKFLOW DESIGN BEST PRACTICES

```
✅ Mỗi N8N workflow nên có:
   - Error branch (HTTP 4xx/5xx)
   - Retry logic (3 lần với backoff)
   - Alert khi fail (Slack/Email)
   - Timeout: không set > 30s

✅ Webhook URL format:
   /api/webhooks/{service}/{event}
   Ví dụ: /api/webhooks/n8n/leave-approved

✅ Payload format nhất quán:
   event, data, timestamp trong mọi webhook

❌ KHÔNG dùng GET cho webhooks nhận data
❌ KHÔNG để webhook không auth
❌ KHÔNG xử lý heavy task đồng bộ → dùng Queue
```

---

## COMBO SKILLS

| Tình huống | Combo |
|---|---|
| Tạo webhook mới | `n8n-workflow` → `api-design-principles` → `security-auditor` |
| Webhook chậm | `n8n-workflow` → `performance-engineer` (queue) |
| Webhook bị lỗi | `n8n-workflow` → `debugger` → `bug-memory-journal` |
| Full integration | `api-design-principles` → `security-auditor` → `n8n-workflow` → `e2e-testing-patterns` |
