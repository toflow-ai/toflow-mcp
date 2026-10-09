---
name: toflow-manage-crm
description: Create, search, and update CRM records (people, companies, deals), manage custom attributes, categories, and deal pipelines. Use this whenever the user wants to look up, add, or edit CRM data, or manage a deal's pipeline stage.
license: MIT
---
# toflow — Manage CRM Records

Covers the core CRM: people, companies, deals, custom attributes, and pipelines.

## Before creating or updating anything

1. Call `record_schema` (or `get_resource_schema` for custom fields) for the resource type (`person`, `company`, `deal`) to get the real attribute titles, types, and which are required — never guess a field name. Pass attribute titles as the keys in the `attributes` dict on `create_record`/`update_record`; for `select`/`multiselect` fields, pass the value from `allowed_values`, not the display label.
2. Call `filter_guide` before any `list_records` call that needs a non-trivial filter. Filters are a list of `FilterGroup` objects (`{"logic": "and"|"or", "conditions": [{"field": "<Attribute Title>", "operator": "...", "value": ...}]}`) — always wrap conditions in an explicit group rather than passing flat conditions. Operators: `is`, `is_not`, `contains`, `not_contains`, `is_empty`, `is_not_empty`, `gt`, `lt`, `gte`, `lte`, `in`. `sort` is a comma-separated `"<Attribute Title>:asc|desc"` string. If the user names a saved view, resolve it to a `view_id` via `list_views` rather than guessing.
3. Call `list_records` with filters/search first to check whether the record already exists — avoid creating duplicates. If you already have record IDs, pass them via `ids` rather than looping `get_person`/`get_company`/`get_deal`. Prefer `add_people_to_list` / `add_companies_to_list` (see `toflow-prospect-and-list`) over `create_record` when the source is a LinkedIn URL — those auto-create and dedupe.

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
