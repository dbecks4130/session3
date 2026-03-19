# Todo App Upgrade Epics and Stories

## Overview

This document translates the confirmed PRD into MVP and Post-MVP epics with actionable user
stories. Each story includes acceptance criteria and technical requirements to guide
implementation, testing, and review.

## MVP Epics

### Epic: Extend Task Data for Due Dates and Priorities

#### Story: Add due date input to task creation and editing

**Acceptance Criteria**

- Users can enter an optional due date for a task.
- The due date input accepts dates in `YYYY-MM-DD` format.
- A task can be saved without a due date.
- When editing a task with an existing due date, the current value is shown in the form.

**Technical Requirements**

- Add a `dueDate` field to the frontend task model.
- Use a date input or equivalent UI that produces `YYYY-MM-DD` values.
- Do not require backend changes or server-side persistence.
- Update frontend tests to cover create and edit flows with and without a due date.

#### Story: Add priority selection to task creation and editing

**Acceptance Criteria**

- Users can assign one of `P1`, `P2`, or `P3` to a task.
- New tasks default to `P3` when no priority is explicitly selected.
- When editing a task, the saved priority is shown in the form.
- Tasks cannot be saved with a priority outside `P1`, `P2`, or `P3`.

**Technical Requirements**

- Add a `priority` field to the frontend task model.
- Constrain allowed values to `P1`, `P2`, and `P3` in form handling and state updates.
- Default missing priority values to `P3` when creating or normalizing task data.
- Add frontend tests for default priority behavior and invalid priority rejection.

#### Story: Validate and normalize upgraded task data

**Acceptance Criteria**

- A task title remains required.
- Invalid due date values are ignored and treated as if no due date was provided.
- Existing tasks without a priority are treated as `P3` after normalization.
- Invalid task data does not crash the UI.

**Technical Requirements**

- Centralize task normalization and validation in shared frontend logic.
- Preserve compatibility with tasks already stored locally before the upgrade.
- Treat invalid `dueDate` values as `null`, `undefined`, or absent in the normalized model.
- Add unit tests for normalization, required title validation, and invalid data handling.

### Epic: Add Date-Based Task Filtering

#### Story: Add All, Today, and Overdue task filters

**Acceptance Criteria**

- Users can switch between `All`, `Today`, and `Overdue` views.
- `All` shows every task regardless of due date.
- `Today` shows only tasks due on the current date.
- `Overdue` shows only tasks with due dates earlier than the current date.

**Technical Requirements**

- Add a filter state to the frontend and connect it to the task list rendering.
- Compare due dates using the browser's current local date.
- Handle tasks without due dates by excluding them from `Today` and `Overdue`.
- Add integration tests for filter switching and filtered list results.

#### Story: Exclude completed tasks from Today and Overdue views

**Acceptance Criteria**

- Completed tasks remain visible in `All`.
- Completed tasks do not appear in `Today`.
- Completed tasks do not appear in `Overdue`.
- Incomplete tasks that match the selected date filter remain visible.

**Technical Requirements**

- Reuse existing completion state in filter predicates.
- Apply completion filtering only in `Today` and `Overdue` views.
- Keep filtering logic readable and isolated from presentation code.
- Add tests covering completed and incomplete tasks across all three filters.

### Epic: Persist Upgraded Tasks Locally Without Backend Changes

#### Story: Save upgraded task fields in local storage

**Acceptance Criteria**

- Tasks retain `title`, `completed`, `dueDate`, and `priority` after a page reload.
- The application continues working without any backend dependency.
- Existing locally stored tasks remain usable after the upgrade.
- Loading stored tasks with missing or invalid new fields does not break the app.

**Technical Requirements**

- Continue using local storage as the only persistence mechanism.
- Update serialization and deserialization logic for the expanded task model.
- Do not add backend endpoints, API calls, or external storage services.
- Add tests for loading legacy task records and upgraded task records from storage.

## Post-MVP Epics

### Epic: Improve Task Visibility for Time-Sensitive Work

#### Story: Visually highlight overdue tasks

**Acceptance Criteria**

- Overdue tasks are visually distinct from non-overdue tasks.
- The highlight is shown consistently anywhere overdue tasks appear.
- Tasks due today are not styled as overdue.
- Tasks without a due date are not styled as overdue.

**Technical Requirements**

- Add a clear visual state for overdue tasks in the frontend task list.
- Derive overdue styling from task status and current date rather than stored flags.
- Keep styling changes limited to the frontend UI layer.
- Add UI tests that verify overdue styling conditions.

### Epic: Sort Tasks by Urgency and Priority

#### Story: Sort tasks with overdue items first

**Acceptance Criteria**

- Overdue incomplete tasks appear before non-overdue tasks.
- Tasks that are not overdue remain after overdue tasks.
- Tasks without due dates are not treated as overdue.

**Technical Requirements**

- Implement sorting with a dedicated comparator rather than inline list mutations.
- Apply overdue detection consistently with the filter logic.
- Add unit tests for overdue versus non-overdue ordering.

#### Story: Sort remaining tasks by priority, due date, and undated status

**Acceptance Criteria**

- After overdue ordering, tasks are sorted by `P1`, then `P2`, then `P3`.
- Tasks with the same priority are sorted by ascending due date.
- Tasks without due dates appear after tasks that have due dates.
- The combined ordering is deterministic for the same input data.

**Technical Requirements**

- Extend the task comparator to support priority ranking and ascending due date ordering.
- Treat undated tasks as the lowest date-order group.
- Keep sorting logic reusable and independently testable.
- Add unit tests that cover mixed priority, due date, and undated scenarios.

## Cross-Cutting Technical Constraints

- Keep the implementation frontend-only.
- Do not introduce backend changes, external APIs, or external storage.
- Follow project coding guidelines for formatting, naming, and small focused components.
- Add or update tests for all new behavior using readable, isolated test cases.