# GPT-5.6 Terra vs GPT-5.6 Sol — same-session A/B observation

Date: 2026-07-16

## Purpose

This note records a same-session comparison observed during the My Tracker implementation experiment. It is not a controlled laboratory benchmark, but it is a strong practical A/B observation because the repository, client, task, session, reasoning level, tools, and local environment remained unchanged while only the selected model changed.

## Fixed conditions

- Client: Codex extension for VS Code
- Session ID: `019f6725-60b5-7852-ad86-6c329eb3c2d4`
- Repository: `Wdivin/my-tracker-codex-v2`
- Branch: `main`
- Workload: one approved multi-stage implementation plan, stages 1–14
- Reasoning level: Medium
- Environment: Windows 11, PHP 8.4, Laravel 13, MySQL 8.4, Redis, OpenServerPanel
- User instruction: continue through the approved plan without returning control after intermediate commits, tests, or small local substeps

## GPT-5.6 Terra result

With GPT-5.6 Terra selected, Codex repeatedly ended the current turn after small successful local steps such as:

- a migration fix;
- a small model or test addition;
- a commit;
- a partial stage result;
- a progress message explicitly stating that more work would follow.

The behavior recurred across many turns. A manual `continue` immediately resumed productive work from the same repository state. Client-side logs captured 16 separate turn boundaries during the affected period. No genuine external blocker, context exhaustion, or required CI wait was identified.

## Switch to GPT-5.6 Sol

The model was changed from GPT-5.6 Terra to GPT-5.6 Sol while keeping Medium reasoning and continuing in the same session, repository, branch, task, and environment.

Only one user continuation command was needed at the switch boundary.

## GPT-5.6 Sol result

After that single continuation, GPT-5.6 Sol continued without further manual prompting and:

- completed the remainder of stage 5;
- completed stages 6–14;
- performed the final acceptance pass;
- edited 133 files during the continuous completion run;
- committed and pushed the final result;
- left `main` synchronized with `origin/main` and the worktree clean.

Final implementation commit:

`b0c11075766a4c32e50e9e4514f5d1f1cd255306`

Final reported checks included:

- backend: 25 tests, 122 assertions;
- CI E2E: 14 passed, 6 intentional skips;
- local E2E and visual: 17 passed, 8 skips;
- TypeScript, ESLint, Prettier, Vite, Pint, and PHPStan successful;
- Composer and npm audits: 0 vulnerabilities;
- OpenAPI valid with 0 warnings;
- clean install and upgrade on MySQL 8.4 successful;
- backup/restore drill with matching database rows and SHA-256 values for all 46 private files.

The final acceptance report is available in the implementation repository at `docs/ACCEPTANCE_REPORT.md`.

## Interpretation

This observation strongly suggests that the repeated premature turn completion was model-specific behavior associated with GPT-5.6 Terra in this workflow rather than a repository, VS Code, tool-access, context, or task-design limitation.

It does not prove the exact internal failure mechanism. Server-side telemetry would still be required to determine whether the root cause was model output, turn-finalization logic, or their interaction.

## Screenshot evidence

Expected screenshot path in this evidence repository:

`evidence/codex-premature-stop/screenshots/13-sol-medium-stages-9-10-and-start-11.png`

The screenshot shows GPT-5.6 Sol selected at Medium reasoning, stages 9 and 10 completed, and stage 11 already running without a manual continuation between those stages.
