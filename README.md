# Setsail OS — Claude Code plugin marketplace

A Claude Code plugin marketplace hosting the **setsail-os** plugin, which connects
Claude to Setsail OS via MCP (the **tools**). The team's skill **playbooks** live
in the PostHog skill store and install separately (see `setsail-os/README.md`).

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
  .claude-plugin/plugin.json        (tools only — skills live in PostHog)
  README.md
```

## Publishing
This folder is the marketplace root, mirrored to its own repo
(`github.com/setsailhq/os-mcp-plugin`). Run `pnpm publish:plugin` from the main
OS repo to sync this folder to it.
