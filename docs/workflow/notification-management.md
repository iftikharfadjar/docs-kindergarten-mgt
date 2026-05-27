# Notification Management Workflow

## 1. Overview
This workflow describes how the system creates, stores, sends, reads, and manages notifications across Admin, Teacher, and Parent roles. Notifications support both in-app notification lists and push notifications through Firebase Cloud Messaging (FCM).

The backend creates notification records when important events happen, such as attendance updates, daily report publication, semester report publication, registration approval/rejection, and student enrollment. Push sending must be asynchronous and must not block the main mutation response.

## 2. API / GraphQL and REST List
The following operations are utilized in this workflow:

- `query GetNotifications` - Fetches notifications for the authenticated user.
- `query GetNotificationById` - Fetches one notification owned by the authenticated user.
- `query GetNotificationsAll` - Admin-only query for all notifications.
- `query GetNotificationsPagination` - Fetches paginated notifications.
- `mutation CreateNotification` - System/Admin creates a notification record.
- `mutation UpdateNotification` - Updates notification metadata.
- `mutation DeleteNotification` - Soft deletes one notification.
- `mutation DeleteNotifications` - Soft deletes multiple notifications.
- `mutation MarkNotificationRead` - Marks one notification as read.
- `mutation MarkAllNotificationsRead` - Marks all current user's notifications as read.
- `POST /api/v1/notifications/register-device` - Registers an FCM device token.

## 3. Domain / Table List
The workflow interacts with the following database tables and services:

- `Notifications` - Stores title, body, type, read state, entity reference, and recipient.
- `DeviceTokens` - Stores FCM tokens for each user/device.
- `Users` - Provides notification recipient identity.
- `ParentStudentLinks` - Finds parents linked to a student.
- `StudentEnrollments` - Finds class/student relationships for notification targeting.
- `Firebase Cloud Messaging` - Sends push notifications.

## 4. API Sequence Diagram

```mermaid
sequenceDiagram
    actor Teacher
    actor Parent
    participant Frontend as Frontend (SolidJS)
    participant API as Backend API
    participant DB as PostgreSQL Database
    participant FCM as Firebase Cloud Messaging

    Teacher->>Frontend: Publishes daily report
    Frontend->>API: mutation CreateDailyReport(...)
    API->>DB: INSERT DailyReports
    API->>DB: Find students enrolled in class
    API->>DB: Find linked parent users
    API->>DB: INSERT Notifications for parents
    API-->>Frontend: Return daily report success
    API->>FCM: Send push notification asynchronously

    Parent->>Frontend: Opens notification list
    Frontend->>API: query GetNotifications(limit, offset)
    API->>DB: SELECT Notifications WHERE user_id = context.userID
    DB-->>API: Return notifications
    API-->>Frontend: Render notification list

    Parent->>Frontend: Clicks notification
    Frontend->>API: mutation MarkNotificationRead(id)
    API->>DB: UPDATE notification is_read = true
    API-->>Frontend: Update unread badge
```

## 5. UI/UX Screen Flow

1. **Notification Trigger**
   - Teacher/Admin performs an action that should notify users.
   - Backend creates `Notifications` rows inside or near the business transaction.
   - Backend schedules async push delivery to FCM device tokens.

2. **Notification Bell**
   - Frontend fetches notifications using TanStack Query polling.
   - Badge shows unread count.

3. **Notification List**
   - User opens notification list.
   - User can click one notification to mark it read.
   - User can click `[Mark All Read]`.

4. **Device Registration**
   - After login, frontend requests browser notification permission.
   - If permission is granted, frontend registers the FCM token through REST.

## 6. UI Wireframe

```text
+-----------------------------------------------------------------------------+
|  [Logo] Kindergarten Mgt                    [Bell: 3] User: Parent | Logout |
+-----------------------------------------------------------------------------+
|                                                                             |
|  Notifications                                                              |
|  -------------------------------------------------------------------------  |
|  [Unread] New daily report from Lion Class A              2026-08-12 10:00  |
|  [Unread] Attendance marked for Timmy Wijaya              2026-08-12 09:00  |
|  [Read]   Semester report published                       2026-08-10 15:30  |
|                                                                             |
|                                                  [Mark All Read]             |
+-----------------------------------------------------------------------------+
```
