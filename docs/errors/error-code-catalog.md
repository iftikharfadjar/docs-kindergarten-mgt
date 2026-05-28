# Error Code Catalog

## Purpose
Canonical error codes for REST envelopes and GraphQL `extensions.code`.

| Code | HTTP Equivalent | Meaning |
|---|---:|---|
| `UNAUTHORIZED` | 401 | Missing, expired, or invalid authentication. |
| `FORBIDDEN` | 403 | Authenticated but not allowed. |
| `NOT_FOUND` | 404 | Resource does not exist or is not visible to caller. |
| `VALIDATION_ERROR` | 400 | Input failed validation. |
| `CONFLICT` | 409 | Duplicate or conflicting state. |
| `RATE_LIMITED` | 429 | Too many requests. |
| `FILE_TOO_LARGE` | 400 | Uploaded file exceeds max size. |
| `UNSUPPORTED_MEDIA_TYPE` | 415 | Uploaded file MIME type not allowed. |
| `STATE_TRANSITION_ERROR` | 400 | Invalid lifecycle transition. |
| `CAPACITY_EXCEEDED` | 400 | Class capacity would be exceeded. |
| `INTERNAL_ERROR` | 500 | Unexpected server error. |

## REST Error Shape

```json
{
  "status": "error",
  "error": {
    "code": "FORBIDDEN",
    "message": "You do not have permission to access this resource"
  }
}
```

## GraphQL Error Shape

```json
{
  "data": null,
  "errors": [
    {
      "message": "You do not have permission to access this resource",
      "extensions": {
        "code": "FORBIDDEN"
      }
    }
  ]
}
```

## Message Rules
- Login errors must be generic.
- Forgot password response must be generic.
- Validation messages can be specific.
- Authorization messages should not leak private resource existence.
