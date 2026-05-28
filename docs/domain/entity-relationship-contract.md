# Entity Relationship Contract

## Purpose
This document defines ownership, scoping, and relationship rules for core entities.

## Root Scope
`AcademicYears` is the root academic container.

Scoped by Academic Year:
- Classes
- Semesters
- SkillCategories
- Skills through SkillCategories
- StudentEnrollments through Classes
- TeacherAssignments through Classes
- Attendance
- Assessments
- DailyReports
- SemesterReports

## Core Relationships

| Entity | Required Parent / Owner | Notes |
|---|---|---|
| `Users` | `Roles` | Auth identity. |
| `Profiles` | `Users` | One profile per user. |
| `Students` | None direct | Linked to parents through `ParentStudentLinks`. |
| `ParentStudentLinks` | `Users`, `Students` | Parent ownership and access control. |
| `AcademicYears` | None | Top-level academic scope. |
| `Semesters` | `AcademicYears` | Exactly two auto-created on academic year creation. |
| `Classes` | `AcademicYears` | Capacity and class identity. |
| `TeacherAssignments` | `Users`, `Classes` | Teacher write-permission source. |
| `StudentEnrollments` | `Students`, `Classes` | Student placement source. |
| `SkillCategories` | `AcademicYears` | Curriculum grouping. |
| `Skills` | `SkillCategories` | Assessment target. |
| `Attendance` | `Students`, `Classes`, `Semesters`, `AcademicYears` | Daily status records. |
| `Assessments` | `Students`, `Skills`, `Semesters`, `AcademicYears` | 0-4 score records. |
| `DailyReports` | `Classes`, `Semesters`, `AcademicYears`, `Users` | Class-level daily summary. |
| `SemesterReports` | `Students`, `Classes`, `Semesters`, `AcademicYears` | Published parent-facing summary. |
| `MediaAssets` | `Users`, optional polymorphic entity | MinIO file metadata. |
| `Notifications` | `Users` | User-scoped in-app notifications. |
| `DeviceTokens` | `Users` | FCM target devices. |

## Soft Delete Entities
Core entities should support `deleted_at`:
- Users
- AcademicYears
- Semesters
- Classes
- Students
- StudentEnrollments
- SkillCategories
- Skills
- Assessments
- Attendance
- DailyReports
- SemesterReports
- MediaAssets
- Notifications

## Audit Fields
Use `created_at` and `updated_at` on all core mutable tables.

Use `created_by` and `updated_by` on sensitive academic records:
- Students
- Attendance
- Assessments
- DailyReports
- SemesterReports
- AcademicYears
- Classes
- Skills / SkillCategories

## Integrity Rules
- Do not cascade delete historical academic records.
- Student transfer creates a new enrollment and closes/soft-deletes old active enrollment.
- Skill deletion must not delete historical assessments.
- Class deletion must not delete historical attendance or reports.
- Media deletion must not break existing reports silently; UI should handle missing media.
