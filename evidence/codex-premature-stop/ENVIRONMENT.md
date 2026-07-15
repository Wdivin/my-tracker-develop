# Environment evidence

The following values were captured from the affected machine on 2026-07-15.

- VS Code extension: **Codex — OpenAI's coding agent**
- Extension identifier: `openai.chatgpt`
- VS Code Codex extension version: `26.707.71524`
- Extension auto-update: enabled
- Codex desktop application version: `26.707.72221`
- Codex desktop application release date shown in About: `2026-07-14`
- Operating system: Windows 11 Pro
- Windows version: `25H2`
- OS build: `26200.8875`
- Model shown in affected sessions: GPT-5.6 Terra
- Reasoning effort shown in the clean reproduction: Medium
- Subscription: ChatGPT Plus
- Primary affected session/thread ID: `019f6725-60b5-7852-ad86-6c329eb3c2d4`
- Repository used for the clean reproduction: `Wdivin/my-tracker-codex-v2`
- Branch: `main`

The same general premature-stop behavior had previously been observed in the Codex desktop application, while the clean, well-documented reproduction was captured in the VS Code extension.

## Screenshot filenames

The binary screenshots are kept in the companion evidence archive and should be copied into `evidence/codex-premature-stop/evidence/screenshots/`:

- `environment-vscode-extension-26.707.71524.png`
- `environment-codex-app-26.707.72221.png`
- `environment-windows-11-25H2-build-26200.8875.png`
