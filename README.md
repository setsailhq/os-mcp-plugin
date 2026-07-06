# Setsail OS — Claude Code plugin marketplace

A Claude Code plugin marketplace hosting the **setsail-os** plugin, which connects
Claude to Setsail OS via MCP (the **tools**) and bundles the `phs` bridge skill
that routes to the team's playbooks in the live PostHog skill store — one
install gets both (see `setsail-os/README.md`).

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
  .claude-plugin/plugin.json        (MCP server config)
  skills/phs/SKILL.md               (bridge skill — routes to the PostHog skill store)
  README.md
```

## Publishing
This folder is the marketplace root, mirrored to its own repo
(`github.com/setsailhq/os-mcp-plugin`). Run `pnpm publish:plugin` from the main
OS repo to sync this folder to it.
