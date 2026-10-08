---
name: toflow-manage-email
description: Read, draft, send, reply to, and forward emails from connected toflow.ai sending accounts, and check open-tracking and engagement stats. Use this for one-off emails outside of a sequence, or to manage an existing email thread.
license: MIT
---
# toflow — Manage Email

Handles one-off, ad-hoc email actions. For emails that are part of a multi-step outreach flow, use the `toflow-create-sequence` skill instead.

## Reading

- `list_emails` — find emails by person, account, status, date range, or search query.
- `get_email` — read a single email's full content, recipients, send status, open tracking, and thread info.
- `list_message_threads` / `get_message_thread` cover LinkedIn/WhatsApp threads, not email — see `toflow-manage-conversations` for those.

## Composing and sending

1. Confirm the sending account with `list_connected_accounts` if more than one email account is connected.
2. Call `draft_email` to create the draft. Do not include a signature — it's appended server-side automatically.
3. Show the drafted content to the user and iterate until approved.
4. Call `send_email` only after explicit approval. This cannot be undone.
5. For replies in an existing thread, use `reply_to_email` (pass the thread ID) rather than starting a new email — keeps the conversation in one thread.
6. Use `forward_email` to forward to new recipients, with an optional message prefixed.
7. Use `update_draft` to edit a draft before sending (only the fields provided are changed), and `delete_draft` to remove one that's no longer needed (cannot be undone).

## Signature and tracking

- `set_email_signature` sets the signature appended server-side for a connected account — don't manually add a sign-off to drafted content, since it would end up duplicated.
- `get_email_tracking` — open stats for a single sent email.
- `list_email_events` — workspace-level aggregate open/click rates across all outreach.

## Guardrails

- Always show composed content for approval before `send_email` — never send without explicit confirmation.
- Never include a signature/sign-off in `draft_email` content.
- No em dashes in generated content.
