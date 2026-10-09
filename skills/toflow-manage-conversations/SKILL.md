---
name: toflow-manage-conversations
description: Send LinkedIn connection requests, direct messages, and premium/InMail-style messages, send WhatsApp messages, and manage connected messaging accounts and threads. Use this for one-off LinkedIn/WhatsApp outreach outside of a sequence.
license: MIT
---
# toflow — Manage LinkedIn & WhatsApp Conversations

Handles one-off conversation actions on LinkedIn and WhatsApp. For messages that are part of a multi-step outreach flow, use the `toflow-create-sequence` skill instead.

## Steps

1. Call `list_message_accounts` to see which LinkedIn and WhatsApp accounts are connected before sending anything.
2. Call `list_message_threads` to find existing conversations with a contact, or to review recent activity across channels.
3. Call `get_message_thread` to read the full history of a specific thread before replying — context matters more on these channels than email, since messages are short and informal.
4. Check connection status (via the prospecting tools in `toflow-prospect-and-list`) before choosing how to reach someone:
   - Already connected → `send_linkedin_message`.
   - Not connected → `send_connection_request`, optionally with a note (ask the user first — a request without a note generally allows a higher daily send volume than one with a note attached).
   - Premium/InMail-style reach to someone not in-network and not accepting a connection request → `send_inmail`. Don't use this on an existing connection; use `send_linkedin_message` instead.
   - WhatsApp → `send_whatsapp_message`.
5. `send_draft_message` / `delete_message_draft` operate on a previously generated message draft (for example, AI-generated enrollment content awaiting review — see `toflow-create-sequence`). Always show the draft's actual content to the user before calling `send_draft_message`; never assume what a draft contains.
6. If the user has multiple connected accounts for a channel, use `set_primary_account` to set the default, or ask which one to use per message if it varies by contact.

## Guardrails

- Always show drafted LinkedIn/WhatsApp content for approval before sending — same as email.
- No em dashes, no superlatives ("best-in-class", "game-changing"), no pressure language ("act now") in generated content — these read as AI-written and hurt reply rates on informal channels even more than on email.
- Open with a specific observation (their post, role change, company milestone), not a pitch; ask one easy question rather than a meeting ask in the first message.
- Don't over-message a thread — check `get_message_thread` for recent activity before sending another follow-up.
- Don't send InMail to someone already a 1st-degree connection, and don't send a second connection request to someone already connected or already pending — check status first.
