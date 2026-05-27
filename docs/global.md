# Global Rules

- When I say "BE", it refers to folder `be-kindergarten-mgmt` for backend services
- When I say "FE", it refers to folder `fe-kindergarten-mgmt` for frontend services

---

# Global Backend Rules

- **API Paradigm:** Hybrid — GraphQL for all relational CRUD, REST for auth and media upload only.
- **ID Format:** UUID v4 (e.g., `550e8400-e29b-41d4-a716-446655440000`)
- EVERY GraphQL query or mutation creation SHOULD HAVE been tested without finding errors.
- Every Domain SHOULD HAVE queries and mutations for:
  1. Create (Mutation)
  2. Update (Mutation)
  3. Delete by Id — Soft Delete (Mutation)
  4. Delete Multiple Ids — Soft Delete (Mutation)
  5. Get One by Id (Query)
  6. Get List All (Query)
  7. Get List Pagination (Query)

---

## REST Response Envelope (Auth & Media Only)

REST is used exclusively for authentication and media upload endpoints. All REST responses follow this envelope:

**Success Response:**

```json
{
  "status": "success",
  "data": {
    "accessToken": "eyJhbGciOiJIUzI1...",
    "user": {
      "id": "550e8400-e29b-41d4-a716-446655440000",
      "role": "TEACHER"
    }
  }
}
```

**Error Response:**

```json
{
  "status": "error",
  "error": {
    "code": "UNAUTHORIZED",
    "message": "Invalid email or password"
  }
}
```

---

## GraphQL Error Response Pattern

GraphQL errors follow the standard GraphQL error format:

```json
{
  "data": null,
  "errors": [
    {
      "message": "Teacher not found",
      "path": ["getTeacherById"],
      "extensions": {
        "code": "NOT_FOUND"
      }
    }
  ]
}
```

Common error codes in `extensions.code`:

| Code | Description |
|------|-------------|
| `UNAUTHORIZED` | Missing or invalid JWT token |
| `FORBIDDEN` | Valid token but insufficient role/scope |
| `NOT_FOUND` | Resource does not exist |
| `VALIDATION_ERROR` | Input validation failure |
| `CONFLICT` | Duplicate or conflicting data |
| `INTERNAL_ERROR` | Unexpected server error |

---

## Example: Teacher Full CRUD (GraphQL)

Below is a complete example of the 7 required operations for the Teacher (User + Profile) domain. All other domains must follow this same pattern.

---

### **1. Create Teacher (Mutation)**

```graphql
mutation CreateTeacher($input: CreateTeacherInput!) {
  createTeacher(input: $input) {
    id
    email
    role
    profile {
      firstName
      lastName
      phone
      gender
      birthPlace
      birthDate
      address
      religion
      nationality
      maritalStatus
      employeeNumber
      joinDate
      position
      educationLevel
      major
      status
      photoUrl
    }
    createdAt
    updatedAt
  }
}
```

**Variables:**

```json
{
  "input": {
    "email": "john.doe@example.com",
    "password": "securePassword123",
    "firstName": "John",
    "lastName": "Doe",
    "phone": "+6281234567890",
    "gender": "male",
    "birthPlace": "Bandung",
    "birthDate": "1990-05-10",
    "address": "Jl. Merdeka No. 10, Bandung",
    "religion": "Islam",
    "nationality": "Indonesia",
    "maritalStatus": "married",
    "employeeNumber": "TCH-2026-0001",
    "joinDate": "2026-05-24",
    "position": "Homeroom Teacher",
    "educationLevel": "Bachelor",
    "major": "Early Childhood Education",
    "status": "active",
    "photoUrl": "https://cdn.example.com/teachers/john-doe.jpg"
  }
}
```

**Success Response:**

```json
{
  "data": {
    "createTeacher": {
      "id": "550e8400-e29b-41d4-a716-446655440000",
      "email": "john.doe@example.com",
      "role": "TEACHER",
      "profile": {
        "firstName": "John",
        "lastName": "Doe",
        "phone": "+6281234567890",
        "gender": "male",
        "birthPlace": "Bandung",
        "birthDate": "1990-05-10",
        "address": "Jl. Merdeka No. 10, Bandung",
        "religion": "Islam",
        "nationality": "Indonesia",
        "maritalStatus": "married",
        "employeeNumber": "TCH-2026-0001",
        "joinDate": "2026-05-24",
        "position": "Homeroom Teacher",
        "educationLevel": "Bachelor",
        "major": "Early Childhood Education",
        "status": "active",
        "photoUrl": "https://cdn.example.com/teachers/john-doe.jpg"
      },
      "createdAt": "2026-05-24T08:00:00Z",
      "updatedAt": "2026-05-24T08:00:00Z"
    }
  }
}
```

---

### **2. Update Teacher (Mutation)**

```graphql
mutation UpdateTeacher($userId: ID!, $input: UpdateTeacherInput!) {
  updateTeacher(userId: $userId, input: $input) {
    id
    profile {
      firstName
      lastName
      phone
      address
      position
      status
      photoUrl
    }
    updatedAt
  }
}
```

**Variables:**

```json
{
  "userId": "550e8400-e29b-41d4-a716-446655440000",
  "input": {
    "firstName": "John Updated",
    "phone": "+6289876543210",
    "address": "Jl. Asia Afrika No. 20, Bandung",
    "position": "Senior Teacher",
    "status": "active",
    "photoUrl": "https://cdn.example.com/teachers/john-doe-updated.jpg"
  }
}
```

**Success Response:**

```json
{
  "data": {
    "updateTeacher": {
      "id": "550e8400-e29b-41d4-a716-446655440000",
      "profile": {
        "firstName": "John Updated",
        "lastName": "Doe",
        "phone": "+6289876543210",
        "address": "Jl. Asia Afrika No. 20, Bandung",
        "position": "Senior Teacher",
        "status": "active",
        "photoUrl": "https://cdn.example.com/teachers/john-doe-updated.jpg"
      },
      "updatedAt": "2026-05-24T09:00:00Z"
    }
  }
}
```

---

### **3. Delete Teacher By Id (Mutation — Soft Delete)**

```graphql
mutation SoftDeleteTeacher($userId: ID!) {
  softDeleteTeacher(userId: $userId) {
    success
    message
  }
}
```

**Variables:**

```json
{
  "userId": "550e8400-e29b-41d4-a716-446655440000"
}
```

**Success Response:**

```json
{
  "data": {
    "softDeleteTeacher": {
      "success": true,
      "message": "Teacher deleted successfully"
    }
  }
}
```

---

### **4. Delete Multiple Teachers (Mutation — Soft Delete)**

```graphql
mutation SoftDeleteTeachers($userIds: [ID!]!) {
  softDeleteTeachers(userIds: $userIds) {
    success
    message
    deletedCount
  }
}
```

**Variables:**

```json
{
  "userIds": [
    "550e8400-e29b-41d4-a716-446655440000",
    "7c9e6679-7425-40de-944b-e07fc1f90ae7",
    "f47ac10b-58cc-4372-a567-0e02b2c3d479"
  ]
}
```

**Success Response:**

```json
{
  "data": {
    "softDeleteTeachers": {
      "success": true,
      "message": "Teachers deleted successfully",
      "deletedCount": 3
    }
  }
}
```

---

### **5. Get Teacher By Id (Query)**

```graphql
query GetTeacherById($userId: ID!) {
  getTeacherById(userId: $userId) {
    id
    email
    role
    profile {
      firstName
      lastName
      phone
      gender
      birthPlace
      birthDate
      address
      religion
      nationality
      maritalStatus
      employeeNumber
      joinDate
      position
      educationLevel
      major
      status
      photoUrl
    }
    createdAt
    updatedAt
  }
}
```

**Variables:**

```json
{
  "userId": "550e8400-e29b-41d4-a716-446655440000"
}
```

**Success Response:**

```json
{
  "data": {
    "getTeacherById": {
      "id": "550e8400-e29b-41d4-a716-446655440000",
      "email": "john.doe@example.com",
      "role": "TEACHER",
      "profile": {
        "firstName": "John",
        "lastName": "Doe",
        "phone": "+6281234567890",
        "gender": "male",
        "birthPlace": "Bandung",
        "birthDate": "1990-05-10",
        "address": "Jl. Merdeka No. 10, Bandung",
        "religion": "Islam",
        "nationality": "Indonesia",
        "maritalStatus": "married",
        "employeeNumber": "TCH-2026-0001",
        "joinDate": "2026-05-24",
        "position": "Homeroom Teacher",
        "educationLevel": "Bachelor",
        "major": "Early Childhood Education",
        "status": "active",
        "photoUrl": "https://cdn.example.com/teachers/john-doe.jpg"
      },
      "createdAt": "2026-05-24T08:00:00Z",
      "updatedAt": "2026-05-24T08:00:00Z"
    }
  }
}
```

---

### **6. Get List Teachers All (Query)**

```graphql
query GetTeachersAll {
  getTeachersAll {
    id
    email
    profile {
      firstName
      lastName
      phone
      position
      status
      photoUrl
    }
  }
}
```

**Success Response:**

```json
{
  "data": {
    "getTeachersAll": [
      {
        "id": "550e8400-e29b-41d4-a716-446655440000",
        "email": "john.doe@example.com",
        "profile": {
          "firstName": "John",
          "lastName": "Doe",
          "phone": "+6281234567890",
          "position": "Homeroom Teacher",
          "status": "active",
          "photoUrl": "https://cdn.example.com/teachers/john-doe.jpg"
        }
      },
      {
        "id": "7c9e6679-7425-40de-944b-e07fc1f90ae7",
        "email": "jane.smith@example.com",
        "profile": {
          "firstName": "Jane",
          "lastName": "Smith",
          "phone": "+6281299988877",
          "position": "Assistant Teacher",
          "status": "active",
          "photoUrl": "https://cdn.example.com/teachers/jane-smith.jpg"
        }
      }
    ]
  }
}
```

---

### **7. Get List Teachers Pagination (Query)**

```graphql
query GetTeachersPagination($page: Int!, $limit: Int!, $search: String) {
  getTeachersPagination(page: $page, limit: $limit, search: $search) {
    items {
      id
      email
      profile {
        firstName
        lastName
        phone
        position
        status
        photoUrl
      }
    }
    pagination {
      page
      limit
      totalItems
      totalPages
      hasNextPage
      hasPreviousPage
    }
  }
}
```

**Variables:**

```json
{
  "page": 1,
  "limit": 10,
  "search": "John"
}
```

**Success Response:**

```json
{
  "data": {
    "getTeachersPagination": {
      "items": [
        {
          "id": "550e8400-e29b-41d4-a716-446655440000",
          "email": "john.doe@example.com",
          "profile": {
            "firstName": "John",
            "lastName": "Doe",
            "phone": "+6281234567890",
            "position": "Homeroom Teacher",
            "status": "active",
            "photoUrl": "https://cdn.example.com/teachers/john-doe.jpg"
          }
        },
        {
          "id": "7c9e6679-7425-40de-944b-e07fc1f90ae7",
          "email": "jane.smith@example.com",
          "profile": {
            "firstName": "Jane",
            "lastName": "Smith",
            "phone": "+6281299988877",
            "position": "Assistant Teacher",
            "status": "active",
            "photoUrl": "https://cdn.example.com/teachers/jane-smith.jpg"
          }
        }
      ],
      "pagination": {
        "page": 1,
        "limit": 10,
        "totalItems": 25,
        "totalPages": 3,
        "hasNextPage": true,
        "hasPreviousPage": false
      }
    }
  }
}
```

---

# Global Frontend Rules

## State Management

- TanStack Query for API state (server state)
- TanStack Store for global UI state (auth, theme, active academic year, selected class)
- Local state for forms only

## Components

- Reusable first
- Use components from Solid UI / Kobalte first
  - Accessibility required
- Mobile-first

## API

- Never call fetch directly
- Use centralized API client (GraphQL client for CRUD, REST client for auth/media)
