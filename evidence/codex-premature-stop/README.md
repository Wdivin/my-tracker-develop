# Codex premature turn completion — evidence pack

Prepared from the My Tracker comparison experiment on 2026-07-15.

## Summary

During a long multi-stage implementation task, Codex repeatedly ended the current turn after a small successful local step, commit, migration fix, or partial stage result even though:

- the overall task explicitly required continuous execution through stages 1–14;
- the current stage remained incomplete in several cases;
- there was no external blocker in most incidents;
- context was not exhausted;
- GitHub Actions on work-branch pushes had been removed;
- a manual `continue` immediately resumed productive work;
- the model itself later stated that it had incorrectly treated a successful local step or commit as a natural turn-completion point, but the behavior still continued.

The behavior was observed in the Codex IDE workflow and had also been observed earlier in the Codex desktop application, so the working hypothesis is a core agent-loop/model-behavior issue rather than a VS Code-only issue.

## Known environment

- Repository: `Wdivin/my-tracker-codex-v2` (private)
- Branch: `main`
- Task type: long, multi-stage Laravel implementation
- Model visible in screenshots: `5.6 Terra`, Medium or High reasoning depending on turn
- Operating system visible from earlier screenshots/context: Windows 11
- Session/thread ID visible in all captured IDE incidents: `019f6725-60b5-7852-ad86-6c329eb3c2d4`
- Context during incidents: approximately 23–30% remaining; previous experience shows Codex compacts automatically around 8–9%, so context exhaustion was not involved
- GitHub Actions: not running on every intermediate push in this clean reproduction
- Local services: OpenServerPanel, PHP 8.4, MySQL 8.4, Redis, Apache

Exact Codex App/IDE extension version and Windows build still need to be collected.

## Reproduction pattern

1. Give Codex a large implementation task split into explicit stages.
2. Explicitly instruct it to continue through all stages without returning control after commits, tests, builds, or completed substeps.
3. Codex works normally and may complete one or more useful actions.
4. It emits a short progress/final-style message such as “stage continues”, “commit pushed”, or “migration now passes”.
5. The turn ends even when the stage and overall task remain incomplete.
6. Send `continue` or an equivalent short follow-up.
7. Codex immediately resumes from the same repository state.
8. The premature completion can recur after another small successful step.

## Similar upstream reports

- openai/codex#27352 — “Codex CLI marks turn complete while follow-up is still needed after progress message”
- openai/codex#32389 — “GPT-5.6 Terra intermittently returns an empty successful final response after tool use, prematurely ending agent loops”
- openai/codex#26860 — “GPT-5.5 xhigh via Amazon Bedrock stops automatically mid-task in Codex CLI”

The closest match is #27352: a progress message is emitted, the turn is marked complete, and the promised next action never happens. #32389 is also relevant because it concerns GPT-5.6 Terra, successful `stop`, and immediate recovery after a manual `continue`.

## Contents

- `INCIDENTS.md` — chronology and known commits
- `PROMPTS.md` — exact continuation and diagnostic prompts used
- `BUG_REPORT_DRAFT.md` — ready-to-adapt GitHub issue draft
- `MISSING_DATA.md` — remaining diagnostics to collect
- `evidence/screenshots/` — screenshots captured in the conversation (included in the downloadable archive; not yet committed to GitHub because the available connector cannot upload binary files)
