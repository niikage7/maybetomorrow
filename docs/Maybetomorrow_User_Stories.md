# User Stories: Maybetomorrow (mt)

Derived from the [Requirements Specification](Maybetomorrow_Requirements_Spec.md). Grouped by feature area.

## 1. Personal Calendar

**US-1.1**
As a user, I want to create my own Personal Calendar, so that I can track and share my availability.

**US-1.2**
As a user, I want my time slots to default to "Unknown," so that I'm never showing false availability I haven't actually confirmed.

**US-1.3**
As a user, I want to set a time slot to Free or Busy (or back to Unknown), so that my calendar accurately reflects my real schedule.

**US-1.4**
As a user, I want to select a range of time in one action (e.g., 12 PM–3 PM busy), so that I don't have to mark each 30-minute slot individually.

**US-1.5**
As a user, I want to set my Personal Calendar as Public or Private, so that I can control who sees my availability by default.

**US-1.6**
As a user, I want my Private calendar to stay hidden unless I merge it into a Group Calendar, so that I only share my schedule with groups I choose.

## 2. Group Calendar

**US-2.1**
As a user, I want to create a Group Calendar, so that I can coordinate schedules with other people.

**US-2.2**
As a user, I want to set a Group Calendar as Public or Private, so that I can control who can view it.

**US-2.3**
As a user, I want to merge my Personal Calendar into a Group Calendar, so that my availability is included when the group looks for shared free time.

**US-2.4**
As a user, I want to un-merge my Personal Calendar from a Group Calendar, so that I can stop sharing my availability with that group when I no longer need to.

**US-2.5**
As a user, I want to view the combined availability of everyone merged into a Group Calendar, so that I can see the group's schedule at a glance.

**US-2.6**
As a user, I want to belong to multiple Group Calendars at once, so that I can coordinate with different groups independently.

**US-2.7**
As a user with a Public Group Calendar link, I want non-members to be able to view but not edit it, so that I can share visibility without losing control over who can add availability.

**US-2.8**
As a user, I want a Private Group Calendar to be accessible only to its participants, so that sensitive scheduling stays within the group.

**US-2.9**
As the Creator of a Group Calendar, I want to generate and share an invitation link, so that I control who is able to join and contribute to my group.

**US-2.10**
As a user, I want to join a Group Calendar via an invitation link, so that I can easily become a Participant and add my availability.

**US-2.11**
As a Participant, I do not want to be able to generate invitation links myself, so that the Creator retains control over who joins the group.

**US-2.12**
As a Participant, I want autosync off by default when I join a group, so that my availability isn't shared with the group until I'm ready, even after I've merged my calendar.

**US-2.13**
As a Participant, I want to manually sync my availability into a Group Calendar, so that I can control exactly when my latest schedule becomes visible to the group.

**US-2.14**
As a Participant, I want to enable autosync for a specific group, so that my availability updates automatically for that group without me syncing every time — without affecting how I share with other groups.

## 3. Automated Suggestions

**US-3.1**
As a user, I want the system to automatically suggest time slots when everyone in the group is free, so that I don't have to manually compare everyone's calendars.

**US-3.2**
As a user, I want to define the date range the system searches for suggestions, so that I only see suggestions relevant to my planning window (e.g., "next 2 weeks").

**US-3.3**
As a user, I want suggested free slots to be ranked soonest first, so that I can quickly see the nearest opportunity to schedule something.

**US-3.4**
As a user, I want suggestions to update automatically when someone's calendar changes, so that I always see current, accurate options.

**US-3.5**
As a user, I want to be clearly told when no common free slot exists, so that I know to ask the group to adjust their availability instead of assuming the system is broken or empty.

---
*Stories reflect v1 scope as defined in the Requirements Spec. Additional stories should be added as new requirements are defined (e.g., third-party sync, notifications) in future phases.*
