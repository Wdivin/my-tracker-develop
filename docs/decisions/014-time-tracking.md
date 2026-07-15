# Decision 014: Time tracking

## Status
Approved.

## Goal
Provide simple, reliable manual time tracking connected to tasks, with clear permissions, editable history, reporting, and identical business rules in web and API.

## MVP scope
- Manual time entry only; no timer or automatic activity tracking.
- Every time entry belongs to a task.
- A time entry contains:
  - work date;
  - duration;
  - required work type;
  - optional comment;
  - author;
  - source (`web`, `api`, or other registered integration source).
- Future dates are not allowed.
- Time cannot be added to a task in a closing status.
- There is no separate maximum-duration limit for a single entry.

## Input and storage behavior
- The UI should accept human-friendly duration input such as `1:30`, `1ч 30м`, `90 минут`, or `1.5 часа`.
- Internal storage is an implementation decision, but calculations must remain exact and hidden rounding is prohibited.
- A configurable minimum increment may be supported, with 5 minutes as a reasonable default.

## Work types
- Work type is required for every entry.
- Work types are configurable.
- A project either uses the global set or its own complete set; no partial override model is required.

## Permissions
- Users normally create time only for themselves.
- Creating time for another user requires a separate permission.
- Users always see their own entries.
- Viewing others' entries requires project-level or global time-view permission.
- Editing or deleting another user's entry requires a separate permission.
- Project managers can view time for their projects by default.

## Editing and deletion
- Users may edit and soft-delete their own entries while the period is open.
- All changes are recorded in task history and system audit where applicable.
- Closed periods block creation, editing, and deletion before the configured date.
- Exceptional administrative changes after period closure require a dedicated permission and must be audited.

## Relation to estimates and progress
- The task has one current estimate in MVP.
- Actual time is the sum of time entries.
- The UI may show estimated, spent, remaining-by-estimate, or exceeded time.
- Remaining time is informational and not a hidden editable field.
- Time spent never changes task progress automatically.

## User experience
- The task card has a compact `Добавить время` action.
- Default date is today.
- A separate `Моё время` screen shows the user's entries by date and period and allows navigation to the task.
- A weekly timesheet grid is outside MVP.

## Reports and export
- MVP includes a basic time report with filters by period, project, subprojects, user, task, and work type.
- Grouping can be by project, user, task, work type, or date.
- Financial rates, billing, payroll, budgets, and billable flags are outside MVP.
- CSV export requires a separate permission and is recorded in audit.

## History and audit
- Adding, editing, and deleting time entries is recorded in task activity.
- Entries remain separate events and are not silently merged.
- Administrative overrides and exports are recorded in system audit.

## API
The REST API must support creation, reading, editing, soft deletion, reporting, filtering, and permitted export with the same rules as the web interface:
- no future time;
- no time on closed tasks;
- no bypass of closed periods;
- no writing for another user without permission;
- no reading others' time without permission.

## Out of MVP
- timers;
- automatic activity tracking;
- desktop screenshots;
- IDE integrations;
- approval workflows for timesheets;
- financial rates and billing;
- payroll;
- project budgets;
- weekly grid timesheet;
- reminders to fill time.
