# Decision 018: Nonfunctional Requirements, Testing, Deployment, and Acceptance

## Status

Approved for MVP.

## Purpose

Define the nonfunctional baseline and the minimum acceptance bar for the tracker. MVP is considered complete only when it is stable, secure, reproducibly deployable, usable on desktop and mobile, and covered by realistic automated and browser-based verification.

## Supported clients

Required browser coverage:

- current Chrome;
- current Edge;
- current Safari on current iPhone;
- current Chrome on current Android;
- Firefox is supported where this does not require a separate implementation path, substantial compromises, or significant extra complexity.

Required visual checkpoints:

- 1440 × 900;
- 1024 × 768;
- 390 × 844;
- preferably also 360 × 800.

Key user scenarios must be tested at these sizes, not only the landing page.

## Mobile capability

Mobile is a full working client, not a read-only companion. On mobile, a user must be able to:

- create a task;
- edit common fields;
- execute workflow transitions;
- add comments and attachments;
- manage checklist items;
- log time;
- search and filter tasks;
- work with notifications.

Administrative editors may be more comfortable on desktop, but must not break on mobile.

## PWA

Basic PWA support is part of MVP.

Required:

- installable application;
- correct app icon and manifest;
- standalone display mode;
- correct mobile viewport behavior;
- safe frontend asset updates.

Out of MVP:

- offline editing;
- background synchronization;
- push notifications;
- offline conflict resolution.

## Performance targets

For a normal supported production environment, the following are acceptance targets rather than universal guarantees:

- most ordinary API GET requests: up to 500 ms server time;
- complex task lists and reports: up to 2 seconds;
- opening a normal task card: up to 2 seconds;
- ordinary saves: up to 1 second when no long-running background work is required.

Large histories and other heavy data must use pagination or incremental loading.

## Expected scale

The design baseline is at least:

- one organization;
- 1,000 active users;
- 1,000 projects and subprojects;
- 1,000,000 tasks;
- several million comments, history events, and time entries;
- attachment growth limited by storage and configured quotas.

The MVP does not need to execute a full million-task load test before first release, but the architecture must not obviously assume only a few hundred records.

## Data reliability

Critical business operations must be atomic, including:

- workflow transition plus required comment;
- task type change plus required fields;
- project transfer;
- task creation with related data;
- soft deletion and restoration;
- bulk changes.

Partial persistence of one business action is unacceptable.

## Concurrent editing

Optimistic locking is mandatory for web and API.

On conflict, the user must see that the object changed and must not silently overwrite newer data.

## Security baseline

Required:

- CSRF protection;
- XSS protection;
- safe Markdown rendering;
- parameterized database access;
- protection from unsafe mass assignment;
- server-side authorization on every operation;
- login throttling;
- secure cookies;
- request size limits;
- MIME and extension validation for files;
- safe HTTP headers;
- no secrets in logs;
- HTTPS-only production operation.

Frontend visibility is never considered an authorization mechanism.

## Domain integration verification

The agreed domain integration must be verified for:

- domain user sign-in;
- creation or update of the local profile;
- access blocking or deactivation;
- temporary domain unavailability;
- preservation of local roles and history;
- local emergency administrator access;
- no automatic project permissions merely from successful domain sign-in.

The implementation technology is chosen after checking the actual domain environment.

## Accessibility baseline

Formal WCAG certification is not required for MVP, but the interface must provide:

- keyboard operation;
- visible focus;
- labeled fields;
- understandable validation errors;
- sufficient contrast;
- actions not distinguishable only by color;
- correct button and link semantics;
- focus containment in dialogs;
- usable forms and tables;
- resilience to browser zoom.

## Localization and time

Russian and Ukrainian interfaces must be tested.

Required:

- no hard-coded interface strings;
- correct date formats and pluralization;
- no text clipping in either locale;
- email and notification language based on recipient locale;
- stable API error codes independent of translated text;
- user timezone display for timestamps;
- date-only fields that do not shift between timezones;
- consistent date handling between web and API.

## Logging and errors

Structured logs are required for:

- application errors;
- queues;
- login and security events;
- API activity;
- administrative actions;
- scheduler activity;
- email;
- domain integration.

Logs must include request IDs where appropriate and must not contain passwords, full tokens, domain secrets, SMTP passwords, file contents, or unnecessary personal data.

Users must never see framework stack traces in production. User-visible errors must be understandable and include a supportable request or error identifier.

## Automated testing

Required test layers:

- unit and domain tests;
- database and service integration tests;
- backend feature tests;
- targeted frontend component tests;
- browser end-to-end tests.

Core coverage must include workflows, permissions, task lifecycle, progress, watchers, time tracking, custom fields, notifications, files, API behavior, queues, and domain authentication.

## End-to-end browser testing

Playwright or an equivalent real-browser tool is mandatory.

Minimum scenarios include:

1. sign-in;
2. task creation;
3. expanded creation form;
4. assignee selection;
5. comment with mention;
6. file upload;
7. workflow transition;
8. required transition comment;
9. close and reopen;
10. subtask and parent-close restriction;
11. filters and saved views;
12. board interaction;
13. time entry;
14. different role permissions;
15. mobile task work;
16. concurrent update conflict;
17. exact task number search;
18. restore deleted task;
19. API task creation;
20. domain sign-in and emergency local administrator sign-in.

Visual verification must cover the approved viewports, both locales, empty states, long text, errors, dialogs, tables, cards, filters, board, administration, and mobile sticky actions.

## Demo data

A reproducible demo or seed dataset is required with representative users, groups, roles, projects, subprojects, task types, workflows, tasks, comments, files, time entries, custom fields, relations, notifications, and overdue or closed cases.

Demo data must not be created automatically in production.

## Continuous integration

Every pull request must run at least:

- backend tests;
- frontend type checking;
- relevant frontend tests;
- production build;
- lint and format checks;
- migration verification;
- dependency and security checks.

A smoke E2E suite may run on every pull request, with a fuller suite before merge or release.

## Dependency vulnerability policy

- Critical-severity dependency vulnerabilities block release.
- High-severity findings normally block release.
- A high-severity finding may be accepted only as a documented exception when no safe compatible fix is available or the vulnerability is demonstrably not applicable to the deployed usage.
- Every exception must include risk assessment, affected component and usage, temporary mitigation, responsible owner, and a review or upgrade plan.
- The exception must be explicit and must not silently suppress the finding.

## Migrations

Migrations must:

- work on a clean database;
- work when upgrading an existing database;
- preserve data;
- avoid manual table editing;
- account for large tables;
- have a rollback or safe recovery plan for risky changes.

## Deployment and operations

Production deployment must be documented and reproducible, covering runtime, database, frontend build, web server, queues, scheduler, storage, SMTP, domain integration, HTTPS, filesystem permissions, and health checks.

Docker may be supported, but is not required to be the only installation method.

Required health checks:

- application process;
- database connectivity;
- storage availability;
- queue operation;
- recent scheduler execution.

Public health endpoints must not disclose sensitive infrastructure details.

## Backup and restore

Before MVP acceptance, backup and restore must be tested in practice:

1. create representative data and files;
2. back up database and storage;
3. restore into a clean environment;
4. verify tasks, comments, permissions, history, time entries, and attachments.

A written backup procedure without a tested restore is insufficient.

## Documentation

Required MVP documentation:

- README;
- installation;
- production deployment;
- upgrade procedure;
- backup and restore;
- queues and scheduler;
- SMTP;
- domain integration;
- file storage;
- API and OpenAPI;
- administrator guide;
- short user guide;
- troubleshooting;
- architectural decisions;
- release notes.

Documentation must match the actual implementation.

## Definition of Done

A feature is not done until:

- business rules are implemented;
- web and API behavior agree;
- authorization is verified;
- history and audit are handled;
- notification consequences are handled;
- tests are present and passing;
- mobile and visual checks pass;
- documentation is updated;
- no known critical defect remains.

## MVP acceptance

MVP is accepted only when:

- all approved product blocks are implemented;
- required tests and production build pass;
- clean-install and upgrade migrations pass;
- deployment is reproducible;
- domain integration is tested in a real or representative environment;
- backup and restore are verified;
- key desktop and mobile scenarios pass;
- OpenAPI matches the API;
- no critical vulnerability remains;
- any accepted high-severity vulnerability has an explicit documented exception;
- documentation is complete;
- known limitations are listed clearly.

## Out of scope

- offline task editing;
- push notifications;
- automatic background sync;
- formal accessibility certification;
- guaranteed Firefox support requiring separate implementations or major compromises;
- infrastructure control panels or automatic application updates from the web UI.
