# Frontend State Management Contract

## Purpose
This document defines what state belongs in TanStack Query, TanStack Store, and local component/form state.

## TanStack Store
Use for global UI/session state:

| Store State | Description |
|---|---|
| `auth.user` | Logged-in user identity and role. |
| `auth.accessToken` | In-memory access token only. |
| `theme` | UI theme preference. |
| `activeAcademicYearId` | Selected or current academic year. |
| `activeSemesterId` | Current semester returned by backend. |
| `selectedClassId` | Teacher selected class. |
| `selectedChildId` | Parent selected child. |
| `notificationUnreadCount` | Optional derived count/cache. |

## TanStack Query
Use for server state:

| Data | Query |
|---|---|
| Users | `getUsers`, user CRUD invalidation |
| Academic years | `getAcademicYears`, `getAcademicYear` |
| Semesters | `getSemestersAll`, `getActiveSemester` |
| Classes | `getClassesPagination`, `myAssignedClasses` |
| Curriculum | `getSkillCategories` |
| Students | student lists and registration lists |
| Enrollments | enrollment queries |
| Attendance | attendance queries |
| Assessments | assessment queries |
| Daily reports | report feed/list queries |
| Semester reports | report queries |
| Notifications | notification list/pagination |
| Analytics | dashboard query results |

## Local State
Use for temporary UI and form state:
- Modal open/closed state.
- Drawer open/closed state.
- Form draft values.
- Unsaved attendance row edits.
- File upload progress before server confirmation.
- Selected table rows for bulk operations.

## Form Rules
- Use TanStack Form for forms.
- Use Zod for validation.
- Do not store form input fields in global store unless needed across pages.
- Save attendance drafts locally if offline-friendly behavior is implemented.

## API Client Rules
- Feature components must not call `fetch` directly.
- Use centralized REST client for auth/media/device registration.
- Use centralized GraphQL client for CRUD and relational data.
- GraphQL client handles access token injection and refresh-once retry.

## Cache Invalidation Rules
After mutations, invalidate only relevant queries:
- User mutation -> users list/detail.
- Academic year mutation -> academic year list/detail.
- Class mutation -> classes for academic year.
- Teacher assignment mutation -> assignments and `myAssignedClasses`.
- Enrollment mutation -> enrollments, class enrolled counts, parent children.
- Attendance mutation -> attendance and analytics.
- Assessment mutation -> assessments and progress/semester report live data.
- Daily report mutation -> daily reports, notifications, parent feed.
- Notification read mutation -> notifications and unread count.
