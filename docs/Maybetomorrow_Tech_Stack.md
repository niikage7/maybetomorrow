# Tech Stack: Maybetomorrow (mt)

Example technology choices for a Java (backend) / TypeScript (frontend) stack. Maps to the components defined in the [Architecture Overview](Maybetomorrow_Architecture_Overview.md).

## 1. Guiding Principles

- **This is an example stack**, not a locked decision — actual scale needs are still uncertain, and choices below should flex with team preference and how the vision develops. Treat this as a reasonable starting point to react to, not a final answer.
- **Language:** Java on the backend, TypeScript on the frontend (per team preference) — though see note on alternatives below.
- Stack-level choices below are a starting proposal — meant to be validated/adjusted by the team, not treated as final.

## 2. Backend

| Concern | Choice | Why |
|---|---|---|
| **Language / Runtime** | Java (LTS version, e.g., 21) | Team preference; mature ecosystem. Worth weighing against **Go** as an alternative — Go tends to be lighter-weight and has strong built-in concurrency support, which can matter more if scale needs grow, while Java offers a larger ecosystem/tooling if that matters more to the team. Since actual scale is still uncertain, either is a reasonable starting choice. |
| **Framework** | Spring Boot (if Java) | Standard for Java web services; strong support for REST APIs, dependency injection, and modular services matching the components in the Architecture Overview. *(If Go is chosen instead, a lighter framework like Gin or Echo would be the equivalent choice.)* |
| **API Style** | REST (JSON) | Simple, well-understood, easy for a TypeScript frontend to consume. |
| **Service structure** | Simple monolith (single deployable) | With scale still uncertain, even a "modular monolith" framing may be over-engineering this early — it implies designing internal module boundaries up front for a scale-out that might not happen. A plain, straightforward monolith is simpler to build and reason about now; the components in the Architecture Overview can still guide internal code organization (e.g., separate packages/classes) without treating it as a formal architectural commitment. Splitting into services is a decision to revisit if and when real usage shows a clear need. |

## 3. Data Storage

| Concern | Choice | Why |
|---|---|---|
| **Primary database** | PostgreSQL | Relational fit for the [Data Model](Maybetomorrow_Data_Model.md) (Users, Calendars, Time Slots, Memberships, Invitation Links) with clear foreign-key relationships; scales well with read replicas. |
| **Caching layer** | Redis | Used for versioned caching of Personal/Group Calendar views (see [Data Model §5](Maybetomorrow_Data_Model.md)) — keying cache entries by `{calendar_id}:{version}` gives cheap, automatic invalidation whenever underlying data changes, without a separate invalidation step. |

## 4. Suggestion Engine

The Suggestion Engine (FR-3.1–3.5) recalculates common free time whenever a merged Personal Calendar changes, for groups up to 100 members (NFR-3) and a user-defined search range (FR-3.4).

| Concern | Choice | Why |
|---|---|---|
| **Recalculation trigger** | Synchronous, on-demand (recalculate when requested/viewed, or directly after a change) | Given group sizes capped at 100 and the current 2-second target, a direct recalculation is simplest to build and likely sufficient. An async, event-driven approach (e.g., a message queue) is a reasonable upgrade path *if* usage patterns later show it's needed — not a default to reach for now. |
| **Delivery to client** | Polling initially; WebSockets/SSE later if "auto-update" needs to feel more instant | Keeps initial implementation simple; can upgrade to push-based updates later without changing the underlying engine. |

## 5. Frontend

| Concern | Choice | Why |
|---|---|---|
| **Language** | TypeScript | Team preference; type safety helps keep the calendar/grid UI logic (30-min slots, ranges, statuses) less error-prone. |
| **Framework** | SolidJS (or React) | Personal preference; SolidJS offers fine-grained reactivity with strong performance for UI-heavy interactions like the calendar grid, and pairs well with TypeScript. React remains a solid, more widely-adopted alternative with a larger ecosystem if that becomes a priority (e.g., hiring, library availability). |
| **State management** | Framework-native reactivity (SolidJS signals) or a server-state caching library (e.g., React Query, if using React) for calendar and suggestion data | Most UI state here is server-derived (calendars, memberships, suggestions) rather than complex client-only state, so leaning on the framework's built-in reactivity/server-state tools fits better than a heavier global store. |

## 6. Infrastructure & Deployment

**Hosting:** self-hosted, on the team's own server(s) — no cloud provider (AWS/GCP/Azure) planned. Choices below reflect that.

| Concern | Choice | Why |
|---|---|---|
| **Containerization** | Podman (or Docker) | Team preference to try Podman — daemonless architecture and rootless containers by default are notable advantages, especially for self-hosted deployment. Docker remains the more widely-documented fallback if tooling/compatibility issues come up. |
| **Orchestration** | Podman/Docker Compose (single server) to start; Kubernetes only if scaling across multiple servers is later needed | With self-hosting and a monolith as the starting point, Compose is enough to run the app + Postgres + (optional) Redis on one server without extra operational overhead. Kubernetes only becomes relevant if the app later needs to run across multiple machines. |
| **Load balancing** | Reverse proxy (e.g., Nginx or Caddy) on the server | A managed cloud load balancer isn't applicable when self-hosting; a reverse proxy on the server handles routing/TLS and can front multiple app instances if needed later. |

## 7. Authentication

| Concern | Choice | Why |
|---|---|---|
| **Primary method** | Simple email + password (no email confirmation step) | Straightforward to implement for v1; skipping confirmation keeps onboarding frictionless, though it does mean unverified emails — worth revisiting if spam/fake accounts become a problem. |
| **Secondary method** | OAuth (e.g., Google/GitHub sign-in) | Offered alongside email+password as a faster, no-new-password option for users who prefer it. |

*Note: since deployment is self-hosted rather than on a managed cloud platform, password storage/hashing and session handling need to be implemented carefully in-app (e.g., proper hashing like bcrypt/argon2) rather than relying on a managed auth service — worth keeping in mind during implementation.*

## 8. Open Points

- **Backend language** — Java vs. Go — left open pending team preference and clearer scale expectations.
- **Frontend framework** — SolidJS is the current default (personal preference), with React as a fallback if ecosystem/hiring considerations end up mattering more.
- **Server specifics** — exact self-hosted server setup (single machine vs. a few, specs, OS) not yet defined.
- **OAuth provider(s)** — which specific providers (Google, GitHub, etc.) to support not yet decided.

---
*This is an example starting stack based on stated preferences — not a final decision. It's intended to be reviewed and adjusted by the team as scale needs and priorities become clearer.*
