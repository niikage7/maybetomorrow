# Requirements Specification: Maybetomorrow (mt)

## 1. Purpose

This document defines the functional and non-functional requirements for Maybetomorrow (mt), a calendar-sharing system that lets users merge individual Personal Calendars into a shared Group Calendar and receive automated suggestions for common free time.

This spec builds on the [Project Manifest](Maybetomorrow_Project_Manifest.md) and translates its goals into concrete, testable requirements.

## 2. User Roles

| Role | Description |
|---|---|
| **User** | Any registered user. Can create and manage their own Personal Calendar, create Group Calendars, and merge their Personal Calendar into any Group Calendar they join. |

Within a Group Calendar specifically, a user has one of two roles:

| Role (Group Calendar-scoped) | Description |
|---|---|
| **Creator** | The user who created the Group Calendar. Only the Creator can generate/share invitation links for that group. There is exactly one Creator per Group Calendar. |
| **Participant** | A user who joined a Group Calendar (via invitation link or, for Public calendars, the shareable link) and merged their Personal Calendar into it. |

*Note: v1 has a single global role (User) — any user can create a Group Calendar, at which point they become that calendar's Creator. Group-scoped roles (Creator/Participant) only govern permissions within that specific Group Calendar, not system-wide.*

## 3. Functional Requirements

### 3.1 Personal Calendar

A user's personal calendar has two visibility types:

| Type | Visibility |
|---|---|
| **Public** | Visible to anyone viewing the user's profile. |
| **Private** | Hidden from everyone unless the user explicitly publishes (merges) it into a Group Calendar. |

- **FR-1.1**: A user must be able to create and maintain their own Personal Calendar, set as Public or Private.
- **FR-1.2**: A time slot's default status is **Unknown** (neither free nor busy) until the user explicitly marks it.
- **FR-1.3**: A user must be able to set a time slot's status to **Free**, **Busy**, or back to **Unknown** on their Personal Calendar.
- **FR-1.4**: Time slots are managed at **30-minute granularity**.
- **FR-1.5**: A user must be able to switch their Personal Calendar's visibility between Public and Private.
- **FR-1.6**: A Private calendar merged into a Group Calendar reveals only the merged availability data to that group — it does not become visible on the user's public profile.

### 3.2 Group Calendar

A Group Calendar has two visibility types:

| Type | Visibility | Editing |
|---|---|---|
| **Public** | Anyone with the link can view it. | Only members can merge/edit their calendar into it — a non-member with the link cannot edit or merge. |
| **Private** | Only participants (members) can view it. | Only members can edit. |

*Note: for a Public Group Calendar, the shareable "view link" (anyone can see it) is distinct from the "invitation link" (lets a recipient join as a Participant). Only the Creator can generate/share the invitation link (see FR-2.8); this is what actually grants edit/merge access, not the public view link itself.*

- **FR-2.1**: Any user must be able to create a Group Calendar, set as Public or Private. The creating user becomes the **Creator** of that Group Calendar.
- **FR-2.2**: A user must be able to merge their Personal Calendar into a Group Calendar (becoming a **Participant**).
- **FR-2.3**: A user must be able to remove (un-merge) their Personal Calendar from a Group Calendar.
- **FR-2.4**: A Group Calendar must display the combined availability of all merged Personal Calendars.
- **FR-2.5**: A user must be able to belong to and merge into multiple Group Calendars.
- **FR-2.6**: A non-member accessing a Public Group Calendar's link can view it but cannot edit or merge a calendar into it.
- **FR-2.7**: A Private Group Calendar must not be accessible to non-members, including via link.
- **FR-2.8**: Only the **Creator** of a Group Calendar can generate and share its invitation link.
- **FR-2.9**: A user who opens a valid invitation link must be able to join the Group Calendar as a **Participant** and merge their Personal Calendar into it.
- **FR-2.10**: Participants cannot generate or share invitation links — this is restricted to the Creator.
- **FR-2.11**: An invitation link is valid for **3 days** from creation, after which it can no longer be used to join.
- **FR-2.12**: The Creator can revoke an invitation link at any time, immediately invalidating it.
- **FR-2.13**: Each Membership has an **"Enable autosync"** setting, defaulting to **off**. When off, changes a Participant makes to their Personal Calendar are **not** automatically reflected in the Group Calendar.
- **FR-2.14**: When autosync is off for a Membership, a Participant must be able to manually **sync** their availability into the Group Calendar on demand.
- **FR-2.15**: When autosync is turned on for a Membership, subsequent changes to that Participant's Personal Calendar automatically propagate to the Group Calendar (matching FR-3.2's "updates automatically" behavior, scoped to that Membership only).
- **FR-2.16**: A Participant must be able to toggle autosync on/off for each Group Calendar they're a member of, independently of other groups they belong to.

### 3.3 Automated Suggestions

- **FR-3.1**: The system must calculate and display time slots where all members of a Group Calendar are marked **Free**. Slots marked **Unknown** are not counted as free.
- **FR-3.2**: Suggestions must update automatically when any merged Personal Calendar changes, **for Memberships with autosync enabled** (FR-2.13–2.15). For Memberships with autosync off, suggestions reflect that member's last-synced availability until they sync manually.
- **FR-3.3**: If no common free slot exists (e.g., due to conflicts or unresolved **Unknown** slots), the system must indicate this clearly rather than showing an empty/ambiguous result.

- **FR-3.4**: The search range for suggestions must be user-defined (e.g., the user specifies a start and end date/time to search within).
- **FR-3.5**: When multiple valid free slots exist, they must be ranked and displayed **soonest first**.

### 3.4 Interface Note — Marking Time Ranges

*Design note, not a finalized requirement — to be validated during product/UX design.*

When a user clicks a cell on the calendar, they should be able to define a **time range** rather than only toggling a single slot. For example: 12 PM–3 PM busy, 4 PM–8 PM free. This suggests the interaction model should support click-and-drag or click-to-select-range, rather than single-cell toggling alone, since a person's day is usually made up of a few continuous blocks rather than isolated 30-minute slots.

## 4. Non-Functional Requirements

- **NFR-1**: The interactive calendar UI should feel responsive, with no noticeable lag when marking/unmarking availability.
- **NFR-2**: Suggestion calculations should complete quickly enough to feel real-time as calendars are merged or updated. Target: suggestions return within **2 seconds** for a group of up to the max size (NFR-3). *(This number is a starting placeholder, not yet validated against real usage or implementation — revisit once there's something to benchmark.)*
- **NFR-3**: The system should support Group Calendars with up to **100 members**.

*(Open note: privacy/security requirements around calendar visibility are intentionally not being explored in depth at this stage — revisit later.)*

## 5. Constraints & Assumptions

- No third-party calendar sync (Google/Outlook) in this phase.
- No push notifications in this phase.
- No native mobile app in this phase — assume web-based access.

## 6. Acceptance Criteria (v1)

- A user can create a Personal Calendar (Public or Private) and set time slots to Free or Busy (default Unknown).
- A user can create a Group Calendar (Public or Private) and merge their Personal Calendar into it.
- Multiple users' Personal Calendars can be merged into the same Group Calendar.
- The system correctly displays at least one common free time slot when one exists across all merged calendars.
- A non-member with a Public Group Calendar link can view but not edit or merge into it.
- A Private Group Calendar is inaccessible to non-members.
- A Private Personal Calendar is not visible on the user's profile, and remains hidden from a Group Calendar unless merged.
- Only the Creator of a Group Calendar can generate and share its invitation link.
- A user who opens a valid invitation link can join as a Participant and merge their Personal Calendar into the group.

---
*This is a living document — sections marked as open questions should be resolved as design decisions are made.*
