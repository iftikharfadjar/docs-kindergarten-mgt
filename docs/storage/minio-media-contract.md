# MinIO Media Contract

## Purpose
This document defines how media files are stored and referenced.

## Storage Provider
Use private MinIO object storage.

## Buckets

| Bucket | Purpose |
|---|---|
| `daily-reports` | Classroom photos attached to daily reports. |
| `student-documents` | Future registration documents and student files. Not included in MVP. |
| `profile-photos` | User or student profile photos. |

## Object Key Pattern

```text
{entityType}/{yyyy}/{mm}/{uuid}-{safeFileName}
```

Example:

```text
DAILY_REPORT/2026/08/550e8400-photo.jpg
```

## Allowed MIME Types
- `image/jpeg`
- `image/png`
- `application/pdf`

## Max Size
Default max size:

```text
10 MB
```

## MediaAssets Record

Required fields:

```text
id
uploader_user_id
entity_type
entity_id
url
file_name
mime_type
created_at
deleted_at
```

## Upload Response

```json
{
  "status": "success",
  "data": {
    "mediaAssetId": "uuid-media",
    "url": "https://minio.example.com/private-signed-url",
    "fileName": "file.jpg",
    "mimeType": "image/jpeg"
  }
}
```

## Security Rules
- Validate JWT before upload.
- Validate MIME type using file content, not only extension.
- Sanitize file names.
- Keep buckets private.
- Return private/signed URLs only when the authenticated user is allowed to view the media.
- Parent can only view media linked to reports for their linked active child.
