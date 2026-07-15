# Remaining data to collect

## Already collected

- VS Code Codex extension version: `26.707.71524`
- Extension identifier: `openai.chatgpt`
- Codex desktop application version: `26.707.72221`
- Desktop application release date shown in About: `2026-07-14`
- Windows edition: Windows 11 Pro
- Windows version: `25H2`
- Windows OS build: `26200.8875`
- Model: GPT-5.6 Terra
- Reasoning effort in the clean reproduction: Medium
- Subscription: ChatGPT Plus
- Primary session/thread ID: `019f6725-60b5-7852-ad86-6c329eb3c2d4`
- Multiple screenshots, prompts, commit IDs and incident chronology

## Highest priority still missing

1. Exact local timestamps for the next 2–3 incidents
   - Record in `YYYY-MM-DD HH:MM:SS Europe/Kyiv` format.
   - Include the moment the turn ended and the moment `continue` was sent.

2. Client logs around the next incident
   - VS Code: `View -> Output`, select Codex/OpenAI/Extension Host channels and save relevant lines.
   - Run `Developer: Open Logs Folder` from Command Palette.
   - Copy Codex-related logs covering roughly two minutes before and after the event.
   - Search for: `turn completed`, `task_complete`, `stopReason`, `needs_follow_up`, `model_needs_follow_up`, `turn/completed`, `response_id`.
   - Redact secrets before sharing publicly.

3. Full transcript export of the affected Codex thread, if the app offers Export/Copy conversation.

## Useful additions

4. A/B reproduction:
   - same minimal task with GPT-5.6 Terra Medium;
   - same task with another available model/reasoning level;
   - note whether only one variant stops prematurely.

5. Minimal public reproducer repository

Suggested prompt:

```text
Create and commit ten files named step-01.txt through step-10.txt, one commit per file. After every commit immediately continue to the next file. Do not end the turn until all ten files exist and all ten commits are complete. Stop only on a genuine external blocker.
```

Record how many files/commits are completed before the first premature turn completion.

6. Desktop-app reproduction session ID and exact timestamps, because it helps show the issue is not IDE-extension-specific.

## Privacy review before publishing

Remove or redact:

- API keys and tokens;
- `.env` contents;
- database passwords and internal hosts;
- private repository URLs if desired;
- personal email addresses;
- private source code not required for reproduction;
- account/session cookies.
