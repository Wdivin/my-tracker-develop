# Decision 016: REST API and integrations

Status: approved

## Goal

Provide a versioned REST JSON API as a full external interface to the same business rules used by the web application, without introducing webhooks or end-user token management in MVP.

## Core principles

- API is not a bypass around permissions, workflow, validation, history, optimistic locking, or audit.
- Web and API must use the same domain rules.
- API must be versioned and documented with OpenAPI.
- Stable machine-readable error codes are required.
- Sensitive credentials must never be logged or returned after creation.

## MVP scope

The REST API should cover the core working entities and actions required by external integrations:

- projects and subprojects;
- tasks;
- comments;
- attachments;
- watchers;
- task relations;
- checklists;
- time entries;
- workflow actions;
- task-form metadata and custom-field definitions;
- reference data needed by clients;
- list filtering, sorting and pagination.

Full administrative API coverage is not mandatory for MVP.

## API format and versioning

- Use JSON REST endpoints under a versioned prefix such as `/api/v1/...`.
- Breaking changes require a new major API version.
- Backward-compatible additions are allowed within the current version.
- OpenAPI specification is mandatory and should include routes, schemas, authentication, errors, filters, pagination and examples.
- A Swagger UI or similar viewer is desirable but secondary to the OpenAPI contract itself.

## Authentication model

- Ordinary users cannot create personal API tokens in MVP.
- Integrations use dedicated service users.
- A service user receives normal project roles and permissions.
- API tokens always act within the service user's rights.
- Token scopes further restrict access; effective access is the intersection of user permissions and token scopes.
- A token can never grant more rights than its owner has.

## Token lifecycle

- Tokens must always have an expiration date.
- Permanent tokens are not allowed.
- The full token value is displayed only once at creation.
- The system stores only a secure representation suitable for validation.
- Token metadata includes name, owner, creation date, expiration date, last use, scopes, status and revocation date.
- Tokens can be revoked immediately.
- Deactivation of a service user invalidates its tokens.
- Token creation, revocation and expiration-sensitive changes are audited.

## Request handling

- Rate limiting is required and configurable.
- Every API request receives a unique request ID returned to the client and written to logs.
- Server-side pagination is required for list endpoints.
- Filtering and sorting must use an explicit allowlist of supported fields and operators.
- Arbitrary SQL-like expressions are not accepted.

## Errors

Use a consistent error envelope with stable machine-readable codes and meaningful HTTP statuses.

Expected status semantics include:

- `400` invalid request format;
- `401` unauthenticated;
- `403` insufficient permissions;
- `404` object missing or not visible;
- `409` state or version conflict;
- `422` validation failure;
- `429` rate limit exceeded.

Validation errors should identify affected fields without leaking inaccessible data.

## Concurrency and idempotency

- Optimistic locking is mandatory for mutable resources.
- Silent overwrite of newer changes is forbidden.
- Creation operations used by integrations should support an idempotency key, at minimum for task creation, comments, time entries and attachment-upload completion.
- Repeating the same request with the same idempotency key must not create duplicates.

## Bulk operations

- Bulk operations follow the same permissions and workflow rules as single-item operations.
- Partial success is allowed and must be explicit.
- The response must report results and errors per object.
- Bulk endpoints must not provide an administrative bypass around workflow or validation.

## Attachments

- Attachment upload, listing, download and soft deletion are supported through API.
- The same file-size, type, quota and access rules apply to API and web.
- Comment attachments remain tied to comment lifecycle rules.
- Direct file access must still verify task permissions.

## Logging and audit

API logs should record diagnostic metadata such as:

- request ID;
- service user or token identifier;
- endpoint and method;
- response status;
- duration;
- timestamp;
- IP address;
- request and response size where useful.

Logs must not contain full tokens, passwords, secrets or unnecessary sensitive request bodies.

Security-significant operations, bulk actions and exports must also be represented in system audit according to the audit decision.

## CORS

- API is not open to arbitrary browser origins by default.
- Allowed origins are configured explicitly by an administrator when browser-based integrations are required.
- Server-to-server integrations do not depend on CORS.

## Explicitly excluded from MVP

- personal API tokens for ordinary users;
- webhooks;
- OAuth2 provider functionality;
- third-party OAuth applications;
- GraphQL;
- marketplace integrations;
- custom scripts or arbitrary code execution;
- Telegram bot;
- email-to-task;
- AI integrations;
- universal CSV import;
- automatic bidirectional synchronization.

These can be evaluated later when there is a concrete integration need.
