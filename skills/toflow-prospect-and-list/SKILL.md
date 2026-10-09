---
name: toflow-prospect-and-list
description: Search LinkedIn for prospects with Sales Navigator-style filters, pull people from existing connections or post engagement, set up a signal agent to monitor for new prospects automatically, and organize results into lists and saved views. Use this whenever the user wants to find new people/companies to target, or build a target account/prospect list.
license: MIT
---
# toflow — Prospect and Build Lists

Find the right people before reaching out, then stage them in a list so they're ready for enrichment and sequencing.

## Part 1 — Finding prospects on LinkedIn

1. Call `get_linkedin_search_guide` and `linkedin_search_parameters` before `search_linkedin` so you use valid filter fields (role, seniority, company size, industry, location, etc.) — don't guess parameter names.
2. Call `search_linkedin` with the confirmed filters. Combine multiple filters to narrow results rather than returning a huge unfiltered set, and confirm the target persona with the user before running a broad search.
3. For warm leads, use `list_linkedin_connections` (optionally with `check_linkedin_connection` to confirm connection status with a specific person) instead of cold search.
4. For intent-based prospecting from a specific post: `get_linkedin_post` / `get_linkedin_post_comments` / `get_linkedin_post_reactions` / `get_linkedin_person_posts` surface people who engaged with relevant content — these are warmer than cold search since they've already shown topical interest. `react_to_linkedin_post` / `comment_on_linkedin_post` let you engage first if the user wants to warm up a prospect before reaching out directly.
5. Show the user a sample of results before staging a large batch into a list.

## Part 2 — Signal agents (ongoing, automatic prospecting)

Use this when the user wants prospects to be surfaced continuously rather than via a one-off search.

1. Call `get_signal_agent_guide` before `setup_signal_agent` — it's the source of truth for what signals are configurable and how they map to lists.
2. Call `get_signal_items` to review what a configured signal agent has surfaced so far.

## Part 3 — Building and managing lists

1. Call `get_all_lists` first to check whether a suitable list already exists — don't create a duplicate list for the same campaign.
2. If none exists, call `create_list` (specify type: people/companies/deals, name, icon, color).
3. Stage found prospects into the list:
   - `add_people_to_list` — people, identified by LinkedIn URL. Creates the CRM record automatically if the person doesn't exist yet.
   - `add_companies_to_list` — companies, identified by LinkedIn URL or website. Same auto-create behavior.
   - `add_records_to_list` — for records that already exist in the CRM by ID.
4. Use `get_list_items` to inspect what's in a list before enriching or sequencing it, and `remove_from_list` to drop entries (does not delete the underlying CRM record).
5. For saved, reusable filtered views of a list (not one-off searches), call `get_view_creation_guide` before `create_view`, then use `list_views` / `get_view` / `update_view` to manage them.

## Guardrails

- Don't create a new list when an existing one already covers the same campaign/segment — always check `get_all_lists` first.
- Don't stage a large batch (hundreds of prospects) without showing the user a sample and getting confirmation on criteria first — a bad filter wastes enrichment credits downstream.
- `delete_list` is destructive (soft-delete) — confirm with the user before calling it.
- Don't comment or react on a post on the user's behalf without confirming the content/action first — it's visible to the prospect and the user's own network.
