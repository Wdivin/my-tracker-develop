# Remaining data to collect

## Highest priority

1. Exact Codex IDE extension version
   - Open Extensions in VS Code.
   - Find Codex/OpenAI extension.
   - Copy the full version number.
   - Also open the Codex panel menu or About dialog if present and capture a screenshot.

2. Exact Codex desktop app version
   - Open Codex App.
   - Use Settings/About.
   - Copy the full version/build number.

3. Windows version and build
   - Press `Win + R`.
   - Run `winver`.
   - Screenshot the dialog or copy edition, version and OS build.

4. Exact local timestamps for the next 2–3 incidents
   - Record in `YYYY-MM-DD HH:MM:SS Europe/Kyiv` format.
   - Include the moment the turn ended and the moment `continue` was sent.

5. Client logs around the next incident
   - VS Code: `View -> Output`, select Codex/OpenAI/Extension Host channels and save relevant lines.
   - Run `Developer: Open Logs Folder` from Command Palette.
   - Copy Codex-related logs covering roughly two minutes before and after the event.
   - Search for: `turn completed`, `task_complete`, `stopReason`, `needs_follow_up`, `model_needs_follow_up`, `turn/completed`, `response_id`.
   - Redact secrets before sharing publicly.

## Useful additions

6. Full transcript export of the affected Codex thread, if the app offers Export/Copy conversation.

7. A/B reproduction:
   - same minimal task with GPT-5.6 Terra Medium;
   - same task with another available model/reasoning level;
   - note whether only one variant stops prematurely.

8. Minimal public reproducer repository

Suggested prompt:

```text
Create and commit ten files named step-01.txt through step-10.txt, one commit per file. After every commit immediately continue to the next file. Do not end the turn until all ten files exist and all ten commits are complete. Stop only on a genuine external blocker.
```

Record how many files/commits are completed before the first premature turn completion.

9. Desktop-app reproduction session ID and screenshots, because it helps show the issue is not IDE-extension-specific.

## Privacy review before publishing

Remove or redact:

- API keys and tokens;
- `.env` contents;
- database passwords and internal hosts;
- private repository URLs if desired;
- personal email addresses;
- private source code not required for reproduction;
- account/session cookies.
