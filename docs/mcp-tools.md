# toflow MCP — Tool Reference

**Server URL:** `https://mcp.toflow.ai/mcp`

**115 tools** across 13 categories.

---

## Prospecting

Find the right prospects and gather intent signals before reaching out.

- Search for prospects using Sales Navigator-style filters (role, company, location, industry, and more)
- Get prospects from your existing connections
- Find prospects from post comments and reactions on popular content

## Sequences

| Tool | Description |
|---|---|
| `get_sequence_schema` | Get node types, config fields, and template variables for sequences. |
| `list_sequences` | List sequences in the workspace with pagination. |
| `get_sequence` | Get full details of a sequence including all nodes and edges. |
| `create_sequence` | Create a sequence with any mix of node types. |
| `update_sequence` | Update a sequence's name, scheduling config, or full structure (nodes + edges). |
| `enroll_in_sequence` | Enroll a person in a sequence with freshly generated, personalized content. |
| `get_enrollment` | Get details of a specific sequence enrollment. |
| `list_enrollments` | List sequence enrollments filtered by sequence or person, with optional status filter. |
| `update_enrollment` | Update a sequence enrollment. |
| `retry_enrollment` | Retry a failed or invalid sequence enrollment. |
| `get_sequence_analytics` | Get detailed analytics and conversion funnel for a sequence. |
| `list_connected_accounts` | List all connected sending accounts for the current user. |
| `get_account_load_stats` | Get current load stats for sending accounts. |

## Email

| Tool | Description |
|---|---|
| `inbox_manager_config` | Inbox Manager configuration for the current workspace and member. |
| `list_emails` | List emails in the workspace with optional filters. |
| `get_email` | Get a single email by ID — returns subject, body, recipients, status, and thread info. |
| `draft_email` | Draft an email in the workspace. |
| `send_email` | Send a drafted email by its ID. |
| `reply_to_email` | Reply to or follow up on an existing email thread. |
| `forward_email` | Forward an email to new recipients. |
| `update_draft` | Update fields on an existing draft email — only provided fields are changed. |
| `delete_draft` | Delete a draft email permanently. |
| `set_email_signature` | Set or update the email signature for a connected email account. |
| `get_email_tracking` | Get open tracking statistics for a sent email. |
| `list_email_events` | Get workspace-level email open analytics. |

## Outreach

| Tool | Description |
|---|---|
| `list_message_threads` | List LinkedIn and WhatsApp conversation threads. |
| `get_message_thread` | Get full message history for a LinkedIn or WhatsApp thread. |
| `list_message_accounts` | List connected LinkedIn and WhatsApp accounts. |
| `set_primary_account` | Set a default LinkedIn or WhatsApp account for sending. |

## Enrichment

| Tool | Description |
|---|---|
| `enrich_person_by_linkedin` | Get or enrich a person's profile using their LinkedIn URL. |
| `enrich_person_email` | Find an email address for a person. |
| `enrich_person_phone` | Find a phone number for a person. |
| `get_person_enrichment_status` | Poll the status of an in-progress enrichment task. |
| `bulk_enrich_emails` | Find email addresses for multiple people at once. |
| `bulk_enrich_phones` | Find phone numbers for multiple people at once. |
| `get_bulk_enrichment_status` | Check status of multiple enrichment tasks at once. |
| `bulk_enrich_list` | Enrich all people in a list for a given enrichment type. |
| `estimate_bulk_enrich_list` | Show the credit cost of bulk_enrich_list before it runs. |

## Lists & Views

| Tool | Description |
|---|---|
| `get_all_lists` | List all accessible lists in the workspace. |
| `create_list` | Create a new list for people, companies, or deals. |
| `update_list` | Update a list's name, description, icon, or color. |
| `delete_list` | Delete a list (soft delete). |
| `add_people_to_list` | Add people to a list using LinkedIn URL as the unique identifier. |
| `add_companies_to_list` | Add companies to a list using LinkedIn URL or website as the unique identifier. |
| `add_records_to_list` | Add existing CRM records to a list by their IDs. |
| `get_list_items` | Get all items in a list. |
| `remove_from_list` | Remove one or more resources from a list by their CRM IDs. |
| `list_views` | List all saved views for a resource type or a specific list. |
| `get_view` | Get full details of a saved view by its ID. |
| `create_view` | Create a new saved view for a specific list. |
| `update_view` | Update an existing view's configuration. |

## AI Automations

| Tool | Description |
|---|---|
| `list_subagents` | List all enabled sub-agents in the workspace. |
| `get_subagent` | Get the full configuration of a sub-agent by ID. |
| `create_subagent` | Create a new sub-agent for this workspace. |
| `update_subagent` | Update an existing sub-agent. |
| `get_worker_instructions` | Get instructions for human-initiated subagent sessions. |
| `get_executor_instructions` | Get instructions for automated/scheduled execution of pending subagent tasks. |
| `get_available_mcp_tools` | List all available MCP tools with names and descriptions. |
| `bulk_create_subagent_tasks` | Bulk-create multiple tasks for a single sub-agent in one call. |
| `list_subagent_tasks` | List sub-agent tasks, optionally filtered by subagent_id and status. |
| `get_subagent_task` | Get full details of a sub-agent task including execution log. |
| `claim_subagent_task` | Claim an open task to start working on it. |
| `append_task_log` | Append a log entry to a running task's execution log. |
| `complete_subagent_task` | Mark a task as completed with optional result data. |
| `fail_subagent_task` | Mark a task as failed with an error message. |

## Tasks

| Tool | Description |
|---|---|
| `list_tasks` | List tasks for the workspace, with optional filters. |
| `get_task` | Retrieve a single task by its ID. |
| `create_task` | Create a new task. |
| `update_task` | Update an existing task. |
| `delete_task` | Delete a task permanently. |

## Calls

| Tool | Description |
|---|---|
| `list_calls` | List calls with optional filters. |
| `get_call` | Retrieve a single call by its ID. |
| `log_call` | Log a call or schedule a future call. |
| `update_call` | Update an existing call. |
| `delete_call` | Delete a call record permanently. |

## Notes

| Tool | Description |
|---|---|
| `list_notes` | List notes for the workspace, with optional filters. |
| `get_note` | Retrieve a single note by its ID. |
| `create_note` | Create a new note. |
| `update_note` | Update an existing note. |
| `delete_note` | Delete a note permanently. |

## Dashboards & Reports

| Tool | Description |
|---|---|
| `list_datasets` | List available datasets and their exact field names. |
| `list_dashboards` | List all dashboards in the workspace. |
| `create_dashboard` | Create a new dashboard. |
| `validate_and_preview_report` | Validate a report configuration and return a 100-row data preview. |
| `create_report` | Save a report permanently to a dashboard. |
| `run_report` | Execute a saved report and return its data rows. |

## CRM Records

| Tool | Description |
|---|---|
| `record_schema` | Attribute schema for a CRM resource type. |
| `filter_guide` | Filter, sort, and list_id reference for all CRM list_* tools. |
| `list_records` | List CRM records for any resource type. |
| `create_record` | Create a new CRM record (person, company, or deal). |
| `update_record` | Update fields on an existing CRM record — PATCH, only provided fields are changed. |
| `bulk_create` | Bulk create CRM records (companies, people, or deals). |
| `get_resource_schema` | Get the attribute schema for a resource type (company, person, deal). |
| `get_person` | Get a single person (contact) by ID with full CRM profile including all attributes. |
| `delete_person` | Soft-delete a person from the workspace. |
| `get_company` | Get a single company by ID with full profile. |
| `delete_company` | Soft-delete a company from the workspace. |
| `list_company_categories` | List all company categories available in the workspace. |
| `get_or_create_category` | Get an existing category by name or create it if it doesn't exist. |
| `get_deal` | Get a single deal by ID with full profile. |
| `delete_deal` | Soft-delete a deal from the workspace. |
| `list_pipelines` | List all sales pipelines in the workspace. |
| `list_stages` | List all stages for a given pipeline. |
| `add_person_to_deal` | Associate a person (contact) with a deal. |
| `remove_person_from_deal` | Remove a person's association with a deal. |

## Workspace

| Tool | Description |
|---|---|
| `get_workspace` | Get the current workspace details and authenticated user info. |
| `list_workspace_members` | List all active workspace members in the workspace. |
