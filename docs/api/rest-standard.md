# REST Standard

## Purpose
REST is used only for HTTP-specific workflows:
- Authentication
- Token refresh/logout cookies
- Password reset email flow
- Media upload
- Push notification device registration

All relational CRUD must use GraphQL.

## Success Envelope

```json
{
  "status": "success",
  "data": {}
}
```

## Error Envelope

```json
{
  "status": "error",
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Human readable error"
  }
}
```

## Auth Endpoints

| Endpoint | Purpose |
|---|---|
| `POST /api/v1/auth/login` | Validate credentials, return access token, set refresh cookie. |
| `POST /api/v1/auth/refresh` | Read refresh cookie, return new access token. |
| `POST /api/v1/auth/logout` | Revoke refresh token and clear cookie. |
| `POST /api/v1/auth/forgot-password` | Send reset email if account exists. |
| `POST /api/v1/auth/reset-password` | Reset password using valid reset token. |

## Cookie Rules
- Refresh token cookie must be `HttpOnly`.
- Refresh token cookie must be `Secure` outside local development.
- Refresh token cookie must use `SameSite=Strict`.
- Backend stores only a hash of the refresh token.

## Media Upload Endpoint

```http
POST /api/v1/media/upload
Authorization: Bearer <accessToken>
Content-Type: multipart/form-data
```

Rules:
- Store file in MinIO.
- Create `MediaAssets` row.
- Return `mediaAssetId` and `url`.
- Validate file size and MIME type.

## Device Registration Endpoint

```http
POST /api/v1/notifications/register-device
Authorization: Bearer <accessToken>
Content-Type: application/json
```

Rules:
- Validate JWT.
- Upsert by `user_id + fcm_token`.
- Do not create duplicate tokens for the same user/device.

## Security Rules
- Rate limit auth endpoints.
- Use strict CORS.
- Use CSRF/origin protection where cookie endpoints require it.
- Never return different forgot-password responses for existing vs non-existing emails.
