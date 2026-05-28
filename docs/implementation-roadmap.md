# Implementation Roadmap

## Purpose
This document defines the recommended implementation order for the Kindergarten School Management Application. Follow this order unless a task explicitly says otherwise.

## Priority Order

| Priority | Workflow | Related Docs | Completion Criteria |
|---:|---|---|---|
| 1 | Authentication & Security | `docs/workflow/authentication-and-security.md`, `docs/workflow/authentication-and-security(low).md` | Login, refresh, logout, forgot password, reset password, JWT middleware, route guards. |
| 2 | User & Role Management | `docs/workflow/user-and-role-management.md`, `docs/workflow/user-and-role-management(low).md` | Admin can create/update/soft-delete users and list by role. |
| 3 | Academic Year Setup & Rollover | `docs/workflow/academic-year-setup-and-rollover.md`, `docs/workflow/academic-year-setup-and-rollover(low).md` | Academic year CRUD, lifecycle, clone, exactly 2 semesters auto-created. |
| 4 | Active Semester Management | `docs/workflow/active-semester-management.md`, `docs/workflow/active-semester-management(low).md` | Semester CRUD and backend `getActiveSemester`. |
| 5 | Class Management | `docs/workflow/class-management.md`, `docs/workflow/class-management(low).md` | Class CRUD, capacity validation, academic-year scoping. |
| 6 | Teacher Assignment | `docs/workflow/teacher-assignment.md`, `docs/workflow/teacher-assignment(low).md` | Assignment CRUD and `myAssignedClasses`. |
| 7 | Curriculum & Skill Configuration | `docs/workflow/curriculum-and-skill-configuration.md`, `docs/workflow/curriculum-and-skill-configuration(low).md` | Category and skill CRUD scoped by academic year. |
| 8 | Parent Self-Service Registration | `docs/workflow/parent-self-service-registration.md`, `docs/workflow/parent-self-service-registration(low).md` | Parent creates account/child, Admin review, reject/resubmit, approve to `ACTIVE`. |
| 9 | Student Enrollment & Transfer | `docs/workflow/student-enrollment-and-transfer.md`, `docs/workflow/student-enrollment-and-transfer(low).md` | Enroll only `ACTIVE` students, transfer, unenroll, capacity checks. |
| 10 | Media Upload & MinIO | `docs/workflow/media-upload-and-minio.md`, `docs/workflow/media-upload-and-minio(low).md` | REST upload, MinIO storage, `MediaAssets` records, link to entities. |
| 11 | Teacher Daily Operations | `docs/workflow/teacher-daily-operations.md`, `docs/workflow/teacher-daily-operations(low).md` | Attendance, daily reports, teacher assignment write checks. |
| 12 | Student Assessment | `docs/workflow/student-assessment.md`, `docs/workflow/student-assessment(low).md` | Score 0-4, active semester, assigned-teacher write access. |
| 13 | Semester Reporting | `docs/workflow/semester-reporting.md`, `docs/workflow/semester-reporting(low).md` | Live report, draft creation, publish to parents. |
| 14 | Notification Management | `docs/workflow/notification-management.md`, `docs/workflow/notification-management(low).md` | Notification CRUD, mark read, FCM device registration and async send. |
| 15 | Parent Monitoring & Engagement | `docs/workflow/parent-monitoring-and-engagement.md`, `docs/workflow/parent-monitoring-and-engagement(low).md` | Parent views active children, attendance, reports, progress, notifications. |
| 16 | Student Promotion | `docs/workflow/student-promotion.md`, `docs/workflow/student-promotion(low).md` | Preview and promote students across academic years/classes. |
| 17 | Analytics Dashboard | `docs/workflow/analytics-dashboard.md`, `docs/workflow/analytics-dashboard(low).md` | Read-only aggregate dashboard queries and frontend widgets. |

## Implementation Rule
Do not implement a workflow before its prerequisites are available. If blocked, implement the smallest prerequisite contract first.

## Definition Of Done
- Backend schema, database migration, resolver/service/repository code exist.
- Required authorization checks are implemented.
- Required GraphQL/REST operation tests pass.
- Frontend route, API client call, loading state, error state, and happy path UI exist.
- Soft-delete and audit rules are respected.
- Documentation names and status values match canonical docs.
