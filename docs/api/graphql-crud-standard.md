# GraphQL CRUD Standard

## Purpose
Every relational domain must follow one consistent GraphQL CRUD shape unless a workflow explicitly documents an additional special operation.

## Required 7 Operations Per Domain

| Operation | Required Shape | Example |
|---|---|---|
| Create | Mutation | `createClass(input: CreateClassInput!): Class!` |
| Update | Mutation | `updateClass(id: ID!, input: UpdateClassInput!): Class!` |
| Delete by ID | Mutation, soft delete | `deleteClass(id: ID!): MutationResult!` |
| Delete multiple IDs | Mutation, soft delete | `deleteClasses(ids: [ID!]!): BulkMutationResult!` |
| Get one by ID | Query | `getClassById(id: ID!): Class` |
| Get all | Query | `getClassesAll(filter...): [Class!]!` |
| Get pagination | Query | `getClassesPagination(page: Int!, limit: Int!, search: String): PaginatedClasses!` |

## Naming Rules
- Use verb + singular domain for create/update/delete one.
- Use plural domain for delete multiple and lists.
- Use `ById` suffix for single read queries.
- Prefer `id` for simple domains; keep established names like `classId`, `studentId`, `userId` when already documented.

## Pagination Type
Every pagination query must return:

```graphql
type Pagination {
  page: Int!
  limit: Int!
  totalItems: Int!
  totalPages: Int!
  hasNextPage: Boolean!
  hasPreviousPage: Boolean!
}
```

Pattern:

```graphql
type PaginatedClasses {
  items: [Class!]!
  pagination: Pagination!
}
```

## Mutation Result Types

```graphql
type MutationResult {
  success: Boolean!
  message: String!
}

type BulkMutationResult {
  success: Boolean!
  message: String!
  deletedCount: Int!
}
```

## Soft Delete Rule
Delete mutations must update `deleted_at = NOW()` for core tables. Do not physically delete rows unless the table is explicitly documented as ephemeral.

## Filtering Rule
Normal queries must exclude `deleted_at IS NOT NULL` rows unless an Admin-only audit query explicitly requests archived/deleted records.

## Error Rule
GraphQL resolvers must return standard GraphQL errors with `extensions.code`, using the catalog in `docs/errors/error-code-catalog.md`.

## Test Rule
Every GraphQL query and mutation must be tested with valid input, invalid input, unauthorized access, forbidden access, and soft-delete visibility where applicable.
