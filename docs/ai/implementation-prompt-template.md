# AI Implementation Prompt Template

## Purpose
Use this template when asking an AI agent to implement one workflow. Replace bracketed values before use.

## Template

```text
You are an AI Native Engineer implementing the Kindergarten School Management Application.

Implement this workflow:
[WORKFLOW_NAME]

Primary documentation:
- [NORMAL_WORKFLOW_DOC_PATH]
- [LOW_WORKFLOW_DOC_PATH]

Global documentation to follow:
- docs/global.md
- docs/implementation-roadmap.md
- docs/api/graphql-crud-standard.md
- docs/api/rest-standard.md
- docs/domain/status-lifecycle.md
- docs/domain/access-control-matrix.md
- docs/domain/entity-relationship-contract.md
- docs/backend/resolver-authorization-patterns.md
- docs/frontend/route-map.md
- docs/frontend/state-management-contract.md
- docs/errors/error-code-catalog.md
- docs/testing/graphql-operation-test-plan.md

Implementation requirements:
1. Read the workflow docs first.
2. Identify backend schema, migration, service, resolver, and test changes.
3. Identify frontend route, component, API client, form, and state changes.
4. Preserve existing patterns in the repository.
5. Implement the smallest complete vertical slice for this workflow.
6. Enforce role-based access control on the backend.
7. Do not trust frontend authorization.
8. Use soft delete for delete operations.
9. Implement all required 7 GraphQL CRUD operations for relational domains.
10. Use REST only where documented: auth, media upload, device registration.
11. Use canonical status values from docs/domain/status-lifecycle.md.
12. Add or update tests.
13. Run relevant tests and report results.

Workflow-specific acceptance criteria:
[PASTE_ACCEPTANCE_CRITERIA]

Return:
- Summary of implemented changes.
- Files changed.
- Tests run and results.
- Any known gaps or blocked items.
```

## Example

```text
Implement this workflow:
Teacher Assignment

Primary documentation:
- docs/workflow/teacher-assignment.md
- docs/workflow/teacher-assignment(low).md

Workflow-specific acceptance criteria:
- Admin can assign teacher to class.
- Duplicate active teacher/class assignment is rejected.
- Teacher can call myAssignedClasses.
- Teacher write checks use TeacherAssignments.
```
