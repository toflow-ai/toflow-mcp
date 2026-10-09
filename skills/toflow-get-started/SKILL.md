---
name: toflow-get-started
description: Confirm the active toflow.ai workspace, connected sending accounts, and account load before doing any prospecting or outreach work. Use this at the start of a new session, or when a tool call fails because no account is connected.
license: MIT
---
# toflow — Get Started

Run this before any multi-step workflow (prospecting, sequences, enrichment) if you haven't already confirmed the workspace this session is operating in.

## Steps

1. Call `get_workspace` to confirm which workspace is active and who is logged in. Tell the user which workspace you're operating in before taking any action on their data.
2. Call `list_workspace_members` if you need to assign a task or attribute an action to a specific teammate.
3. Call `list_connected_accounts` to see which email, LinkedIn, and WhatsApp accounts are connected. Outreach tools (`send_email`, `send_linkedin_message`, `send_whatsapp_message`, sequence enrollment) all require at least one connected account for that channel.
   - If no account is connected for the channel the user wants to use, tell them directly — don't attempt the action and let it fail.
4. If multiple accounts are connected for a channel, call `get_account_load_stats` to check daily send limits and current utilization, and ask the user which account to use rather than picking one silently.
5. Call `list_message_accounts` and `set_primary_account` only if the user wants to change their default sending account for LinkedIn/WhatsApp.

## Guardrails

- Don't guess which workspace or account to act on — confirm explicitly if more than one option exists.
- Don't proceed into a prospecting, enrichment, or sequence workflow if the required connected account is missing; tell the user what to connect first.
