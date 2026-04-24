# toflow MCP Server

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
