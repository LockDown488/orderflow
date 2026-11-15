# OrderFlow — progress journal

This file is a **restart point**, not a productivity scorecard. A 10-minute session counts. Missed days do not need to be made up.

## Current status
- **Stage:** Sprint 0 — project foundation
- **Completed:** OF-001 accepted by developer; manual ` .\gradlew.bat build -x test` succeeded (developer-reported). Spring Boot 4.1.2 retained and requirements corrected.
- **Current focus:** OF-002 accepted by developer and marked `Done`; preparing approved local commits on `feature/OF-002-postgres`.
- **Blockers:** No confirmed PostgreSQL blocker. Full application build with tests remains unverified.

## Next tiny step
1. Review the local commits, then obtain explicit approval for push and a PR into `dev`. No push, PR or new implementation task is authorized yet.

## Session log

### 2026-11-15 — OF-002 commit preparation
- **Decision:** Developer approved separate local commits for collaboration rules and PostgreSQL setup/journal. `docs/project-context.md` stays local and is excluded via `.git/info/exclude`; inspection found no tracked file or commits for that path in local branch history.
- **Checks:** Diff reviewed; `.env` is ignored. No additional application tests run.
- **Next action:** Review the resulting local commits before separately authorizing push and PR into `dev`.

### 2026-11-15 — OF-002 accepted
- **Done:** Developer explicitly accepted OF-002; backlog status changed to `Done`. Developer reported that the database password differed from `.env`, changed it to match, and successfully connected from IDEA. Cause of the original password mismatch remains unknown.
- **Checks:** Prior Compose validation, healthcheck, SQL, TCP authentication and volume persistence checks passed. Developer confirmed successful PowerShell and IDEA connections. No additional build or tests run at closure.
- **Blockers:** No confirmed PostgreSQL blocker. Files remain uncommitted; no commit, push, merge or PR performed.
- **Next action:** Review the diff and agree commit contents, including how to handle existing untracked project documentation, before the OF-002 PR into `dev`. No new implementation task started.

### 2026-11-15 — OF-002 PostgreSQL (awaiting acceptance)
- **Done:** Created approved branch `feature/OF-002-postgres`, preserving existing untracked documents. Added `compose.yaml` with PostgreSQL 18.7, localhost port, named volume and healthcheck; `.env.example`; `.env` ignore rule; setup instructions in `README.md`. Generated a local password in ignored `.env` without printing it. No application code or dependencies changed.
- **Checks:** Compose validation passed; container reached `healthy`; `SELECT 1` returned 1; a probe value of 42 survived `--force-recreate`, and the test table/schema were removed afterward. Git confirmed `.env` is ignored. No application tests run for this infrastructure task.
- **Blockers:** Sandbox blocked Git metadata writes and Docker access; approved elevated commands successfully created the branch and operated Docker. Existing untracked guidance/journal files remain uncommitted.
- **Next action:** Developer reviews Compose and README, checks connection if desired, and accepts OF-002. Status remains `In Progress`; no commit, push, PR or next task started. Container remains running.

### 2026-11-15 — OF-001 accepted
- **Done:** Developer reported successful manual `.\gradlew.bat build -x test` and explicitly accepted OF-001; backlog status changed to `Done`.
- **Tests:** Excluded by `-x test`; no passing test result claimed. Codex did not rerun the build.
- **Blockers:** Earlier cache access failure did not prevent the developer's manual build; its cause was not established.
- **Next action:** Developer independently configures and runs PostgreSQL in Docker. OF-002 status remains `Todo`; no implementation started by Codex.

### 2026-11-15 — OF-001 baseline verification (initial findings)
- **Done:** Inspected project and dependencies. Kept Spring Boot 4.1.2 by developer decision and corrected `docs/requirements.md` to 4.x. Wrapper successfully ran Gradle 9.8.2. Confirmed installed Temurin JDK 21.0.13.
- **Checks:** `git diff --check` passed before this journal update. Initial Wrapper launch could not write to `C:\.gradle`; using `--gradle-user-home .gradle/user-home` worked. Build under default JDK 23 failed to detect toolchain 21. Retried under explicit JDK 21; build reached `compileJava` and failed accessing `spring-boot-starter-jackson-4.1.2.jar` in `.gradle/user-home/caches`. Tests did not run; successful compilation is not confirmed.
- **Command (PowerShell, project root):** `$env:JAVA_HOME = 'C:/Users/artem/.jdks/temurin-21.0.13'; .\gradlew.bat --gradle-user-home .gradle/user-home build --console=plain`
- **Blockers:** `java.nio.file.AccessDeniedException` for the cached dependency JAR; cause not yet established. No build scripts, system settings, or tests changed.
- **Next action:** Repeat the command above with `--stacktrace` and identify the cache access failure before changing project configuration. OF-001 remains `In Progress`, pending developer review; no subsequent task started.

### 2026-11-15 — Setup
- **Done:** Created the repository and pushed the initial project; prepared documentation and Codex guidance.
- **Next action:** Review the Codex plan for the first small Sprint 0 task.
- **Start here:** Open `docs/progress.md` and the Codex panel in IntelliJ IDEA.
- **Blockers:** None confirmed.

## Template for future sessions

### YYYY-MM-DD — Short session title
- **Done:**
- **Next action (one specific action):**
- **Start here (file or command):**
- **Blockers:**
