# Devin plugin

The Devin MCP server (`https://mcp.devin.ai/mcp`): private repository wikis, wiki
generation and the organization-wide `devin_*` tools.

The public DeepWiki endpoint (`mcp.deepwiki.com/mcp`) is a separate plugin
(`deepwiki`) — it needs no authentication and rejects any `Authorization` header,
so the two must not be mixed in one server entry.

## Configuration

```json
{
  "mcpServers": {
    "devin": {
      "type": "http",
      "url": "https://mcp.devin.ai/mcp",
      "headers": {
        "Authorization": "Bearer ${user_config.api_key}",
        "X-Org-Id": "${user_config.org_id}"
      },
      "timeoutMs": 120000
    }
  }
}
```

## Authentication

**API key only.** The endpoint advertises an MCP OAuth 2.1 challenge
(`scope: devin`, authorization code + PKCE, login at `app.devin.ai`), but its
authorization server `https://api.devin.ai` publishes **no dynamic client
registration endpoint** (`registration_endpoint` absent, `POST /oauth/register`
returns `404 "No such endpoint"`). ZCode's callback URI also uses a random local
port, so a statically pre-registered client cannot be used either — no browser
login can complete against this server.

The supported path is the Devin API key, sent as `Authorization: Bearer <key>`
(create one at https://app.devin.ai/settings/api-keys). The key is stored as a
sensitive plugin setting and never enters the repository.

Note that the server only rejects an invalid key when a tool is called, so a
successful connection does not prove the key is valid.

## Settings (Plugin Management → devin)

| Setting | Header | Purpose |
|---|---|---|
| `api_key` | `Authorization: Bearer …` | Required. Authenticates the session. |
| `org_id` | `X-Org-Id` | Selects the Devin organization the organization-scoped tools act in. Without it the server resolves the first organization the key belongs to; one request targets one organization. |

## Files

- `.zcode-plugin/plugin.json` — manifest and user settings
- `.mcp.json` — MCP server definition
- `skills/devin/SKILL.md` — tool workflow, authentication and organization notes
