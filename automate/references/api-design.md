# API Design Principles

## Response Format (NHẤT QUÁN)

```json
{
  "data": { ... },
  "meta": {
    "page": 1,
    "per_page": 20,
    "total": 100
  },
  "errors": null
}
```

## Error Response

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

## HTTP Status Codes

| Code | Khi nào |
|---|---|
| 200 | Success (GET, PUT, PATCH) |
| 201 | Created (POST) |
| 204 | No Content (DELETE) |
| 400 | Bad Request (invalid body) |
| 401 | Unauthorized (no/invalid token) |
| 403 | Forbidden (valid token, no permission) |
| 404 | Not Found |
| 409 | Conflict (duplicate) |
| 422 | Unprocessable (validation error) |
| 429 | Too Many Requests (rate limit) |
| 500 | Internal Server Error |

## REST Conventions

```
GET    /api/resources         ← List (paginated)
GET    /api/resources/:id     ← Show one
POST   /api/resources         ← Create
PUT    /api/resources/:id     ← Full update
PATCH  /api/resources/:id     ← Partial update
DELETE /api/resources/:id     ← Delete
```

## Security Checklist

```
[ ] Input validation: DTO / schema (Zod, Joi, class-validator)
[ ] Auth: @Roles() decorator / middleware
[ ] Rate limiting: throttle per IP + per user
[ ] Secrets: env variables only
[ ] SQL injection: parameterized queries
[ ] CORS: restrict to known origins
[ ] Audit log: userId, action, before/after, IP
```
