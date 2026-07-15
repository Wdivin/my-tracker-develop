# Decision 015: Custom fields, task types, and configurable reference data

## Status

Approved.

## Context

The tracker must adapt to different company processes without requiring code changes, while avoiding an overcomplicated form-builder or ERP-like configuration model.

## Decision

### Task types

A task type defines:

- name and icon;
- workflow;
- enabled standard fields;
- custom fields;
- standard-field requiredness;
- whether progress is enabled;
- whether time tracking is enabled;
- whether subtasks are allowed;
- field order and section placement in create and task-card views.

Task types are global. A project selects which global task types are available.

Changing an existing task's type is an explicit action. The system must:

- check permissions;
- show fields that will appear or disappear;
- map the workflow safely;
- retain values of fields hidden by the new type;
- require new mandatory fields;
- prevent an invalid task state.

### Standard task fields

Configurable standard fields include:

- description;
- assignee;
- priority;
- start date;
- due date;
- progress;
- time estimate;
- parent task;
- tags;
- watchers;
- checklist;
- attachments;
- relations.

Fields required for task identity and integrity cannot be disabled, including project, task type, title, status, author, and task identifier.

### Custom field types in MVP

Supported custom field types:

- short text;
- multiline text;
- integer;
- decimal;
- date;
- date and time;
- boolean;
- single select;
- multi-select;
- project reference.

The following custom field types are not included in MVP:

- user reference;
- task reference;
- file;
- formulas or computed fields;
- nested structures or tables;
- JSON;
- geolocation;
- signatures;
- external dynamic reference data.

### Global custom fields

Custom fields are global. Project-specific custom fields are not supported in MVP.

A custom field can be assigned:

- to selected task types; or
- to all task types.

For each task-type assignment, the system can configure:

- requiredness;
- order;
- form section;
- visibility during creation;
- visibility in the task card;
- availability for quick editing.

### Field description and hint

Each custom field supports:

- an administrative description explaining its business purpose;
- a user-facing hint or help text shown with the input.

These texts are configurable independently of the field name.

### Form sections

Fields can be arranged into configurable sections, such as:

- Main information;
- Planning;
- Technical data;
- Client;
- Result.

For each section, the administrator configures:

- title;
- order;
- contained fields;
- default expanded or collapsed state.

Mobile displays sections vertically.

### Requiredness

A field may be required:

- during task creation;
- on every save;
- for a specific workflow transition.

A field required for closing does not need to be required at creation.

### Default values

Only explicit and visible defaults are allowed.

Allowed defaults include:

- static administrator-defined values;
- expected system values such as the current date where appropriate.

Not allowed in MVP:

- last value used by the user;
- most frequent value;
- hidden behavior-based defaults;
- AI-generated values without explicit confirmation.

### Validation

Supported validation rules:

- text minimum and maximum length;
- numeric minimum and maximum;
- decimal precision;
- date range constraints;
- allowing or disallowing past dates;
- allowing or disallowing future dates;
- maximum number of selected values for multi-select fields.

Regular-expression validation is not exposed in the MVP administration UI.

### Select values

Single-select and multi-select fields use their own managed value lists.

Each value has:

- name;
- optional color;
- order;
- active or archived state.

Used values are archived rather than deleted. Historical tasks retain the value, and filters can still find it.

### Field type immutability

After a field has been used, its data type cannot be freely changed.

Safe changes include:

- name;
- description;
- user hint;
- order;
- requiredness;
- compatible validation limits;
- selectable values;
- active state.

Unsafe type changes require creating a new field and, if necessary, a separate administrative data-migration procedure.

### Archiving and deletion

A used custom field is archived rather than physically deleted.

An archived field:

- disappears from new forms;
- retains historical values;
- remains in history;
- remains available for historical filtering;
- can be restored.

Physical deletion is allowed only to a system administrator and only when the field has never been used.

### Scope of custom fields

In MVP, custom fields apply only to tasks.

Custom fields are not supported for:

- projects;
- users;
- groups;
- time entries.

### Tags, priorities, and work types

Tags remain a separate flexible classification mechanism.

Priorities remain a separate global reference list with:

- name;
- order;
- color;
- active state;
- one optional default value.

Used priorities are archived rather than deleted.

Work types for time tracking remain a separate reference list. A project either uses the global set or its own complete set; partial mixing is not supported.

### Conditional logic

Complex conditional visibility and dependencies between fields are not part of MVP.

The form depends only on:

- task type;
- user permissions;
- create or view/edit mode.

### API

The API exposes form metadata, including:

- available task types;
- available fields;
- field data types;
- requiredness;
- reference values;
- validation limits;
- order and section placement;
- editability.

Web UI and API use the same validation and business rules.

## Consequences

- The system remains configurable without becoming a generic process engine.
- Custom field reuse is predictable across projects and reports.
- Redundant field types are avoided because assignees, watchers, and task relations already cover users and task links.
- Administrators can expose fields across all task types without repetitive configuration.
- Users receive contextual help directly in forms through field hints.

## Not in MVP

- project-specific custom fields;
- user-reference custom fields;
- task-reference custom fields;
- computed fields;
- conditional visibility;
- dependencies between fields;
- external dynamic dictionaries;
- regular expressions in the admin UI;
- uniqueness constraints for custom fields;
- custom fields for projects, users, groups, or time entries.
