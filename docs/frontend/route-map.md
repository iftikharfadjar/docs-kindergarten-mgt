# Frontend Route Map

## Purpose
This document defines routes, allowed roles, and expected page responsibilities.

## Public Routes

| Route | Page | Notes |
|---|---|---|
| `/login` | Login page | Calls REST login. |
| `/forgot-password` | Forgot password page | Calls REST forgot password. |
| `/reset-password` | Reset password page | Reads token from URL. |
| `/register-parent` | Parent self-service registration | Creates parent account and child registration. |

## Admin Routes

| Route | Page | Main Data |
|---|---|---|
| `/admin/dashboard` | Admin dashboard | `getDashboardSummary` |
| `/admin/users` | User management | `getUsers`, user CRUD |
| `/admin/academic-years` | Academic year list | academic year CRUD |
| `/admin/academic-years/:id` | Academic year detail | complete year data |
| `/admin/academic-years/:id/classes` | Class management | class CRUD |
| `/admin/academic-years/:id/curriculum` | Curriculum config | skill categories/skills |
| `/admin/academic-years/:id/semesters` | Semester management | semesters and active semester |
| `/admin/teacher-assignments` | Teacher assignments | assignment CRUD |
| `/admin/students` | Student management | student list/profile |
| `/admin/students/registrations` | Registration review | pending/rejected child registrations |
| `/admin/students/enrollments` | Enrollment/transfer | enrollments and transfer |
| `/admin/students/promotion` | Promotion | promotion preview and execute |
| `/admin/media` | Media audit | media list, optional |
| `/admin/notifications` | Notification monitoring | admin notification list |
| `/admin/analytics` | Analytics dashboard | analytics queries |

## Teacher Routes

| Route | Page | Main Data |
|---|---|---|
| `/teacher/dashboard` | Teacher dashboard | `myAssignedClasses` |
| `/teacher/classes` | Assigned classes | `myAssignedClasses` |
| `/teacher/attendance` | Attendance | selected class, active semester |
| `/teacher/assessments` | Assessments | selected class, curriculum, active semester |
| `/teacher/daily-reports` | Daily reports | selected class, media upload |
| `/teacher/semester-reports` | Semester reports | selected class, live report data |
| `/teacher/notifications` | Notifications | own notifications |

## Parent Routes

| Route | Page | Main Data |
|---|---|---|
| `/parent/dashboard` | Parent dashboard | `getParentChildren`, latest reports |
| `/parent/attendance` | Attendance monitoring | child attendance |
| `/parent/progress` | Skill progress | child assessments |
| `/parent/daily-reports` | Daily report feed | parent-visible daily reports |
| `/parent/reports` | Semester reports | published reports only |
| `/parent/history` | Historical academic records | child records by academic year |
| `/parent/notifications` | Notifications | own notifications |

## Route Guard Rules
- Public routes redirect authenticated users to their role dashboard when appropriate.
- Admin routes require `ADMIN`.
- Teacher routes require `TEACHER`.
- Parent routes require `PARENT`.
- Unauthorized users redirect to `/login`.
- Authenticated users with wrong role redirect to their own dashboard or a forbidden page.
