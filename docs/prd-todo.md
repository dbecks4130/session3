# Product Requirements Document (PRD) - Todo App Upgrade

## 1. Overview

We are upgrading the basic TODO app so users can organize tasks with due dates, priorities, and date-based filters while keeping the product simple and teachable. The MVP must remain frontend-only, use local storage only, and avoid backend or external storage changes.

---

## 2. MVP Scope

- Keep the implementation simple and teachable.
- Do not make backend changes.
- Persist task data locally only; no external storage.
- Extend the task model with a required `title` field.
- Add an optional `dueDate` field using ISO `YYYY-MM-DD` format.
- Treat invalid `dueDate` values as absent rather than failing or storing bad data.
- Add a `priority` field with allowed values `P1`, `P2`, and `P3`.
- Default `priority` to `P3` when the user does not choose a value.
- Add filters for `All`, `Today`, and `Overdue`.
- In `All`, continue showing completed tasks.
- In `Today` and `Overdue`, show incomplete tasks only.

---

## 3. Post-MVP Scope

- Visually highlight overdue tasks.
- Add task sorting in this order:
- Overdue tasks first.
- Then by priority from `P1` to `P3`.
- Then by due date in ascending order.
- Tasks without a due date last.

---

## 4. Out of Scope

- Notifications.
- Recurring tasks.
- Multi-user support.
- Keyboard navigation.
- External storage.