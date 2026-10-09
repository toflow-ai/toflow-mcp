---
name: toflow-manage-email
description: Read, draft, send, reply to, and forward emails from connected toflow.ai sending accounts, and check open-tracking and engagement stats. Use this for one-off emails outside of a sequence, or to manage an existing email thread or draft.
license: MIT
---
# toflow — Manage Email

Handles one-off, ad-hoc email actions. For emails that are part of a multi-step outreach flow, use the `toflow-create-sequence` skill instead.

## Reading

- `list_emails` — find emails by person, account, status, date range, or search query.
- `get_email` — read a single email's full content, recipients, send status, open tracking, and thread info.

## Composing and sending

1. Call `get_draft_email_guide` before `draft_email`. Always call `list_connected_accounts` first (even with one account) and have the user explicitly choose — don't default to one. Before composing anything, check that account's signature is set up (`has_signature`); if not, stop and have the user set one (Settings > Email Accounts, or `set_email_signature`) before drafting.
2. Resolve the recipient: if given a name, search for the person (`list_records`) and confirm if there are multiple matches. If the person has no email on file but has a `linkedin_url`, enrich it automatically (`enrich_person_email`) rather than asking the user for it.
3. `draft_email` takes `html_body`, not plain text — compose it as HTML using `<p>` tags with inline styles (e.g. `<p style="margin: 0; margin-bottom: 12px; font-size: 14px;">Hi John,</p>`). Do not include a sign-off/signature — appended server-side automatically.
4. Show the full draft (from, to, subject, body) to the user and iterate until approved.
5. Call `draft_email` only after explicit approval, then share the returned draft URL and ask if they want it sent now.
6. Call `send_email` only after explicit approval. This cannot be undone.
7. For replies in an existing thread, use `reply_to_email` (pass the thread ID) rather than starting a new email — keeps the conversation in one thread.
8. Use `forward_email` to forward to new recipients, with an optional message prefixed.
9. Use `update_draft` to edit a draft before sending (only the fields provided are changed), and `delete_draft` to remove one that's no longer needed (cannot be undone). For drafted LinkedIn/WhatsApp messages, see `toflow-manage-conversations`.

## Signature and tracking

- `set_email_signature` sets the signature appended server-side for a connected account — don't manually add a sign-off to drafted content, since it would end up duplicated.
- `get_email_tracking` — open stats for a single sent email.
- `list_email_events` — workspace-level aggregate open/click rates across all outreach.

## Guardrails

- Always show composed content for approval before `send_email` — never send without explicit confirmation.
- Never include a signature/sign-off in `draft_email` content.
- No em dashes in generated content.
