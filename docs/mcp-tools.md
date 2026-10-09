# toflow MCP — Tool Reference

**Server URL:** `https://mcp.toflow.ai/mcp`

**132 tools** across 14 categories.

---

## Prospecting

| Tool | Description |
|---|---|
| `get_linkedin_search_guide` | Workflow, rate limits, and full filter reference for search_linkedin. |
| `linkedin_search_parameters` | Resolve filter values (location, industry, company, school, etc.) to the IDs search_linkedin needs. |
| `search_linkedin` | Search for prospects using classic or Sales Navigator-style filters (role, company, location, industry, and more). |
| `list_linkedin_connections` | Get prospects from your existing LinkedIn connections. |
| `check_linkedin_connection` | Check connection status with a specific LinkedIn profile. |
| `get_linkedin_person_posts` | Get recent LinkedIn posts by a specific person. |
| `get_linkedin_post` | Get a single LinkedIn post by URL. |
| `get_linkedin_post_comments` | Get comments on a LinkedIn post — a source of engaged prospects. |
| `get_linkedin_post_reactions` | Get reactions on a LinkedIn post — a source of engaged prospects. |
| `react_to_linkedin_post` | React to a LinkedIn post. |
| `comment_on_linkedin_post` | Comment on a LinkedIn post. |

## Signal Agents

Ongoing, automatic prospecting — surfaces new prospects continuously instead of via one-off search.

| Tool | Description |
|---|---|
| `get_signal_agent_guide` | Setup guide for a specific signal type — prerequisites, ICP fields, and allowed values. |
| `setup_signal_agent` | Configure a signal agent (`linkedin_job_change`, `linkedin_hiring`, `linkedin_profile_viewers`, `linkedin_connections`). |
| `get_signal_items` | Review what a configured signal agent has surfaced. |

## Sequences

| Tool | Description |
|---|---|
| `get_sequence_creation_guide` | Mandatory step-by-step guide for creating a sequence. |
| `get_sequence_schema` | Get node types, config fields, and template variables for sequences. |
| `list_sequences` | List sequences in the workspace with pagination. |
| `list_sequence_templates` | List available sequence templates to start from. |
| `get_sequence` | Get full details of a sequence including all nodes and edges. |
| `create_sequence` | Create a sequence with any mix of node types. |
| `update_sequence` | Update a sequence's name, scheduling config, or full structure (nodes + edges). |
| `get_enroll_in_sequence_guide` | Mandatory step-by-step guide for enrolling a person in a sequence. |
| `enroll_in_sequence` | Enroll a person in a sequence with freshly generated, personalized content. |
| `get_enrollment` | Get details of a specific sequence enrollment. |
| `list_enrollments` | List sequence enrollments filtered by sequence or person, with optional status filter. |
| `get_update_enrollment_guide` | Guide for updating a sequence enrollment's status or node content. |
| `update_enrollment` | Update a sequence enrollment's status or node content. |
| `set_enrollment_node_content` | Set personalized content for a specific pending node in an enrollment. |
| `retry_enrollment` | Retry a failed or invalid sequence enrollment. |
| `resolve_invalid_enrollments` | Get guidance for fixing enrollments that came back invalid. |
| `get_sequence_analytics` | Get detailed analytics and conversion funnel for a sequence. |
| `list_connected_accounts` | List all connected sending accounts for the current user. |
| `get_account_load_stats` | Get current load stats for sending accounts. |

## Email

| Tool | Description |
|---|---|
| `list_emails` | List emails in the workspace with optional filters. |
| `get_email` | Get a single email by ID — returns subject, body, recipients, status, and thread info. |
| `get_draft_email_guide` | Mandatory step-by-step guide for drafting an email. |
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
| `send_connection_request` | Send a LinkedIn connection request, with an optional note. |
| `send_linkedin_message` | Send a direct message to a LinkedIn connection. |
| `send_inmail` | Send a premium/InMail-style LinkedIn message (requires a premium account). |
| `send_whatsapp_message` | Send a WhatsApp message. |
| `send_draft_message` | Send a previously generated message draft. |
| `delete_message_draft` | Delete a message draft that's no longer needed. |
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

## AI Enrichment

Custom, AI-derived fields beyond standard email/phone enrichment.

| Tool | Description |
|---|---|
| `get_ai_enrichment_guide` | Guide for creating an AI enrichment — model options, template variables, and approval steps. |
| `create_ai_enrichment` | Create a custom AI enrichment that writes its output into a CRM attribute. |
| `list_ai_enrichments` | List configured AI enrichments. |
| `get_ai_enrichment` | Get a specific AI enrichment's configuration and results. |
| `update_ai_enrichment` | Update an AI enrichment's prompt or target field. |
| `run_ai_enrichment` | Execute an AI enrichment. |

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
| `get_view_creation_guide` | Step-by-step guide for creating a saved view. |
| `create_view` | Create a new saved view for a specific list. |
| `update_view` | Update an existing view's configuration. |

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
| `get_report_guide` | Guide for validate_and_preview_report — prerequisites, query/chart/filter syntax. |
| `validate_and_preview_report` | Validate a report configuration and return a data preview. |
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
| `get_resource_schema` | Get the attribute schema for a resource type (company, person, deal), including custom attributes. |
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
| `list_attributes` | List custom attributes for a resource type. |
| `get_attribute` | Get a single custom attribute's configuration. |
| `create_attribute` | Create a new custom attribute for a resource type. |
| `update_attribute` | Update an existing custom attribute. |
| `delete_attribute` | Delete a custom attribute. |

## Workspace

| Tool | Description |
|---|---|
| `get_workspace` | Get the current workspace details and authenticated user info. |
| `list_workspace_members` | List all active workspace members in the workspace. |
