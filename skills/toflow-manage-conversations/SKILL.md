---
name: toflow-manage-conversations
description: Read and manage LinkedIn and WhatsApp conversation threads, connected accounts, and the default sending account for those channels. Use this for one-off LinkedIn/WhatsApp messages or replies outside of a sequence.
license: MIT
---
# toflow — Manage LinkedIn & WhatsApp Conversations

Handles one-off conversation actions on LinkedIn and WhatsApp. For messages that are part of a multi-step outreach flow, use the `toflow-create-sequence` skill instead.

## Steps

1. Call `list_message_accounts` to see which LinkedIn and WhatsApp accounts are connected before sending anything.
2. Call `list_message_threads` to find existing conversations with a contact, or to review recent activity across channels.
3. Call `get_message_thread` to read the full history of a specific thread before replying — context matters more on these channels than email, since messages are short and informal.
4. When composing a reply, match the tone of the existing thread. Show the draft to the user for approval before sending.
5. If the user has multiple connected accounts for a channel, use `set_primary_account` to set the default, or ask which one to use per message if it varies by contact.

## Guardrails

- Always show drafted LinkedIn/WhatsApp content for approval before sending — same as email.
- No em dashes, no superlatives ("best-in-class", "game-changing"), no pressure language ("act now") in generated content — these read as AI-written and hurt reply rates on informal channels even more than on email.
- Open with a specific observation (their post, role change, company milestone), not a pitch; ask one easy question rather than a meeting ask in the first message.
- Don't over-message a thread — check `get_message_thread` for recent activity before sending another follow-up.
