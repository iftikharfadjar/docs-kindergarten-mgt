# AI Code Review Checklist

## Purpose
Use this checklist when reviewing AI-generated implementation for this project.

## Documentation Alignment
- Does the implementation reference the correct workflow docs?
- Does it follow `docs/global.md`?
- Does it follow the implementation priority dependency order?
- Does it use canonical status values?
- Does it avoid `APPROVED` as a student status?

## API Standards
- Are auth/media/device registration implemented as REST only?
- Are relational CRUD operations implemented as GraphQL?
- Does each relational domain include the required 7 CRUD operations?
- Do pagination queries return standard pagination metadata?
- Do REST responses use the standard envelope?
- Do GraphQL errors use `extensions.code`?

## Authorization
- Are backend resolver guards implemented?
- Are Admin-only operations restricted to `ADMIN`?
- Are Teacher writes validated through `TeacherAssignments`?
- Are Parent reads validated through `ParentStudentLinks` and `Students.status = ACTIVE`?
- Does the code avoid trusting frontend-only role checks?
- Are soft-deleted users blocked from auth?

## Data Integrity
- Are delete operations soft deletes?
- Are historical records preserved during transfer, promotion, and skill deletion?
- Are class capacity limits enforced on backend?
- Are duplicate active records prevented where required?
- Are academic-year scopes enforced?
- Are semester scopes enforced?

## Security
- Are passwords hashed?
- Are refresh tokens hashed at rest?
- Are password reset tokens hashed, expiring, and single-use?
- Are access tokens kept out of localStorage?
- Are auth endpoints rate-limited?
- Are upload MIME type and file size validated?
- Are MinIO object names safe?

## Frontend
- Are routes protected by role?
- Are feature components using centralized API clients?
- Is TanStack Query used for server state?
- Is TanStack Store used for global UI/session state?
- Is local state used only for forms/temporary UI?
- Are loading, error, empty, and success states present?
- Is mobile behavior considered for parent pages?

## Testing
- Are happy paths tested?
- Are unauthorized and forbidden cases tested?
- Are validation errors tested?
- Are soft-deleted records hidden from normal queries?
- Are pagination metadata tests included?
- Are workflow acceptance scenarios covered?

## Red Flags
- Raw SQL `DELETE` on core tables.
- Parent access by class only without `ParentStudentLinks`.
- Teacher write access by role only without `TeacherAssignments`.
- Student status `APPROVED`.
- Direct `fetch` calls inside frontend feature components.
- Access token in localStorage.
- Media upload through GraphQL.
- FCM push blocking the main mutation response.
- Parent FCM push for attendance or daily reports in MVP.
