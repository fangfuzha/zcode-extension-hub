# ZCode Extension Hub

A local ZCode plugin marketplace. The repository root is the marketplace root:
`marketplace.json` lists the plugins, and each plugin lives under `plugins/<name>/`.

## Plugins

| Plugin | Description |
|---|---|
| [context7](plugins/context7) | Context7 MCP server with built-in OAuth — current library/framework documentation for the agent. |
| [deepwiki](plugins/deepwiki) | DeepWiki MCP server for public GitHub repositories — AI-generated documentation and Q&A, no authentication. |
| [devin](plugins/devin) | Devin MCP server with API key authentication — private repository wikis, wiki generation and the organization-wide `devin_*` tools. |

## Using this marketplace in ZCode

1. Open **插件市场 / Plugin Marketplace → 添加 / Add → 添加插件市场 / Add Plugin Marketplace**.
2. Paste the marketplace root directory:

   ```
   D:\Desktop\zcode-extension-hub
   ```

3. Open **个人 / Personal → zcode-extension-hub → Context7 → 安装 / Install**.
4. Manage the installed plugin under **设置 → 插件 / Settings → Plugins** and its
   MCP status under **设置 → MCP / Settings → MCP**.

## Updating a plugin

1. Edit the plugin source under `plugins/<name>/` and bump the version in
   `plugins/<name>/.zcode-plugin/plugin.json`.
2. Sync the same version/description into the entry in `marketplace.json`.
3. In the marketplace page, open the gear button → **市场源 / Marketplace Sources** →
   refresh this marketplace.
4. Back in **个人 / Personal**, open the plugin details → **更新 / Update**.

## Layout

```
marketplace.json                  # marketplace catalog (market root = repo root)
plugins/
  context7/
    .zcode-plugin/plugin.json     # plugin manifest
    .mcp.json                     # MCP server definition (HTTP + built-in OAuth)
    skills/context7/SKILL.md      # skill: workflow + auth troubleshooting
  deepwiki/
    .zcode-plugin/plugin.json     # plugin manifest
    .mcp.json                     # MCP server definition (HTTP, anonymous)
    skills/deepwiki/SKILL.md      # skill: workflow + troubleshooting
  devin/
    .zcode-plugin/plugin.json     # plugin manifest
    .mcp.json                     # MCP server definition (HTTP, API key headers)
    skills/devin/SKILL.md         # skill: workflow + auth/organization notes
```
