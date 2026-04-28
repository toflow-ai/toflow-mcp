# toflow.ai MCP — Prompt Gallery

**37 ready-to-use prompts** organized by workflow. Copy any prompt into your AI client with toflow MCP connected.

---

## Research

### Research a Prospect from LinkedIn URL
```
Research this prospect for me: [LinkedIn URL or name + company]

Use the toflow MCP tools in this order:
1. Call `enrich_person_by_linkedin` with their LinkedIn URL to pull their full profile and create/update them in toflow
2. Call `get_person` on the returned person_id to get their email, phone, ICP Score, Key Talking Points, and LinkedIn Outreach Stage
3. Call `get_company` using person.company reference to get company size, funding, website, and description
4. If email is missing, call `enrich_person_email` to find it
5. Call `list_notes` (person_id=person_id) to pull any prior CRM history
6. Call `list_emails` with their domain to check recent email threads

Output a structured research brief with: profile summary, company overview, CRM history, qualification signals, and a recommended next step.
```

---

### Morning Sales Briefing
```
Give me my daily sales briefing.

Use the toflow MCP tools in this order:
1. Call `list_records` (resource_type=deal, filters: status is not won/lost, sort by Expected Close Date asc) to pull all open deals — flag any closing within 7 days
2. Call `list_tasks` (task_group='today') to get overdue and due-today tasks
3. Call `list_emails` (page_size=20, sort_by='email_date', sort_order='desc') to identify unread from prospects and sent emails waiting on replies
4. Call `list_message_threads` (page_size=20) to surface unread LinkedIn/WhatsApp messages
5. Call `list_enrollments` to check sequences with recent activity

Prioritize output: #1 Priority action → Today's pipeline alerts (closing soon, high intent) → Tasks due → Email priorities → LinkedIn/WhatsApp unread → 3 suggested actions.
```

---

### Find and Research Accounts with Recent Funding
```
Find companies in [Industry] that raised a Series A or B in the last 90 days. For each one:

1. Use web search to identify recently funded companies matching the criteria
2. For each company, call `list_records` (resource_type=company, search=company_name) to check if they're already in toflow
3. If not in toflow, call `create_record` (resource_type=company) — first call `record_schema(resource_type=company)` to get valid field names
4. Find their key decision-maker contacts on LinkedIn, then call `enrich_person_by_linkedin` for each
5. Call `get_all_lists` to find or create a 'Recently Funded' list, then call `add_people_to_list` and `add_companies_to_list` to add them
6. Call `create_record` (resource_type=person) with tag 'Recently Funded' for new contacts

Return a table of all found companies with their funding round, key contacts, and enrichment status.
```

---

### Batch Research All Prospects in a List
```
Research every prospect in my '[List Name]' list and give me a brief on each one.

Use toflow MCP tools in this order:
1. Call `get_all_lists` to find the target list — confirm with me if multiple match
2. Call `get_list_items` (list_id=...) to retrieve all contacts
3. For each contact:
   a. Call `get_person` (person_id=...) to pull their title, company reference, email, ICP Score, LinkedIn Outreach Stage
   b. Call `get_company` (company_id from person.company) to get size, funding, website, description
   c. If they have a LinkedIn URL but thin data, call `enrich_person_by_linkedin` to refresh their profile
   d. Call `list_notes` (person_id=person_id) to check any prior CRM history
4. Use web search to supplement: recent company news, funding signals, hiring patterns for any high-priority contacts
5. Compile a research brief for each person

Output a summary table first (name, title, company, ICP Score, outreach stage, recommended action), followed by individual briefs for the top prospects. Flag anyone already in an active sequence — do not recommend cold outreach to them.
```

---

## Prospecting

### Search LinkedIn and Build a Prospect List
```
Build a prospect list for me. Search LinkedIn for: [Job Title] at companies with [Employee Range] employees in [Location / Industry].

Use toflow MCP tools in this order:
1. Call `linkedin_search_parameters` to understand valid search filters before building the query
2. Call `search_linkedin` with the appropriate filters (title, company_size, location, industry) to find matching prospects
3. For each result, call `enrich_person_by_linkedin` to pull their full profile and create them in toflow
4. Call `get_all_lists` to find my existing prospect lists — ask me which list to use, or call `create_list` to create a new one
5. Call `add_people_to_list` to add all enriched contacts to the list
6. Call `enrich_person_email` for any contacts missing an email address

Return the total found, enriched, and added — with a summary table of contacts and their companies.
```

---

### Find VP Sales Contacts at Mid-Market Companies
```
Find VP of Sales, Head of Revenue, and Sales Director contacts at companies with 50–200 employees in [Industry]. Add them to my 'Mid-Market Prospects' list.

Use toflow MCP tools:
1. Call `linkedin_search_parameters` to check valid title and company_size filter values
2. Call `search_linkedin` filtering by titles (VP Sales, Head of Revenue, Sales Director) and company size 50–200
3. For each result, call `enrich_person_by_linkedin` to create/update their toflow record
4. Call `get_all_lists` and find the 'Mid-Market Prospects' list — if it doesn't exist, call `create_list` to create it
5. Call `add_people_to_list` to add all contacts
6. For contacts without email, call `enrich_person_email` to find their work email

Return a summary table with name, title, company, size, and email enrichment status.
```

---

### Score and Qualify a Prospect (ICP Fit Check)
```
Qualify this prospect against our ICP: [LinkedIn URL, name, or person ID]

Use toflow MCP tools:
1. If LinkedIn URL: call `enrich_person_by_linkedin` to fetch their profile and create them in toflow
2. If name/email: call `list_records` (resource_type=person, search=name) to find them in the CRM
3. Call `get_person` to pull title, company reference, ICP Score field (check if already scored)
4. Call `get_company` to get company size, industry, funding, website
5. Use web search to check their tool stack (job postings, LinkedIn tech mentions, integrations page)
6. Score across 5 dimensions: Outbound Motion (25pts), Company Fit (20pts), Tool Stack Pain (20pts), Role Fit (20pts), Buying Signals (15pts)
7. Ask me before updating — if I approve, call `update_record` (resource_type=person) to set ICP Score and Key Talking Points fields

Output a full ICP Qualification Report with score breakdown, tier (Hot/Warm/Lukewarm/Weak/Disqualify), recommended action, and key talking points.
```

---

### Import People from LinkedIn Post Comments
```
Import everyone who commented on this LinkedIn post into toflow: [paste LinkedIn post URL]

Use toflow MCP tools in this order:
1. Call `list_message_accounts(provider_type="linkedin")` to get a connected LinkedIn account
2. Call `get_all_lists` to find an existing list to add people to — if none, call `create_list`
3. Process page by page in a loop:
   a. Call `get_linkedin_post_comments(message_account_id=..., post_url=..., cursor=...)` to fetch one page
   b. For each comment, extract linkedin_url, name, job_title, and description
   c. Call `add_people_to_list(list_id=..., people=[...])` with all extracted people from this page
   d. Report progress: "Page X done — Y added, Z already in list, W newly created"
   e. If cursor is returned, repeat with new cursor — otherwise stop

Return: total pages, comments fetched, people added, already in list, newly created, errors.
```

---

### Import People from LinkedIn Post Reactions
```
Import everyone who reacted to this LinkedIn post into toflow: [paste LinkedIn post URL]

Use toflow MCP tools in this order:
1. Call `list_message_accounts(provider_type="linkedin")` to get a connected LinkedIn account
2. Call `get_all_lists` to find an existing list — if none, call `create_list`
3. Process page by page in a loop:
   a. Call `get_linkedin_post_reactions(message_account_id=..., post_url=..., cursor=...)` to fetch one page
   b. For each reaction, extract linkedin_url, name, job_title from author (skip companies)
   c. Call `add_people_to_list(list_id=..., people=[...])` with all extracted people
   d. Report progress after each page
   e. If paging.cursor is returned, repeat — otherwise stop

Return: total pages, reactions fetched, people added, already in list, companies skipped, errors.
```

---

### Sync My LinkedIn Connections into toflow
```
Import my LinkedIn connections into toflow and add them to a list.

Use toflow MCP tools in this order:
1. Call `list_message_accounts(provider_type="linkedin")` to get connected LinkedIn accounts
2. Call `get_all_lists` to find a "LinkedIn Connections" list — if none, call `create_list`
3. Process page by page in a loop:
   a. Call `list_linkedin_connections(message_account_id=..., cursor=...)` to fetch one page
   b. Extract linkedin_url, first_name, last_name, job_title for each connection
   c. Call `add_people_to_list(list_id=..., people=[...])` with all people from this page
   d. Report progress after each page
   e. If cursor is returned, repeat — otherwise stop

Return: total pages, connections fetched, added to list, already in list, newly created.
```

---

## Enrichment

### Enrich Missing Emails in a List
```
Enrich all contacts in my '[List Name]' list that are missing an email address.

Use toflow MCP tools in this order:
1. Call `get_all_lists` to find the list — confirm with me if multiple match
2. Call `get_list_items` (list_id=...) to retrieve all contacts
3. Filter for contacts where email_addresses is empty or null
4. For each contact without an email, call `enrich_person_email` (person_id=...)
5. Call `get_person_enrichment_status` (task_id=...) to poll if enrichment is still in progress
6. After enrichment completes, call `get_person` to confirm the email was added

Return: total contacts, missing email, successfully enriched, not found.
```

---

### Bulk Enrich a Full List
```
Bulk enrich all contacts in my '[List Name]' list — find missing emails and phone numbers.

Use toflow MCP tools:
1. Call `get_all_lists` to locate the target list
2. Call `estimate_bulk_enrich_list` (list_id=...) first — show me the credit estimate and wait for confirmation
3. After I confirm, call `bulk_enrich_list` (list_id=...) to start enrichment
4. Call `get_bulk_enrichment_status` (job_id=...) to poll until complete
5. Once done, call `get_list_items` to show the enrichment results

Return before/after stats: total contacts, emails found, phones found, enrichment success rate.
```

---

### Enrich Phone Numbers for a List
```
Find and add phone numbers for all contacts in my '[List Name]' list that are missing one.

Use toflow MCP tools in this order:
1. Call `get_all_lists` to find the list — confirm with me if multiple match
2. Call `get_list_items` (list_id=...) to retrieve all contacts
3. Filter for contacts where phone_numbers is empty or null
4. If 5+ contacts are missing phones, ask if I want bulk enrichment:
   - Call `estimate_bulk_enrich_list` first — show credit estimate and wait for confirmation
   - After confirmation: call `bulk_enrich_phones` (list_id=...)
   - Poll with `get_bulk_enrichment_status` until complete
5. For individual enrichment: call `enrich_person_phone` (person_id=...) for each

Return: total contacts, missing phones before, successfully enriched, not found.
```

---

### Create an AI Enrichment Configuration
```
Help me create a new AI enrichment configuration in toflow.

Use toflow MCP tools in this order:
1. Call `list_ai_enrichments` to check if a similar enrichment already exists
2. Ask me: What to enrich? Which resource type (person, company, deal)? Which fields to write back?
3. Call `get_resource_schema(resource_type=...)` to discover all available attributes and their IDs
4. Propose a configuration: name, model (default gpt-4o-mini), base_prompt with {{resource.field}} variables, and field_mappings
5. Show me the full config summary and wait for explicit approval before proceeding
6. Only after I confirm: call `create_ai_enrichment(name, resource_type, model, field_mappings, base_prompt)`

Return: enrichment config ID, name, model, resource type, number of field mappings created.
```

---

### Run AI Enrichment on a List
```
Run an AI enrichment on my '[List Name]' list.

Use toflow MCP tools in this order:
1. Call `list_ai_enrichments` to show all configured AI enrichments — ask me which to run
2. Call `get_ai_enrichment(provider_id=...)` to show the full config — confirm this is the right enrichment
3. Call `get_all_lists` to find the target list
4. Call `get_list_items` to get a count of records
5. Show me a pre-run summary: enrichment name, list, record count, estimated wait time — confirm I want to proceed
6. Only after I confirm: call `run_ai_enrichment(provider_id=..., list_id=...)`

Return: enrichment name, list name, records queued, estimated completion time.
```

---

## Outreach

### Draft Personalized Outreach Email
```
Draft a personalized outreach email to: [LinkedIn URL, name, or person ID]

Use toflow MCP tools in this order:
1. Call `enrich_person_by_linkedin` (if LinkedIn URL) or `list_records` to find them
2. Call `get_person` — check ICP Score, Key Talking Points, LinkedIn Outreach Stage (do NOT cold outreach if in active sequence)
3. Call `get_company` to get website, funding, size for personalization
4. If no email: call `enrich_person_email` first
5. Call `list_notes` and `list_emails` to check prior relationship context
6. Call `inbox_manager_config` to load workspace email rules — follow agent_instructions strictly
7. Call `list_connected_accounts` to get available sender accounts — ask me which to use
8. Draft: personalized hook from research, their likely pain point, one proof point, one clear CTA. HTML format, no signature.
9. Show me the full draft and iterate until I approve
10. Only after approval: call `draft_email` (to_person_id, from_account_id, subject, html_body)

Also prepare a LinkedIn connection request variant (<300 chars, no pitch).
```

---

### Send LinkedIn Connection Requests to a List
```
Send LinkedIn connection requests to all people in my '[List Name]' list who are not yet connected.

Use toflow MCP tools in this order:
1. Call `get_all_lists` to find the list
2. Call `get_list_items` to retrieve all contacts
3. For each contact, call `check_linkedin_connection` to see if already connected — skip those who are
4. For unconnected contacts, call `get_person` to get name, title, company for personalization
5. Draft a personalized connection note for each (<300 chars, genuine, no pitch)
6. Show me a batch preview before sending — iterate if needed
7. After approval, call `send_connection_request` (person_id=..., message=...) for each
8. Call `update_record` to set LinkedIn Outreach Stage = 'Connection Request Sent'

Return: total processed, already connected (skipped), requests sent.
```

---

### Follow Up with Unresponsive Leads
```
Find all people who received an outreach email from me more than 7 days ago but haven't replied. Draft short follow-up emails for each.

Use toflow MCP tools:
1. Call `list_emails` (status='sent', sort_by='email_date', sort_order='desc', page_size=50)
2. For each sent email older than 7 days, call `list_emails` (thread_id=..., status='received') — filter for threads with zero received replies
3. Call `get_person` for each unresponsive contact
4. Call `inbox_manager_config` and `list_connected_accounts`
5. Draft a short follow-up for each: references original message, brings a new angle, ends with soft CTA
6. Show me all drafts in a batch for review — iterate until approved
7. After approval: call `reply_to_email` (email_id=original_email_id) to thread correctly

Return: unresponsive leads found, drafts created.
```

---

### Start WhatsApp Conversations with a List
```
Start personalised WhatsApp conversations with all contacts in my '[List Name]' list.

Use toflow MCP tools in this order:
1. Call `get_all_lists` to find the list
2. Call `get_list_items` to retrieve all contacts
3. For each contact, call `get_person` — check phone number and LinkedIn Outreach Stage (skip if in active conversation)
4. Call `inbox_manager_config` to load workspace messaging rules
5. Draft a personalised WhatsApp opening message for each: casual, short, warm — no cold pitch
6. Show me all drafts in a batch preview before sending — iterate until approved
7. After approval, call `start_whatsapp_conversation` (person_id=..., message=...) for each
8. Call `update_record` to set LinkedIn Outreach Stage = 'WhatsApp Conversation Started'

Return: total processed, skipped, messages sent.
```

---

### Find Active Prospects from a List Based on Recent LinkedIn Posts
```
Check which prospects in my '[List Name]' list have posted on LinkedIn recently and surface the best ones to engage with.

Use toflow MCP tools in this order:
1. Call `get_all_lists` and `get_list_items` to get all contacts
2. Call `list_message_accounts(provider_type="linkedin")` to get a connected account
3. For each person: call `get_person` to check LinkedIn URL and Outreach Stage — skip anyone in an active conversation
4. Call `get_linkedin_person_posts` to fetch their recent posts
5. Filter for posts in the last 30 days — score relevance: High / Medium / Low
6. For High/Medium prospects, draft contextual outreach referencing the specific post content
7. Show all drafts for review — do NOT send without approval
8. After approval: call `send_connection_request`, `start_linkedin_conversation`, or `send_linkedin_message` based on connection status

Return: total checked, no recent posts (skipped), Low relevance (skipped), High/Medium surfaced, drafts created.
```

---

### Reply to All Unread LinkedIn & WhatsApp Messages
```
Show me all my unread LinkedIn and WhatsApp messages and help me reply to each one.

Use toflow MCP tools in this order:
1. Call `list_message_threads(provider_type="linkedin", page_size=50)`
2. Call `list_message_threads(provider_type="whatsapp", page_size=50)`
3. For each thread with unread activity: call `get_message_thread` to read the full conversation — skip if last message was sent by me
4. Call `get_person` for each thread's linked person
5. Group threads by urgency: Hot (direct question / meeting request / buying signal) / Warm / Cold
6. Draft a contextual reply for each — show all drafts grouped by urgency for review
7. After approval: call `send_linkedin_message` or `send_whatsapp_message` for each

Return: total unread (LinkedIn / WhatsApp split), by urgency tier, replies sent.
```

---

### Send InMail to Prospects Who Ignored Your Connection Request
```
Send LinkedIn InMail to prospects in my '[List Name]' list whose connection requests were ignored.

Use toflow MCP tools in this order:
1. Call `list_message_accounts(provider_type="linkedin")` — confirm the account has a premium plan
2. Call `get_all_lists` and `get_list_items` to retrieve all contacts
3. For each contact, call `check_linkedin_connection` — only proceed for those with status "ignored"
4. For each ignored prospect: call `get_person` and `get_company` for personalization context
5. Draft InMail: short subject, personalized opening, single value proposition, one CTA — under 200 words
6. Show all drafts in a batch for review — iterate until approved
7. After approval: call `send_inmail(person_id=..., message_account_id=..., subject=..., message=..., premium_type=...)`
8. Call `update_record` to set LinkedIn Outreach Stage = 'InMail Sent'

Return: total in list, ignored (eligible), InMails sent, failed.
```

---

## Sequences

### Build a Multi-Channel Outreach Sequence
```
Help me build a multi-channel outreach sequence in toflow.

Use toflow MCP tools in this order:
1. Call `get_sequence_schema` to understand node types and template variables
2. Call `list_connected_accounts` to show available email, LinkedIn, and WhatsApp accounts
3. Ask me: timezone, send window, weekends yes/no, thread follow-ups, blackout dates
4. Ask me: target persona, goal, number of steps, which channels
5. Propose a sequence structure (step type + timing + content preview) — get my approval on structure before writing content
6. Draft all step content using template variables — HTML for emails, <300 chars for LinkedIn notes, no signatures
7. Show ALL content at once for review — iterate until I approve every step
8. After full approval: call `create_sequence` with complete nodes and edges arrays
9. Ask if I want to enroll anyone now — if yes, call `enroll_in_sequence`

Return: sequence name, step count, total duration, enrollment option.
```

---

### Enroll a List in a Sequence
```
Enroll everyone in my '[List Name]' list in a toflow sequence.

Use toflow MCP tools:
1. Call `list_sequences` to show available sequences — ask me which one to use
2. Call `get_all_lists` and `get_list_items` to get all contacts
3. For each contact, call `get_person` to check LinkedIn Outreach Stage — skip anyone already in an active sequence
4. Show me a summary of who will be enrolled vs. skipped before proceeding
5. After my confirmation: call `enroll_in_sequence` for each eligible contact
6. Call `list_enrollments` to confirm enrollment status

Return: total in list, skipped (already in sequence), successfully enrolled.
```

---

### Check Sequence Performance
```
Give me a performance report for my outreach sequences.

Use toflow MCP tools in this order:
1. Call `list_sequences` to get all sequences
2. For each active sequence:
   a. Call `get_sequence` to pull the full step structure
   b. Call `list_enrollments` to get all enrollments
3. Calculate: total enrolled, currently active, completed, failed, completion rate
4. Identify best and worst performing sequences by completion rate
5. For high-failure sequences, call `list_enrollments(status='failed')` to identify patterns

Output: performance table (sequence, enrolled, active, completed, completion rate, failed), top/worst performer, 2-3 recommendations.
```

---

### Deep Sequence Performance Analysis
```
Give me a deep performance analysis for my sequence '[Sequence Name or ID]' — break it down step by step.

Use toflow MCP tools in this order:
1. Call `list_sequences` to find the target sequence
2. Call `get_sequence` to pull the full step structure
3. Call `get_sequence_analytics` to pull full analytics — top-level KPIs and per-step breakdown
4. Analyse per-step data to identify the biggest bottleneck:
   - Email: low open rate (<20%) = subject issue; low reply rate (<5%) = body/CTA issue
   - LinkedIn connection: low accepted rate = profile or note quality issue
   - High skipped count = sequencing logic issue
5. Rank all steps from worst to best
6. For the bottom 2 steps, suggest a specific fix

Output: summary KPI table, funnel with % conversion at each stage, per-step performance table, top 3 issues ranked by impact, recommended fixes.
```

---

### Fix and Retry Failed Sequence Enrollments
```
Find all failed and invalid enrollments in my sequence '[Sequence Name]' and fix + retry them.

Use toflow MCP tools in this order:
1. Call `list_sequences` to find the target sequence
2. Call `list_enrollments(sequence_id=..., status="failed")` and `list_enrollments(status="invalid")`
3. For each failed/invalid enrollment: call `get_enrollment` to read exit_reason and current_node_id
4. Group by root cause: missing email / unresolved template variable / LinkedIn note too long / bounce / API error
5. For content fixes: call `update_enrollment` with corrected content — show me for approval first
6. After fixes confirmed: call `retry_enrollment` for each fixable enrollment
7. List unfixable cases with required manual action

Return: total found, breakdown by root cause, fixed + retried, unfixable (with reason).
```

---

## Pipeline

### Pipeline Health Review
```
Review my sales pipeline health and give me a prioritized action plan.

Use toflow MCP tools in this order:
1. Call `list_pipelines` — if multiple, ask me which to review
2. Call `list_stages` to build a stage map
3. Call `filter_guide` to confirm filter syntax
4. Call `list_records` (resource_type=deal, filters: status is_not won AND is_not lost, sort: Expected Close Date asc)
5. Flag each deal: Stale (updated_at < 14 days), Stuck (age > 30 days in early stage), Past close date, Single-threaded, Missing data
6. Prioritize: Focus Now / Keep Warm / Nurture

Output: Pipeline Health Score (0–100), top 3 priority actions, deal matrix by tier, risk flags, hygiene issues, recommended updates.
```

---

### Sales Forecast with Scenarios
```
Generate a sales forecast for [this month / Q2 / this quarter — specify period].

Use toflow MCP tools:
1. Ask me: what's my quota for this period?
2. Call `list_pipelines` and `list_stages` to get stage probability mappings
3. Call `list_records` for open deals closing in the period and already-won deals
4. Calculate three scenarios:
   - Commit: deals at 80%+ probability
   - Likely (weighted): sum(deal_value × stage_probability)
   - Best case: all open deals close
   - Worst case: only Commit deals close
5. Flag risk deals: past close date, no activity 14+ days, early stage but closing this week

Output: summary table (quota, closed, weighted forecast, gap), three scenarios, commit vs. upside deal lists, gap analysis.
```

---

### Prep for a Sales Call
```
Prep me for my call with [Company Name / Deal Name].

Use toflow MCP tools:
1. Call `list_records` (resource_type=deal, search=company_name) to find the active deal
2. Call `get_deal` to pull stage, value, probability, associated people and company
3. For each attendee: call `get_person` — check title, ICP Score, Key Talking Points
4. Call `get_company` — get website, size, funding, description
5. Call `list_notes` and `list_emails` for prior context
6. Call `list_message_threads` for LinkedIn/WhatsApp conversation history
7. Call `list_tasks` for outstanding action items
8. Use web search for recent company news (last 30–60 days)

Output: Deal Snapshot, attendee profiles, history & context, suggested agenda, 5 discovery questions, potential objections with responses.
```

---

### Re-engage Stale Deals
```
Find all my open deals with no activity in the last 14+ days and help me re-engage them.

Use toflow MCP tools in this order:
1. Call `filter_guide` to confirm filter syntax
2. Call `list_records` (resource_type=deal, filters: status is_not won AND is_not lost, sort: updated_at asc)
3. For each deal with updated_at older than 14 days: call `list_notes` and `get_deal`
4. Group stale deals by urgency: closing within 30 days (HIGH), 30-90 days (MEDIUM), 90+ days (LOW)
5. For HIGH and MEDIUM deals: draft a short re-engagement email referencing the last interaction
6. Show all drafts for review — call `draft_email` only after approval
7. For each stale deal: call `create_task` to ensure follow-through

Return: total stale deals, breakdown by urgency, drafts created.
```

---

## Skill Creator

### Create a Skill: Daily Prospect Scoring
```
Create a new toflow AI skill that automatically scores new prospects added to a list against our ICP criteria.

Produce a complete SKILL.md file with:
- Frontmatter: name, description (trigger phrases), compatibility
- How It Works: ASCII diagram showing tool flow
- Execution Flow: exact toflow tool calls per step
- Output Format: exact markdown template
- Notes: edge cases and guardrails

The skill must:
- Call `record_schema(resource_type=person)` before any create/update
- Call `filter_guide()` before using `list_records` with filters
- Use `list_records` with search before creating records to avoid duplicates
- Save scores back using `update_record` — always confirm with user first
- Handle `enrich_person_by_linkedin` timeouts via `get_person_enrichment_status`

Show me the complete SKILL.md for review before saving.
```

---

### Create a Skill: Post-Demo Follow-Up Automation
```
Create a new toflow AI skill that handles everything after a demo call — log the call, update the deal, create tasks, and draft the follow-up email.

Produce a complete SKILL.md file covering:
- Step 1: Find the Deal and Contact (list_records, get_deal, get_person)
- Step 2: Extract from call notes/transcript (decisions, action items, objections, buying signals, next step)
- Step 3: Log to toflow (create_note, update_record for deal stage, create_task for each action item — confirm with user first)
- Step 4: Draft Follow-Up Email (inbox_manager_config, list_connected_accounts, compose HTML body, show draft, iterate, draft_email after approval)

Output format: Internal Summary + Follow-Up Email draft.
Notes: always confirm deal updates before applying; always thread follow-up as reply to last email via reply_to_email.

Show me the complete SKILL.md for review before saving.
```

---

### Create a Skill: Competitive Battlecard Builder
```
Create a new toflow AI skill that builds a competitive battlecard by combining toflow win/loss data with web research.

Produce a complete SKILL.md file covering:
- Step 1: Pull toflow Win/Loss Data (list won/lost deals, list_notes for competitor mentions)
- Step 2: Web Research Per Competitor (features, pricing, recent news, G2 reviews, job postings)
- Step 3: Structure Per-Competitor Data (win rate, objections, where they win, where you win, pricing, talk tracks)
- Step 4: Generate HTML Battlecard (dark theme, tab navigation per competitor, expandable sections)

Output: Win/Loss Summary table, Field Intel from notes, HTML battlecard artifact.
Notes: refresh monthly / before major deals; never trash-talk competitors.

Show me the complete SKILL.md for review before saving.
```

---

### Create a Skill: LinkedIn Engagement Monitor
```
Create a new toflow AI skill that monitors recent LinkedIn posts from a list of prospects and drafts contextual outreach using post content as a personalization hook.

Produce a complete SKILL.md file covering:
- Step 1: Pull Prospects from List (get_all_lists, get_list_items, get_person — skip anyone in active conversation)
- Step 2: Fetch Recent LinkedIn Posts (get_linkedin_person_posts — filter for last 30 days)
- Step 3: Score Post Relevance (High / Medium / Low — only draft for High and Medium)
- Step 4: Draft Outreach (check_linkedin_connection → connection request or DM — genuine, post-specific, no pitch)
- Step 5: Send After Approval (send_connection_request or send_linkedin_message, update LinkedIn Outreach Stage)

Notes: always read the full post before drafting; never send to anyone mid-sequence.

Show me the complete SKILL.md for review before saving.
```

---

### Create a Skill: Inbound Lead Auto-Qualify
```
Create a new toflow AI skill that qualifies and routes inbound leads — score against ICP, enrich, and enroll in the right sequence based on score.

Produce a complete SKILL.md file covering:
- Step 1: Find or Create the Lead (filter_guide, list_records search, enrich_person_by_linkedin or create_record — no duplicates)
- Step 2: Enrich (get_person, get_company, enrich_person_email if missing)
- Step 3: Score Against ICP (5 dimensions, 100pts: Outbound Motion, Company Fit, Tool Stack Pain, Role Fit, Buying Signals)
- Tiers: 80-100 Hot / 60-79 Warm / 40-59 Lukewarm / <40 Weak — do not auto-enroll below 60
- Step 4: Save Score (update_record — confirm with user first)
- Step 5: Enroll in Right Sequence (list_sequences, confirm tier→sequence mapping, enroll_in_sequence after confirmation)

Show me the complete SKILL.md for review before saving.
```

---

### Create a Skill: Deal Health Weekly Alert
```
Create a new toflow AI skill that runs a weekly pipeline health check, flags at-risk deals, and creates action tasks.

Produce a complete SKILL.md file covering:
- Step 1: Pull All Open Deals (filter_guide, list_pipelines, list_stages, list_records for all open deals)
- Step 2: Flag by Risk Type (Stale 14+ days, Past Close Date, Stuck in Stage 30+ days, Single-Threaded, Missing Data)
- Step 3: Prioritize (CRITICAL / HIGH / MEDIUM / LOW by combined risk score)
- Step 4: Create Action Tasks (show proposed tasks to user before creating — confirm before bulk create)
- Step 5: Suggest Deal Updates (for past-close-date and stale Very Hot deals — always confirm before update_record)

Output: weekly health report table with risk breakdown, tasks created, and suggested deal updates.
Notes: always confirm before creating tasks; never mark deals as lost without explicit instruction; run Mondays.

Show me the complete SKILL.md for review before saving.
```
