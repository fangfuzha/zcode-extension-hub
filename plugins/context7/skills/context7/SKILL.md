---
name: context7
description: Use when the user asks about a library, framework, SDK, API, CLI tool or cloud service and the answer depends on current API surface, version-specific behavior, setup steps, or migration; also use when a Context7 tool call fails with an authorization/401 error or the server shows as needing authorization.
---

# Context7

Query the Context7 MCP server for current documentation. Prefer it over memory and
over web search for anything library-specific.

## Tools

| Tool | Purpose |
|---|---|
| `resolve-library-id` | Package/product name → Context7 library ID (`/org/project`). Call first. |
| `query-docs` | Fetch docs for a library ID and a focused question. |

Tool names come from the server; trust the names in the tool list.

## Workflow

1. `resolve-library-id` with the official name (`Next.js`, not `nextjs`) and a
   `query` describing the need. Skip only if the user gave an `/org/project` ID.
   At most 3 calls per question.
2. Pick the best match: exact name, then source reputation, then snippet count.
   Use `/org/project/version` when a version was named.
3. `query-docs` with that ID and one narrow, self-contained question.
4. Answer from the returned docs; never invent APIs. If the docs do not cover the
   question, say so and query again with different wording.

## Authentication (only when a call fails)

The server uses Context7's OAuth endpoint (`https://mcp.context7.com/mcp/oauth`),
so unauthenticated calls get `401`.

1. **Settings → MCP** → the `context7` entry → **需要授权 / Open authorization**,
   sign in to Context7 in the browser, then reconnect.
2. Read the inline error (same details in `~/.zcode/cli/log`, `mcp.oauth.*`).
   `MCP_OAUTH_INTERACTIVE_REQUIRED` means the login is still pending — finish it
   and retry; a rejected token needs the authorization opened again.
3. If the account cannot be used, ask the user to set the plugin's `api_key`
   (a `ctx7sk…` key from https://context7.com/dashboard) under
   Plugin Management → context7 → settings. Never ask them to paste a key into
   the chat.

## Scope

Library documentation only — not refactoring, code review, or general
programming concepts that do not depend on a library API.
