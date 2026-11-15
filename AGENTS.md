# OrderFlow — Codex collaboration guide

## Project context
OrderFlow is a backend-only educational MVP for a **single online store** with **one shared warehouse** and an **internal mock payment**. Roles: `CUSTOMER`, `MANAGER`, `ADMIN`. Architecture: **modular monolith**, not microservices.

Read `docs/requirements.md`, `docs/business-rules.md`, and `docs/data-model.md` before making domain changes. Those documents are draft specifications; surface contradictions or missing decisions instead of inventing rules.

## Technology baseline
- Java 21, Spring Boot, Gradle Wrapper, PostgreSQL, Liquibase, Lombok.
- Spring MVC, Spring Data JPA, Jakarta Validation; Spring Security/JWT when implementing identity.
- JUnit 5 and Mockito; Testcontainers/PostgreSQL for integration and concurrency tests.
- Use the actual versions and dependencies in `build.gradle` as the source of truth. Do not upgrade or add libraries without explaining why and asking first.
- UUID identifiers; `BigDecimal` for monetary values; database migrations through Liquibase.

## Working agreement (important)
1. **Plan before coding.** When asked to inspect, plan, or explain, do not edit files. Describe scope, files affected, tests, and risks; wait for explicit approval to implement.
2. **Keep tasks small.** Prefer one testable change that fits approximately 20–40 minutes. Avoid multi-module implementations in one step.
3. **Teach, don't replace.** Explain significant design choices, trade-offs, and what the developer should review in the diff. Do not assume the developer wants all code generated automatically.
4. **Stay within MVP.** No Redis, RabbitMQ, Kubernetes, external payments, shipping module, frontend, or microservice split unless explicitly requested. Put nice-to-have ideas in a backlog.
5. **Preserve project boundaries.** Keep business rules in application/domain services, HTTP concerns in controllers, persistence in repositories. Avoid bidirectional JPA relationships by default; use use-case-specific DTOs/records. Do not use Lombok `@Data` on JPA entities.
6. **Respect invariants.** Reservation must be atomic, inventory cannot become negative, order prices are server-calculated snapshots, and repeat payment handling must not double-change stock. Design concurrency tests before considering these features complete.
7. **Do not hide failures.** Never silently change requirements, disable tests, delete unrelated code, or commit/push without being asked. Ask before destructive or broad changes.
8. **Finish with a handoff.** Summarize changes, tests actually run (or why not), and one concrete next action. Propose an update to `docs/progress.md`; only write it when authorized as part of the task.

## Definition of done for a task
- Requested behavior works and is scoped to the agreed task.
- Relevant tests are added or updated and run where possible.
- Error handling, validation, and security implications are considered.
- No unexplained new dependencies or architectural changes.
- The developer can explain the main implementation and knows the next small step.

## First session
If this is the first interaction, inspect the repository and the docs, report gaps and a short Sprint 0 plan. **Do not create or change files until the developer approves a specific task.**

## Backlog Management

The project backlog is maintained in `docs/backlog.md`.

Rules:

1. Use `docs/backlog.md` as the source of truth for planned work.
2. Each task must have a unique ID, title, priority, status, and phase.
3. Task statuses: `Todo`, `In Progress`, `Blocked`, `Done`.
4. Priorities: `P0` (required for MVP), `P1` (important), `P2` (optional).
5. Break large tasks into small, independently verifiable implementation steps.
6. Keep at most one implementation task `In Progress`.
7. Do not expand the MVP scope without explicit user approval.
8. Add optional ideas and future technologies to the backlog instead of implementing them immediately.
9. Do not mark tasks `Done` until the user has reviewed and accepted the result.
10. After completing an accepted task, update `docs/backlog.md` and `docs/progress.md`.
11. Always record the next small, concrete action in `docs/progress.md`.
12. Do not start a new task automatically after finishing the current one.

When proposing the next task, explain its purpose, expected files, acceptance criteria, and estimated scope.

Communicate with the user in Russian.

## Git Workflow

OrderFlow uses a simplified GitFlow workflow. The initial project setup was performed on `main`; the task-branch workflow starts with `OF-002`.

### Branches
- `main` — stable project baseline and releases. Do not implement regular tasks directly on `main`.
- `dev` — integration branch for ongoing development.
- `feature/OF-XXX-short-description` — implementation branch for one backlog task (for example, `feature/OF-002-postgres`).
- `fix/OF-XXX-short-description` — branch for a bug fix linked to a backlog task.

Create task branches from the latest `dev`. Merge completed branches back into `dev` through Pull Requests. Merge `dev` into `main` when preparing a stable release.

### Before starting a task
1. Read `docs/backlog.md` and `docs/progress.md`; identify the approved task ID.
2. Inspect the current branch and working tree with `git branch --show-current` and `git status`.
3. Suggest the task branch name and ask for approval before creating or switching branches.
4. If uncommitted changes exist, discuss how to handle them before switching branches. Do not discard or silently carry unrelated changes into another task.
5. Keep each task branch focused on one backlog item; do not mix unrelated work.

### Git operation safety
- Do not run `git commit`, `git push`, `git merge`, `git rebase`, `git reset --hard`, delete branches, or create Pull Requests without explicit user approval.
- Never force-push without separate, explicit approval.
- Do not modify project files directly on `main` or `dev` unless explicitly authorized.
- Explain the intended operation and its effects before changing repository history or remote state.
- Do not start the next backlog task automatically after completing the current one.

### Commits and Pull Requests
- Suggest concise Conventional Commit messages containing the task ID, such as `chore(OF-002): add PostgreSQL Docker Compose` or `feat(OF-003): add initial Liquibase migrations`.
- Before proposing a Pull Request, report the relevant build and test results honestly. Do not treat a build with skipped tests as a fully verified build.
- Propose a Pull Request from the task branch into `dev` only after the agreed acceptance criteria are met or any remaining blockers are clearly documented.
- Follow the existing backlog and progress management rules: do not mark a task `Done` before user review and acceptance, and update `docs/backlog.md` and `docs/progress.md` only as authorized.

## Session Progress Management

Maintain `docs/progress.md` as a lightweight development journal.

At the beginning of each development session:
1. Read `docs/backlog.md` and `docs/progress.md`.
2. Identify the current task and the last recorded stopping point.
3. Suggest one small, concrete action for the current session.
4. Wait for user approval before changing code.

At the end of each development session:
1. Summarize what was actually implemented.
2. Record the current backlog task ID.
3. Record which tests were run and their results.
4. Document blockers and unfinished work.
5. Write one specific first action for the next session.
6. Update `docs/progress.md` when the user asks to finish the session.
7. Propose backlog status changes, but do not mark tasks Done without user acceptance.

Keep the progress file concise. Preserve a short history of completed sessions.

Do not invent completed work or successful test results.

Do not automatically start another task after updating progress.

Communicate with the user in Russian.