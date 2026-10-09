---
name: toflow-prospect-and-list
description: Search LinkedIn for prospects with Sales Navigator-style filters, pull people from existing connections or post engagement, set up a signal agent to monitor for new prospects automatically, and organize results into lists and saved views. Use this whenever the user wants to find new people/companies to target, or build a target account/prospect list.
license: MIT
---
# toflow — Prospect and Build Lists

Find the right people before reaching out, then stage them in a list so they're ready for enrichment and sequencing.

## Part 1 — Finding prospects on LinkedIn

1. Call `list_message_accounts(provider_type='linkedin')` to get the connected account, and `get_linkedin_search_guide` for the full filter reference. For any filter needing an ID (location, industry, company, school, etc.), call `linkedin_search_parameters` first to resolve the correct IDs rather than guessing them.
2. Choose the API mode with the user: `classic` (keywords, location, industry, company, network distance, company size, etc.) or `sales_navigator` (richer — seniority, function, tenure, revenue, technologies, recent activity, saved/recent searches, include/exclude on most filters) — Sales Navigator requires that tier of LinkedIn account.
3. Call `search_linkedin` with the confirmed filters, and pass the returned `cursor` back on the next call to paginate. Never mention internal fields like `api`, `category`, `message_account_id`, or `cursor` to the user. Respect the account's own pacing — there's a jittered gap between pages and a capped number of pages per 15 minutes and per 24 hours; if rate-limited, stop and tell the user when it'll reset rather than retrying immediately.
4. When importing a result into the CRM, map fields explicitly: `name` → `first_name`/`last_name`, `headline` → `job_title`, `public_profile_url` → `linkedin_url`. Skip any result with a null `public_identifier`.
5. For warm leads, use `list_linkedin_connections` (optionally with `check_linkedin_connection` to confirm connection status with a specific person) instead of cold search.
6. For intent-based prospecting from a specific post: `get_linkedin_post` / `get_linkedin_post_comments` / `get_linkedin_post_reactions` / `get_linkedin_person_posts` surface people who engaged with relevant content — these are warmer than cold search since they've already shown topical interest. `react_to_linkedin_post` / `comment_on_linkedin_post` let you engage first if the user wants to warm up a prospect before reaching out directly.
7. Show the user a sample of results before staging a large batch into a list.

## Part 2 — Signal agents (ongoing, automatic prospecting)

Use this when the user wants prospects to be surfaced continuously rather than via a one-off search. Signal types include things like `linkedin_job_change`, `linkedin_hiring`, `linkedin_profile_viewers`, and `linkedin_connections` — confirm the current set with the user's use case in mind.

1. Call `get_signal_agent_guide(signal_type=...)` before `setup_signal_agent` for the specific signal type — it returns that signal's prerequisites (e.g. `linkedin_job_change` requires a Sales Navigator account, not Basic/Premium), its ICP fields and allowed values, and which account ID to pass.
2. Call `list_connected_accounts` to get the right account ID for the signal (per the guide's prerequisites) before setup.
3. Collect the ICP fields the guide asks for from the user — don't invent values for fields it says to ask about.
4. Call `get_signal_items` to review what a configured signal agent has surfaced so far.

## Part 3 — Building and managing lists

1. Call `get_all_lists` first to check whether a suitable list already exists — don't create a duplicate list for the same campaign.
2. If none exists, call `create_list` (specify type: people/companies/deals, name, icon, color).
3. Stage found prospects into the list:
   - `add_people_to_list` — people, identified by LinkedIn URL. Creates the CRM record automatically if the person doesn't exist yet.
   - `add_companies_to_list` — companies, identified by LinkedIn URL or website. Same auto-create behavior.
   - `add_records_to_list` — for records that already exist in the CRM by ID.
4. Use `get_list_items` to inspect what's in a list before enriching or sequencing it, and `remove_from_list` to drop entries (does not delete the underlying CRM record).
5. For saved, reusable filtered views of a list (not one-off searches): call `record_schema` for the list's resource type first to get real attribute titles, then `get_view_creation_guide` before `create_view`. By default all fields are visible — only pass `visible_fields`/`column_order` if the user wants a focused subset, and only pass `filters` (same syntax as `filter_guide`) if they want it pre-filtered. `layout: 'kanban'` is valid only for deal lists. Use `list_views` / `get_view` / `update_view` to manage existing views.

## Guardrails

- Don't create a new list when an existing one already covers the same campaign/segment — always check `get_all_lists` first.
- Don't stage a large batch (hundreds of prospects) without showing the user a sample and getting confirmation on criteria first — a bad filter wastes enrichment credits downstream.
- `delete_list` is destructive (soft-delete) — confirm with the user before calling it.
- Don't comment or react on a post on the user's behalf without confirming the content/action first — it's visible to the prospect and the user's own network.
