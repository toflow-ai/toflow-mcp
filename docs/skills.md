# toflow.ai MCP — Skills Reference

**115 tools** organized by workflow: find prospects → build lists → enrich → sequence → manage CRM.

---

## Prospecting

Find the right people before reaching out.

**Search for prospects**
Search for prospects using Sales Navigator-style filters — role, seniority, company size, industry, location, and more. Use this as the starting point when you have a target persona in mind. Combine multiple filters to narrow results. Pair with enrichment tools after to get verified emails and phones.

**Get prospects from connections**
Pull prospects from your existing LinkedIn connections. Use this when you want to start with warm leads — people who already know you — instead of cold outreach. Returns profiles you are already connected with that match your criteria.

**Find prospects from post engagement**
Find people who liked or commented on a specific LinkedIn post. Use this to identify prospects who have already shown interest in a topic relevant to your product. Pass the post URL and get back a list of engaged profiles ready to enrich and contact.

---

## Lists & Views

Organize prospects into lists before enriching or sequencing.

**get_all_lists**
Returns all lists in the workspace — people lists, company lists, and deal lists — with their IDs, names, and record counts. Call this first when you need a list ID to pass to other tools, or to show the user what lists already exist. Lists are the core organizing unit in toflow.ai.

**create_list**
Creates a new list for people, companies, or deals. Specify the type, name, icon, and color. Call this before adding prospects when no suitable list exists. Returns the new list ID needed for add_people_to_list and other list operations.

**update_list**
Updates a list's name, description, icon, or color. Use this to rename or reorganize existing lists. Only the fields you provide are changed.

**delete_list**
Soft-deletes a list from the workspace. The list and its members are removed from view but not permanently destroyed. Use with caution — confirm with the user before deleting.

**add_people_to_list**
Adds people to a list using LinkedIn URL as the unique identifier. Use this after finding prospects to stage them in a list before enriching or enrolling. If a person does not exist in the CRM yet, they are created automatically.

**add_companies_to_list**
Adds companies to a list using LinkedIn URL or website as the unique identifier. Use when building target account lists. Companies are created in the CRM if they do not already exist.

**add_records_to_list**
Adds existing CRM records to a list by their IDs. Use this when the people or companies are already in the CRM and you want to group them into a new list for a campaign.

**get_list_items**
Returns all items in a list with their CRM IDs and profile data. Use this to inspect list contents, verify who is in a list before sequencing, or to pass IDs to enrichment and CRM tools.

**remove_from_list**
Removes one or more records from a list by their CRM IDs. Does not delete the CRM record — only removes the list membership.

**list_views**
Lists all saved views for a resource type or a specific list. Views are filtered, sorted snapshots of list data. Call this to find a view ID before calling get_view.

**get_view**
Returns the full configuration of a saved view — filters, sort order, visible columns. Use this to understand how a view is set up before modifying it.

**create_view**
Creates a new saved view for a list with custom filters and sort configuration. Use this to help users set up persistent filtered views of their prospect lists.

**update_view**
Updates an existing view's filter and sort configuration. Use this when the user wants to change how a saved view works.

---

## Enrichment

Find verified contact data before reaching out.

**enrich_person_by_linkedin**
Gets or enriches a person's full profile using their LinkedIn URL. Returns name, title, company, location, and available contact data. Call this first when you have a LinkedIn URL and need to create or update a CRM record. Results are cached — if the person was enriched recently, the cached result is returned immediately.

**enrich_person_email**
Finds a verified email address for a person. Pass the person's CRM ID. Returns the email address and confidence score when found. Call get_person_enrichment_status to poll if the result is pending. Always enrich email before enrolling in an email sequence.

**enrich_person_phone**
Finds a phone number for a person. Pass the person's CRM ID. Returns the phone number and type (mobile, direct, etc.) when found. Call get_person_enrichment_status to poll if the result is pending.

**get_person_enrichment_status**
Polls the status of an in-progress enrichment task. Call this after enrich_person_email or enrich_person_phone when the result is pending. Returns the current status and result when complete.

**bulk_enrich_emails**
Finds email addresses for multiple people at once. Pass a list of CRM person IDs. More efficient than calling enrich_person_email in a loop. Returns a task ID — use get_bulk_enrichment_status to poll for completion.

**bulk_enrich_phones**
Finds phone numbers for multiple people at once. Pass a list of CRM person IDs. Returns a task ID — use get_bulk_enrichment_status to poll for completion.

**get_bulk_enrichment_status**
Checks the status of multiple enrichment tasks at once. Pass the task IDs returned by bulk_enrich_emails or bulk_enrich_phones. Returns per-person results as they complete.

**bulk_enrich_list**
Enriches all people in a list for a given enrichment type — email or phone. Use this to enrich an entire prospect list in one call instead of enriching person by person. Call estimate_bulk_enrich_list first to check the credit cost.

**estimate_bulk_enrich_list**
Returns the credit cost of running bulk_enrich_list before it executes. Always call this first when enriching a large list so the user can confirm before credits are spent.

---

## Sequences

Build and run multichannel outreach sequences.

**get_sequence_schema**
Returns the full schema for building sequences — all available node types (email, LinkedIn message, LinkedIn connection request, WhatsApp, wait, condition), their configuration fields, and available template variables. Call this first before create_sequence so you know the exact structure required.

**list_sequences**
Lists all sequences in the workspace with their IDs, names, and status. Call this to find a sequence ID before enrolling someone, or to show the user what sequences exist.

**get_sequence**
Returns the full configuration of a sequence — all nodes, edges, scheduling config, and template variables. Use this to inspect a sequence before enrolling, or to understand its structure before updating.

**create_sequence**
Creates a new sequence with any mix of node types — email, LinkedIn message, LinkedIn connection request, WhatsApp, wait steps, and conditions. Call get_sequence_schema first to get the required structure. Returns the sequence ID needed for enrollment.

**update_sequence**
Updates a sequence's name, scheduling configuration, or full node/edge structure. Only the fields you provide are changed. Use this to add steps, change timing, or fix template content in an existing sequence.

**enroll_in_sequence**
Enrolls a person in a sequence with freshly generated, personalized content. This is the key action that starts outreach for a contact. Pass the person's CRM ID and sequence ID. Personalization variables are filled automatically from the person's CRM profile. Always verify the person has a verified email (if the sequence includes email steps) before enrolling.

**get_enrollment**
Returns the details of a specific sequence enrollment — current step, status, scheduled send times, and generated message content. Use this to check where someone is in a sequence or to review the personalized messages that were generated.

**list_enrollments**
Lists sequence enrollments filtered by sequence or person, with optional status filter (active, completed, paused, failed). Use this to audit who is enrolled in a sequence or to find a specific person's enrollment.

**update_enrollment**
Updates a sequence enrollment — pause, resume, or change its status. Use this when the user wants to stop or pause outreach for a specific contact.

**retry_enrollment**
Retries a failed or invalid sequence enrollment. Use this when an enrollment failed due to a missing email, sending account issue, or other recoverable error that has since been resolved.

**get_sequence_analytics**
Returns detailed analytics and conversion funnel for a sequence — sent, delivered, opened, clicked, replied, and conversion rates per step. Use this to evaluate sequence performance or to help the user decide which sequences are working.

**list_connected_accounts**
Lists all connected sending accounts for the current user — email accounts, LinkedIn accounts, and WhatsApp accounts. Use this before creating a sequence or enrolling someone to confirm which sending accounts are available.

**get_account_load_stats**
Returns current load stats for sending accounts — how many emails or messages are queued, daily limits, and current utilization. Use this to pick the right sending account when multiple are connected, or to check if an account is near its limit.

---

## Email

Read, draft, send, and manage emails.

**inbox_manager_config**
Returns the Inbox Manager configuration for the current workspace — reply detection settings, auto-categorization rules, and connected accounts. Call this to understand how the inbox is set up before managing emails.

**list_emails**
Lists emails in the workspace with optional filters — by person, account, status, date range, or search query. Use this to find emails for a specific contact or to review recent outreach.

**get_email**
Returns a single email by ID — subject, body, recipients, send status, open tracking, and thread info. Use this to read the full content of an email or to get thread context before replying.

**draft_email**
Creates a draft email in the workspace. Use this to prepare an email for review before sending. Returns the draft ID needed for send_email.

**send_email**
Sends a drafted email by its ID. Always draft first and confirm content with the user before calling send_email. This action cannot be undone.

**reply_to_email**
Replies to or follows up on an existing email thread. Pass the thread ID and reply content. Use this to continue a conversation in the same thread rather than starting a new email.

**forward_email**
Forwards an email to new recipients. Pass the email ID and the forwarding addresses. Adds an optional message before the forwarded content.

**update_draft**
Updates fields on an existing draft email — subject, body, recipients, or scheduled send time. Only the fields provided are changed. Use this to edit a draft before sending.

**delete_draft**
Permanently deletes a draft email. Use this to clean up drafts that are no longer needed. This cannot be undone.

**set_email_signature**
Sets or updates the email signature for a connected email account. Pass the account ID and signature HTML or plain text. Use this when the user wants to change or set up their outreach signature.

**get_email_tracking**
Returns open tracking statistics for a sent email — open count, open timestamps, and device/location data when available. Use this to check if a specific email was opened.

**list_email_events**
Returns workspace-level email open analytics — aggregate open rates, click rates, and per-account performance. Use this for a broad view of email engagement across all outreach.

---

## Outreach (LinkedIn & WhatsApp)

Manage conversations across LinkedIn and WhatsApp.

**list_message_threads**
Lists LinkedIn and WhatsApp conversation threads with recent message previews. Use this to find threads for a specific contact or to review recent conversations across channels.

**get_message_thread**
Returns the full message history for a LinkedIn or WhatsApp thread. Use this to read a conversation before replying, or to understand the context of a relationship with a contact.

**list_message_accounts**
Lists all connected LinkedIn and WhatsApp accounts in the workspace. Use this to see which accounts are available before sending messages or setting a primary account.

**set_primary_account**
Sets a default LinkedIn or WhatsApp account for sending. Use this when the user has multiple connected accounts and wants to set a preferred one for outreach.

---

## AI Automations

Create and run sub-agents that operate autonomously on the workspace.

**list_subagents**
Lists all enabled sub-agents in the workspace with their IDs, names, and descriptions. Call this first to find a sub-agent ID before creating tasks or checking its configuration.

**get_subagent**
Returns the full configuration of a sub-agent — its instructions, enabled tools, schedule, and status. Use this to understand what a sub-agent is set up to do before creating tasks for it.

**create_subagent**
Creates a new sub-agent with a name, description, and instructions. Sub-agents run autonomously to complete tasks like prospecting, enriching lists, or following up with contacts. Define the sub-agent's goals and constraints clearly in the instructions field.

**update_subagent**
Updates an existing sub-agent's name, instructions, or enabled tools. Use this to change what a sub-agent does or to refine its instructions based on performance.

**get_worker_instructions**
Returns instructions for human-initiated sub-agent sessions — what the agent should do when a user triggers it manually. Call this at the start of a sub-agent session to load the correct context and goals.

**get_executor_instructions**
Returns instructions for automated or scheduled execution of pending sub-agent tasks. Call this when running tasks on a schedule or in batch mode without a human in the loop.

**get_available_mcp_tools**
Lists all available MCP tools with names and descriptions. Use this inside a sub-agent session when you need to know what tools are available to accomplish a task.

**bulk_create_subagent_tasks**
Creates multiple tasks for a single sub-agent in one call. Use this to queue a batch of work items — for example, a list of LinkedIn URLs to enrich, or a set of contacts to sequence. More efficient than creating tasks one by one.

**list_subagent_tasks**
Lists sub-agent tasks with optional filters by sub-agent ID and status. Use this to see what tasks are pending, in progress, or completed for a given sub-agent.

**get_subagent_task**
Returns the full details of a sub-agent task — its input, current status, execution log, and result. Use this to inspect what a sub-agent did or to debug a failed task.

**claim_subagent_task**
Claims an open task to start working on it. Call this at the start of automated execution to mark a task as in progress and prevent other agents from picking it up simultaneously.

**append_task_log**
Appends a log entry to a running task's execution log. Use this during task execution to record what steps were taken, what was found, or what decisions were made. Helps with debugging and auditing.

**complete_subagent_task**
Marks a task as completed with optional result data. Call this after successfully finishing a task. Pass a summary of what was accomplished so the result is visible in the task log.

**fail_subagent_task**
Marks a task as failed with an error message. Call this when a task cannot be completed due to an unrecoverable error. Include a clear error message so the cause is visible in the task log.

---

## Tasks

Create and track follow-up tasks.

**list_tasks**
Lists tasks for the workspace with optional filters — by assignee, status, due date, or linked record. Use this to review open tasks or to find a specific task before updating it.

**get_task**
Returns a single task by ID with full details — title, description, due date, assignee, status, and linked CRM records. Use this before updating a task to confirm its current state.

**create_task**
Creates a new task. Link it to a person, company, or deal to keep follow-ups organized in the CRM. Assign it to a team member and set a due date.

**update_task**
Updates an existing task — status, due date, assignee, or description. Use this to mark tasks complete, reassign them, or change the due date.

**delete_task**
Permanently deletes a task. Use with caution — confirm with the user before deleting.

---

## Calls

Log and manage call records.

**list_calls**
Lists calls with optional filters — by person, company, date range, or outcome. Use this to review call history for a contact or to find a specific call record.

**get_call**
Returns a single call record by ID — participants, duration, outcome, notes, and linked CRM records.

**log_call**
Logs a completed call or schedules a future call. Link it to a person or company. Include outcome and notes to keep the CRM record complete.

**update_call**
Updates an existing call record — outcome, notes, duration, or scheduled time. Use this to add notes after a call or correct an entry.

**delete_call**
Permanently deletes a call record. Confirm with the user before deleting.

---

## Notes

Attach notes to CRM records.

**list_notes**
Lists notes for the workspace with optional filters — by linked record, author, or date. Use this to review notes on a contact or account.

**get_note**
Returns a single note by ID with full content and linked records.

**create_note**
Creates a new note and links it to a person, company, or deal. Use this to capture meeting summaries, research findings, or context about a contact.

**update_note**
Updates an existing note's content. Use this to correct or expand a note after it was created.

**delete_note**
Permanently deletes a note. Confirm with the user before deleting.

---

## Dashboards & Reports

Build reports and dashboards from CRM data.

**list_datasets**
Lists all available datasets and their exact field names. Call this first before create_report to understand what data is available and what field names to use in report configuration.

**list_dashboards**
Lists all dashboards in the workspace with their IDs and names. Use this to find a dashboard ID before adding a report to it.

**create_dashboard**
Creates a new dashboard. Use this when the user wants a dedicated view for a new set of reports.

**validate_and_preview_report**
Validates a report configuration and returns a 100-row data preview. Always call this before create_report to catch configuration errors and confirm the data looks correct before saving.

**create_report**
Saves a report permanently to a dashboard. Call validate_and_preview_report first. Returns the report ID.

**run_report**
Executes a saved report and returns its full data rows. Use this to fetch the latest data from an existing report.

---

## CRM Records

Create, search, and update people, companies, and deals.

**record_schema**
Returns the attribute schema for a CRM resource type — all available fields, their types, and whether they are required. Call this before create_record or update_record to know exactly which fields are available.

**filter_guide**
Returns the filter, sort, and list_id reference for all list_records calls. Call this when you need to build a complex filter query — it explains the filter syntax and available operators.

**list_records**
Lists CRM records for any resource type — people, companies, or deals — with optional filters, sort, and pagination. Use this to search the CRM, find records matching criteria, or get a list of records to update.

**create_record**
Creates a new CRM record — person, company, or deal. Call record_schema first to know the required and optional fields. Use add_people_to_list or add_companies_to_list instead if you are adding prospects from LinkedIn URLs.

**update_record**
Updates fields on an existing CRM record. PATCH — only the fields you provide are changed. Use this to update a person's stage, add attributes, or correct data.

**bulk_create**
Bulk-creates CRM records — companies, people, or deals — in a single call. More efficient than calling create_record in a loop for large imports.

**get_resource_schema**
Returns the attribute schema for a resource type including all custom attributes defined in the workspace. More detailed than record_schema — includes custom fields added by the team.

**get_person**
Returns a single person (contact) by ID with full CRM profile — all attributes, email addresses, phone numbers, social profiles, linked companies, deals, notes, tasks, and enrichment status. Use this to get the complete picture of a contact before deciding next steps.

**delete_person**
Soft-deletes a person from the workspace. The record is hidden from views but not permanently removed. Confirm with the user before deleting.

**get_company**
Returns a single company by ID with full profile — all attributes, linked contacts, deals, notes, and tasks. Use this to review an account before outreach or to find contacts at a company.

**delete_company**
Soft-deletes a company from the workspace. Confirm with the user before deleting.

**list_company_categories**
Lists all company categories available in the workspace. Use this to find the right category before tagging a company, or to show the user what categories exist.

**get_or_create_category**
Gets an existing category by name or creates it if it does not exist. Use this when tagging a company with a category — avoids duplicates by checking for the category first.

**get_deal**
Returns a single deal by ID with full profile — stage, pipeline, value, linked contacts, linked company, notes, tasks, and activity history. Use this to review a deal before updating it or deciding next actions.

**delete_deal**
Soft-deletes a deal from the workspace. Confirm with the user before deleting.

**list_pipelines**
Lists all sales pipelines in the workspace with their IDs and names. Call this to find the right pipeline ID before creating a deal or listing stages.

**list_stages**
Lists all stages for a given pipeline with their IDs, names, and order. Use this to find the correct stage ID when creating or updating a deal.

**add_person_to_deal**
Associates a person (contact) with a deal. Use this to link a contact to an opportunity they are involved in.

**remove_person_from_deal**
Removes a person's association with a deal. Use this when a contact is no longer involved in an opportunity.

---

## Workspace

Inspect workspace and member info.

**get_workspace**
Returns the current workspace details — name, ID, plan, settings — and the authenticated user's info. Call this at the start of a session to confirm which workspace is active and who is logged in.

**list_workspace_members**
Lists all active workspace members — names, emails, roles, and IDs. Use this to find a member ID for task assignment or to show the user who is on the team.
