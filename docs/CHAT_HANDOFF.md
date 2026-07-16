# My Tracker — clean chat handoff

Updated: 2026-07-16.

## Purpose

Continue the My Tracker comparison and acceptance work in a fresh chat without loading archived tooling-incident details into the active context.

## Project goal

Build and objectively compare two independent implementations of the same My Tracker 1.0 brief, then perform acceptance, identify gaps, and decide which implementation or combination should become the basis for further development.

## Repositories

- Requirements, evaluation materials, and shared project documentation: `Wdivin/my-tracker-develop`
- Codex implementation: `Wdivin/my-tracker-codex-v2`
- Claude implementation: `Wdivin/my-tracker-claude`
- Earlier Codex attempt retained only for history: `Wdivin/my-tracker-codex`

## Technology target

- Laravel 13
- PHP 8.4
- MySQL 8.4
- Vue with Inertia
- Redis
- Local development under OpenServerPanel/Windows

## Current implementation status

### Codex implementation

My Tracker 1.0 stages 1–14 are reported complete.

- Repository: `Wdivin/my-tracker-codex-v2`
- Branch: `main`
- Final reported commit: `b0c11075766a4c32e50e9e4514f5d1f1cd255306`
- Acceptance report: `docs/ACCEPTANCE_REPORT.md`
- Implementation plan: `docs/IMPLEMENTATION_PLAN.md`
- Reported final checks include backend tests, browser E2E/visual checks, TypeScript, ESLint, Prettier, Vite, Pint, PHPStan, dependency audits, OpenAPI validation, MySQL clean-install/upgrade checks, Redis, backup/restore, access control, localization, accessibility, optimistic locking, API, search, and operational scenarios.
- Declared limitation: mobile testing used browser device profiles rather than physical iOS/Android devices.

### Claude implementation

Claude reported My Tracker 1.0 complete earlier.

- Repository: `Wdivin/my-tracker-claude`
- Reported completion commit: `04288936b5191a62f4194285501c3ae0a121ca2c`
- Formal acceptance was intentionally postponed until the Codex implementation was complete so both versions could be evaluated consistently.

## Next main task

Perform structured acceptance and comparison of the Codex and Claude implementations against the same brief.

Recommended sequence:

1. Verify both repositories are on the expected final commits and have clean `main` branches.
2. Read each implementation's acceptance/status documentation.
3. Build a single acceptance matrix from the original brief.
4. Verify high-risk and load-bearing requirements rather than trusting only self-reported completion.
5. Run equivalent installation, migration, backend, frontend, E2E, security, API, and operational checks where practical.
6. Record missing, partial, divergent, or over-engineered areas for each implementation.
7. Compare architecture, maintainability, UX, test quality, documentation, deployment readiness, and implementation scope.
8. Decide whether to select one implementation, merge ideas manually, or issue targeted correction tasks.

## Working rules

Follow `codex-project-workflow.md`:

- discuss and agree scope before giving Codex a command;
- discuss implementation alternatives when more than one reasonable option exists;
- use a clean branch from current `main` for development changes;
- keep each accepted task focused;
- evaluate Codex reports by scope, changed modules, and checks, requesting full diff review only when risk or anomalies justify it;
- do not mix unrelated diagnostic history into active product-development decisions.

## External monitoring reminder

Periodically check the external issue recorded in `docs/ISSUE_WATCH.md` and report only meaningful status changes or requests requiring action. The archived evidence itself is not part of the active development context.
