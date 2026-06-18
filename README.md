# Setsail OS — Claude Code plugin marketplace

A Claude Code plugin marketplace hosting the **setsail-os** plugin, which connects
Claude to Setsail OS via MCP and bundles team playbook skills.

## Add this marketplace + install

```bash
claude plugin marketplace add https://github.com/setsailhq/os-mcp-plugin
claude plugin install setsail-os@setsail-os --scope project
/reload-plugins
```

Then run `/mcp` and sign in to the OS in the browser.

## Layout
```
.claude-plugin/marketplace.json   ← this marketplace
setsail-os/                       ← the plugin (see its README)
  .claude-plugin/plugin.json
  skills/{account-prep,account-triage,log-discipline}/SKILL.md
  README.md
```

## Publishing
This folder is the marketplace root. Push it to its own repo
(`github.com/setsailhq/os-mcp-plugin`) — or keep it as a subdirectory and point
teammates at the repo + path. Update the `url` in
`setsail-os/.claude-plugin/plugin.json` and the install URL above if the repo
name differs. See `docs/modules/mcp-server.md` in the main OS repo for the full
server + auth design.
