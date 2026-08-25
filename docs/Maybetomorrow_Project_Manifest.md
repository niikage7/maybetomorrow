# Project Manifest: Maybetomorrow (mt)

## 1. Project Overview

**Project Name:** Maybetomorrow (mt)

**Project Type:** Software / Product Build

**Summary:**
Maybetomorrow is a calendar-sharing system designed to help groups of people find shared free time. At its core is an interactive **Group Calendar**, into which individual users can merge their own **Public Calendars**. The system then summarizes the merged data to automatically surface time slots when all participants are available.

## 2. Problem Statement

Coordinating a shared activity across multiple people's schedules is time-consuming and error-prone when done manually (e.g., via chat threads, spreadsheets, or back-and-forth messages). Maybetomorrow removes this friction by centralizing availability into one collaborative view and automating the search for common free time.

## 3. Goals

- Enable multiple users to visualize and share their availability in one place.
- Allow individual "Public Calendars" to be merged into a shared "Group Calendar."
- Automatically identify and suggest free time slots common to all merged calendars.
- Provide a simple, interactive calendar experience for scheduling group activities.

## 4. Core Components

| Component | Description |
|---|---|
| **Group Calendar** | The central interactive calendar where merged availability is displayed and shared among participants. |
| **Public Calendar** | Each user's individual calendar of busy/free time, which can be merged into one or more Group Calendars. |
| **Merge Engine** | The mechanism that combines multiple Public Calendars into a single Group Calendar view. |
| **Suggestion Engine** | Analyzes merged calendar data to automatically suggest free time slots when all participants are available. |

## 5. Key Features

- **Interactive Calendar** — visual, user-friendly calendar interface.
- **Collaborative Scheduling** — many users contributing to a single shared Group Calendar.
- **Automated Free-Time Suggestions** — system-generated recommendations based on merged availability.

## 6. Out of Scope (for now)

The following are intentionally excluded from this phase and may be revisited in a later release:

- Third-party calendar sync (e.g., Google Calendar, Outlook)
- Push notifications
- Native mobile app

## 7. Stakeholders

- **Development Team** — builds and maintains the system.
- **Product** — defines requirements, priorities, and scope.

## 8. Success Criteria

- The core flow works end-to-end: a user can create/merge Public Calendars into a Group Calendar, and the system correctly suggests a free time slot common to all merged calendars.

---
*This manifest reflects the current high-level understanding of the project and is intended to evolve as scope is refined.*
