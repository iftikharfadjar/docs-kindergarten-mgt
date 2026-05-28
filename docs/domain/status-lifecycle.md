# Status Lifecycle Contract

## Purpose
This document defines canonical lifecycle states. Use these exact values in code, database constraints, GraphQL schema, frontend UI, and tests.

## Academic Year

```text
DRAFT -> ACTIVE -> CLOSED -> ARCHIVED
```

Rules:
- Only one Academic Year can be `ACTIVE`.
- `DRAFT` can be configured.
- `ACTIVE` is the current operational year.
- `CLOSED` is no longer operational but can be used for reports/promotion source.
- `ARCHIVED` is read-only historical storage.
- Do not skip states.

## Student

```text
PENDING -> ACTIVE
PENDING -> REJECTED -> PENDING
ACTIVE -> ARCHIVED
```

Rules:
- `ACTIVE` means registration is approved.
- Do not use `APPROVED` as a student status.
- `PENDING` means waiting for Admin review.
- `REJECTED` means parent may edit and resubmit.
- `ARCHIVED` is final for normal operations.

## Semester Report

```text
DRAFT -> PUBLISHED
```

Rules:
- Parent can view only `PUBLISHED`.
- Teacher/Admin can view `DRAFT`.
- There is no Admin approval step for semester reports in MVP.

## Attendance

Allowed values:

```text
PRESENT
ABSENT
EXCUSED
LATE
```

Rules:
- Attendance is scoped to student, class, semester, academic year, and date.
- One active attendance record should exist per student/class/date.

## User Role

Allowed values:

```text
ADMIN
TEACHER
PARENT
```

Rules:
- Role controls route access, resolver permissions, and UI navigation.
- Teacher write permissions also require active `TeacherAssignments`.
- Parent access also requires `ParentStudentLinks`.

## Media Asset

Recommended lifecycle:

```text
UPLOADED -> ATTACHED -> DELETED
```

Rules:
- `UPLOADED` means file exists in MinIO and has a `MediaAssets` row.
- `ATTACHED` means `entityType` and `entityId` are set.
- `DELETED` means hidden/soft-deleted.
