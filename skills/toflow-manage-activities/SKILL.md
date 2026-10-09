---
name: toflow-manage-activities
description: Log calls, create follow-up tasks, and attach notes to CRM records (people, companies, deals). Use this whenever the user wants to record a call outcome, set a reminder/to-do, or capture context about a contact or account.
license: MIT
---
# toflow — Notes, Tasks, and Calls

Lightweight activity records attached to CRM people, companies, or deals.

## Notes

- `list_notes` / `get_note` — review existing notes on a record before adding a new one, to avoid duplicating context already captured.
- `create_note` — link it to a person, company, or deal. Use this to capture meeting summaries, research findings, or context about a contact.
- `update_note` — correct or expand a note after creation.
- `delete_note` — confirm with the user before deleting.

## Tasks

- `list_tasks` / `get_task` — review open tasks or confirm a task's current state before updating it.
- `create_task` — link to a person, company, or deal; assign it to a team member (see `list_workspace_members` in `toflow-get-started`) and set a due date.
- `update_task` — mark complete, reassign, or change the due date.
- `delete_task` — confirm with the user before deleting.

## Calls

- `list_calls` / `get_call` — review call history for a contact or find a specific record.
- `log_call` — log a completed call or schedule a future one. Link it to a person or company; include outcome and notes to keep the CRM record complete.
- `update_call` — add notes after a call or correct an entry.
- `delete_call` — confirm with the user before deleting.

## Guardrails

- Always link a note/task/call to the relevant person, company, or deal rather than leaving it unattached, unless the user explicitly wants a standalone record.
- All deletes here are destructive from the user's point of view — confirm before calling any of them.
