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

This repo contains the pre-built static site at its root. To publish:

**Settings → Pages → Build and deployment → Source: _Deploy from a branch_ →
Branch: `main` / `/ (root)`.**

The `.nojekyll` file is required (already present) so Pages keeps the `_next/`
folder that Jekyll would otherwise strip.

## Regenerating this build

The site is generated from the main app repo (`sellmycars-dashboard`). From that
repo's `dashboard/` folder:

```bash
NEXT_PUBLIC_BASE_PATH=/demo-sell-my-car BASE_PATH=/demo-sell-my-car npm run build:demo
```

Then copy the contents of `dashboard/.next-demo/` into this repo's root (keep
`.nojekyll`). The base path **must** match this repo's name (`/demo-sell-my-car`),
or the `_next/` asset URLs will 404 on the Pages URL.

> This folder is generated output — edit the source in the main app repo, not here.
