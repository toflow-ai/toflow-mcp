---
name: toflow-create-sequence
description: Build a multi-channel outreach sequence in toflow.ai (email, LinkedIn, WhatsApp) and enroll people into it. Use this whenever the user asks to create, set up, or launch a sequence, campaign, or drip/outreach flow, or to enroll a person/list into one.
license: MIT
---
# toflow — Create and Enroll a Sequence

This skill orchestrates toflow's sequence tools into one workflow: build the sequence, get it approved, then enroll people with correctly personalized (or AI-generated) content. The guide tools below are the live source of truth for schema, required fields, and node/edge structure — always call them before the matching mutating call, even if this skill summarizes the shape. This skill's own guidance covers what the guides don't own: message content quality and writing style, and the order to call things in.

## Part 1 — Building the sequence

1. Call `get_sequence_creation_guide` (and `get_sequence_schema` for the full field/node-type reference) before calling `create_sequence`. These cover sequence-level scheduling config (timezone, send window, weekends, holidays), sending accounts, node types, field requirements, edge shape, and template variables. Node types include `email`, `send_linkedin_connection`, `is_in_linkedin_network`, `linkedin_message`, `linkedin_inmail`, `whatsapp_message`, and `view_linkedin_profile` — confirm the current full list via `get_sequence_schema` since it may expand. `send_linkedin_connection` and `is_in_linkedin_network` are the only branching node types (`true`/`false` edges, never a merge point); `is_in_linkedin_network`'s true branch can't contain another conditional, and its false branch allows at most one `send_linkedin_connection`.
2. Before drafting account IDs into any node: call `list_connected_accounts` and `get_account_load_stats`, present accounts by label with their load stats, and let the user choose. Email nodes take a `from_account_ids` list (even for one account — never the singular `from_account_id`); LinkedIn/WhatsApp nodes take a single `from_account_id`. For every selected email account, confirm it has a signature configured before drafting any content — if not, stop and have the user set one via `set_email_signature` or in Settings first.
3. Call `list_sequences` and `list_sequence_templates` first — check whether an existing sequence or template already covers this campaign before building from scratch.
4. Never skip straight to `create_sequence`. Show the full plan (steps, channels, timing) and wait for explicit confirmation before creating it — treat a request like "make me a sequence for X" as the start of planning, not permission to create immediately.
5. See `references/writing-and-scheduling-rules.md` for message-writing quality (subject/body rules, tone, avoiding AI writing tells) and suggested step-to-step timing gaps — these are proposable defaults to confirm with the user, not enforced by the schema, so don't apply them silently.
6. If `thread_emails` is enabled, only the first email node gets an original subject — every later email node's subject must be `Re: <first email's subject>` since they land as replies in one thread.

## Part 2 — Enrolling people

1. Once a sequence exists (new, or found via `get_sequence`), call `get_enroll_in_sequence_guide` before `enroll_in_sequence`. It governs: inspecting the sequence's nodes to tell `ai_prompt` nodes (never generate content for these — omit them entirely) from manual nodes; the personalize-vs-as-is decision (always ask explicitly — as-is/`skip_personalization=True` is the default if the user doesn't say); per-channel contact-data checks (email nodes need an email, `whatsapp_message` needs a phone, `linkedin_*` nodes need a `linkedin_url`, `linkedin_inmail` also needs a premium account with InMail credits); and the review-before-send gate.
2. Content you draft for manual nodes must have zero unresolved `{{template variables}}` left in it — every variable must be resolved to the person's real value or you stop and ask.
3. If enrolling more than one person (a list), repeat per person — content must be fresh per person, not copy-pasted.
4. After enrollment, check status with `list_enrollments` / `get_enrollment`. Enrollment status values are `active`, `queued`, `paused`, `invalid`, `verifying` (email being verified asynchronously, no action needed), `skipped` (email verification failed or missing — fix the email then `retry_enrollment`, or cancel), and `cancelled`. For `invalid`, inform the user of the `exit_reason`, then fix the issue and `retry_enrollment`, or cancel via `update_enrollment(status='cancelled')`.
5. Call `get_update_enrollment_guide` before any `update_enrollment` call — it covers both status transitions (`active`/`paused`/`queued`/`invalid` → their allowed next states) and fixing node content for pending nodes. When changing a sending account, all nodes on that same channel must move to the new account together, or the consistency check rejects the call.
6. Use `get_sequence_analytics` to review performance (sent, delivered, opened, clicked, replied) when the user wants to evaluate how a sequence is doing, and `update_sequence` to adjust an existing sequence's steps or scheduling (only the fields provided are changed).

## Guardrails that apply to both parts

- No em dashes or hyphen-as-dash in any generated message content.
- Write like a person who knows this prospect, not an AI filling in a template — avoid AI writing tells (banned phrases, three-sentence paragraph formula, stacked CTAs, uniform sentence length). Full list in `references/writing-and-scheduling-rules.md`. Re-read every draft before showing it to the user and rewrite if it reads templated.
- Never invent a `{{template variable}}` value — resolve it from real person/company data or ask the user.
- Never add a sign-off/signature to email content — it's appended server-side automatically.
- `send_linkedin_connection` without a note allows roughly 20-25 requests/day vs. 8-10/day with one attached — omit the message unless the user explicitly asks for one.
- `linkedin_inmail` requires a premium account (Sales Navigator/Recruiter/Premium Business) and a non-empty subject, reads best under ~400 characters, and should only be sent Monday-Thursday. Never use it on an existing 1st-degree connection — use `linkedin_message` instead.
