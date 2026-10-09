# toflow MCP Server

[![MCP Badge](https://lobehub.com/badge/mcp/toflow-ai-toflow-mcp)](https://lobehub.com/mcp/toflow-ai-toflow-mcp)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

[toflow.ai](https://toflow.ai) is an AI-native multi-channel outreach platform for sales teams. This MCP server gives AI assistants direct access to your toflow.ai workspace — prospecting, multi-channel outreach (email, LinkedIn, WhatsApp), sequences, enrichment, CRM, and AI automations.

## Server URL

```
https://mcp.toflow.ai/mcp
```

## Authentication

OAuth 2.0 (Authorization Code). Your MCP client will redirect to the toflow.ai login page on first connect. Tokens are valid for 90 days.

| Endpoint | URL |
|---|---|
| Authorization | `https://mcp.toflow.ai/oauth/authorize` |
| Token | `https://mcp.toflow.ai/oauth/token` |
| Dynamic Registration | `https://mcp.toflow.ai/oauth/register` |
| Discovery | `https://mcp.toflow.ai/.well-known/oauth-authorization-server` |

## Connect

### Claude Desktop / Cursor / Windsurf

```json
{
  "mcpServers": {
    "toflow": {
      "type": "http",
      "url": "https://mcp.toflow.ai/mcp"
    }
  }
}
```

### Grok Build

```bash
grok plugin install toflow-ai/toflow-mcp --trust
```

Installing as a plugin also adds the skills below, which teach Grok how to chain toflow's tools into full workflows (prospecting, sequencing, enrichment) instead of calling them one at a time.

## Skills

| Skill | Covers |
|---|---|
| `toflow-get-started` | Confirm workspace and connected sending accounts |
| `toflow-prospect-and-list` | LinkedIn search, signal agents, lists and views |
| `toflow-enrich-contacts` | Verified emails/phones, and custom AI enrichment |
| `toflow-create-sequence` | Build multi-channel sequences and enroll people |
| `toflow-manage-email` | Read, draft, send, and reply to email |
| `toflow-manage-conversations` | LinkedIn connection requests/DMs/InMail, WhatsApp |
| `toflow-manage-crm` | People, companies, deals, attributes, pipelines |
| `toflow-manage-activities` | Notes, tasks, call logs |
| `toflow-build-report` | Query CRM data into reports and dashboards |

## What you can do

- **Prospecting** — search for prospects using Sales Navigator-style filters; find prospects from your connections, post comments, and reactions
- **Sequences** — build multi-step outreach sequences across email, LinkedIn, and WhatsApp; enroll contacts with personalised content; track open and reply rates
- **Email** — draft, send, reply, forward, and track emails from connected accounts
- **LinkedIn & WhatsApp** — message connections, send connection requests, and start conversations across channels
- **Enrichment** — find verified emails and phone numbers from LinkedIn profiles; bulk enrich entire lists
- **Lists** — organise prospects into lists with saved filtered views
- **AI Automations** — create and run AI agents that operate on your workspace autonomously
- **Tasks, Notes & Calls** — log calls, create follow-up tasks, and attach notes to any record
- **Dashboards** — build custom reports and dashboards from your CRM data
- **CRM** — create, search, and update people, companies, and deals

## Tool Reference

See [docs/mcp-tools.md](docs/mcp-tools.md) for the full list of available tools.

## Requirements

- A toflow.ai account — [sign up free](https://app.toflow.ai/signup?utm_source=github&utm_medium=mcp&utm_campaign=toflow-mcp)

## Support

support@toflow.ai · [toflow.ai](https://toflow.ai)
