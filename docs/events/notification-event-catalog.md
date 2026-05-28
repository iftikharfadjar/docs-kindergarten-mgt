# Notification Event Catalog

## Purpose
This document defines events that create in-app notifications and the subset that sends FCM pushes.

## Events

| Event | Trigger | Recipients | Entity | In-App | FCM Push |
|---|---|---|---|---|---|
| `ATTENDANCE_MARKED` | Teacher/Admin marks attendance | Linked parents of students | `ATTENDANCE` | Yes | No |
| `DAILY_REPORT_CREATED` | Teacher creates daily report | Parents of students enrolled in class | `DAILY_REPORT` | Yes | No |
| `SEMESTER_REPORT_PUBLISHED` | Teacher/Admin publishes report | Linked parent of student | `SEMESTER_REPORT` | Yes | Yes |
| `REGISTRATION_ACTIVATED` | Admin sets student status `ACTIVE` | Parent linked to student | `STUDENT` | Yes | No |
| `REGISTRATION_REJECTED` | Admin sets student status `REJECTED` | Parent linked to student | `STUDENT` | Yes | No |
| `STUDENT_ENROLLED` | Admin enrolls student | Parent linked to student | `STUDENT_ENROLLMENT` | Yes | No |
| `PASSWORD_RESET_REQUESTED` | Forgot password requested | Email only, optional audit notification | `USER` | No | No |

## Notification Payload Fields

```text
userId
title
body
type
entityType
entityId
isRead = false
createdAt
```

## FCM Rules
- Create database notification first.
- Send FCM asynchronously only for `SEMESTER_REPORT_PUBLISHED` in MVP.
- If FCM fails, keep the database notification.
- Remove invalid device tokens when FCM reports invalid/expired token.

## Targeting Rules
- Parent notifications require `ParentStudentLinks`.
- Class-level report notifications target parents of currently enrolled active students.
- Admin notifications may target all Admin users for system-level events.

## Example

```json
{
  "type": "DAILY_REPORT_CREATED",
  "title": "New daily report",
  "body": "Lion Class A has a new daily report.",
  "entityType": "DAILY_REPORT",
  "entityId": "uuid-report"
}
```
