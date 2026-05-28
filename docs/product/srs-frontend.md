
# Software Requirements Specification (SRS)

# Frontend System

# Kindergarten School Management Application

Based on the backend SRS and PRD requirements. 

---

# 1. Introduction

## 1.1 Purpose

This document defines the Software Requirements Specification (SRS) for the frontend application of the Kindergarten School Management System.

The frontend application provides web interfaces for:

* Administrators
* Teachers
* Parents

The frontend system will interact with backend services through:

* GraphQL APIs
* REST APIs
* Push notification services

The frontend stack is based on:

* SolidJS
* TanStack Start
* TanStack Query
* TanStack Router
* TanStack Form
* TanStack Store
* TypeScript
* Tailwind CSS

---

# 2. Scope

The frontend application supports:

* Authentication and authorization
* Academic year management
* User management
* Teacher assignment
* Student enrollment
* Attendance management
* Skill assessments
* Daily reports
* Semester reports
* Analytics dashboards
* Parent monitoring
* Push notifications

The frontend system must support:

* Responsive web interface
* Desktop-first administration workflows
* Mobile-friendly parent dashboards
* Periodic data synchronization via polling
* Offline-friendly form handling
* Role-based access control

---

# 3. System Overview

## 3.1 User Roles

| Role    | Description                    |
| ------- | ------------------------------ |
| Admin   | Full administration access     |
| Teacher | Classroom and reporting access |
| Parent  | Child monitoring access        |

---

# 4. Frontend Architecture

## 4.1 Architectural Style

The frontend application uses:

* Component-based architecture
* Feature-based modular structure
* Reactive state management
* Server-state synchronization
* Route-based code splitting

---

## 4.2 Frontend Technology Stack

| Category              | Technology                 |
| --------------------- | -------------------------- |
| Framework             | SolidJS                    |
| Application Framework | TanStack Start             |
| Routing               | TanStack Router            |
| Server State          | TanStack Query             |
| Form Handling         | TanStack Form              |
| Global State          | TanStack Store             |
| Styling               | Tailwind CSS               |
| UI Components         | Kobalte / Solid UI         |
| Validation            | Zod                        |
| HTTP Client           | GraphQL Client + Fetch API |
| Language              | TypeScript                 |
| Build Tool            | Vite                       |
| Notifications         | Firebase Cloud Messaging   |
| File Upload           | Multipart REST API         |
| Charts                | Chart.js / ApexCharts      |

---

# 5. Frontend Application Structure

## 5.1 Folder Structure

```txt
src/
├── app/
├── routes/
├── components/
├── modules/
│   ├── auth/
│   ├── academic-years/
│   ├── classes/
│   ├── teachers/
│   ├── students/
│   ├── attendance/
│   ├── assessments/
│   ├── reports/
│   ├── notifications/
│   └── analytics/
├── services/
├── stores/
├── graphql/
├── hooks/
├── layouts/
├── lib/
├── types/
├── utils/
└── styles/
```

---

# 6. Routing Design

## 6.1 Public Routes

| Route              | Description         |
| ------------------ | ------------------- |
| `/login`           | User login          |
| `/forgot-password` | Password reset      |
| `/register-parent` | Parent registration |

---

## 6.2 Admin Routes

| Route                   | Description              |
| ----------------------- | ------------------------ |
| `/admin/dashboard`      | Admin dashboard          |
| `/admin/users`          | User management          |
| `/admin/academic-years` | Academic year management |
| `/admin/classes`        | Class management         |
| `/admin/teachers`       | Teacher management       |
| `/admin/students`       | Student management       |
| `/admin/skills`         | Skill management         |
| `/admin/analytics`      | Analytics dashboard      |

---

## 6.3 Teacher Routes

| Route                       | Description           |
| --------------------------- | --------------------- |
| `/teacher/dashboard`        | Teacher dashboard     |
| `/teacher/classes`          | Class selection (multi-class support) |
| `/teacher/attendance`       | Attendance management |
| `/teacher/assessments`      | Assessment management |
| `/teacher/daily-reports`    | Daily reports         |
| `/teacher/semester-reports` | Semester reports      |

---

## 6.4 Parent Routes

| Route                | Description           |
| -------------------- | --------------------- |
| `/parent/dashboard`  | Parent dashboard      |
| `/parent/attendance` | Attendance monitoring |
| `/parent/progress`   | Student progress      |
| `/parent/reports`    | Semester reports      |
| `/parent/history`    | Historical records    |

---

# 7. Authentication & Authorization

Requirements derived from PRD authentication module. 

## 7.1 Authentication Features

| Feature            | Description                 |
| ------------------ | --------------------------- |
| JWT Authentication | Access token authentication |
| Refresh Token      | Silent session refresh      |
| Auto Logout        | Logout on token expiration  |
| Remember Session   | Persistent login            |
| Password Reset     | Email reset workflow        |

---

## 7.2 Authorization

Frontend must implement RBAC:

| Role    | Permissions              |
| ------- | ------------------------ |
| Admin   | Full access              |
| Teacher | Read all classes, write assigned classes only |
| Parent  | Read-only child access   |

Unauthorized routes must redirect users.


## 7.3 Permission Matrix

| Feature                   | Admin | Teacher | Parent |
| ------------------------- | ----- | ------- | ------ |
| Login                     | ✅     | ✅       | ✅      |
| Logout                    | ✅     | ✅       | ✅      |
| Manage Users              | ✅     | ❌       | ❌      |
| Manage Roles              | ✅     | ❌       | ❌      |
| Manage Academic Years     | ✅     | ❌       | ❌      |
| Clone Academic Year       | ✅     | ❌       | ❌      |
| Manage Classes            | ✅     | ❌       | ❌      |
| Assign Teachers           | ✅     | ❌       | ❌      |
| Enroll Students           | ✅     | ❌       | ❌      |
| Promote Students          | ✅     | ❌       | ❌      |
| Manage Skill Categories   | ✅     | Limited | ❌      |
| Manage Skills             | ✅     | Limited | ❌      |
| Mark Attendance           | ✅     | ✅ (assigned)| ❌      |
| View Attendance           | ✅     | ✅ (all)     | ✅      |
| Create Assessments        | ❌     | ✅ (assigned)| ❌      |
| View Assessments          | ✅     | ✅ (all)     | ✅      |
| Create Daily Reports      | ❌     | ✅ (assigned)| ❌      |
| View Daily Reports        | ✅     | ✅ (all)     | ✅      |
| Upload Classroom Photos   | ❌     | ✅ (assigned)| ❌      |
| Generate Semester Reports | ❌     | ✅ (assigned)| ❌      |
| View Semester Reports     | ✅     | ✅ (all)     | ✅      |
| Export Reports            | ✅     | ✅       | ✅      |
| View Analytics Dashboard  | ✅     | Limited | ❌      |
| Receive Notifications     | ✅     | ✅       | ✅      |
| Resubmit Rejected Student | ✅     | ❌       | ❌      |
| Close Academic Year       | ✅     | ❌       | ❌      |


---

# 8. Functional Requirements

---

# 8.1 Authentication Module

| ID          | Requirement                    |
| ----------- | ------------------------------ |
| FE-AUTH-001 | User can login                 |
| FE-AUTH-002 | User can logout                |
| FE-AUTH-003 | User can refresh session       |
| FE-AUTH-004 | User can reset password        |
| FE-AUTH-005 | System stores token securely   |
| FE-AUTH-006 | System protects private routes |
| FE-AUTH-007 | Parent can register new account with child (self-service) |
| FE-AUTH-008 | System handles multi-class teacher dashboard |

---

# 8.2 Academic Year Module

Based on PRD academic year workflows. 

| ID        | Requirement                           |
| --------- | ------------------------------------- |
| FE-AY-001 | Admin can create academic year        |
| FE-AY-002 | Admin can activate academic year (4 states: DRAFT/ACTIVE/CLOSED/ARCHIVED) |
| FE-AY-003 | Admin can archive academic year       |
| FE-AY-004 | Admin can clone academic year         |
| FE-AY-005 | System displays active academic year  |
| FE-AY-006 | System validates only one active year |
| FE-AY-007 | Admin can close academic year         |
| FE-AY-008 | System auto-creates 2 semesters when academic year is created |

---

# 8.3 Class Management Module

| ID           | Requirement                     |
| ------------ | ------------------------------- |
| FE-CLASS-001 | Admin can create class          |
| FE-CLASS-002 | Admin can edit class            |
| FE-CLASS-003 | Admin can archive class         |
| FE-CLASS-004 | Admin can assign class capacity |
| FE-CLASS-005 | Admin can assign teacher        |
| FE-CLASS-006 | Admin can assign students       |

---

# 8.4 Student Management Module

| ID             | Requirement                |
| -------------- | -------------------------- |
| FE-STUDENT-001 | Admin can enroll student   |
| FE-STUDENT-002 | Admin can transfer student |
| FE-STUDENT-003 | Admin can promote student  |
| FE-STUDENT-004 | Admin can archive student  |
| FE-STUDENT-005 | Parent can register child  |
| FE-STUDENT-006 | Admin can resubmit rejected registration |
| FE-STUDENT-007 | System shows student status transitions |

---

# 8.5 Attendance Module

| ID         | Requirement                          |
| ---------- | ------------------------------------ |
| FE-ATT-001 | Teacher can mark attendance          |
| FE-ATT-002 | Teacher can add attendance remarks   |
| FE-ATT-003 | Parent can view attendance           |
| FE-ATT-004 | System displays attendance analytics |
| FE-ATT-005 | Attendance updates in real time      |

---

# 8.6 Assessment Module

| ID            | Requirement                           |
| ------------- | ------------------------------------- |
| FE-ASSESS-001 | Teacher can input assessment          |
| FE-ASSESS-002 | Teacher can edit assessment           |
| FE-ASSESS-003 | Teacher can add assessment notes      |
| FE-ASSESS-004 | Parent can view assessment results    |
| FE-ASSESS-005 | System displays skill progress charts |

---

# 8.7 Daily Reports Module

| ID            | Requirement                         |
| ------------- | ----------------------------------- |
| FE-REPORT-001 | Teacher can create daily report     |
| FE-REPORT-002 | Teacher can upload classroom photos |
| FE-REPORT-003 | Parent can view daily reports       |
| FE-REPORT-004 | Parent can view weekly reports (client-side aggregation) |
| FE-REPORT-005 | Parent can view monthly reports (client-side aggregation) |

---

# 8.8 Semester Reports Module

| ID         | Requirement                          |
| ---------- | ------------------------------------ |
| FE-SEM-001 | Teacher can generate semester report |
| FE-SEM-002 | Teacher can submit semester report   |
| FE-SEM-003 | Parent can view semester report      |
| FE-SEM-004 | Parent can download semester report  |

---

# 8.9 Analytics Dashboard Module

| ID        | Requirement                             |
| --------- | --------------------------------------- |
| FE-AN-001 | Admin can view attendance analytics     |
| FE-AN-002 | Admin can view teacher activity         |
| FE-AN-003 | Admin can view assessment completion    |
| FE-AN-004 | Admin can view student progress summary |
| FE-AN-005 | Dashboard supports filtering            |

---

# 8.10 Notification Module

| ID           | Requirement                             |
| ------------ | --------------------------------------- |
| FE-NOTIF-001 | System receives push notifications      |
| FE-NOTIF-002 | Parent receives in-app attendance updates      |
| FE-NOTIF-003 | Parent receives push notifications for published semester reports only |
| FE-NOTIF-004 | Notification badge updates in real time |

---

# 9. UI/UX Requirements

## 9.1 General UI Principles

The system must provide:

* Responsive layouts
* Accessible UI components
* Minimal learning curve
* Mobile-friendly parent pages
* Fast navigation
* Consistent component patterns

---

## 9.2 Design System

Frontend must use reusable UI primitives:

| Component | Usage                |
| --------- | -------------------- |
| Button    | Actions              |
| Modal     | Confirmation dialogs |
| Table     | Data management      |
| Drawer    | Mobile navigation    |
| Form      | Input management     |
| Tabs      | Progress sections    |
| Card      | Dashboard display    |
| Toast     | Notifications        |
| Chart     | Analytics            |

---

# 10. State Management

## 10.1 Global State

TanStack Store manages:

* Authentication state
* User session
* Theme settings
* Notification state
* Active academic year
* Selected class (for multi-class teachers)

---

## 10.2 Server State

TanStack Query manages:

* API caching
* Pagination
* Refetching
* Background synchronization
* Optimistic updates

---

# 11. Form Management

## 11.1 Form Features

TanStack Form + Zod validation must support:

* Client validation
* Async validation
* Error handling
* Debounced validation
* Nested forms
* Dynamic forms

---

# 12. API Integration

Backend APIs defined in backend SRS. Frontend integrates using GraphQL and REST APIs. 

---

## 12.1 GraphQL Usage

GraphQL used for:

* Queries
* Mutations
* Dashboard aggregation
* Student progress
* Attendance reports

---

## 12.2 REST API Usage

REST APIs used for:

* Authentication (login, logout, refresh, password reset)
* File upload
* Push notification device registration
* Report export/download

---

# 13. File Upload Requirements

| Requirement     | Description      |
| --------------- | ---------------- |
| Supported Files | JPG, PNG, PDF    |
| Max Upload Size | 10 MB            |
| Upload Type     | Multipart upload |
| Preview Support | Yes              |
| Upload Progress | Yes              |

---

# 14. Performance Requirements

| Requirement            | Target      |
| ---------------------- | ----------- |
| Initial page load      | < 3 seconds |
| Dashboard load         | < 2 seconds |
| API response rendering | < 500 ms    |
| Attendance submission  | < 1 second  |
| Lighthouse score       | > 85        |

---

# 15. Security Requirements

| ID      | Requirement                   |
| ------- | ----------------------------- |
| SEC-001 | Secure JWT storage            |
| SEC-002 | CSRF protection               |
| SEC-003 | XSS prevention                |
| SEC-004 | Route protection              |
| SEC-005 | Role-based UI rendering       |
| SEC-006 | Secure file upload validation |
| SEC-007 | Session expiration handling   |

---

# 16. Accessibility Requirements

| ID      | Requirement                 |
| ------- | --------------------------- |
| ACC-001 | Keyboard navigation support |
| ACC-002 | Screen reader support       |
| ACC-003 | Color contrast compliance   |
| ACC-004 | Accessible forms            |
| ACC-005 | Focus indicators visible    |

---

# 17. Responsive Requirements

| Device  | Support          |
| ------- | ---------------- |
| Desktop | Full support     |
| Tablet  | Full support     |
| Mobile  | Parent optimized |

---

# 18. Error Handling

## 18.1 Error Types

Frontend must handle:

* Authentication errors
* Validation errors
* API failures
* Upload failures
* Network interruptions
* Permission errors

---

## 18.2 Error UI

System must provide:

* Toast notifications
* Inline validation messages
* Retry actions
* Empty states
* Loading skeletons

---

# 19. Offline & Caching Strategy

| Feature          | Strategy        |
| ---------------- | --------------- |
| API cache        | TanStack Query  |
| Attendance draft | Local storage   |
| Form recovery    | Auto-save       |
| Periodic sync    | TanStack Query refetching (no WebSocket) |
| Retry requests   | Automatic retry |
| Optimistic UI    | Supported       |

---

# 20. Logging & Monitoring

Frontend must support:

* Error logging
* Performance monitoring
* API tracing
* User activity tracking
* Crash reporting

Suggested tools:

* Sentry
* OpenTelemetry
* Grafana Faro

---

# 21. Frontend Build & Deployment

| Category              | Requirement         |
| --------------------- | ------------------- |
| CI/CD                 | GitHub Actions      |
| Hosting               | Vercel / Cloudflare |
| Environment Variables | Secure injection    |
| Build Target          | SSR + SPA hybrid    |
| CDN Support           | Required            |

---

# 22. Browser Support

| Browser | Minimum Version   |
| ------- | ----------------- |
| Chrome  | Latest 2 versions |
| Edge    | Latest 2 versions |
| Firefox | Latest 2 versions |
| Safari  | Latest 2 versions |

---

# 23. Future Enhancements

Aligned with PRD future roadmap. 

## Planned Features

* Mobile applications
* Parent-teacher messaging
* Real-time chat
* AI analytics dashboards
* Offline-first PWA mode
* Multi-school support

---

# 24. Recommended Frontend Development Standards

## 24.1 Coding Standards

* Strict TypeScript mode
* ESLint
* Prettier
* Conventional commits
* Feature-based modules
* Reusable UI components

---

## 24.2 Testing Standards

| Test Type      | Tool            |
| -------------- | --------------- |
| Unit Test      | Vitest          |
| Component Test | Testing Library |
| E2E Test       | Playwright      |

---

# 25. Suggested Frontend Module Ownership

| Module             | Priority |
| ------------------ | -------- |
| Authentication     | Critical |
| Academic Years     | Critical |
| Attendance         | Critical |
| Daily Reports      | Critical |
| Parent Monitoring  | Critical |
| Analytics          | High     |
| Push Notifications | High     |
| Semester Reports   | High     |
