# VS Code log archive manifest

Source archive: `!VSCode_Logs.zip`

Captured session folder: `20260715T215332`

Primary affected Codex session:

- conversation/session ID: `019f6725-60b5-7852-ad86-6c329eb3c2d4`
- affected turn near 23:20–23:22: `019f6770-0a1f-7661-a441-8dd66954fdd2`
- later manual-resume turn beginning at 23:21:34: `019f6771-1d38-7130-9328-0500db7c4c62`

## Relevant files

| Path inside archive | Size | SHA-256 | Notes |
|---|---:|---|---|
| `20260715T215332/window1/exthost/openai.chatgpt/Codex.log` | 492897 | `b71e21ffb70f1bf95636916595d8daa29ee96dbf9fcd83cbbc702aa1b417e19f` | Primary Codex extension log |
| `20260715T215332/window1/exthost/exthost.log` | 2841 | `14a763ed05a1a13b4107088501bcc2e99cd2b3c160db21af8333bd6ed20a1abd` | Extension-host lifecycle log |
| `20260715T215332/window1/renderer.log` | 2569 | `de72d5db746cea1bd08a057390912baca5adb2f5d04b852bc94c2d8f8e8e27cc` | VS Code renderer log |
| `20260715T215332/terminal.log` | 28977 | `aace55dfc269d1970f9824a4637826bf1740f29e90d8e14acfebddbadd0d6973` | Terminal service log |
| `20260715T215332/window1/exthost/vscode.git/Git.log` | 117723 | `78d5e4e997276b031c98ef494feb542b1913f631aec5114fe380a4bf75c25613` | Git extension log |
| `20260715T215332/window1/output_20260715T215334/agentSessionsOutput.log` | 0 | `e3b0c44298fc1c149afbf4c8996fb92427ae41e4649b934ca495991b7852b855` | Agent Sessions output was empty |

The archive also contains logs from GitHub Copilot and authentication extensions. Those are not needed for the Codex bug report unless OpenAI support explicitly requests the entire original archive.

## Privacy check

A keyword scan of the primary `Codex.log` found no obvious API keys, bearer headers, passwords or token assignments. The log still contains local Windows paths and internal session/turn identifiers; review these before publishing in a public issue.
