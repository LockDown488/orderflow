# OrderFlow — Product Backlog

> Source of truth for planned work. This is a **draft** based on our agreed MVP scope, not a report of completed work. Codex must inspect the repository and ask for confirmation before changing task statuses.

## Project scope

- Single online store, one shared warehouse.
- Roles: `CUSTOMER`, `MANAGER`, `ADMIN`.
- Modular monolith, REST API, backend only.
- Java 21, Spring Boot, Gradle, PostgreSQL, Liquibase, Lombok.
- Internal mock payment processing.
- **MVP first**. Advanced technologies are not release blockers.

## How to use this backlog

- **Statuses:** `Todo`, `In Progress`, `Blocked`, `Done`.
- **Priorities:** `P0` — necessary for MVP; `P1` — useful improvement; `P2` — later/optional.
- **Tracks:** `Sprint 0`, `MVP`, `Learning`, `Advanced`.
- Keep **at most one implementation task** `In Progress`.
- Entries marked `Epic` must be split into small verifiable tasks before coding; aim for roughly 20–40 minutes per session when practical.
- Codex may propose edits, but must not expand MVP scope or mark work `Done` without user review and acceptance.
- A task is `Done` only when its acceptance criteria are met and the user confirms it.
- After a session, update `docs/progress.md` with a specific first action for the next session.
- Do not start the next task automatically.

## Sprint 0 — foundation

| ID | Task | Priority | Status | Acceptance criteria |
|---|---|---|---|---|
| OF-001 | Verify Spring Boot project, JDK 21, Gradle Wrapper, dependencies and baseline build | P0 | Done | Repository inspected; build/test command succeeds or blockers documented |
| OF-002 | Configure local PostgreSQL using Docker Compose | P0 | Done | Database starts; connection settings are documented; secrets are not committed |
| OF-003 | Configure Liquibase and create first migration | P0 | Todo | Migration applies successfully to a clean local database |
| OF-004 | Agree module boundaries and package conventions | P0 | Todo | Short architecture note and initial package layout reviewed |
| OF-005 | Add basic CI checks for build and tests | P1 | Todo | GitHub Actions workflow passes on push/PR |

## MVP — functional epics

| ID | Epic | Priority | Status | Acceptance criteria |
|---|---|---|---|---|
| OF-006 | Identity: registration, authentication and role-based access | P0 | Todo | CUSTOMER can register/sign in; access controls are tested |
| OF-007 | Catalog: categories and products | P0 | Todo | Public read API; MANAGER/ADMIN manage catalog; validation works |
| OF-008 | Cart: add, change quantity, remove items | P0 | Todo | CUSTOMER manages own cart; invalid quantities are rejected |
| OF-009 | Inventory: stock and reservation | P0 | Todo | Available/reserved quantities remain valid under concurrent checkout |
| OF-010 | Orders: checkout and state transitions | P0 | Todo | Order stores item price/name snapshots; valid transitions enforced |
| OF-011 | Mock payment: success and failure flows | P0 | Todo | Successful payment finalizes order; failure releases reservation; repeated requests are safe |
| OF-012 | End-to-end MVP verification and release documentation | P0 | Todo | Core flows tested; README contains setup/run instructions and MVP limitations |

## Learning backlog — not MVP blockers

| ID | Task | Priority | Status | Why |
|---|---|---|---|---|
| OF-013 | Learn Codex Skills and create one useful project skill | P2 | Todo | Reuse a proven recurring workflow (e.g. code review or session wrap-up) |
| OF-014 | Explore ArchUnit for module boundary tests | P2 | Todo | Guard modular monolith boundaries when the codebase grows |

## Advanced backlog — after MVP

| ID | Task | Priority | Status | Why |
|---|---|---|---|---|
| OF-015 | Redis caching and cache invalidation | P2 | Todo | Learn caching trade-offs |
| OF-016 | RabbitMQ, events, outbox and DLQ | P2 | Todo | Explore asynchronous processing and reliability |
| OF-017 | Observability: metrics, tracing and dashboards | P2 | Todo | Learn operational diagnostics |
| OF-018 | Load testing, rate limiting and performance tuning | P2 | Todo | Measure and improve behavior under load |
| OF-019 | Kubernetes / Helm deployment experiments | P2 | Todo | Learn deployment and scaling after MVP |

## Parking lot

Put new ideas here instead of interrupting the current task. Codex should clarify whether an idea is MVP-critical before promoting it to a task.

- _No ideas recorded yet._

## Review checklist for Codex

Before proposing the next task:
1. Read this file, `docs/progress.md`, and relevant requirements.
2. Inspect actual repository state; do not infer completion from conversation history.
3. Suggest **one** small next action with files to touch and how to verify it.
4. Wait for user approval before modifying code.

At the end of a session:
1. Summarize changed files and tests actually run (distinguish passing, failing, not run).
2. Propose backlog status changes and ask the user to confirm acceptance.
3. Update `docs/progress.md` with a clear starting point for next time.
4. Do not silently start another task.
