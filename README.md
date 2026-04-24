# toflow MCP Server

[toflow](https://toflow.ai) is an AI-native multi-channel outreach platform for sales teams. This MCP server gives AI assistants direct access to your ToFlow workspace — CRM, email, sequences, enrichment, analytics, and AI automations.

## Server URL

```
https://mcp.toflow.ai/mcp
```

## Authentication

OAuth 2.0 (Authorization Code). Your MCP client will redirect to the ToFlow login page on first connect. Tokens are valid for 90 days.

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

- **CRM** — create, search, update people, companies, and deals
- **Email** — draft, send, reply, forward, and track emails from connected accounts
- **Sequences** — build and manage multi-step outreach sequences, enroll contacts, view analytics
- **Enrichment** — find verified emails and phone numbers from LinkedIn profiles; bulk enrich lists
- **Lists** — organise contacts into lists with saved filtered views
- **Tasks & Notes** — log calls, create tasks, and attach notes to CRM records
- **Dashboards** — build reports and dashboards from CRM data
- **AI Automations** — create and run AI agents that operate on your workspace autonomously

## Tool Reference

See [docs/mcp-tools.md](docs/mcp-tools.md) for the full list of available tools.

## Requirements

- A ToFlow account — [sign up at toflow.ai](https://toflow.ai)

## Support

support@toflow.ai · [toflow.ai](https://toflow.ai)
