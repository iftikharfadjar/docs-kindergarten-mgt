# Backend Resolver Authorization Patterns

## Purpose
This document gives repeatable authorization patterns for GraphQL resolvers and REST handlers.

## Context Values
JWT middleware must inject:

```text
userID
role
email
```

## Helper Functions
Recommended helpers:

```text
requireAuthenticated(ctx)
requireRole(ctx, role)
requireAnyRole(ctx, roles...)
assertTeacherAssignedToClass(ctx, teacherUserId, classId)
assertParentLinkedToStudent(ctx, parentUserId, studentId)
assertStudentActive(ctx, studentId)
assertAcademicYearEditable(ctx, academicYearId)
```

## Admin-Only Resolver

```text
func Resolver(ctx, input) {
  auth := requireAuthenticated(ctx)
  requireRole(auth, ADMIN)
  return service.DoAdminAction(input)
}
```

## Teacher Assigned-Class Write Resolver

```text
func MarkAttendance(ctx, classId, records) {
  auth := requireAuthenticated(ctx)
  requireRole(auth, TEACHER)
  assertTeacherAssignedToClass(auth.userID, classId)
  return attendanceService.Mark(classId, records, auth.userID)
}
```

## Parent Child Read Resolver

```text
func GetStudentAttendance(ctx, studentId, academicYearId) {
  auth := requireAuthenticated(ctx)
  if auth.role == PARENT {
    assertParentLinkedToStudent(auth.userID, studentId)
    assertStudentActive(studentId)
  }
  return attendanceService.GetByStudent(studentId, academicYearId)
}
```

## Mixed Role Resolver

```text
func GetDailyReportsForParentOrStaff(ctx, input) {
  auth := requireAuthenticated(ctx)

  switch auth.role {
  case ADMIN:
    return service.GetReports(input)
  case TEACHER:
    return service.GetReports(input)
  case PARENT:
    assertParentLinkedToStudent(auth.userID, input.studentId)
    assertStudentActive(input.studentId)
    return service.GetParentVisibleReports(input)
  default:
    return FORBIDDEN
  }
}
```

## REST Auth Handler Pattern

```text
func Login(w, r) {
  validateInput()
  user := findActiveUserByEmail()
  verifyPassword()
  accessToken := createJWT()
  refreshToken := createSecureRandomToken()
  storeHash(refreshToken)
  setHttpOnlyCookie(refreshToken)
  return successEnvelope(accessToken, user)
}
```

## Error Rules
- Missing or invalid JWT -> `UNAUTHORIZED`.
- Valid JWT but wrong role -> `FORBIDDEN`.
- Parent accessing unlinked child -> `FORBIDDEN`.
- Teacher writing unassigned class -> `FORBIDDEN`.
- Invalid state transition -> `VALIDATION_ERROR`.
- Duplicate records -> `CONFLICT`.

## Anti-Patterns
- Do not trust frontend role checks.
- Do not authorize parent access by class alone; validate `ParentStudentLinks`.
- Do not authorize teacher write by role alone; validate `TeacherAssignments`.
- Do not return soft-deleted records in normal queries.
