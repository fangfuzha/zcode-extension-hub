# DeepWiki plugin

DeepWiki MCP server for ZCode. It gives the agent AI-generated documentation and
Q&A about any indexed GitHub repository.

## Configuration

The plugin declares one HTTP MCP server in `.mcp.json`:

```json
{
  "mcpServers": {
    "deepwiki": {
      "type": "http",
      "url": "https://mcp.deepwiki.com/mcp",
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

| Setting | Header | Purpose |
|---|---|---|
| `api_key` | `Authorization: Bearer <key>` | Raises the limits and unlocks private repositories. Empty means anonymous (public repositories only). |
| `org_id` | `X-Org-Id: <id>` | Selects the Devin organization. Required for the organization-wide `devin_*` tools. |

Both settings live in Plugin Management → the `deepwiki` plugin → settings; the
API key is stored as a sensitive plugin value and is never written to the
repository.

DeepWiki's remote MCP server does not expose OAuth metadata
(`/.well-known/oauth-protected-resource` returns 404), so this plugin
authenticates with an API key rather than a browser login.

## Tools

`read_wiki_structure`, `read_wiki_contents`, `ask_wiki_question` — plus the
organization-wide `devin_*` tools when a key and organization are configured.

## Files

- `.zcode-plugin/plugin.json` — manifest and user settings
- `.mcp.json` — MCP server definition
- `skills/deepwiki/SKILL.md` — tool workflow plus authentication troubleshooting
