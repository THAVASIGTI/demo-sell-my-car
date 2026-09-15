# SellMyCars — Live Demo

A **backend-free** static build of the SellMyCars dashboard, hosted on GitHub Pages.
Every API call is served by an in-browser mock (backed by `localStorage`) with
seeded dummy data — no server, database, or login backend required.

## Live site

**https://THAVASIGTI.github.io/demo-sell-my-car/**

## Logging in

Open the site → go to **`/panel/signin`** and sign in. **Any password works**, and
the username selects the role:

| Username | Role | Lands on |
|----------|------|----------|
| `admin` (or anything) | Admin | full dashboard |
| `inspect` | Inspection staff | role overview |
| `buyer` | Buyer staff | role overview |
| `reception` | Reception staff | role overview |
| `sales` | Sales | deal vehicle |

- All data (and any edits you make) lives in your browser's `localStorage` under
  the key `demo:db:v1`. Delete that key in DevTools → Application → Local Storage
  and reload to reset to the seed data.
- Invoice / agreement / report buttons return a small sample PDF.

## GitHub Pages setup

This repo contains the pre-built static site at its root. It is published by
[`.github/workflows/deploy.yml`](.github/workflows/deploy.yml), so Pages must be
set to:

**Settings → Pages → Build and deployment → Source: _GitHub Actions_.**

Do not switch this to "Deploy from a branch" — the workflow uploads the site as
a Pages artifact, and the branch source would deploy nothing.

The `.nojekyll` file is required (already present) so Pages keeps the `_next/`
folder that Jekyll would otherwise strip.

## Regenerating this build

The site is generated from the main app repo (`sellmycars-dashboard`). From that
repo's `dashboard/` folder:

```bash
BASE_PATH=/demo-sell-my-car npm run build:demo
```

`BASE_PATH` is the single input: `next.config.ts` feeds it to Next's `basePath`
and mirrors it into `NEXT_PUBLIC_BASE_PATH`, which `src/lib/basePath.ts` uses to
prefix the `public/` asset paths and the marketing pages' raw `<a href>` links
that `basePath` alone does not rewrite. It **must** match this repo's name
(`/demo-sell-my-car`) or those URLs will 404 on the Pages URL.

The build writes `demo_step/` at the root of the app repo; copy its contents
into this repo's root (keep `.git`, `.github`, `README.md` and `.gitignore`):

```bash
rsync -a --delete --exclude='.git/' --exclude='.github/' \
      --exclude='README.md' --exclude='.gitignore' \
      ../sellmycars-dashboard/demo_step/ ./
```

Do not hand-edit the exported files to change paths. The marketing pages are
injected with `dangerouslySetInnerHTML`, so their HTML lands inside a
length-prefixed React Flight row (`<id>:T<hexlen>,`) in the RSC payload; editing
that text without recomputing the byte count desyncs the stream and the page
dies with `enqueueModel is not a function`.

> This folder is generated output — edit the source in the main app repo, not here.
