# toflow.ai MCP — Skills Reference

**132 tools** organized by workflow: find prospects → build lists → enrich → sequence → manage CRM.

---

## Prospecting

Find the right people before reaching out.

**get_linkedin_search_guide**
Returns the workflow, rate limits, and full filter reference for search_linkedin. Call this before search_linkedin so you use valid filter fields and respect pacing limits.

**linkedin_search_parameters**
Resolves filter values — location, industry, company, school, and more — to the internal IDs search_linkedin needs. Call this before search_linkedin whenever a filter requires an ID rather than plain text.

**search_linkedin**
Search for prospects using classic or Sales Navigator-style filters — role, seniority, company size, industry, location, and more. Use this as the starting point when you have a target persona in mind. Combine multiple filters to narrow results. Paginate with the returned cursor. Pair with enrichment tools after to get verified emails and phones.

**list_linkedin_connections**
Pull prospects from your existing LinkedIn connections. Use this when you want to start with warm leads — people who already know you — instead of cold outreach. Returns profiles you are already connected with that match your criteria.

**check_linkedin_connection**
Checks whether a specific LinkedIn profile is already a connection. Use this before deciding whether to send a connection request or message directly.

**get_linkedin_person_posts**
Returns recent LinkedIn posts by a specific person. Use this to find a relevant post to engage with, or to understand what a prospect has been talking about recently.

**get_linkedin_post**
Returns a single LinkedIn post by its URL — content, author, and engagement counts.

**get_linkedin_post_comments**
Returns comments on a LinkedIn post. Use this to identify prospects who have already shown interest in a topic relevant to your product.

**get_linkedin_post_reactions**
Returns reactions on a LinkedIn post. Like comments, these surface people who have already engaged with relevant content — warmer than cold search.

**react_to_linkedin_post**
Reacts to a LinkedIn post on the user's behalf. Use this to warm up a prospect before reaching out directly — always confirm the action with the user first.

**comment_on_linkedin_post**
Comments on a LinkedIn post on the user's behalf. Same warm-up use case as react_to_linkedin_post — always confirm the comment content with the user first.

---

## Signal Agents

Surface new prospects continuously instead of via one-off search.

**get_signal_agent_guide**
Returns the setup guide for a specific signal type (linkedin_job_change, linkedin_hiring, linkedin_profile_viewers, linkedin_connections) — prerequisites, ICP fields, and allowed values. Always call this before setup_signal_agent for the signal type you're configuring.

**setup_signal_agent**
Configures a signal agent to continuously surface prospects matching an ICP — e.g. people who recently changed jobs, companies actively hiring, or profile viewers. Requires the account ID and ICP fields the matching guide specifies.

**get_signal_items**
Returns what a configured signal agent has surfaced so far. Use this to review and act on new prospects a signal agent has found.

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

**get_view_creation_guide**
Returns the step-by-step guide for creating a saved view — required fields and workflow. Always call this before create_view.

**create_view**
Creates a new saved view for a list with custom filters and column configuration. Use this to help users set up persistent filtered views of their prospect lists.

**update_view**
Updates an existing view's filter and column configuration. Use this when the user wants to change how a saved view works.

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

## AI Enrichment

Derive custom fields beyond standard email/phone enrichment, using an AI model against real record data.

**get_ai_enrichment_guide**
Returns the guide for creating an AI enrichment — available model tiers and their cost, template variable syntax, and the approval steps required before running. Always call this before create_ai_enrichment.

**create_ai_enrichment**
Creates a custom AI enrichment that writes its output into a CRM attribute — e.g. deriving a person's likely pain point or a company's tech stack. Requires an explicit model choice and user approval of the full prompt/field mapping before creating.

**list_ai_enrichments**
Lists configured AI enrichments in the workspace. Call this before creating a new one to check whether a similar enrichment already exists.

**get_ai_enrichment**
Returns a specific AI enrichment's configuration and results.

**update_ai_enrichment**
Updates an AI enrichment's prompt or target field.

**run_ai_enrichment**
Executes an AI enrichment against the configured records.

---

## Sequences

Build and run multichannel outreach sequences.

**get_sequence_creation_guide**
Returns the mandatory step-by-step guide for creating a sequence — scheduling config, account selection, node/edge structure, and writing quality rules. Always call this before create_sequence and follow every step.

**get_sequence_schema**
Returns the full schema for building sequences — all available node types (email, send_linkedin_connection, is_in_linkedin_network, linkedin_message, linkedin_inmail, whatsapp_message, view_linkedin_profile), their configuration fields, and available template variables. Call this before create_sequence so you know the exact structure required.

**list_sequences**
Lists all sequences in the workspace with their IDs, names, and status. Call this to find a sequence ID before enrolling someone, or to show the user what sequences exist.

**list_sequence_templates**
Lists available sequence templates. Call this when the user wants to start from a template rather than a blank sequence.

**get_sequence**
Returns the full configuration of a sequence — all nodes, edges, scheduling config, and template variables. Use this to inspect a sequence before enrolling, or to understand its structure before updating.

**create_sequence**
Creates a new sequence with any mix of node types — email, LinkedIn message, LinkedIn connection request, WhatsApp, wait steps, and conditions. Call get_sequence_creation_guide and get_sequence_schema first to get the required structure. Returns the sequence ID needed for enrollment.

**update_sequence**
Updates a sequence's name, scheduling configuration, or full node/edge structure. Only the fields you provide are changed. Use this to add steps, change timing, or fix template content in an existing sequence.

**get_enroll_in_sequence_guide**
Returns the mandatory step-by-step guide for enrolling a person in a sequence — identifying AI-generated vs manual nodes, the personalize-vs-as-is decision, per-channel contact-data checks, and the review-before-send gate. Always call this before enroll_in_sequence.

**enroll_in_sequence**
Enrolls a person in a sequence with freshly generated, personalized content. This is the key action that starts outreach for a contact. Pass the person's CRM ID and sequence ID. Personalization variables are filled automatically from the person's CRM profile. Always verify the person has a verified email (if the sequence includes email steps) before enrolling.

**get_enrollment**
Returns the details of a specific sequence enrollment — current step, status, scheduled send times, and generated message content. Use this to check where someone is in a sequence or to review the personalized messages that were generated.

**list_enrollments**
Lists sequence enrollments filtered by sequence or person, with optional status filter (active, queued, paused, invalid, verifying, skipped, cancelled). Use this to audit who is enrolled in a sequence or to find a specific person's enrollment.

**get_update_enrollment_guide**
Returns the guide for updating a sequence enrollment's status or node content — allowed status transitions and the per-node-type config shape. Always call this before update_enrollment when changing node content or an account.

**update_enrollment**
Updates a sequence enrollment — change its status (e.g. pause, cancel) and/or fix content for pending nodes. Use this when the user wants to stop, pause, or correct outreach for a specific contact.

**set_enrollment_node_content**
Sets personalized content for a specific pending node in an enrollment. Use this when drafting manual-node content as part of the enrollment review flow.

**retry_enrollment**
Retries a failed or invalid sequence enrollment. Use this when an enrollment failed due to a missing email, sending account issue, or other recoverable error that has since been resolved.

**resolve_invalid_enrollments**
Returns guidance for fixing enrollments that came back with an invalid status. Call this when handling an enrollment's exit_reason.

**get_sequence_analytics**
Returns detailed analytics and conversion funnel for a sequence — sent, delivered, opened, clicked, replied, and conversion rates per step. Use this to evaluate sequence performance or to help the user decide which sequences are working.

**list_connected_accounts**
Lists all connected sending accounts for the current user — email accounts, LinkedIn accounts, and WhatsApp accounts. Use this before creating a sequence or enrolling someone to confirm which sending accounts are available.

**get_account_load_stats**
Returns current load stats for sending accounts — how many emails or messages are queued, daily limits, and current utilization. Use this to pick the right sending account when multiple are connected, or to check if an account is near its limit.

---

## Email

Read, draft, send, and manage emails.

**list_emails**
Lists emails in the workspace with optional filters — by person, account, status, date range, or search query. Use this to find emails for a specific contact or to review recent outreach.

**get_email**
Returns a single email by ID — subject, body, recipients, send status, open tracking, and thread info. Use this to read the full content of an email or to get thread context before replying.

**get_draft_email_guide**
Returns the mandatory step-by-step guide for drafting an email — account selection, signature check, recipient resolution, and content format. Always call this before draft_email.

**draft_email**
Creates a draft email (HTML body) in the workspace. Use this to prepare an email for review before sending. Returns the draft ID and a shareable URL needed for send_email.

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

Send and manage conversations across LinkedIn and WhatsApp.

**send_connection_request**
Sends a LinkedIn connection request, with an optional note. Omitting the note allows a materially higher daily send volume — ask the user before including one.

**send_linkedin_message**
Sends a direct message to an existing LinkedIn connection.

**send_inmail**
Sends a premium/InMail-style LinkedIn message. Requires a premium LinkedIn account (Sales Navigator, Recruiter, or Premium Business) and consumes an InMail credit. Don't use this on an existing 1st-degree connection — use send_linkedin_message instead.

**send_whatsapp_message**
Sends a WhatsApp message to a contact.

**send_draft_message**
Sends a previously generated message draft (e.g. AI-generated enrollment content awaiting review). Always show the draft's actual content to the user before calling this.

**delete_message_draft**
Deletes a message draft that's no longer needed.

**list_message_threads**
Lists LinkedIn and WhatsApp conversation threads with recent message previews. Use this to find threads for a specific contact or to review recent conversations across channels.

**get_message_thread**
Returns the full message history for a LinkedIn or WhatsApp thread. Use this to read a conversation before replying, or to understand the context of a relationship with a contact.

**list_message_accounts**
Lists all connected LinkedIn and WhatsApp accounts in the workspace. Use this to see which accounts are available before sending messages or setting a primary account.

**set_primary_account**
Sets a default LinkedIn or WhatsApp account for sending. Use this when the user has multiple connected accounts and wants to set a preferred one for outreach.

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

**get_report_guide**
Returns the guide for validate_and_preview_report — prerequisites, and the exact query/chart/filter shape. Always call this before configuring a report.

**validate_and_preview_report**
Validates a report configuration and returns a data preview. Always call this before create_report to catch configuration errors and confirm the data looks correct before saving.

**create_report**
Saves a report permanently to a dashboard. Call validate_and_preview_report first. Returns the report ID.

**run_report**
Executes a saved report and returns its full data rows. Use this to fetch the latest data from an existing report.

---

## CRM Records

Create, search, and update people, companies, and deals.

**record_schema**
Returns the attribute schema for a CRM resource type — all available attribute titles, their types, and whether they are required. Call this before create_record or update_record to know exactly which fields are available; pass attribute titles as keys in the attributes dict.

**filter_guide**
Returns the filter, sort, and list_id reference for all list_records calls. Call this when you need to build a complex filter query — it explains the FilterGroup syntax, available operators, and how to resolve a saved view by name.

**list_records**
Lists CRM records for any resource type — people, companies, or deals — with optional filters, sort, and pagination. Use this to search the CRM, find records matching criteria, or get a list of records to update. Pass known IDs via `ids` instead of looping single-record fetches.

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

**list_attributes**
Lists custom attributes defined for a resource type. Use this to check whether a suitable field already exists before creating a new one.

**get_attribute**
Returns a single custom attribute's configuration — type, allowed values, and whether it's editable.

**create_attribute**
Creates a new custom attribute for a resource type (text, select, multiselect, number, boolean, etc.). Confirm the resource type and field type with the user first.

**update_attribute**
Updates an existing custom attribute's configuration.

**delete_attribute**
Deletes a custom attribute. Confirm with the user before deleting — existing record data on that field may be affected.

---

## Workspace

Inspect workspace and member info.

**get_workspace**
Returns the current workspace details — name, ID, plan, settings — and the authenticated user's info. Call this at the start of a session to confirm which workspace is active and who is logged in.

**list_workspace_members**
Lists all active workspace members — names, emails, roles, and IDs. Use this to find a member ID for task assignment or to show the user who is on the team.
