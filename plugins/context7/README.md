# Context7 plugin

Context7 MCP server for ZCode. It gives the agent current library and framework
documentation through two tools — `resolve-library-id` and `query-docs` — both
provided by the Context7 server itself.

## Configuration

The plugin declares one HTTP MCP server in `.mcp.json`:

```json
{
  "mcpServers": {
    "context7": {
      "type": "http",
      "url": "https://mcp.context7.com/mcp/oauth",
      "oauth": { "type": "authorization_code" },
      "timeoutMs": 60000
    }
  }
}
```

- The endpoint is Context7's **OAuth-enforced** one. `https://mcp.context7.com/mcp/oauth`
  answers every unauthenticated request with `401 Unauthorized` and
  `WWW-Authenticate: Bearer resource_metadata="https://mcp.context7.com/.well-known/oauth-protected-resource/mcp/oauth"`.
  (`https://mcp.context7.com/mcp` is the anonymous variant and never challenges.)
- `oauth` enables ZCode's built-in MCP OAuth 2.1 flow: it reads the resource
  metadata, registers a client dynamically (PKCE, S256), opens the browser to the
  Clerk sign-in page and stores the tokens in its own credential store. The
  plugin ships no runtime code and no credentials.
- No `Authorization` header is set on purpose — a static `Authorization` header
  tells the client that credentials are already present and would skip the OAuth
  flow entirely.

## Files

- `.zcode-plugin/plugin.json` — manifest and version
- `.mcp.json` — MCP server definition
- `skills/context7/SKILL.md` — tool workflow plus authentication troubleshooting
