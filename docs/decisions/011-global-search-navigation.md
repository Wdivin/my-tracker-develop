# Decision 011: Global Search and Navigation

## Status

Approved.

## Goal

Provide predictable navigation to tasks and projects without introducing developer-oriented command palettes or overlapping concepts.

## Global search

- Global search is available from any application screen.
- MVP uses a regular search interface rather than a command palette.
- Search covers only:
  - tasks;
  - projects.
- Users are not included as a searchable global object in MVP.
- Search respects the same access rules as normal application screens and API endpoints.
- An inaccessible task or project must not be disclosed through exact identifiers, snippets, counts, or indirect hints.

## Search behavior

- Exact task-number matches have the highest priority.
- A task with an exact accessible number match can be opened directly from the search result.
- Task search covers:
  - number;
  - title;
  - description;
  - comments;
  - supported text custom fields.
- Project search covers:
  - name;
  - code;
  - description.
- Closed tasks are searched by default and are clearly marked in results.
- Archived projects are searchable and clearly marked.
- Soft-deleted tasks are hidden from ordinary users. A system administrator may include them through an explicit filter.
- Search result snippets show where the match was found and highlight the matching fragment.
- A match inside a comment should open the task at the relevant activity entry when practical.

## Search interfaces

- A compact search field provides a small set of immediate results for navigation.
- A separate full results page provides filtering, sorting, and pagination.
- Results are grouped by object type: tasks and projects.
- Recent local search queries may be stored only for convenience, must be clearable, and must not become a long-term server-side history requirement.

## Recent items

- `Recent` is a separate navigation section, not part of `My Work`.
- It includes recently opened tasks and projects.
- The order is based on the user's most recent opening time.
- This is personal navigation history, not audit history.
- The user can clear the recent-items list.

## Favorites

- Favorites are used only for navigation.
- Users can favorite:
  - projects;
  - personal saved views;
  - project saved views available to them.
- Individual tasks cannot be added to favorites in MVP.
- Task observation remains the mechanism for following a task and receiving notifications.

## Main navigation

The main working navigation should include at least:

- My Work;
- Projects;
- All Tasks;
- Boards;
- Recent;
- Notifications;
- Favorites.

Administration is shown separately and only to authorized users.

## Project navigation

Project navigation includes, according to permissions:

- overview;
- tasks;
- board;
- participants;
- reports;
- settings.

Subproject navigation uses breadcrumbs and a compact list of parent and child projects. Mobile navigation must not depend on a large expandable tree.

## Rejected for MVP

- global user search;
- favorite tasks;
- command palette;
- VS Code-style action launcher;
- long-term automatic search-history storage;
- any search behavior that bypasses normal access control.
