# Authentication & Security Workflow (AI-Optimized)

## 1. Context & Business Rules (Explicit Constraints)
- **Constraint 1 (REST Only For Auth):** Login, refresh, logout, forgot password, and reset password MUST be REST endpoints. Do NOT implement these as GraphQL mutations.
- **Constraint 2 (GraphQL For App Data):** After login, relational CRUD data still uses GraphQL with `Authorization: Bearer <accessToken>`.
- **Constraint 3 (Access Token Storage):** Frontend MUST store the access token in memory, not localStorage.
- **Constraint 4 (Refresh Token Cookie):** Refresh token MUST be stored in an `HttpOnly`, `Secure`, `SameSite=Strict` cookie.
- **Constraint 5 (Refresh Token Hash):** Backend MUST store only a hash of the refresh token in `Users.refresh_token_hash`. Do not store raw refresh tokens.
- **Constraint 6 (Password Hashing):** User passwords MUST be hashed with `bcrypt` or `Argon2`. Do not store plain passwords.
- **Constraint 7 (Generic Login Error):** Login failure must return a generic message. Do not reveal whether email or password was wrong.
- **Constraint 8 (Generic Forgot Password Response):** Forgot password response must be the same whether the email exists or does not exist.
- **Constraint 9 (Password Reset Token Hash):** Password reset tokens MUST be stored hashed, expire quickly, and be single-use.
- **Constraint 10 (Reset Invalidates Sessions):** Successful password reset MUST revoke existing refresh tokens.
- **Constraint 11 (Logout Clears Both Sides):** Logout MUST clear backend refresh token hash and clear browser refresh cookie.
- **Constraint 12 (Soft Deleted Users):** Soft-deleted users where `deleted_at IS NOT NULL` cannot login, refresh, or reset password.
- **Constraint 13 (RBAC Context):** GraphQL middleware MUST inject `userID`, `role`, and `email` into request context after validating JWT.
- **Constraint 14 (CSRF Protection):** Cookie-based refresh/logout endpoints MUST be protected against CSRF through SameSite cookie policy and, if needed, CSRF token/origin checks.
- **Constraint 15 (Rate Limit):** Login, refresh, forgot password, and reset password endpoints MUST be rate-limited.

## 2. Exact Data Contracts (REST & GraphQL Auth Context)

### A. Login
**Request (REST):**
```http
POST /api/v1/auth/login
Content-Type: application/json

{
  "email": "teacher@school.com",
  "password": "securePassword123"
}
```

**Success Response:**
```json
{
  "status": "success",
  "data": {
    "accessToken": "jwt-access-token",
    "user": {
      "id": "uuid-user",
      "email": "teacher@school.com",
      "role": "TEACHER",
      "profile": {
        "firstName": "Jane",
        "lastName": "Doe"
      }
    }
  }
}
```

**Cookie Side Effect:**
```text
Set-Cookie: refresh_token=<raw-refresh-token>; HttpOnly; Secure; SameSite=Strict; Path=/api/v1/auth; Max-Age=<configured>
```

**Required Backend Behavior:**
```text
1. Find user by email where deleted_at IS NULL.
2. Verify password against password_hash.
3. Generate short-lived access token.
4. Generate refresh token.
5. Store hash(refresh token) in Users.refresh_token_hash.
6. Set refresh token cookie.
7. Return access token and user role/profile.
```

### B. Refresh Access Token
**Request (REST):**
```http
POST /api/v1/auth/refresh
Cookie: refresh_token=<raw-refresh-token>
```

**Success Response:**
```json
{
  "status": "success",
  "data": {
    "accessToken": "new-jwt-access-token"
  }
}
```

**Required Backend Behavior:**
```text
1. Read refresh_token from HttpOnly cookie.
2. Hash incoming refresh token.
3. Find active user with matching Users.refresh_token_hash.
4. Reject if user is soft deleted.
5. Issue new short-lived access token.
6. Optionally rotate refresh token and update refresh_token_hash.
7. Return new access token.
```

### C. Logout
**Request (REST):**
```http
POST /api/v1/auth/logout
Cookie: refresh_token=<raw-refresh-token>
Authorization: Bearer <accessToken>
```

**Success Response:**
```json
{
  "status": "success",
  "data": null
}
```

**Required Backend Behavior:**
```text
1. Identify user from access token or refresh token.
2. Clear Users.refresh_token_hash.
3. Clear refresh token cookie by setting expired Set-Cookie.
4. Return success even if token is already missing or expired.
```

### D. Forgot Password
**Request (REST):**
```http
POST /api/v1/auth/forgot-password
Content-Type: application/json

{
  "email": "parent@email.com"
}
```

**Success Response:**
```json
{
  "status": "success",
  "data": null
}
```

**Required Backend Behavior:**
```text
1. Always return generic success response.
2. If active user exists:
   - create random reset token.
   - store hash(reset token), expires_at, used=false.
   - send reset link by email.
3. If user does not exist:
   - do not reveal that fact.
```

### E. Reset Password
**Request (REST):**
```http
POST /api/v1/auth/reset-password
Content-Type: application/json

{
  "token": "raw-reset-token-from-email",
  "newPassword": "NewSecurePassword123!"
}
```

**Success Response:**
```json
{
  "status": "success",
  "data": null
}
```

**Required Backend Behavior:**
```text
1. Hash incoming reset token.
2. Find PasswordResetTokens row where token_hash matches, used=false, expires_at > now.
3. Validate new password strength.
4. Hash new password.
5. Update Users.password_hash.
6. Clear Users.refresh_token_hash.
7. Mark reset token used.
8. Return success.
```

### F. GraphQL Auth Middleware
**Request Pattern:**
```http
POST /query
Authorization: Bearer <accessToken>
Content-Type: application/json
```

**Required Middleware Behavior:**
```text
1. Read Authorization header.
2. Validate "Bearer " prefix.
3. Verify JWT signature and expiry.
4. Load or validate user is active.
5. Inject into context:
   - userID
   - role
   - email
6. If invalid, return 401 Unauthorized before resolver runs.
```

## 3. UI to Data Mapping

| UI Element (Screen) | REST / GraphQL Data Source | Action / Trigger |
| ------------------- | -------------------------- | ---------------- |
| **Email Input** | `login.email` | Sent to `POST /api/v1/auth/login` |
| **Password Input** | `login.password` | Sent to `POST /api/v1/auth/login` |
| **Login Button** | N/A | Calls login endpoint |
| **Role Redirect** | `login.data.user.role` | Routes user to Admin, Teacher, or Parent dashboard |
| **Access Token Store** | `login.data.accessToken` | Store in memory auth store |
| **GraphQL Authorization Header** | in-memory `accessToken` | Added by centralized GraphQL client |
| **Refresh Request** | HttpOnly cookie | Calls `POST /api/v1/auth/refresh` after 401/UNAUTHORIZED |
| **Forgot Password Email Input** | `forgotPassword.email` | Sent to forgot-password endpoint |
| **Reset Token** | URL query token | Sent to reset-password endpoint |
| **New Password Input** | `resetPassword.newPassword` | Sent to reset-password endpoint |
| **Logout Button** | Current session | Calls logout endpoint, clears memory auth state |

## 4. API Sequence Diagram

```mermaid
sequenceDiagram
    actor User
    participant UI as Frontend Components
    participant REST as REST Client
    participant GQL as GraphQL Client
    participant API as Backend System
    participant Email as Email Service

    %% Login
    User->>UI: Enter email and password
    UI->>REST: POST /api/v1/auth/login
    REST->>API: Validate credentials
    API-->>REST: accessToken + Set-Cookie refresh_token
    REST-->>UI: Store accessToken in memory
    UI->>UI: Redirect based on role

    %% GraphQL with access token
    UI->>GQL: Execute app query/mutation
    GQL->>API: POST /query with Authorization Bearer token
    API->>API: Validate JWT and inject context
    API-->>GQL: Return data
    GQL-->>UI: Render protected screen

    %% Refresh after expiry
    UI->>GQL: Execute GraphQL request
    GQL->>API: POST /query with expired token
    API-->>GQL: 401 Unauthorized
    GQL-->>UI: Trigger refresh flow
    UI->>REST: POST /api/v1/auth/refresh
    REST->>API: Validate refresh cookie
    API-->>REST: Return new accessToken
    REST-->>UI: Replace in-memory accessToken
    UI->>GQL: Retry original request once

    %% Forgot and reset password
    User->>UI: Submit forgot password email
    UI->>REST: POST /api/v1/auth/forgot-password
    REST->>API: Create reset token if user exists
    API->>Email: Send reset email
    API-->>REST: Generic success

    User->>UI: Submit new password with reset token
    UI->>REST: POST /api/v1/auth/reset-password
    REST->>API: Validate token, update password, revoke refresh token
    API-->>REST: Success
    REST-->>UI: Redirect to login

    %% Logout
    User->>UI: Click Logout
    UI->>REST: POST /api/v1/auth/logout
    REST->>API: Clear refresh_token_hash and cookie
    API-->>REST: Success
    REST-->>UI: Clear accessToken and redirect to login
```

## 5. UI/UX Screen Flow & Component Wireframe

### Components to Build:
1. `<LoginPage />` - Public route for login.
2. `<LoginForm />` - TanStack Form + Zod form for email and password.
3. `<ForgotPasswordPage />` - Public route for requesting reset email.
4. `<ForgotPasswordForm />` - Sends email to forgot-password endpoint.
5. `<ResetPasswordPage />` - Public route that reads token from URL.
6. `<ResetPasswordForm />` - Sends reset token and new password.
7. `<AuthProvider />` - Holds in-memory access token and user session.
8. `<ProtectedRoute />` - Guards routes by authentication and role.
9. `<LogoutButton />` - Calls logout endpoint and clears frontend state.
10. `graphqlClient` - Centralized GraphQL client that attaches access token and handles refresh-once retry.
11. `restClient` - Centralized REST client for auth endpoints.

### Component Wireframe Representation:

```text
=============================================================================
[<LoginPage /> component]                                Public Route
=============================================================================
[<LoginForm /> component]

Email:
[ user@school.com                                      ]

Password:
[ ********                                             ]

Button: [Login]

Link: Forgot password?

Error area:
Invalid email or password
=============================================================================
```

```text
=============================================================================
[<ForgotPasswordPage /> component]                      Public Route
=============================================================================
[<ForgotPasswordForm /> component]

Email:
[ user@school.com                                      ]

Button: [Send Reset Instructions]

Success text:
If an account exists, password reset instructions have been sent.
=============================================================================
```

```text
=============================================================================
[<ResetPasswordPage /> component]                       Public Route
=============================================================================
[<ResetPasswordForm /> component]

New Password:
[ ********                                             ]

Confirm Password:
[ ********                                             ]

Button: [Reset Password]

After success:
Redirect to /login
=============================================================================
```

```text
=============================================================================
[Auth Runtime Behavior]
=============================================================================
Login success:
  authStore.accessToken = response.data.accessToken
  authStore.user = response.data.user
  redirect by role

GraphQL request:
  add Authorization: Bearer authStore.accessToken

If 401/UNAUTHORIZED:
  call POST /api/v1/auth/refresh
  if success: retry original request once
  if failure: clear authStore and redirect /login

Logout:
  call POST /api/v1/auth/logout
  clear authStore
  redirect /login
=============================================================================
```

## 6. AI Execution Checklist

Use this checklist when implementing the workflow:

```text
1. Implement REST endpoints:
   POST /api/v1/auth/login
   POST /api/v1/auth/refresh
   POST /api/v1/auth/logout
   POST /api/v1/auth/forgot-password
   POST /api/v1/auth/reset-password

2. Do not implement auth endpoints as GraphQL mutations.

3. Login endpoint:
   - find active user by email
   - verify password hash
   - generate access token
   - generate refresh token
   - store hash(refresh token)
   - set HttpOnly Secure SameSite=Strict cookie
   - return access token and user role/profile

4. Refresh endpoint:
   - read refresh cookie
   - validate token hash
   - reject soft-deleted users
   - return new access token
   - optionally rotate refresh token

5. Logout endpoint:
   - clear refresh_token_hash
   - expire refresh cookie
   - frontend clears memory token

6. Forgot password endpoint:
   - always return generic success
   - if user exists, create hashed reset token
   - send email with raw reset token link

7. Reset password endpoint:
   - validate hashed reset token
   - verify token not expired and not used
   - validate password strength
   - hash new password
   - revoke refresh token
   - mark reset token used

8. GraphQL middleware:
   - validate Authorization Bearer token
   - inject userID, role, email
   - reject invalid token before resolver

9. Frontend auth store:
   - keep access token in memory
   - do not use localStorage for access token
   - use TanStack Store for auth state

10. Centralized API clients:
    - REST client for auth
    - GraphQL client for app data
    - never call fetch directly from feature components

11. Refresh behavior:
    - on GraphQL 401/UNAUTHORIZED, call refresh once
    - retry original request once
    - if refresh fails, redirect to /login

12. Route guards:
    - Admin routes require ADMIN
    - Teacher routes require TEACHER
    - Parent routes require PARENT

13. Security protections:
    - rate limit login, refresh, forgot password, reset password
    - use strict CORS
    - use CSRF/origin checks for cookie endpoints if needed
    - log security events

14. Test the full path:
    Login as Admin -> access admin dashboard.
    Login as Teacher -> access teacher dashboard.
    Login as Parent -> access parent dashboard.
    Expire access token -> refresh succeeds -> request retries.
    Forgot password -> reset password -> old session revoked.
    Logout -> refresh cookie cleared -> protected route redirects to login.
```
