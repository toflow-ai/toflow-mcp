---
name: toflow-prospect-and-list
description: Search for B2B prospects using Sales Navigator-style filters, pull prospects from existing LinkedIn connections or post engagement, and organize results into lists and saved views. Use this whenever the user wants to find new people/companies to target, or build a target account/prospect list.
license: MIT
---
# toflow — Prospect and Build Lists

Find the right people before reaching out, then stage them in a list so they're ready for enrichment and sequencing.

## Part 1 — Finding prospects

Pick the matching entry point based on what the user describes:

- **Cold search with a target persona in mind** (role, seniority, company size, industry, location): use the prospect search tool. Combine multiple filters to narrow results rather than returning a huge unfiltered set.
- **Warm leads from existing connections**: pull prospects from the user's LinkedIn connections matching their criteria.
- **Intent signal from a LinkedIn post**: pass the post URL to find people who liked or commented on it — these are warmer than cold search since they've already shown topical interest.

Always confirm the target criteria (persona, company attributes, warm vs. cold) with the user before running a broad search, and show them a sample of results before staging a large batch into a list.

## Part 2 — Building and managing lists

1. Call `get_all_lists` first to check whether a suitable list already exists — don't create a duplicate list for the same campaign.
2. If none exists, call `create_list` (specify type: people/companies/deals, name, icon, color).
3. Stage found prospects into the list:
   - `add_people_to_list` — people, identified by LinkedIn URL. Creates the CRM record automatically if the person doesn't exist yet.
   - `add_companies_to_list` — companies, identified by LinkedIn URL or website. Same auto-create behavior.
   - `add_records_to_list` — for records that already exist in the CRM by ID.
4. Use `get_list_items` to inspect what's in a list before enriching or sequencing it, and `remove_from_list` to drop entries (does not delete the underlying CRM record).
5. For saved, reusable filtered views of a list (not one-off searches), use `list_views` / `get_view` / `create_view` / `update_view`.

## Guardrails

- Don't create a new list when an existing one already covers the same campaign/segment — always check `get_all_lists` first.
- Don't stage a large batch (hundreds of prospects) without showing the user a sample and getting confirmation on criteria first — a bad filter wastes enrichment credits downstream.
- `delete_list` is destructive (soft-delete) — confirm with the user before calling it.
