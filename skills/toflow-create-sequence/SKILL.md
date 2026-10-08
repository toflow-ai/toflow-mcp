---
name: toflow-create-sequence
description: Build a multi-channel outreach sequence in toflow.ai (email, LinkedIn, WhatsApp) and enroll people into it. Use this whenever the user asks to create, set up, or launch a sequence, campaign, or drip/outreach flow, or to enroll a person/list into one.
license: MIT
---
# toflow — Create and Enroll a Sequence

This skill orchestrates toflow's sequence tools into one workflow: build the sequence, get it approved, then enroll people with correctly personalized (or AI-generated) content. For structure and configuration that the tools already validate — sequence-level scheduling config (timezone, send window, weekends, holidays), sending accounts, node types, field requirements, edge shape — always defer to `get_sequence_schema`; it's the live source of truth and will stay correct as the product changes. This skill's own guidance covers what the schema doesn't own: message content quality and writing style, the *value* to propose for per-step `wait_value`/`wait_unit` gaps (the schema only validates that it's a number >=1, it gives no cadence judgment), suggested sequence structures by channel scope, and the order to call things in.

## Part 1 — Building the sequence

1. Call `get_sequence_schema` and follow it exactly before calling `create_sequence`. It's the single source of truth for sequence-level scheduling config, accounts, template variables, node/edge structure, and branching — don't rely on this skill alone for any of that.
2. Call `list_sequences` first if the user might already have a sequence covering this campaign — don't create a duplicate.
3. Never skip straight to `create_sequence`. Show the full plan (steps, channels, timing) and wait for explicit confirmation before creating it — treat a request like "make me a sequence for X" as the start of planning, not permission to create immediately.
4. See `references/writing-and-scheduling-rules.md` for message-writing quality (subject/body rules, tone, avoiding AI writing tells), suggested step-to-step timing gaps (`wait_value`/`wait_unit`) by channel and condition, and suggested sequence structures by channel scope (LinkedIn-only, email-only, multi-channel) — all of these are proposable defaults to confirm with the user, not enforced by the tool schema, so don't apply them silently.
5. Call `list_connected_accounts` to confirm which sending accounts are available for the channels in the sequence before finalizing it.

## Part 2 — Enrolling people (the messaging step)

1. Once a sequence exists (new or pre-existing, found via `get_sequence`), confirm the person has the contact data the sequence's channels require (verified email for email steps, phone for WhatsApp) — use the `toflow-enrich-contacts` skill first if not.
2. Decide personalize-vs-as-is with the user explicitly — never assume. Never generate content for a node that has `ai_prompt` set — that content is generated server-side at enrollment time. Only draft content for manual nodes, and show it for approval before calling `enroll_in_sequence`.
3. If enrolling more than one person (a list), repeat per person — content must be fresh per person, not copy-pasted.
4. After enrollment, check status with `list_enrollments` / `get_enrollment` and handle by status: `invalid` → fix the underlying data issue, then `retry_enrollment`. `skipped` → fix the person's missing contact info, then `retry_enrollment`. Use `update_enrollment` to pause/resume an enrollment.
5. Use `get_sequence_analytics` to review performance (sent, delivered, opened, clicked, replied) when the user wants to evaluate how a sequence is doing.

## Guardrails that apply to both parts

- No em dashes or hyphen-as-dash in any generated message content.
- Write like a person who knows this prospect, not an AI filling in a template — avoid AI writing tells (banned phrases, three-sentence paragraph formula, stacked CTAs, uniform sentence length). Full list in `references/writing-and-scheduling-rules.md`. Re-read every draft before showing it to the user and rewrite if it reads templated.
- Never invent a `{{template variable}}` value — resolve it from real person/company data or ask the user.
- Never add a sign-off/signature to email content — it's appended server-side automatically.
- Avoid including a message on `send_linkedin_connection` — it drops daily send limits from 20-25/day to 8-10/day.
