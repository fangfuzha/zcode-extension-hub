# Repository guidance

This repository is a ZCode plugin marketplace. The repository root is the
marketplace root; `marketplace.json` is the catalog and plugin sources live under
`plugins/<name>/`.

## Adding or changing a plugin

- The plugin directory name and the manifest `name` must match
  (`plugins/<name>/.zcode-plugin/plugin.json`).
- Bump the manifest `version` on every change and copy the same version into the
  matching `marketplace.json` entry — they must agree.
- Only declare resources that exist. `mcpServers` may point to `./.mcp.json`.
- MCP server entries use the canonical schema only: `type` (`stdio` | `http` | `sse`),
  `url`, `headers`, `oauth`, `timeoutMs`, `enabled`. Unknown keys make ZCode drop
  the server.
- `${...}` templates are expanded for plugin-provided MCP servers only
  (`${ZCODE_PLUGIN_ROOT}`, `${ZCODE_PROJECT_DIR}`, `${user_config.<key>}`).
  Plugin configuration values are declared in the manifest's `userConfig` object.
- Never commit secrets. Use `userConfig` with `sensitive: true` or environment
  variables.
- Keep `displayName`, `displayName_i18n`, `description_i18n` and `category`
  (one of `developer-tools`, `productivity`, `utilities`, `legal`, `template`,
  `finance`, `other`) in the marketplace entry.

## Verification

`zcode plugins validate` can be run when a working ZCode CLI is available. File
inspection alone does not prove schema validation or runtime behavior.

## Git

- **Never commit on your own.** Do not run `git commit` unless the user explicitly
  asks for a commit in that turn. Leaving changes in the working tree (or staged)
  is the normal end state.
- Do not create commits for changes the user did not ask for, and do not create
  throwaway commits ("wip", "temp", "fix formatting" on unrelated files). One
  commit should correspond to one intentional, coherent change.
- Do not amend, rebase, reset, force-push, or rewrite history unless the user
  explicitly asks for it.
- Stage nothing implicitly either; `git add` is fine when preparing for a
  commit the user requested, but do not stage unrelated files.
- When reporting what changed, list the files and the effect, and let the user
  decide when to commit.
