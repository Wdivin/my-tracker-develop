# Draft GitHub bug report

## Title

Codex prematurely completes turns after small successful tool/commit steps while a long multi-stage task remains incomplete

## Product / variant

Observed in Codex IDE workflow and previously in the Codex desktop application.

## Environment

- Model: GPT-5.6 Terra
- Reasoning effort: Medium/High (visible in screenshots)
- OS: Windows 11
- Repository: private Laravel project
- Session/thread ID: `019f6725-60b5-7852-ad86-6c329eb3c2d4`
- Task: long multi-stage coding task with many file edits, MySQL migrations, tests, commits and pushes
- Context during incidents: about 23–30% remaining
- Exact client version / Windows build: to be added

## Summary

Codex repeatedly marks the current turn complete after a small successful local step, migration fix, commit, or progress message even though the current stage and overall task are still incomplete. The prompt explicitly instructs Codex to continue through stages 1–14 and not stop after tool calls, commits, pushes, tests, or completed substeps.

A short manual `continue` immediately resumes work from the same state. The premature completion then recurs after another small step.

This occurred without GitHub Actions running on every push and with substantial context remaining. Similar behavior was also observed in the desktop application, so it does not appear limited to the VS Code UI.

## Steps to reproduce

1. Open an existing trusted repository in Codex.
2. Give Codex a long implementation task containing many explicit stages.
3. Explicitly instruct it not to return control after commits, tests, pushes, or stage boundaries and to stop only on an external blocker or complete delivery.
4. Let it work through one or more tool calls.
5. Observe that it emits a short progress/final-style message after a local success and the turn ends while the task is incomplete.
6. Send `continue`.
7. Observe that work immediately resumes and may prematurely complete again after another small step.

## Actual behavior

Examples from one session:

- Completed stages 1–2, then stopped despite explicit continuous-execution instruction.
- Worked 1m04s on a partial stage 3 and stopped after five files.
- Completed stage 3 and stopped instead of starting stage 4.
- Fixed a MySQL migration issue and stopped while stage 4 remained incomplete.
- Committed workflow foundation and stopped after 31 seconds.
- After an explicit self-diagnostic prompt, added only TaskType + one test, committed, and stopped after 37 seconds.
- The model explicitly said it had incorrectly treated successful local steps/commits as natural completion points and claimed to change strategy; it then added only Task + migration, committed, and stopped again after 32 seconds.

No external blocker was reported for the majority of these turns.

## Expected behavior

When the task is incomplete, Codex should continue with another tool call or provide a non-empty blocking question/error. It should not mark the turn complete merely because a local step or commit succeeded, especially when the assistant message itself says that the stage “continues”.

## Workaround

Send a short manual follow-up such as:

```text
Continue from the current state. The current stage is not complete.
```

This immediately resumes work, but the premature completion can recur.

## Evidence

- Session ID and commit chronology are available.
- Multiple screenshots show `Worked for 27s–1m09s`, tiny file changes, incomplete stage text, and the input box becoming available again.
- Exact prompts explicitly forbidding intermediate stops are preserved.
- Repository commits involved include `deb6b4c`, `e95d8e0`, `0d556fc`, and `f356596`.

## Possibly related

- #27352 — Codex CLI marks turn complete while follow-up is still needed after progress message
- #32389 — GPT-5.6 Terra intermittently returns an empty successful final response after tool use, prematurely ending agent loops
- #26860 — model stops automatically mid-task

## Additional notes

This is not explained by context exhaustion: incidents occurred with roughly 23–30% context remaining, and the same setup normally auto-compacts near 8–9% and continues.

Automatic CI noise was removed in the clean reproduction. Local PHP/MySQL/Redis services were available and tests/migrations continued to work after each manual resume.
