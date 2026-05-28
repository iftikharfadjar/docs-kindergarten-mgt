# Acceptance Test Scenarios

## Purpose
This document defines end-to-end Given/When/Then scenarios for major workflows.

## Authentication
```text
Given an active Admin user
When the user logs in with valid credentials
Then the system returns an access token
And redirects to /admin/dashboard
```

```text
Given an expired access token and valid refresh cookie
When a GraphQL request returns 401
Then the frontend refreshes the token
And retries the original request once
```

## Academic Year
```text
Given an Admin user
When the Admin creates an academic year
Then the year is created in DRAFT
And exactly 2 semesters are created
```

## Parent Registration
```text
Given a public parent registration page
When a parent submits parent and child data
Then a Parent user is created
And a Student is created with status PENDING
And a ParentStudentLink is created
```

```text
Given a rejected child registration
When the parent edits the child data and resubmits
Then the student status changes from REJECTED to PENDING
```

## Enrollment
```text
Given an ACTIVE student and a class with capacity
When Admin enrolls the student
Then a StudentEnrollment is created
```

```text
Given a PENDING student
When Admin tries to enroll the student
Then the system rejects the request
```

## Teacher Daily Operations
```text
Given a Teacher assigned to a class
When the Teacher marks attendance for that class
Then attendance records are saved
```

```text
Given a Teacher not assigned to a class
When the Teacher tries to mark attendance for that class
Then the system returns FORBIDDEN
```

## Parent Monitoring
```text
Given a Parent linked to an ACTIVE child
When the Parent opens attendance
Then only that child's attendance is visible
```

```text
Given Parent A and Parent B
When Parent A requests Parent B's child data
Then the system returns FORBIDDEN
```

## Media Upload
```text
Given an authenticated Teacher
When the Teacher uploads a valid JPG under 10 MB
Then the file is stored in MinIO
And a MediaAssets row is created
And the response returns mediaAssetId and url
```

## Notifications
```text
Given a Teacher publishes a daily report
When the backend creates notifications
Then linked parents receive in-app notification rows
And FCM sending is not attempted for daily reports in MVP
```

```text
Given a Teacher publishes a semester report
When the backend creates notifications
Then the linked parent receives an in-app notification
And FCM sending is attempted asynchronously
```

## Semester Reports
```text
Given a DRAFT semester report
When the Teacher publishes it
Then the status becomes PUBLISHED
And Parent can view it
```
