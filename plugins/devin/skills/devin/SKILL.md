---
name: devin
description: Use when the user needs the Devin MCP server - private repository documentation, generating a repository wiki, listing the repositories DeepWiki has indexed, or running the organization-wide devin_* tools (sessions, automations, code scans, billing tags, integrations). Also use when a Devin tool call fails with an authentication, 401, or missing-organization error.
---

# Devin MCP

The Devin MCP server (`https://mcp.devin.ai/mcp`) covers everything the public
DeepWiki endpoint does not: private repositories, wiki generation and the
organization-wide `devin_*` tools.

## Tools

Wiki tools (same names as the public DeepWiki server):

| Tool | Purpose |
|---|---|
| `read_wiki_structure` | List the documentation topics of a repository. |
| `read_wiki_contents` | Read the repository documentation (optionally one page). |
| `ask_wiki_question` | Ask a question about the repository. |
| `list_wiki_repos` | List the repositories that have a DeepWiki index. |
| `generate_wiki` | Generate a codebase wiki. **Only when the user explicitly asks for it.** |

Organization tools (`devin_*`): sessions, automations, code scans, billing tags,
campaigns, playbooks, integrations, knowledge, MCP server management and more.
Tool names come from the server; trust the names in the tool list.

## Workflow

1. Pick the tool by intent: `read_wiki_structure` to see the topics,
   `ask_wiki_question` for a specific question, `read_wiki_contents` for the
   documentation itself, `list_wiki_repos` to find indexed repositories,
   `generate_wiki` only on an explicit request.
2. For a `devin_*` tool, make sure the target organization is the intended one.
3. Report the organization you acted in when the result depends on it.

## Authentication

This server has **no usable OAuth flow** — its authorization server publishes no
dynamic client registration endpoint, so no browser login can complete. It
authenticates with a **Devin API key**, sent as `Authorization: Bearer <key>`.

1. **Settings → MCP** → the `devin` entry → check status and error text.
2. If the key is missing or wrong, ask the user to set `api_key` under
   Plugin Management → devin → settings (create one at
   https://app.devin.ai/settings/api-keys). Never ask them to paste a key into
   the chat.
3. Restart the session and retry.

An invalid key is only rejected when a tool is called, so a connected server can
still fail at call time — read the tool error before assuming the key is fine.

## Organization

`org_id` (plugin setting → `X-Org-Id`) selects the Devin organization the
organization-scoped tools act in. Without it the server resolves the first
organization the key belongs to, and one request targets a single
organization. A wrong or missing organization surfaces as
"requires an organization-scoped API context, but no org_id could be resolved"
or as a "not found" result — treat both as an organization problem first.

## Notes

- Public repositories are better served by the `deepwiki` plugin (no
  authentication, same wiki tools).
- Verify specifics (signatures, options, versions) against the actual source.
