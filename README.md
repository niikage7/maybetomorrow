# Maybetomorrow (mt)

A calendar-sharing system for finding time to hang out. Merge your calendar into a group, and let the app figure out when everyone's actually free.

## What it does

Maybetomorrow is built around one core idea: coordinating a group's free time shouldn't require a group chat full of "does Tuesday work for everyone?" messages.

- **Personal Calendar** — your own calendar of Free / Busy / Unknown time slots.
- **Group Calendar** — a shared calendar that combines everyone's merged availability.
- **Automated suggestions** — the system finds and ranks common free time across the group, so you don't have to eyeball it.

## Key features

- 🗓️ **Interactive calendar** — click a cell (or drag a range) to set your availability.
- 👥 **Collaborative** — many people, one shared Group Calendar.
- 🔗 **Invitation links** — Creators generate 3-day, revocable links to invite people into a group.
- 🔒 **Visibility controls** — Personal and Group Calendars can each be Public or Private.
- ⚡ **Autosync (opt-in)** — off by default; when enabled, your availability updates in the group automatically instead of requiring a manual sync.
- 🤖 **Smart suggestions** — free slots ranked soonest-first, over a search range you define.

## How it works

1. Create your Personal Calendar and mark your time as Free or Busy (defaults to Unknown until you set it).
2. Create a Group Calendar, or join one via an invitation link from its Creator.
3. Merge your Personal Calendar into the group.
4. The Group Calendar shows everyone's combined availability, and the Suggestion Engine surfaces common free slots automatically.

## Project documentation

This repo's `/docs` folder contains the full project documentation:

| Doc | Description |
|---|---|
| [Project Manifest](docs/Maybetomorrow_Project_Manifest.md) | High-level scope, goals, and stakeholders. |
| [Requirements Specification](docs/Maybetomorrow_Requirements_Spec.md) | Detailed functional & non-functional requirements. |
| [User Stories](docs/Maybetomorrow_User_Stories.md) | Feature-by-feature user stories. |
| [Architecture Overview](docs/Maybetomorrow_Architecture_Overview.md) | High-level system components and how they interact. |
| [Data Model](docs/Maybetomorrow_Data_Model.md) | Entities, fields, relationships, and the ER diagram. |
| [Tech Stack](docs/Maybetomorrow_Tech_Stack.md) | Proposed technologies (example stack, subject to change). |
| [API Spec](docs/Maybetomorrow_API_Spec.md) | High-level REST endpoint list. |

## Tech stack (proposed)

> This is an example stack, not a locked-in decision — see [Tech Stack](docs/Maybetomorrow_Tech_Stack.md) for full reasoning and open alternatives.

- **Backend:** Java (or Go)
- **Frontend:** TypeScript + SolidJS (or React)
- **Database:** PostgreSQL
- **Cache:** Redis
- **Containers:** Podman (or Docker)
- **Hosting:** Self-hosted

## Status

🚧 In active design/planning — documentation-first, implementation not yet started.

## Acknowledgments

The project documentation in `/docs` (manifest, requirements, user stories, architecture, data model, tech stack, API spec) was structured and drafted with the help of [Claude](https://claude.ai) (Anthropic) — used as a thinking/writing aid to organize ideas, flag open questions, and keep the docs consistent as the design evolved. All product decisions and direction are the author's own.

## License

This project is licensed under the [GNU Affero General Public License v3.0](LICENSE).
