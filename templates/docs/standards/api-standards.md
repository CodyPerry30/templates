# API Standards

> **Scope**: REST endpoints, request/response contracts

## URL Structure

| Pattern | Example |
|---------|---------|
| Base URL | `/api/v1` |
| Collection | `/api/v1/users` |
| Resource | `/api/v1/users/{id}` |
| Nested | `/api/v1/users/{id}/orders` |
| Query params | `?page=1&page_size=20` |

- Use plural nouns for collections
- Use kebab-case for multi-word resources
- Max 2 levels of nesting; use query params beyond that

## HTTP Methods

| Method | Purpose | Response |
|--------|---------|----------|
| GET | Retrieve | 200 OK |
| POST | Create | 201 Created |
| PUT | Replace | 200 OK |
| PATCH | Update | 200 OK |
| DELETE | Remove | 204 No Content |

## Response Format

```json
// ✅ Success (single resource)
{
    "id": 123,
    "name": "John",
    "email": "john@example.com"
}

// ✅ Success (collection with pagination)
{
    "data": [...],
    "pagination": {
        "page": 1,
        "pageSize": 20,
        "totalItems": 156,
        "totalPages": 8
    }
}

// ✅ Error (RFC 7807)
{
    "type": "https://api.example.com/errors/validation",
    "title": "Validation Error",
    "status": 422,
    "detail": "Email is required",
    "errors": [{ "field": "email", "code": "required" }]
}
```

## Conventions

- Use consistent casing for JSON properties (camelCase or snake_case)
- Dates in ISO 8601 format: `2025-01-26T14:30:00Z`
- Omit null fields; use empty arrays instead of null
- Bearer token authentication via `Authorization` header

## Boundaries

- ✅ Always: Version APIs, use proper status codes, document in OpenAPI
- ⚠️ Ask first: Breaking changes, new endpoints, auth changes
- 🚫 Never: Sensitive data in URLs, stack traces in error responses

## Notes

- Filter with query params: `?status=active&created_after=2025-01-01`
- Sort with: `?sort=created_at:desc`
- Pagination required for all collection endpoints
