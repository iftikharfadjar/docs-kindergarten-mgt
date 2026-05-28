# Seed Data Plan

## Purpose
This document defines minimum seed data for development and testing.

## Required Users

| Role | Email | Notes |
|---|---|---|
| Admin | `admin@school.test` | Full access. |
| Teacher 1 | `teacher1@school.test` | Assigned to Lion Class A. |
| Teacher 2 | `teacher2@school.test` | Assigned to Tiger Class B and Lion Class A. |
| Parent 1 | `parent1@school.test` | Linked to active child. |
| Parent 2 | `parent2@school.test` | Linked to another active child. |

## Required Academic Years

| Name | Status | Notes |
|---|---|---|
| `2025/2026` | `CLOSED` | Promotion source. |
| `2026/2027` | `ACTIVE` | Main testing year. |
| `2027/2028` | `DRAFT` | Promotion/clone target. |

Each year must have exactly 2 semesters.

## Required Classes

| Academic Year | Class | Capacity |
|---|---|---:|
| `2026/2027` | Lion Class A | 20 |
| `2026/2027` | Tiger Class B | 20 |
| `2027/2028` | Lion Class A | 20 |

## Required Students

| Student | Status | Parent | Class |
|---|---|---|---|
| Timmy Wijaya | `ACTIVE` | Parent 1 | Lion Class A |
| Susie Smith | `ACTIVE` | Parent 2 | Tiger Class B |
| Pending Child | `PENDING` | Parent 1 | None |
| Rejected Child | `REJECTED` | Parent 1 | None |

## Required Curriculum

Categories:
- Cognitive Development
- Motor Skills
- Social & Emotional

Skills:
- Recognizes primary colors
- Counts from 1 to 10
- Holds pencil correctly
- Shares toys with friends

## Required Operational Data
- Attendance records for Timmy and Susie.
- At least one daily report with MinIO media.
- Assessments for at least 2 skills.
- One `DRAFT` semester report.
- One `PUBLISHED` semester report.
- Notifications for Parent 1.

## Seed Password
Use a development-only password such as:

```text
Password123!
```

Never use seed passwords in production.
