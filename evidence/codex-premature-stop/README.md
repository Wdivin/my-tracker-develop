# Codex premature turn completion — evidence pack

Prepared from the My Tracker comparison experiment on 2026-07-15 and updated on 2026-07-16.

## Summary

During a long multi-stage implementation task, Codex repeatedly ended the current turn after a small successful local step, commit, migration fix, or partial stage result even though:

- the overall task explicitly required continuous execution through stages 1–14;
- the current stage remained incomplete in several cases;
- there was no external blocker in most incidents;
- context was not exhausted;
- GitHub Actions on work-branch pushes had been removed;
- a manual `continue` immediately resumed productive work;
- the model itself later stated that it had incorrectly treated a successful local step or commit as a natural turn-completion point, but the behavior still continued.

The behavior was observed in the Codex IDE workflow and had also been observed earlier in the Codex desktop application, so the original working hypothesis was a core agent-loop/model-behavior issue rather than a VS Code-only issue.

A later same-session comparison strengthened the model-specific hypothesis: after switching from GPT-5.6 Terra to GPT-5.6 Sol at the same Medium reasoning level, Sol completed the remaining stages 5–14 after one continuation command and did not require further manual prompting. See `SOL_VS_TERRA_AB_TEST.md`.

## Known environment

- Evidence repository: `Wdivin/my-tracker-develop`
- Implementation repository: `Wdivin/my-tracker-codex-v2`
- Branch: `main`
- Task type: long, multi-stage Laravel implementation
- Affected model: `GPT-5.6 Terra`, primarily Medium reasoning
- Comparison model: `GPT-5.6 Sol`, Medium reasoning
- Operating system: Windows 11 Pro 25H2, build `26200.8875`
- VS Code Codex extension: `openai.chatgpt` version `26.707.71524`
- Codex desktop app: version `26.707.72221`
- Session/thread ID: `019f6725-60b5-7852-ad86-6c329eb3c2d4`
- Context during incidents: approximately 23–30% remaining; context exhaustion was not involved
- GitHub Actions: not running on every intermediate push in this clean reproduction
- Local services: OpenServerPanel, PHP 8.4, MySQL 8.4, Redis, Apache

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
- `ENVIRONMENT.md` — exact client and OS versions
- `LOG_ANALYSIS_2026-07-15.md` — analysis of the full Codex client log
- `SOL_VS_TERRA_AB_TEST.md` — same-session comparison between Terra and Sol
- `logs/` — full and sanitized client-side logs, turn boundaries, privacy review, and incident excerpts
- `screenshots/` — captured IDE, environment, and diagnostic screenshots
