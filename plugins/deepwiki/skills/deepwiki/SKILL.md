---
name: deepwiki
description: Use when the user asks how a GitHub repository works, wants an overview or architecture of an unfamiliar codebase, or asks a question about a specific repository's code; also use when a DeepWiki tool call fails with an authentication/401 error or the server is not connected.
---

# DeepWiki

AI-generated documentation and Q&A about indexed GitHub repositories. Use it for
repository-level understanding.

## Tools

| Tool | Purpose |
|---|---|
| `read_wiki_structure` | List the documentation topics of a repository. Start here. |
| `read_wiki_contents` | Read the repository documentation (optionally one page). |
| `ask_wiki_question` | Ask a question about the repository, get an AI-powered answer. |

Tool names come from the server; trust the names in the tool list.

## Workflow

1. `read_wiki_structure` with `owner/repo` (for example `facebook/react`).
2. `read_wiki_contents` for the documentation itself or one page;
   `ask_wiki_question` for a specific question — usually the better first move.
3. Answer from the wiki and name the repository it came from.

## Authentication (only when a call fails)

Public repositories work anonymously — leave both settings empty for them.

| Setting | Sent as | Purpose |
|---|---|---|
| `api_key` | `Authorization: Bearer …` | Raises the limits and unlocks private repositories. |
| `org_id` | `X-Org-Id` | Selects the Devin organization that private-repo and `devin_*` tools act in. |

Both values come from https://app.devin.ai/settings/api-keys.

Without `org_id` the server uses the first organization the user joined, and one
request can target only one organization — so set it when the user belongs to
several and a tool reports "not found". A wrong organization id looks exactly
like a missing resource.

1. **Settings → MCP** → check the `deepwiki` entry status.
2. Ask the user to set `api_key` / `org_id` under Plugin Management → deepwiki →
   settings. Never ask them to paste a key into the chat.
3. Restart the session and retry.

## Notes

- If the wiki is missing for a repository, the tool says so — do not invent
  documentation, read the code instead.
- Verify specifics (signatures, options, versions) against the actual source.
