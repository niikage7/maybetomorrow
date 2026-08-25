# API Spec: Maybetomorrow (mt)

High-level REST endpoint list (method, path, description only). Based on the [Requirements Specification](Maybetomorrow_Requirements_Spec.md) and [Data Model](Maybetomorrow_Data_Model.md). Request/response body details are intentionally omitted at this stage.

## 1. Auth

| Method | Path | Description |
|---|---|---|
| POST | `/auth/register` | Create a new account via email + password. |
| POST | `/auth/login` | Log in with email + password. |
| POST | `/auth/oauth/{provider}` | Log in / register via an OAuth provider (e.g., Google). |
| POST | `/auth/logout` | End the current session. |

## 2. Users

| Method | Path | Description |
|---|---|---|
| GET | `/users/me` | Get the current logged-in user's profile. |
| GET | `/users/{userId}` | Get a user's public profile (includes their Public Personal Calendar, if set). |
| PATCH | `/users/me` | Update the current user's profile info. |

## 3. Personal Calendar

| Method | Path | Description |
|---|---|---|
| GET | `/users/me/personal-calendar` | Get the current user's Personal Calendar, including time slots. |
| PATCH | `/users/me/personal-calendar` | Update Personal Calendar settings (e.g., visibility: Public/Private). |
| GET | `/users/me/personal-calendar/time-slots` | List the current user's time slots (optionally filtered by date range). |
| POST | `/users/me/personal-calendar/time-slots` | Set status (Free/Busy/Unknown) for a time slot or range. |
| PATCH | `/users/me/personal-calendar/time-slots/{slotId}` | Update an existing time slot's status or range. |
| DELETE | `/users/me/personal-calendar/time-slots/{slotId}` | Remove a time slot (reverts that range to Unknown). |

## 4. Group Calendars

| Method | Path | Description |
|---|---|---|
| POST | `/group-calendars` | Create a new Group Calendar (creator becomes Creator role). |
| GET | `/group-calendars/{groupId}` | Get a Group Calendar's details and combined availability (subject to visibility/role checks). |
| PATCH | `/group-calendars/{groupId}` | Update Group Calendar settings (e.g., name, visibility). Creator only. |
| DELETE | `/group-calendars/{groupId}` | Delete a Group Calendar. Creator only. |
| GET | `/users/me/group-calendars` | List Group Calendars the current user is a member of. |
| POST | `/group-calendars/{groupId}/merge` | Merge the current user's Personal Calendar into the group (join as Participant). |
| DELETE | `/group-calendars/{groupId}/merge` | Un-merge the current user's Personal Calendar from the group. |
| GET | `/group-calendars/{groupId}/members` | List members (Creator + Participants) of a Group Calendar. |
| PATCH | `/group-calendars/{groupId}/membership` | Update the current user's Membership settings for this group (e.g., toggle `autosync_enabled`). |
| POST | `/group-calendars/{groupId}/sync` | Manually sync the current user's Personal Calendar into this Group Calendar (used when autosync is off). |

## 5. Invitations

| Method | Path | Description |
|---|---|---|
| POST | `/group-calendars/{groupId}/invitations` | Generate a new invitation link. Creator only. |
| GET | `/group-calendars/{groupId}/invitations` | List active invitation links for a group. Creator only. |
| DELETE | `/invitations/{invitationId}` | Revoke an invitation link. Creator only. |
| POST | `/invitations/{token}/accept` | Accept an invitation link (joins the group as Participant, merges Personal Calendar). |

## 6. Suggestions

| Method | Path | Description |
|---|---|---|
| GET | `/group-calendars/{groupId}/suggestions` | Get suggested common free-time slots for a group, ranked soonest-first, within a user-defined date range (query params). |

---
*Body schemas, status codes, and error formats are intentionally out of scope for this high-level pass — a natural follow-up once endpoints are validated against the flows they need to support.*
