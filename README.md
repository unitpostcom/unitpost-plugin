# Unitpost plugin

Your AI Agent sends the messages. The official Unitpost plugin gives your
agent email and SMS infrastructure: send transactional email and SMS, run
campaigns, and manage contacts, templates, sending domains, and webhooks in
your Unitpost workspace. It bundles Unitpost's hosted MCP server and the
Unitpost skill, which teaches the agent the API and its send-safety rules.

## Install

| Client | How |
| --- | --- |
| Claude Code | `/plugin marketplace add unitpostcom/unitpost-plugin`, then `/plugin install unitpost@unitpost` |
| Cursor (and Grok Bot) | Cursor Marketplace → Unitpost |
| VS Code (GitHub Copilot) | Add the server to `.vscode/mcp.json` (below) |
| Any MCP client | Add `https://mcp.unitpost.com/mcp` as a remote (Streamable HTTP) server |

Per-client guides: https://www.unitpost.com/docs/ai

## Configure

Unitpost is a hosted, remote MCP server. There is nothing to clone, build, or
run, and no API key to paste. In VS Code, add it to `.vscode/mcp.json` (or your
user MCP settings):

```json
{
  "servers": {
    "unitpost": {
      "type": "http",
      "url": "https://mcp.unitpost.com/mcp"
    }
  }
}
```

Start **unitpost** from the MCP view. A browser window opens Unitpost: sign
in, pick a workspace, and approve the scopes. Copilot then lists the Unitpost
tools in agent mode. Full guide: https://www.unitpost.com/docs/ai/copilot

## Use

The workspace tools follow the Unitpost REST API, one per operation, named
`resource_action`: `email_send`, `sms_send`, `email_campaigns_create`,
`contacts_list`, `email_domains_verify`, `webhooks_create`, and so on. They
enforce the same scopes, sending gate, suppression list, and billing as the
API. Three read-only helpers search the docs, the email design library, and
integration guides. Ask in plain language, for example:

- "Send a test of the welcome template to me@example.com."
- "Which of my sending domains aren't verified yet, and what DNS records are missing?"
- "Create a campaign to the Customers segment from the October newsletter template, and schedule it for Tuesday 9:00."
- "Show this week's email stats: delivered, opened, bounced."

Your client asks before any send tool runs.

## Contents

- `.mcp.json` / `mcp.json` — the remote MCP server `https://mcp.unitpost.com/mcp`
  (`.mcp.json` for Claude Code, `mcp.json` in the Agent Plugins format)
- `skills/unitpost/SKILL.md` — when to use Unitpost and the send-safety rules
- `plugin.json` (Agent Plugins), `.claude-plugin/`, `.cursor-plugin/`, `gemini-extension.json` — manifests
- `assets/` — logo

## Auth and safety

OAuth 2.1 in the browser: on first use your client opens Unitpost, you pick a
workspace and approve the scopes. No credentials are stored in this plugin.
Send tools are marked destructive, so your client asks before anything goes
out. Email sends need a verified domain; SMS is on paid plans and enforces
recipient consent and STOP opt-out.

## Network endpoints called

- `https://mcp.unitpost.com/mcp` (MCP)
- `https://www.unitpost.com/oauth/*` (OAuth authorize/token)
- `https://www.unitpost.com/.well-known/oauth-*` (discovery)

## Links

- Docs: https://www.unitpost.com/ai/mcp
- Privacy: https://www.unitpost.com/privacy
- Terms: https://www.unitpost.com/terms
- Support: support@unitpost.com

## Where this comes from

This repository is generated. Unitpost's CI publishes it from our main
repository on every release, with a freshly built `skills/unitpost/SKILL.md`
so the skill matches the live API. Each sync is a normal commit on top of
the previous one, so any commit SHA pinned by a marketplace stays reachable.
Pull requests here are overwritten by the next sync — email
support@unitpost.com instead.
