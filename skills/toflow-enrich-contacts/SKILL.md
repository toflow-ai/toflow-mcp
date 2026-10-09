---
name: toflow-enrich-contacts
description: Find verified emails and phone numbers for people in the CRM, one at a time or in bulk across a list, or run a custom AI enrichment to derive a field from available data. Use this before enrolling someone in an email sequence, or whenever contact data is missing, unverified, or needs AI-derived research.
license: MIT
---
# toflow — Enrich Contacts

Get verified contact data before reaching out. Enrichment is asynchronous for phone/email lookups — expect to poll for results.

## Single person — email/phone

1. If you only have a LinkedIn URL and no CRM record yet, call `enrich_person_by_linkedin` first — it gets-or-creates the person and returns name, title, company, location, and any available contact data. Results are cached, so a recently-enriched person returns immediately.
2. For a verified email: call `enrich_person_email` with the person's CRM ID. If the result is pending, poll with `get_person_enrichment_status`.
3. For a phone number: call `enrich_person_phone` the same way, polling with `get_person_enrichment_status` if pending.
4. Always enrich email before enrolling someone in a sequence that includes email steps — don't enroll on a guess.

## Bulk (a whole list)

1. Call `estimate_bulk_enrich_list` first and show the user the credit cost — always confirm before spending credits on a large batch.
2. Call `bulk_enrich_list` with the list ID and enrichment type (email or phone).
3. To enrich specific people rather than a whole list, use `bulk_enrich_emails` / `bulk_enrich_phones` with a set of CRM person IDs instead of looping `enrich_person_email` one at a time.
4. Poll completion with `get_bulk_enrichment_status`, passing the task IDs returned by the bulk call.

## AI enrichment (custom, derived fields)

Use this when the user wants something beyond email/phone — e.g. deriving a company's tech stack, a person's likely pain point, or any field the standard enrichment tools don't cover.

1. Call `get_ai_enrichment_guide` before `create_ai_enrichment` — it's the source of truth for how to define the enrichment prompt/target field.
2. Call `list_ai_enrichments` to check whether a similar enrichment already exists before creating a new one.
3. Call `run_ai_enrichment` to execute it, `get_ai_enrichment` to inspect a specific enrichment's config/results, and `update_ai_enrichment` to adjust its prompt or target field.

## Guardrails

- Never skip `estimate_bulk_enrich_list` before a bulk run — credits are spent on the real call, not the estimate.
- Don't re-enrich a person who was just enriched (cached) unless the user explicitly wants a refresh.
- If enrichment comes back empty for a contact, tell the user plainly rather than silently proceeding to send — a sequence enrollment with a missing email will fail or be skipped downstream (see `toflow-create-sequence` for handling `invalid`/`skipped` enrollments).
