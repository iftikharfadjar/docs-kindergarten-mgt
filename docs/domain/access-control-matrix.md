# Access Control Matrix

## Purpose
This document defines role permissions for backend resolvers and frontend route guards.

## Global Role Rules

| Role | Scope |
|---|---|
| `ADMIN` | Full system access. |
| `TEACHER` | Read broad academic data; write only assigned classes where applicable. |
| `PARENT` | Read-only access to linked `ACTIVE` children. |

## Entity Permissions

| Area | Admin | Teacher | Parent |
|---|---|---|---|
| Auth login/logout/refresh | Yes | Yes | Yes |
| User management | Full | No | No |
| Academic years | Full | Read active/current | Read child history only |
| Semesters | Full | Read active/current | Read through child reports |
| Classes | Full | Read all, write none | Read own child class only |
| Teacher assignments | Full | Read own assignments through `myAssignedClasses` | No |
| Students | Full | Read enrolled/visible students | Read linked `ACTIVE` children |
| Parent registration review | Full | No | Submit own child, resubmit rejected |
| Student enrollments | Full | Read class enrollments | Read own child enrollment |
| Skill categories/skills | Full | Limited if school permits | Read through progress only |
| Attendance | Full | Write assigned classes, read all | Read linked child only |
| Assessments | Read all | Write assigned classes, read all | Read linked child only |
| Daily reports | Full | Write assigned classes, read all | Read linked child class reports |
| Semester reports | Full | Write assigned classes, read all | Read published linked child reports |
| Media assets | Full | Upload/attach assigned workflow media | Read linked child report media |
| Notifications | Full/Admin monitoring | Own notifications | Own notifications |
| Analytics | Full | Limited assigned-class analytics if enabled | No |
| Promotion | Full | No | No |

## Resolver Guard Patterns

Admin-only:

```text
requireRole(ctx, ADMIN)
```

Teacher assigned-class write:

```text
requireRole(ctx, TEACHER)
assertTeacherAssignedToClass(ctx.userID, classId)
```

Parent child-scope read:

```text
requireRole(ctx, PARENT)
assertParentLinkedToStudent(ctx.userID, studentId)
assertStudentStatus(studentId, ACTIVE)
```

Mixed role read:

```text
if role == ADMIN: allow
if role == TEACHER: apply teacher policy
if role == PARENT: validate parent-child link
```

## Frontend Route Guards

| Route Prefix | Allowed Roles |
|---|---|
| `/admin/*` | `ADMIN` |
| `/teacher/*` | `TEACHER` |
| `/parent/*` | `PARENT` |
| `/login` | Public |
| `/forgot-password` | Public |
| `/reset-password` | Public |
| `/register-parent` | Public |

## Forbidden Behavior
- Parent must never access another parent's child.
- Teacher must never write attendance/report/assessment for unassigned class.
- Soft-deleted users must not authenticate.
- Archived students must not be enrolled or monitored as active children.
