---
name: api-design
description: REST API design with OpenAPI spec, error handling, pagination, and versioning
category: development
tags: [api, rest, openapi, endpoints, http]
author: claude-skills
version: 1.0.0
---

# REST API Design

Design a well-structured REST API from requirements. Produce an OpenAPI specification, route definitions, error handling patterns, and implementation scaffolding.

## Execution Steps

### 1. Identify Resources

From the requirements, extract:
- Core domain entities (nouns become resources)
- Relationships between entities (1:1, 1:N, N:N)
- Actions that map to standard CRUD vs custom operations
- Access patterns (who reads what, who writes what)

Name resources as plural nouns: `/users`, `/orders`, `/products`.

### 2. Design URL Structure

Follow these conventions:
- Collection: `GET /api/v1/users`
- Single resource: `GET /api/v1/users/:id`
- Nested resource: `GET /api/v1/users/:id/orders`
- Limit nesting to 2 levels maximum. Beyond that, promote to top-level with a filter: `GET /api/v1/orders?userId=123`
- Use kebab-case for multi-word paths: `/order-items`
- Never use verbs in URLs (except for non-CRUD actions: `/api/v1/orders/:id/cancel`)

### 3. Map HTTP Methods

| Method | Purpose | Idempotent | Response Code |
|--------|---------|------------|---------------|
| GET | Read resource(s) | Yes | 200 |
| POST | Create resource | No | 201 + Location header |
| PUT | Full replace | Yes | 200 |
| PATCH | Partial update | Yes | 200 |
| DELETE | Remove resource | Yes | 204 |

### 4. Design Request/Response Schemas

For each endpoint, define:
- Request body schema (POST/PUT/PATCH) with required vs optional fields
- Response body schema with consistent envelope:

```json
{
  "data": { ... },
  "meta": { "requestId": "abc-123" }
}
```

For collections, include pagination:

```json
{
  "data": [ ... ],
  "meta": {
    "page": 1,
    "perPage": 20,
    "total": 142,
    "totalPages": 8
  },
  "links": {
    "self": "/api/v1/users?page=1",
    "next": "/api/v1/users?page=2",
    "last": "/api/v1/users?page=8"
  }
}
```

### 5. Error Handling

Use a consistent error envelope for ALL error responses:

```json
{
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Human-readable explanation",
    "details": [
      { "field": "email", "issue": "must be a valid email address" }
    ],
    "requestId": "abc-123"
  }
}
```

Standard error codes and HTTP status mapping:
- 400 — VALIDATION_ERROR, BAD_REQUEST
- 401 — UNAUTHORIZED (not authenticated)
- 403 — FORBIDDEN (authenticated but not allowed)
- 404 — NOT_FOUND
- 409 — CONFLICT (duplicate, version mismatch)
- 422 — UNPROCESSABLE_ENTITY (valid syntax, invalid semantics)
- 429 — RATE_LIMITED
- 500 — INTERNAL_ERROR (never expose stack traces)

### 6. Pagination, Filtering, Sorting

- Pagination: `?page=1&perPage=20` (default 20, max 100)
- Filtering: `?status=active&createdAfter=2025-01-01`
- Sorting: `?sort=createdAt:desc,name:asc`
- Field selection: `?fields=id,name,email`
- Search: `?q=search+term`

### 7. Versioning Strategy

Use URL path versioning: `/api/v1/...`
- Increment major version only for breaking changes
- Support previous version for minimum 6 months with deprecation headers
- Add `Sunset` and `Deprecation` headers to deprecated versions

### 8. Generate OpenAPI Specification

Produce a valid OpenAPI 3.0 YAML file containing:
- Info block with title, version, description
- Server URLs for dev and production
- All paths with operations
- Request/response schemas using $ref to shared components
- Security schemes (Bearer token, API key, or OAuth2)
- Example values for every schema

Save the spec to `/docs/openapi.yaml`.

### 9. Generate Route Scaffolding

Create the router/controller files with:
- Input validation middleware (zod, joi, or class-validator)
- Authentication middleware on protected routes
- Consistent error handling middleware
- Request logging with correlation IDs
- Rate limiting configuration

### 10. Checklist

- [ ] All resources use plural nouns
- [ ] Every endpoint has defined request and response schemas
- [ ] Error responses follow the standard envelope
- [ ] Pagination is implemented on all collection endpoints
- [ ] Authentication is specified for non-public endpoints
- [ ] OpenAPI spec is valid (no $ref errors)
- [ ] No sensitive data (passwords, tokens) in response bodies
