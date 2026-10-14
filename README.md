# The Knhdixh Directory

A homepage for **knhdixh.com**, set like an old New York telephone directory. It lists every service on the domain; click a listing to go to it.

**Live:** https://knhdixh-directory.sieubadaokhanh04.workers.dev (knhdixh.com once the domain is attached)

![The Knhdixh Directory](docs/screenshot.png)

## Listings

| Listing | Address | What it is |
|---|---|---|
| Attendance80 | [epita.knhdixh.com](https://epita.knhdixh.com) | Class attendance tracker |
| Attendance80 Rehearsal | [epita-staging.knhdixh.com](https://epita-staging.knhdixh.com) | Staging copy of the above |
| n8n Workflow Bureau | [n8n.knhdixh.com](https://n8n.knhdixh.com) | Self-hosted n8n automation |
| Nicotine Taper | [ciggs.knhdixh.com](https://ciggs.knhdixh.com) | Step-down plan from cigarettes to 0 mg |

## Features

- Phonebook layout: letter headings, dot leaders, old-style exchange numbers
- **Operator** search box: type to filter, press Enter to go to the first match
- Keys `1`–`4` open the listings in order
- Light "morning" and dark "evening" editions that follow the system setting
- A matching "Number Not Listed" 404 page
- No build step, no framework: one static HTML file

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

To add a listing, copy an `<a class="entry">` block in [`public/index.html`](public/index.html) and file it under the right letter.
