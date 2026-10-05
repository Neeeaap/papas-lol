# Project guidance

## Stack and layout

This is an Astro 5 blog on Cloudflare Workers, using the Cloudflare adapter,
MDX, RSS, and sitemap integrations. Use Node.js 22 or later and npm with the
existing package-lock.json. Run commands from the directory containing
package.json, astro.config.mjs, and wrangler.json.

- Shared theme: src/styles/global.css. Preserve the dark palette and readable
  text, links, navigation, metadata, and code blocks.
- Pages: src/pages/; shared components: src/components/.
- Post layout: src/layouts/BlogPost.astro.
- Blog content and schema: src/content/blog/ and src/content.config.ts.
- Public assets: public/; URLs omit the public/ prefix.

## Skills and documentation

Use the installed superpowers workflow for development, scaled to the task.
Use imagegen for requested raster image generation and editing. These skills
are already available in this environment; do not duplicate them in the repo.
Use available browser tools to inspect visual changes at desktop and mobile
sizes when a preview can run.

Prefer the astro_docs MCP for Astro questions and cloudflare_docs MCP for
Workers, Wrangler, and adapter questions. Check guidance against the versions
in package.json: current documentation may target newer releases. If MCPs are
unavailable, use official Astro and Cloudflare documentation.

## Local checks and publishing

- Install dependencies: npm ci.
- Development: npm run dev.
- Production check: npm run check (Astro build, TypeScript, Wrangler dry run).
- Worker preview: npm run preview (build followed by wrangler dev).
- Regenerate binding types after binding changes: npm run cf-typegen.

Keep the existing Workers deployment model and wrangler.json configuration.
For requested publishing, validate first, then run npm run deploy. Wrangler
authentication is separate from MCP authentication. In an interactive local
terminal, npx wrangler login authenticates and npx wrangler whoami checks the
selected account. In CI, use Cloudflare credentials from the CI secret store.

Before a first production publish, obtain the actual site URL and replace
https://example.com in astro.config.mjs so canonical URLs, sitemap, and RSS
point to the deployed site. Do not invent a domain or account ID. A tooling
setup request alone does not request a production deployment.

If dependencies, network, credentials, or preview tools are unavailable,
report the blocked check accurately. Do not treat a dry run as a deployment.
Keep secrets out of tracked config files and generated assets.

## Generated images

Use the built-in image generation tool through imagegen when available; no
additional image MCP or API key is required for that path. Use the existing
charcoal and soft blue theme as context for website art unless the brief says
otherwise. Derive subject, composition, crop, and dimensions from the requested
placement. Keep headings and interface text in HTML.

Copy selected generated assets into public/images/ with descriptive names;
reference them as /images/filename. Existing blog heroImage fields are URL
strings, so keep that convention unless an asset-pipeline change is requested.
Preserve transparency when requested, set image dimensions to limit layout
shift, and use useful alt text for meaningful images or empty alt for decoration.
Verify the actual saved asset and its page rendering before claiming integration.

## MCP configuration

.codex/config.toml enables Astro Docs and Cloudflare Docs. Cloudflare
Observability is configured but disabled; enable and authenticate it when
deployed Worker logs or analytics are needed. See docs/agent-setup.md for
activation and verification. Configured servers are not necessarily connected
in an existing session.
