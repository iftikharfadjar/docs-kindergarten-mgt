# Product Requirements Document (PRD)
# Kindergarten School Management Application

# 1. Product Overview

## Product Name
Kindergarten School Management Application

## Purpose

A centralized web-based application for kindergarten schools to manage:
- Academic years
- Students
- Teachers
- Parents
- Classes
- Attendance
- Skill-based assessments
- Semester reports
- Student development reports
- Daily learning activities
- Analytics and progress tracking

The application improves:
- School administration
- Parent-teacher communication
- Student development tracking
- Academic year management
- Curriculum and skill configuration

# 2. Product Objectives

## Business Objectives
- Digitize school administration
- Reduce manual paperwork
- Improve parent engagement
- Centralize student progress tracking
- Standardize assessment workflows
- Support long-term academic history

## User Objectives

### Admin
- Configure academic years and school structure
- Manage teachers, classes, students, and curriculum
- Monitor school analytics and academic progress

### Teacher
- Manage attendance, reports, and assessments efficiently
- Configure classroom skills and assessments

### Parent
- Monitor child progress and attendance in real time

# 3. User Roles

| Role | Description |
|---|---|
| Admin | Full system administrator |
| Teacher | Classroom and student management |
| Parent | Child monitoring and reporting access |

# 4. Core System Concepts

## Academic Year as Root Configuration

Academic Year is the top-level academic container.
It operates on a 4-state lifecycle: DRAFT → ACTIVE → CLOSED → ARCHIVED.
Each academic year automatically creates 2 semesters.

All academic operational data belongs to an academic year:
- Classes
- Teacher assignments
- Student enrollments
- Skill categories
- Skills
- Attendance
- Assessments
- Semester reports
- Daily reports

# 5. System Modules

## Authentication & Authorization
- Login
- Logout
- Password reset
- JWT authentication
- Role-based access control (RBAC)

## Admin Module

### User Management
- Create users
- Edit users
- Disable users
- Reset passwords

### Academic Year Management
- Create academic years
- Activate/deactivate academic years
- Close academic years
- Archive academic years
- Clone previous academic year configuration

### Class Management
- Create classes
- Edit classes
- Archive classes
- Configure class capacity

### Teacher Management
- Add teacher profiles
- Assign teachers to classes (teachers can be assigned to multiple classes)
- Reassign teachers yearly
- View teacher activity

### Student Management
- Enroll students
- Transfer students
- Promote students automatically
- Resubmit rejected students
- Archive students (Final state)

### Skill Category Management
- Create skill categories
- Configure skill categories by academic year

### Skill Management
- Create skills
- Configure skills by academic year

### Analytics Dashboard
- Student attendance analytics
- Teacher activity analytics
- Assessment completion rates
- Student progress summaries

## Teacher Module

### Attendance Management
- Mark daily attendance (assigned classes only)
- Add remarks
- View attendance (any class)

### Assessment Management
- Input skill assessments (assigned classes only)
- Add assessment notes
- View assessments (any class)

### Daily Reports
- Create daily reports
- Add activity summaries
- Upload classroom photos

### Semester Reports
- Generate semester reports
- Submit semester summaries

## Parent Module

### Parent Registration
- Register accounts
- Register children
- Upload required documents

### Progress Monitoring
- View attendance
- View daily reports
- View semester reports
- View historical academic records

### Push Notifications
- Attendance updates
- Daily report submissions
- Semester report availability

---

# 6. Workflows

---

# 6.1 Academic Year Configuration Workflow

```text id="academic-year-workflow"
Admin Login
      │
      ▼
Create Academic Year
      │
      ▼
Configure Classes
      │
      ▼
Assign Teachers
      │
      ▼
Enroll Students
      │
      ▼
Configure Skill Categories
      │
      ▼
Configure Skills
      │
      ▼
Auto-create Semesters
      │
      ▼
Academic Year Ready (Active)
```

---

# 6.2 Parent Registration Workflow

```text id="parent-registration"
Parent Creates Account
        │
        ▼
Register Child Information
        │
        ▼
Upload Documents
        │
        ▼
Submit Registration
        │
        ▼
Admin Reviews Registration
        │
 ┌──────┴──────┐
 │             │
 ▼             ▼
Rejected    Approved
  │               │
  │               ▼
  │     Assign Academic Year & Class
  │               │
  │               ▼
  │        Student Activated
  ▼
Can be resubmitted
to PENDING
```

---

# 6.3 Teacher Daily Workflow

```text id="teacher-workflow"
Teacher Login
      │
      ▼
Open Assigned Class
      │
      ▼
Mark Attendance
      │
      ▼
Input Skill Assessments
      │
      ▼
Add Daily Notes
      │
      ▼
Upload Photos
      │
      ▼
Submit Daily Report
      │
      ▼
Parent Receives Push Notification
```

---

# 6.4 Semester Reporting Workflow

```text id="semester-report-workflow"
Teacher Reviews Assessments
        │
        ▼
Generate Semester Summary
        │
        ▼
Add Recommendations
        │
        ▼
Submit Semester Report
        │
        ▼
Parent Receives Notification
        │
        ▼
Parent Views Semester Report
```

---

# 6.5 Student Promotion Workflow

```text id="promotion-workflow"
Academic Year Ends (CLOSED state)
        │
        ▼
System Evaluates Promotion Rules
        │
        ▼
Generate New Enrollment
        │
        ▼
Assign Next Class
        │
        ▼
Archive Previous Enrollment
        │
        ▼
Student Promoted
```

---

# 6.6 Parent Monitoring Workflow

```text id="parent-monitoring"
Parent Login
      │
      ▼
Open Dashboard
      │
      ▼
View Attendance
      │
      ▼
View Daily Reports
      │
      ▼
View Skill Assessments
      │
      ▼
View Weekly Progress
      │
      ▼
View Monthly Progress
      │
      ▼
View Semester Reports
      │
      ▼
View Historical Academic Records
```

---

# 7. Functional Requirements

| ID | Requirement |
|---|---|
| FR-001 | User can login securely |
| FR-002 | Admin can manage users |
| FR-003 | Admin can manage academic years |
| FR-004 | Only one academic year can be active |
| FR-005 | Admin can clone previous academic year configuration |
| FR-006 | Admin can configure classes inside academic years |
| FR-007 | Admin can assign teachers into academic year classes |
| FR-008 | Admin can enroll students into academic year classes |
| FR-009 | Admin can configure skill categories |
| FR-010 | Admin can configure skills |
| FR-011 | Teacher can manage attendance |
| FR-012 | Teacher can input skill assessments |
| FR-013 | Teacher can create daily reports |
| FR-014 | Teacher can upload classroom photos |
| FR-015 | Teacher can configure skills if permitted |
| FR-016 | Parent can monitor attendance |
| FR-017 | Parent can monitor daily progress |
| FR-018 | Parent can monitor weekly/monthly progress |
| FR-019 | Parent can view semester reports |
| FR-020 | Parent can view historical academic records |
| FR-021 | System sends push notifications |
| FR-022 | System archives academic year data |
| FR-023 | System supports automated student promotion |
| FR-024 | System provides analytics dashboard |
| FR-025 | Teacher can generate semester reports |
| FR-026 | System auto-creates 2 semesters per academic year |
| FR-027 | Admin can close academic year |
| FR-028 | Admin can resubmit rejected student registration |
| FR-029 | System supports token refresh |
| FR-030 | User can reset password via email |

# 8. Database Design

## Core Entities
- Users
- Roles
- Profiles
- AcademicYears
- Classes
- Students
- StudentEnrollments
- TeacherAssignments
- SkillCategories
- Skills
- Assessments
- Attendance
- DailyReports
- SemesterReports
- MediaAssets
- Semesters
- Notifications
- DeviceTokens

# 9. Suggested Tech Stack

## Frontend
- Tanstack-start
- Solidjs
- Tanstack Query
- Tanstack Store
- Tailwind CSS
- Solid UI
- TypeScript

## Backend
- Go
- Hybrid API (GraphQL for CRUD, REST for auth/media)

## Database
- PostgreSQL

## Storage
- AWS S3 or MinIO

## Authentication
- JWT
- Refresh Tokens

## Push Notification Service
- Firebase Cloud Messaging (FCM)

# 10. MVP / Phase 1 Scope

Included:
- Authentication (Login, Logout, Refresh, Password Reset)
- Academic year management
- User & role management
- Parent registration
- Class management
- Teacher assignment
- Student enrollment
- Attendance management
- Skill category management
- Skill management
- Skill assessments
- Daily reports
- Push notifications
- Semester reports
- Analytics dashboard
- Automated student promotion

Excluded:
- Payment system
- Live chat
- AI analytics
- Mobile applications
- Multi-school support

# 11. Future Enhancements

## Phase 2
- Parent-teacher messaging
- Mobile applications
- Advanced analytics
- Multi-school support

## Phase 3
- AI student development insights
- E-learning modules
- Curriculum recommendation engine

# 12. Success Metrics

| Metric | Target |
|---|---|
| Teacher daily reporting completion | 100% |
| Parent weekly engagement | > 70% |
| Attendance completion rate | 100% |
| Daily report submission time | < 10 minutes/class |
| Notification delivery success | > 95% |
| Semester report completion | 100% |