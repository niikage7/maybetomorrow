# Architecture Overview: Maybetomorrow (mt)

Stack-agnostic overview of the system's high-level components and how they interact. Based on the [Requirements Specification](Maybetomorrow_Requirements_Spec.md).

## 1. Purpose

This document describes the logical building blocks of Maybetomorrow (mt) and their responsibilities, independent of any specific technology choice. It's intended as a shared reference before technology/stack decisions are made.

## 2. System Components

### 2.1 User & Access Management

Responsible for user identity and access control.

- Manages user accounts and authentication.
- Tracks Group Calendar-scoped roles (**Creator**, **Participant**) per group.
- Validates invitation links and grants Participant access when a user joins via a valid link.
- Enforces who can view/edit a given calendar based on its visibility (Public/Private) and the requester's role.

### 2.2 Personal Calendar Service

Owns each user's individual availability data.

- Stores a user's time slots and their status (**Unknown** / **Free** / **Busy**) at 30-minute granularity.
- Handles setting a status for a single slot or a time range in one action.
- Tracks a Personal Calendar's visibility (Public/Private) and enforces that Private calendars aren't exposed except through an explicit merge.

### 2.3 Group Calendar Service

Owns the shared, group-level view of merged availability.

- Manages Group Calendar creation, visibility (Public/Private), and metadata (Creator, list of Participants).
- Handles merging a Personal Calendar into a Group Calendar, and un-merging.
- Aggregates merged Personal Calendars into a combined view for display.
- Enforces edit/merge restrictions (only Participants can merge/edit; non-members can only view a Public group).

### 2.4 Invitation Service

Manages the invitation-link mechanism.

- Generates invitation links, restricted to the Group Calendar's Creator.
- Validates an invitation link when opened and hands off to User & Access Management to add the user as a Participant.
- Distinguishes the invitation link (grants join/edit) from a Public Group Calendar's plain view link (view-only, no join).

### 2.5 Suggestion Engine

Computes automated free-time suggestions for a Group Calendar.

- Reads the merged availability from the Group Calendar Service for a given group.
- Given a user-defined search range, finds slots where all Participants are marked **Free** (treating **Unknown** as not-free).
- Ranks results soonest-first and returns them, or indicates clearly when no common slot exists.
- Recalculates when a merged Personal Calendar changes (per FR-3.2), so results stay current.

### 2.6 Interactive Calendar UI

The user-facing interface layer (web-based per current scope).

- Renders both Personal and Group Calendars, including the merged/combined view.
- Supports the click/select interaction for setting a time range's status in one action.
- Displays suggested free slots from the Suggestion Engine.
- Surfaces visibility/role state (e.g., view-only banner for non-member viewers of a Public group).

## 3. Component Interaction (Typical Flow)

1. A user creates a Group Calendar → **Group Calendar Service** registers it and assigns the user as **Creator** (**User & Access Management**).
2. The Creator shares an invitation link → **Invitation Service** generates it.
3. Another user opens the link → **Invitation Service** validates it → **User & Access Management** grants them **Participant** role → **Group Calendar Service** merges their **Personal Calendar**.
4. As Participants set their availability via the **Interactive Calendar UI**, updates flow into the **Personal Calendar Service**.
5. The **Group Calendar Service** reflects the combined availability; the **Suggestion Engine** recalculates common free slots and the UI displays them.

## 4. Notes & Open Points

- This is a logical/component view, not a deployment or infrastructure diagram — no assumptions are made yet about specific databases, frameworks, or hosting.
- Whether these are separate services or modules within a single application is a later implementation decision, not addressed here.
- Data model details (entities, fields, relationships) are not covered in this document and could be a useful follow-up.

---
*This document reflects the logical structure implied by the current Requirements Spec and User Stories, and should evolve alongside them.*
