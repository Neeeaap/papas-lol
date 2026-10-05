# Agent setup

This repository uses Cloudflare Workers through Wrangler. Its agent setup keeps
framework documentation, publishing, image creation, and debugging separate.

| Capability | Setup | Status |
| --- | --- | --- |
| Development workflow | Installed superpowers skills | Available in the current environment |
| Raster artwork and image edits | Installed imagegen skill and built-in image tool | Available in the current environment |
| Astro documentation | astro_docs remote MCP | Configured in .codex/config.toml |
| Cloudflare documentation | cloudflare_docs remote MCP | Configured in .codex/config.toml |
| Publishing | Existing Wrangler dependency and npm scripts | Requires dependencies and Cloudflare authentication |
| Deployed logs and analytics | cloudflare_observability remote MCP | Configured, disabled until needed |

AGENTS.md captures project-specific instructions. No custom skill is needed
for these conventions, and no extra image service is needed for the built-in
image generation path. The installed skills belong to this environment;
cloning the repository onto another computer does not install them.

## Activate the documentation MCPs

Open the actual repository folder in Codex:

```text
C:\Users\kamil\Desktop\papas-lol\papas-lol
```

The parent folder currently used as the workspace contains this repository as
a child. Project config inside the child applies when working in the child,
not when starting Codex from its parent.

Trust the repository using the client's normal project trust flow, then start
a new session or restart the MCP connections. Project config is loaded only
for trusted projects. In a local terminal at the repository root:

```powershell
codex mcp list
```

Use `/mcp` in the Codex CLI to inspect active connections. In the desktop app
or IDE extension, use the MCP server settings and connection status. Seeing
entries in `codex mcp list` verifies registration, not a successful request.
Confirm connectivity by asking the agent to search Astro documentation and
Cloudflare Workers documentation through their respective servers.

The documentation endpoints are:

- Astro: https://mcp.docs.astro.build/mcp
- Cloudflare: https://docs.mcp.cloudflare.com/mcp

## Cloudflare publishing

Run in an interactive local terminal at the repository root:

```powershell
npm ci
npx wrangler login
npx wrangler whoami
npm run check
```

Set `site` in astro.config.mjs to your actual production URL before publishing.
When ready to publish, run `npm run deploy` after checks pass. The deploy script
does not build the site itself; `npm run check` includes the build.

Wrangler deploys this site without a Cloudflare API MCP. MCP login does not
authenticate Wrangler, and Wrangler login does not authenticate an MCP server.

## Optional production debugging

When Worker logs and analytics are needed, change the Observability entry's
`enabled` value to `true` in .codex/config.toml, then authenticate:

```powershell
codex mcp login cloudflare_observability
```

Restart the connection and verify that it can access the intended Cloudflare
account and Worker. Use this focused server for deployed logs; add broader
Cloudflare API access only when a task requires it.

## Image requests

For example: "Generate a 2:1 space-themed blog cover that fits the dark theme,
save it under public/images, and use it for the first post."

The agent should invoke imagegen, copy the selected image into the project,
update the relevant image URL, and check the rendered page. No image generation
was requested or performed during this setup.

## Official references

- [Codex MCP configuration](https://developers.openai.com/codex/mcp/)
- [Codex project configuration and trust](https://developers.openai.com/codex/config-basic/)
- [Astro Docs MCP](https://mcp.docs.astro.build/)
- [Cloudflare MCP catalog](https://github.com/cloudflare/mcp-server-cloudflare)
- [Cloudflare tools for agents](https://developers.cloudflare.com/docs-for-agents/)
