---
name: toflow-manage-crm
description: Create, search, and update CRM records (people, companies, deals), manage custom attributes, categories, and deal pipelines. Use this whenever the user wants to look up, add, or edit CRM data, or manage a deal's pipeline stage.
license: MIT
---
# toflow — Manage CRM Records

Covers the core CRM: people, companies, deals, custom attributes, and pipelines.

## Before creating or updating anything

1. Call `record_schema` (or `get_resource_schema` for custom fields) for the resource type (`person`, `company`, `deal`) to get valid field names and required values — never guess a field name.
2. Call `filter_guide` before any `list_records` call that needs a non-trivial filter — it covers filter/sort syntax and available operators.
3. Call `list_records` with filters/search first to check whether the record already exists — avoid creating duplicates. Prefer `add_people_to_list` / `add_companies_to_list` (see `toflow-prospect-and-list`) over `create_record` when the source is a LinkedIn URL — those auto-create and dedupe.

## Records

- `create_record` / `update_record` (only provided fields change) / `bulk_create` for batches.
- `get_person` / `get_company` / `get_deal` return the full profile including linked records and enrichment status (for people) — use these to get the complete picture before deciding next steps.
- `delete_person` / `delete_company` / `delete_deal` are soft-deletes — confirm with the user before calling any of them.
- `list_company_categories` / `get_or_create_category` — check for an existing category before creating one, to avoid duplicates.
- `list_pipelines` / `list_stages` — get valid IDs before setting a deal's stage via `create_record`/`update_record`.
- `add_person_to_deal` / `remove_person_from_deal` — link/unlink a contact to an opportunity.

## Custom attributes

- `list_attributes` / `get_attribute` — check existing attributes for a resource type before adding a new one.
- `create_attribute` — add a new custom field. Confirm the resource type and field type with the user first; attributes are harder to remove cleanly than to add.
- `update_attribute` / `delete_attribute` — confirm with the user before deleting, since existing record data on that field may be affected.

## Guardrails

- Never call `create_record`/`update_record` without checking `record_schema` first for that resource type.
- Never skip the `list_records` duplicate check before creating a person, company, or deal.
- All deletes are destructive from the user's point of view even though some are soft-deletes internally — always confirm first.
- Don't create a new attribute that duplicates an existing one in meaning — check `list_attributes` first.
