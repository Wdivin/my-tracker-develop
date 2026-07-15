# Decision 008: Roles and permissions

Status: Approved

## Core model

The system uses three access contexts:

1. System-wide administration.
2. Project roles.
3. The user's relationship to a specific task, such as author, assignee, watcher, or project manager.

System administrators manage global configuration and have full system visibility. Regular users receive capabilities through project membership and task context.

## System level

The MVP has two system-level user classes:

- system administrator;
- regular user.

The system administrator may manage users, global dictionaries, task types, statuses, workflow templates, custom fields, system settings, audit data, and recovery of soft-deleted tasks.

Administrative status does not create a universal workflow bypass. Status changes still use configured transitions.

## Project roles

Project roles are configurable. The default starter roles are:

- Project manager;
- Member;
- Reader.

These are editable role templates rather than hidden hard-coded modes.

A user may have multiple project roles. Allowed permissions are additive: if any assigned role grants an action, the action is allowed.

The MVP has no explicit deny permissions. Roles only add capabilities.

## Role inheritance in subprojects

When project membership is inherited by a subproject, inherited users retain their inherited roles.

A subproject may add local roles to inherited users, but it cannot locally remove inherited roles.

To use a completely different participant and role set, the subproject disables participant inheritance and manages its own membership.

The interface must show whether membership and roles are inherited or local and identify their source.

## Project visibility

Projects have two visibility modes:

- closed: visible only to project participants, inherited participants, and system administrators;
- internal: visible to all active organization users.

A user who can see an internal project but has no project role receives read-only access. Such a user cannot comment, edit, change status, upload files, or perform other task actions.

There are no public projects without authentication in the MVP.

## Task visibility

If a user can see a project, the user can see all tasks in that project.

The MVP does not include private tasks, per-task visibility lists, confidential fields, or visibility rules by task type.

Confidential processes should use a separate closed project.

## User groups

User groups are included in the MVP.

Groups may be added to projects and assigned project roles. Groups may also be used in watcher and notification selection where applicable.

A group cannot be assigned as the task assignee. Every task has one concrete responsible person.

## Permission structure

Permissions are grouped into understandable capability areas rather than one checkbox for every individual field.

Recommended groups include:

- project access and project editing;
- task viewing and creation;
- editing basic task fields;
- managing responsibility;
- changing task structure;
- comments, attachments, and checklist;
- time tracking;
- project participants;
- reports and saved views;
- bulk operations and soft deletion.

### Basic task fields

The basic-field permission covers fields such as title, description, due date, priority, tags, progress, estimate, and custom fields, subject to workflow-specific requirements.

### Responsibility management

Responsibility management covers changing the assignee and managing watchers.

The task author is immutable after creation and cannot be changed by a project manager or any regular role.

### Structural changes

Structural permissions cover moving a task, changing its type, changing its parent task, and managing task relations.

## Workflow permissions

There is no separate generic permission named "change status".

Available status changes are determined by workflow transitions, project roles, the user's relationship to the task, required fields, and comment requirements.

## Author, assignee, and watcher behavior

### Author

The author is permanently recorded at task creation and is never changed.

Being the author does not automatically grant task editing rights. The author may act only through project-role permissions and workflow transitions that explicitly allow the author relation.

### Assignee

The assignee receives operational capabilities required for work, such as commenting, updating progress, maintaining the checklist, logging time, and using transitions explicitly available to the assignee.

The assignee does not automatically receive administrative or structural permissions.

### Watchers

Any user who can view a task may add themselves as a watcher and may stop watching it.

A watcher does not receive additional access rights merely by watching the task.

Adding or removing other users as watchers is controlled by project-role permissions.

## Participant management

Project participant management uses separate permissions for viewing participants, adding users or groups, changing roles, and removing participants.

Removing a participant does not delete history, comments, time entries, or tasks.

If open tasks remain assigned to a removed participant, the system shows the consequences clearly and allows them to remain temporarily unassigned or be reassigned according to the operation chosen by an authorized user.

## Deletion and recovery

Task deletion is soft deletion.

Project roles may receive permission to soft-delete tasks.

Only a system administrator may restore a deleted task or physically delete it.

Deletion, restoration, and physical deletion are recorded in audit history.

## Role administration UX

The role editor groups permissions into clear sections with short descriptions of their effects.

The interface must avoid a single unstructured wall of checkboxes.
