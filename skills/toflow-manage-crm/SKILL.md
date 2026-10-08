---
name: toflow-manage-crm
description: Create, search, and update CRM records (people, companies, deals) plus linked notes, tasks, and call logs. Use this whenever the user wants to look up, add, or edit CRM data, log a call, attach a note, or manage a deal pipeline.
license: MIT
---
# toflow — Manage CRM Records

Covers the core CRM: people, companies, deals, and the notes/tasks/calls attached to them.

## Before creating or updating anything

1. Call `record_schema` (or `get_resource_schema` for custom fields) for the resource type (`person`, `company`, `deal`) to get valid field names and required values — never guess a field name.
2. Call `filter_guide` before any `list_records` call that needs a non-trivial filter — it covers filter/sort syntax and available operators.
3. Call `list_records` with filters/search first to check whether the record already exists — avoid creating duplicates. Prefer `add_people_to_list` / `add_companies_to_list` (see `toflow-prospect-and-list`) over `create_record` when the source is a LinkedIn URL — those auto-create and dedupe.

## Records

- `create_record` / `update_record` (PATCH — only provided fields change) / `bulk_create` for batches.
- `get_person` / `get_company` / `get_deal` return the full profile including linked records, notes, tasks, and (for people) enrichment status — use these to get the complete picture before deciding next steps.
- `delete_person` / `delete_company` / `delete_deal` are soft-deletes — confirm with the user before calling any of them.
- `list_company_categories` / `get_or_create_category` — check for an existing category before creating one, to avoid duplicates.
- `list_pipelines` / `list_stages` — get valid IDs before `create_record`/`update_record` on a deal.
- `add_person_to_deal` / `remove_person_from_deal` — link/unlink a contact to an opportunity.

## Notes, Tasks, Calls

- Notes: `list_notes` / `get_note` / `create_note` (link to a person, company, or deal) / `update_note` / `delete_note` (confirm before deleting).
- Tasks: `list_tasks` / `get_task` / `create_task` (link to a record, assign, set due date) / `update_task` / `delete_task` (confirm before deleting).
- Calls: `list_calls` / `get_call` / `log_call` (link to a person/company, include outcome and notes) / `update_call` / `delete_call` (confirm before deleting).

## Guardrails

- Never call `create_record`/`update_record` without checking `record_schema` first for that resource type.
- Never skip the `list_records` duplicate check before creating a person, company, or deal.
- All deletes are destructive from the user's point of view even though they're soft-deletes internally — always confirm first.
