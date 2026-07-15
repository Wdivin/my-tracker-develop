# Log analysis — 2026-07-15 premature turn completion

## Confirmed environment

- VS Code Codex extension: `26.707.71524`
- Codex desktop app: `26.707.72221`
- OS: Windows 11 Pro 25H2, build `26200.8875`
- Model: GPT-5.6 Terra
- Reasoning effort: Medium
- Session ID: `019f6725-60b5-7852-ad86-6c329eb3c2d4`

## Captured incident

- Turn started in the Codex log at approximately `2026-07-15 23:20:23.635`.
- The user-visible turn ended after about 30 seconds with commit `7afbbb5` and only three changed files (`+11`).
- The task and stage were still incomplete, but the input box became available again.
- At approximately `2026-07-15 23:22:04`–`23:22:06`, the Codex output channel repeatedly logged:

  - `[IpcClient] Received broadcast but no handler is configured method=thread-stream-state-changed`
  - `[desktop-notifications][global-error] ResizeObserver loop completed with undelivered notifications.`

- The available output did not include an explicit `task_complete`, `turn/completed`, `stopReason`, `model_needs_follow_up`, or blocking error for this incident.

## Interpretation

The timing correlation makes the `thread-stream-state-changed` burst relevant evidence, but it is not enough to identify the root cause. The `ResizeObserver` messages look UI-related and should not be presented as the confirmed cause. The important fact is that the client received many thread stream state broadcasts around turn completion while no handler was configured, and the turn ended without an explicit external blocker.

## Other observations from the same log

- `Agent Sessions` output channel was empty.
- `Extension Host` mostly contained extension activation information and did not expose the model stop reason.
- The Codex extension successfully spawned the Codex app-server and created the affected conversation.
- WSL detection failed because WSL was not installed, but the project was intentionally running locally on Windows/OpenServerPanel and continued to execute successfully; this is not considered the cause.
- PowerShell shell snapshot support was unavailable, but normal commands, migrations, tests, commits, and pushes continued to work.
- Several MCP servers returned unsupported `resources/list` warnings. These warnings did not prevent the agent from continuing after manual `continue` messages and are not currently considered the cause.

## Next evidence to capture

1. Use both commands from the Command Palette:
   - `Developer: Open Logs Folder`
   - `Developer: Open Extension Logs Folder`
2. Copy the complete current VS Code session log directory before restarting VS Code.
3. Preserve the Codex output log as a text file.
4. Search copied logs for:
   - the session ID;
   - the turn ID for the incident;
   - `turn/completed`;
   - `task_complete`;
   - `stopReason`;
   - `needs_follow_up`;
   - `model_needs_follow_up`;
   - `thread-stream-state-changed`.
5. Record the exact local time of the next premature completion and the next manual `continue`.

## Privacy

Before publishing, redact account tokens, cookies, environment secrets, database passwords, private repository URLs if desired, and private source content not required for reproduction.
