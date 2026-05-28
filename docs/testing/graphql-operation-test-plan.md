# GraphQL Operation Test Plan

## Purpose
Every GraphQL operation must be tested before it is considered complete.

## Required Test Cases Per Operation

| Test Case | Required For |
|---|---|
| Happy path | All queries/mutations |
| Unauthorized request | All protected operations |
| Forbidden role | Role-scoped operations |
| Validation error | Mutations |
| Not found | Get by ID, update, delete |
| Soft-deleted hidden | Queries |
| Duplicate/conflict | Create operations with uniqueness |
| Pagination metadata | Pagination queries |

## CRUD Domain Checklist
For each domain, test:

```text
create
update
delete by id
delete multiple ids
get by id
get all
get pagination
```

## Authorization Tests

Admin:
- Can perform Admin-only operations.
- Can read all scoped academic data.

Teacher:
- Can read allowed academic data.
- Can write only assigned classes.
- Cannot write unassigned class records.

Parent:
- Can read linked `ACTIVE` child data.
- Cannot read unlinked child data.
- Cannot mutate academic records.

## State Tests
- Student enrollment rejects `PENDING`, `REJECTED`, and `ARCHIVED`.
- Parent monitoring hides non-`ACTIVE` children.
- Academic year status transition cannot skip states.
- Semester report parent visibility requires `PUBLISHED`.

## Pagination Tests
Verify:
- `page`
- `limit`
- `totalItems`
- `totalPages`
- `hasNextPage`
- `hasPreviousPage`
- empty results

## Error Assertion
All GraphQL errors must include:

```json
{
  "extensions": {
    "code": "ERROR_CODE"
  }
}
```
