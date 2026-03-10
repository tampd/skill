# API Reference Template

```markdown
# 📡 API Reference — [Project Name]
> **Base URL**: `https://domain.com/api`
> **Auth**: [Bearer Token / API Key / etc.]

## 1. Authentication
### POST `/api/login`
| Param | Type | Required | Mô tả |
|---|---|---|---|
| email | string | ✅ | User email |
| password | string | ✅ | User password |

**Request:**
```bash
curl -X POST https://domain.com/api/login \
  -H "Content-Type: application/json" \
  -d '{"email": "user@example.com", "password": "secret"}'
```

**Response (200):**
```json
{
  "data": { "token": "eyJ...", "user": { "id": 1, "name": "..." } },
  "errors": null
}
```

**Error (401):**
```json
{
  "data": null,
  "errors": { "message": "Invalid credentials" }
}
```

## N. Error Response Format
```json
{
  "data": null,
  "errors": {
    "message": "Validation failed",
    "details": [
      { "field": "email", "message": "Email is required" }
    ]
  }
}
```

## N+1. Rate Limiting
| Endpoint | Limit |
|---|---|
| `/api/login` | 5 req/min |
| `/api/*` | 60 req/min |
```
