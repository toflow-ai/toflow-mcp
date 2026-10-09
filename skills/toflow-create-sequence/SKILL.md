---
name: toflow-create-sequence
description: Build a multi-channel outreach sequence in toflow.ai (email, LinkedIn, WhatsApp) and enroll people into it. Use this whenever the user asks to create, set up, or launch a sequence, campaign, or drip/outreach flow, or to enroll a person/list into one.
license: MIT
---
# toflow — Create and Enroll a Sequence

This skill orchestrates toflow's sequence tools into one workflow: build the sequence, get it approved, then enroll people with correctly personalized (or AI-generated) content. The guide tools below are the live source of truth for schema, required fields, and node/edge structure — always call them before the matching mutating call, even if this skill summarizes the shape. This skill's own guidance covers what the guides don't own: message content quality and writing style, and the order to call things in.

## Part 1 — Building the sequence

1. Call `get_sequence_creation_guide` (and `get_sequence_schema` for the full field/node-type reference) before calling `create_sequence`. These cover sequence-level scheduling config (timezone, send window, weekends, holidays), sending accounts, node types, field requirements, edge shape, and template variables.
2. Call `list_sequences` and `list_sequence_templates` first — check whether an existing sequence or template already covers this campaign before building from scratch.
3. Never skip straight to `create_sequence`. Show the full plan (steps, channels, timing) and wait for explicit confirmation before creating it — treat a request like "make me a sequence for X" as the start of planning, not permission to create immediately.
4. See `references/writing-and-scheduling-rules.md` for message-writing quality (subject/body rules, tone, avoiding AI writing tells) and suggested step-to-step timing gaps — these are proposable defaults to confirm with the user, not enforced by the schema, so don't apply them silently.
5. Call `list_connected_accounts` to confirm which sending accounts are available for the channels in the sequence before finalizing it.

## Part 2 — Enrolling people

1. Once a sequence exists (new, or found via `get_sequence`), confirm the person has the contact data the sequence's channels require (verified email for email steps, phone for WhatsApp) — use `toflow-enrich-contacts` first if not.
2. Call `get_enroll_in_sequence_guide` before `enroll_in_sequence` — it governs identifying `ai_prompt` vs manual nodes, the personalize-vs-as-is decision (always ask, never assume), per-channel contact-data checks, and the review-before-send gate.
3. Never generate content for a node that has `ai_prompt` set — that content is generated server-side at enrollment time (`set_enrollment_node_content` is for manual-node content you draft and the user approves, not for `ai_prompt` nodes).
4. If enrolling more than one person (a list), repeat per person — content must be fresh per person, not copy-pasted.
5. After enrollment, check status with `list_enrollments` / `get_enrollment` and handle by status: `invalid` → call `resolve_invalid_enrollments` and follow its returned guidance. `skipped` → fix the person's missing contact info, then `retry_enrollment`. Call `get_update_enrollment_guide` before `update_enrollment` when pausing/resuming or otherwise changing an enrollment's state.
6. Use `get_sequence_analytics` to review performance (sent, delivered, opened, clicked, replied) when the user wants to evaluate how a sequence is doing, and `update_sequence` to adjust an existing sequence's steps or scheduling (only the fields provided are changed).

## Guardrails that apply to both parts

- No em dashes or hyphen-as-dash in any generated message content.
- Write like a person who knows this prospect, not an AI filling in a template — avoid AI writing tells (banned phrases, three-sentence paragraph formula, stacked CTAs, uniform sentence length). Full list in `references/writing-and-scheduling-rules.md`. Re-read every draft before showing it to the user and rewrite if it reads templated.
- Never invent a `{{template variable}}` value — resolve it from real person/company data or ask the user.
- Never add a sign-off/signature to email content — it's appended server-side automatically.
- Confirm node types and per-channel send-limit tradeoffs (e.g. connection requests with vs. without a note) against `get_sequence_schema` rather than assuming — these details change as the product evolves.
