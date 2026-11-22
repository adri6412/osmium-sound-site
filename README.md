# osmium-sound-site

Source of **osmiumsound.it** — the Osmium Sound marketing site and its Cloudflare
Pages Functions. Split out of the image repos (`osmium-sound`,
`osmium-sound-raspberry`), where the site used to be duplicated under `website/`.

## Layout
- Site pages at the repo root (`index.html`, `manual.html`, `quickstart.html`, …).
- `functions/` — Cloudflare Pages Functions (middleware, counters, API routes).
- `analytics/` — download/stats SQL.

## Deploy
`.github/workflows/deploy.yml` deploys the whole snapshot to the Cloudflare Pages
project **`osmium-sound`** (custom domain, D1 binding and secrets live on the
project). It runs on push to `main`, on `workflow_dispatch`, and on a
`repository_dispatch` of type `ota-updated`.

🚨 The deploy also carries the **x86 OTA manifests**: Cloudflare Pages serves the
whole snapshot at once, and devices read
`osmium-sound.pages.dev/ota/latest-<channel>.json`. The workflow pulls `ota/` from
the public `gh-pages` branch of `adri6412/osmium-sound` (written by its
`build-ui-ota.yml`) and includes it in every deployment. After a release writes
`ota/`, `osmium-sound` dispatches `ota-updated` here.

## Branches
`alpha` → `svil` → `main`, same convention as the other repos. Work on `alpha`,
promote to `main` to go live.

## Required secret
- `CLOUDFLARE_API_TOKEN` — same token as the `osmium-sound` Pages project.
