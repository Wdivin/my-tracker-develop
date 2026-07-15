# Log excerpt: premature turn completion near 2026-07-15 23:22

Environment and session:

- Codex extension: `26.707.71524`
- model: GPT-5.6 Terra Medium
- conversation ID: `019f6725-60b5-7852-ad86-6c329eb3c2d4`
- turn ID: `019f6770-0a1f-7661-a441-8dd66954fdd2`
- visible UI result: Codex worked for about 30 seconds, committed a small local step, returned control while the wider stage remained incomplete

Relevant raw-log lines from `Codex.log`:

```text
2026-07-15 23:20:23.635 [info] Reasoning summary turn-start config resolved concurrentReasoningSummariesFeatureOverrideEnabled=false conversationId=019f6725-60b5-7852-ad86-6c329eb3c2d4 requestApprovalPolicy=null requestApprovalsReviewer=null requestPermissionProfile=null requestRuntimeWorkspaceRootCount=1 requestSandboxPolicyType=null requestSandboxWritableRootCount=0 resolvedApprovalPolicy=on-request resolvedApprovalsReviewer=auto_review summary=none useAppServerPermissionDefault=true
2026-07-15 23:20:23.652 [warning] [IpcClient] Received broadcast but no handler is configured method=thread-stream-state-changed
2026-07-15 23:20:26.965 [info] Reasoning summary item completed itemId=rs_03685aee08c75eab016a57eb8aa3608191976f31037dcd74b5 summary=[] summaryPartCount=0 threadId=019f6725-60b5-7852-ad86-6c329eb3c2d4 turnId=019f6770-0a1f-7661-a441-8dd66954fdd2
2026-07-15 23:20:27.024 [error] [desktop-notifications][global-error] ResizeObserver loop completed with undelivered notifications.
2026-07-15 23:20:45.874 [info] Reasoning summary item completed itemId=rs_03685aee08c75eab016a57eb9ba2cc8191817d04682e9aebc5 summary=[] summaryPartCount=0 threadId=019f6725-60b5-7852-ad86-6c329eb3c2d4 turnId=019f6770-0a1f-7661-a441-8dd66954fdd2
2026-07-15 23:20:58.766 [info] Reasoning summary item completed itemId=rs_03685aee08c75eab016a57ebaa7d508191b286f1bd2bd27dcf summary=[] summaryPartCount=0 threadId=019f6725-60b5-7852-ad86-6c329eb3c2d4 turnId=019f6770-0a1f-7661-a441-8dd66954fdd2
```

The next manually initiated turn starts shortly afterwards:

```text
2026-07-15 23:21:34.093 [info] Reasoning summary turn-start config resolved concurrentReasoningSummariesFeatureOverrideEnabled=false conversationId=019f6725-60b5-7852-ad86-6c329eb3c2d4 requestApprovalPolicy=null requestApprovalsReviewer=null requestPermissionProfile=null requestRuntimeWorkspaceRootCount=1 requestSandboxPolicyType=null requestSandboxWritableRootCount=0 resolvedApprovalPolicy=on-request resolvedApprovalsReviewer=auto_review summary=none useAppServerPermissionDefault=true
2026-07-15 23:21:38.217 [info] Reasoning summary item completed itemId=rs_03685aee08c75eab016a57ebd12fe48191b927e3aa1f2d2080 summary=[] summaryPartCount=0 threadId=019f6725-60b5-7852-ad86-6c329eb3c2d4 turnId=019f6771-1d38-7130-9328-0500db7c4c62
```

## Interpretation

- The log confirms that the first turn stopped and a separate turn began only after manual continuation.
- No explicit external blocker, fatal exception, `task_complete`, `turn/completed`, `stopReason`, `needs_follow_up` or `model_needs_follow_up` record is present in the available client log.
- Repeated `thread-stream-state-changed` messages correlate with normal streaming activity and turn boundaries. They are evidence of state changes, not proof of the root cause.
- `ResizeObserver` is a renderer/UI warning and should not currently be treated as the cause.
- Server-side telemetry keyed by the conversation and turn IDs is likely required to determine why the turn was accepted as complete while the requested multi-stage work remained incomplete.
