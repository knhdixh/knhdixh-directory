# knhdixh

The homepage for **knhdixh.com**: an Apple-style grouped list of everything running on the domain, built with [Puppertino](https://puppertino.com), with a blue WebGL water trail behind it.

**Live:** https://knhdixh.com

![knhdixh](docs/screenshot.png)

## Listings

| Group | Name | Address | What it is |
|---|---|---|---|
| School | epita | [epita.knhdixh.com](https://epita.knhdixh.com) | Class attendance tracker |
| School | classroom | [classroom.knhdixh.com](https://classroom.knhdixh.com) | Private French classroom ([ma-classe-prod](https://github.com/knhdixh/ma-classe-prod)) |
| Health | ciggs | [ciggs.knhdixh.com](https://ciggs.knhdixh.com) | Nicotine taper plan |
| Automation | n8n | [n8n.knhdixh.com](https://n8n.knhdixh.com) | Self-hosted n8n |
| Staging | epita-staging | [epita-staging.knhdixh.com](https://epita-staging.knhdixh.com) | Staging copy of epita |

One static HTML file, no build step. Puppertino's CSS (typography, colors, shadows, materials, lists) comes from `cdn.knhdixh.com`; it uses system fonts and follows the system light/dark setting. Entrances use one spring curve and animate only transform and opacity. The cursor trail is a small stable-fluids simulation in WebGL2 that sleeps a few seconds after the mouse stops, brightens on dark backgrounds, and is skipped for reduced-motion users or browsers without WebGL2.

## Stack

Static assets served by a Cloudflare Worker ([`wrangler.jsonc`](wrangler.jsonc)), on the custom domains `knhdixh.com` and `www.knhdixh.com`.

## Run locally

```bash
npm install
npm run dev        # http://localhost:8787
```

## Deploy

Every push to `main` deploys automatically through Cloudflare Workers Builds (about 30 seconds). To deploy by hand:

```bash
npm run deploy
```

To add a listing, copy an `<a class="p-list-row">` into the right group in [`public/index.html`](public/index.html), or add a new `<section>` for a new group.

## Style

This page uses Puppertino by explicit choice. Other knhdixh.com sites follow the [knhdixh cookbook](docs/knhdixh-cookbook.md) by default; its reference implementation is [`docs/template.html`](docs/template.html) ([gist](https://gist.github.com/knhdixh/848f66f3b777da8428bfe5bb9503cc66)).
