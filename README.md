<div align="center">

# E-commerce starter

**A Turborepo monorepo with three apps that install, build, lint, type-check and run: a Next.js 16 storefront, a Next.js 16 admin dashboard and an Express 5 + MongoDB REST API.**

[![Next.js 16](https://img.shields.io/badge/Next.js-16-000000?logo=nextdotjs&logoColor=white)](https://nextjs.org)
[![React 19](https://img.shields.io/badge/React-19-61DAFB?logo=react&logoColor=black)](https://react.dev)
[![Express 5](https://img.shields.io/badge/Express-5-000000?logo=express&logoColor=white)](https://expressjs.com)
[![MongoDB](https://img.shields.io/badge/MongoDB-Mongoose_9-47A248?logo=mongodb&logoColor=white)](https://mongoosejs.com)
[![TypeScript strict](https://img.shields.io/badge/TypeScript-strict-3178C6?logo=typescript&logoColor=white)](https://www.typescriptlang.org)
[![Tailwind CSS v4](https://img.shields.io/badge/Tailwind_CSS-v4-06B6D4?logo=tailwindcss&logoColor=white)](https://tailwindcss.com)
[![Turborepo](https://img.shields.io/badge/Turborepo-pnpm-EF4444?logo=turborepo&logoColor=white)](https://turborepo.com)
[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](./LICENSE)

**Maintained by** [github.com/devpayoub](https://github.com/devpayoub)

</div>

---

## What this repository is

A **full-stack scaffold**, not a finished shop. Every app is at foundation state: the wiring, tooling, design system and server plumbing are complete and working. The product features — catalogue, sign-in, cart, checkout, orders, dashboard screens — are **not implemented yet**.

| App | Stack | Port | What actually exists |
| :-- | :-- | :-: | :-- |
| `apps/api` | Express 5 · Mongoose 9 · strict TypeScript (NodeNext ESM) | 8000 | Server plumbing done, **one endpoint** (`/api/health`), no models or routes |
| `apps/web` | Next.js 16 App Router · React 19 · Tailwind v4 · shadcn/ui | 3000 | Design system + primitives done, **one placeholder page** |
| `apps/admin` | Next.js 16 App Router · React 19 · Tailwind v4 · shadcn/ui | 3001 | Design system + dashboard primitives done, **one placeholder page** |

### In place and working

- **Monorepo:** pnpm workspaces, the Turborepo task pipeline, one root `.env`, strict TypeScript in every app, ESLint, Vitest, a GitHub Actions CI gate and a first-run setup script.
- **API plumbing:** helmet security headers, CORS with credentials, compression, JSON body limits, a MongoDB connection that retries with backoff, a per-request id on every response, a readiness gate that answers JSON `503` while the database is down, one JSON error envelope, JSON `404`s, keep-alive tuning and graceful shutdown on `SIGTERM`/`SIGINT`.
- **Design system:** the complete token stylesheet in each app's `globals.css` (colours, radii, shadows, motion, typography), Public Sans + TikTok Sans, ~30 shadcn/ui primitives restyled onto those tokens, the logo mark, and utility libs covered by Vitest.
- **Asset pack:** 50 product, editorial, people and promo photos in `apps/web/public/images/`, plus JSON seed content in `apps/api/data/grovia/`.

### Not built yet

- Mongoose models, controllers, route modules, API docs — nothing reads the seed JSON
- Sign-in, sessions, JWT cookies, OAuth
- Catalogue, cart, checkout, payments, orders, refunds
- Storefront chrome (header, footer), the real home page, shop and product pages
- Dashboard shell (sidebar, search, status banner) and every admin screen
- Email, background jobs, image uploads, the seed script
- `packages/` is empty — reserved for shared code

---

## Quick start

### Prerequisites

| Tool | Version | Notes |
| :-- | :-- | :-- |
| [Node.js](https://nodejs.org) | **22** (20.9 or newer) | `.nvmrc` pins 22, so `nvm use` picks it up |
| [pnpm](https://pnpm.io) | **11** | `corepack enable pnpm`, or `npm install -g pnpm@11` |
| [MongoDB](https://www.mongodb.com) | 7 or newer | Local, Docker, or a free [Atlas](https://www.mongodb.com/cloud/atlas/register) cluster |
| Git | any | |

### 1. Clone

Replace `/E-commerce-starter` with the repository name under your GitHub account.

```bash
git clone https://github.com/devpayoub/E-commerce-starter.git
cd E-commerce-starter
```

### 2. Set up

**Automatic (recommended):**

```bash
node setup.mjs
```

It checks your Node version, makes sure pnpm is available, runs `pnpm install`, creates `.env` from `_.env.example`, asks for your MongoDB connection string, and writes a random `JWT_SECRET`. Add `--yes` to accept every default.

**Manual:**

```bash
pnpm install
cp _.env.example .env          # Windows (PowerShell): Copy-Item _.env.example .env
openssl rand -base64 32        # paste the output into JWT_SECRET in .env
```

Then set `MONGO_URI` in `.env`.

### 3. Run

```bash
pnpm run dev
```

| App | URL | What you should see |
| :-- | :-- | :-- |
| Storefront | http://localhost:3000 | The logo, a headline, three buttons and six colour swatches — the token/primitive test page |
| Dashboard | http://localhost:3001 | The "Dashboard" page header |
| API | http://localhost:8000/api/health | `{"ok":true,"status":"healthy","db":"connected",…}` |

If the API answers `503` with `"db":"disconnected"`, it is running but can't reach MongoDB. See [Troubleshooting](#troubleshooting).

The step-by-step guide, including MongoDB on macOS, Windows, Linux, Docker and Atlas, is in **[SETUP.md](./SETUP.md)**.

---

## Project structure

```text
.
├── apps/
│   ├── api/                      Express 5 REST API              → :8000
│   │   ├── config/
│   │   │   ├── env.ts            finds and loads the one root .env, checks required keys
│   │   │   └── db.ts             MongoDB connection that keeps retrying (5 s up to 30 s)
│   │   ├── middleware/
│   │   │   ├── errorMiddleware.ts      one JSON error envelope + JSON 404s
│   │   │   ├── readinessMiddleware.ts  503 while the DB is down, request timeout
│   │   │   └── requestIdMiddleware.ts  X-Request-Id on every response
│   │   ├── data/grovia/          seed JSON, image manifest, gallery (nothing reads these yet)
│   │   └── server.ts             helmet, CORS, compression, /api/health, graceful shutdown
│   ├── web/                      Next.js 16 storefront           → :3000
│   │   ├── public/images/        asset pack: products, journal, people, shops…
│   │   └── src/
│   │       ├── app/              globals.css (every design token), layout, (storefront)/page
│   │       ├── components/ui/    shadcn/ui primitives, Icon, SmartImage
│   │       ├── components/       Logo, SiteSettingsProvider
│   │       └── lib/utils.ts      cn(), discountPercent(), slugify() (+ tests)
│   └── admin/                    Next.js 16 dashboard            → :3001
│       └── src/
│           ├── app/              globals.css (same tokens + KPI palette), layout, (dashboard)/page
│           ├── components/ui/    primitives + Drawer, ConfirmDialog, DatePicker, TableFilters,
│           │                     Primitives, Icon, PriceTag, Rating
│           └── lib/              utils, pagination, list-query, recover, observability (+ tests)
├── packages/                     empty — reserved for shared packages
├── _.env.example                 the one environment template (copy to .env)
├── setup.mjs                     first-run setup
├── turbo.json                    the task pipeline
├── pnpm-workspace.yaml           workspaces + allowed install scripts
└── package.json                  root scripts
```

Each app documents its own details: [apps/api](./apps/api/README.md) · [apps/web](./apps/web/README.md) · [apps/admin](./apps/admin/README.md).

---

## The API today

A working Express 5 server whose only route is the health check. Everything else — models, guards, routes — is yours to add.

| Method | Path | Answer |
| :-- | :-- | :-- |
| `GET` | `/api/health` | `200 { ok: true, status: "healthy", db: "connected", uptime, environment, timestamp }`, or `503` with `db: "disconnected"` |
| any | `/api/<anything else>` | `404 { success: false, code: "not_found", message, path, requestId }` |

Every response carries `X-Request-Id`. Anything needing the raw request body (a payment webhook, later) must mount **above** `express.json()` in `apps/api/server.ts`.

## The frontends today

- **Storefront** — one page proving fonts, tokens and primitives render: the logo, a headline, `primary`/`secondary`/`outline` buttons and six colour swatches.
- **Dashboard** — one page rendering `PageHeader` from `components/ui/Primitives.tsx`.
- Both apps share the same token vocabulary, so anything you build next lands on a consistent design.

---

## Environment variables

There is **one** `.env`, at the repo root. The API loads it through `apps/api/config/env.ts`, and both Next.js apps load it in their `next.config.ts`. Don't add per-app `.env` files.

| Variable | Required | Default in the template | Used by |
| :-- | :-: | :-- | :-- |
| `MONGO_URI` | ✅ | `mongodb://127.0.0.1:27017/grovia` | API |
| `JWT_SECRET` | ✅ | placeholder: `setup.mjs` replaces it | API |
| `JWT_EXPIRES_IN` | | `30d` | API |
| `NODE_ENV` | | `development` | all |
| `PORT` | | `8000` | API |
| `CLIENT_URL` / `ADMIN_URL` | | `http://localhost:3000` / `:3001` | API (CORS) |
| `NEXT_PUBLIC_API_URL` | | `http://localhost:8000` | web, admin |
| `NEXT_PUBLIC_WEB_URL` / `NEXT_PUBLIC_ADMIN_URL` | | `http://localhost:3000` / `:3001` | web, admin |

The API refuses to start without `MONGO_URI` and `JWT_SECRET`. `NEXT_PUBLIC_*` values are compiled into the browser bundle, so never put a secret in one. The template also lists, commented out, the variables a fuller build needs (Stripe, ImageKit or Cloudinary, SMTP, Google sign-in, Redis, Sentry). **This code reads none of them yet** — there is no payment, email, upload or job code to read them with.

> **Never commit `.env`.** It is git-ignored. If a secret is ever pushed, even for a moment, treat it as leaked and rotate it.

---

## Scripts

Run from the repo root. With no suffix, a script runs for every app through Turbo. With `:api`, `:web` or `:admin`, it runs for that app only.

| Script | What it does |
| :-- | :-- |
| `node setup.mjs` | First-run setup (install, `.env`, secrets) |
| `pnpm run dev` | Start all three apps in watch mode |
| `pnpm run dev:api` · `dev:web` · `dev:admin` | Start one app |
| `pnpm run build` | Production build of every app |
| `pnpm run lint` | ESLint (`no-explicit-any` is an error) |
| `pnpm run type-check` | `tsc --noEmit` in every app |
| `pnpm run test` | Vitest in the storefront and the dashboard |

The full gate before you push (CI runs the same thing):

```bash
pnpm run type-check && pnpm run lint && pnpm run test && pnpm run build
```

---

## Ground rules

The conventions every app already follows:

1. **No `any`.** Use `unknown` and narrow it, a real interface, or a generic. ESLint fails the build on `any`.
2. **Semantic tokens only.** Write `bg-primary` or `text-muted-foreground`, never `bg-green-600` or `bg-[#0b6a5b]`. Every colour, radius, shadow and animation is a token in `src/app/globals.css`.
3. **Light theme only.** No `dark:` variants.
4. **One `.env`, at the root.**
5. **API imports end in `.js`** (NodeNext ESM): `import x from "./foo.js"`, even from a `.ts` file.
6. **Next.js imports use `@/`:** `import { cn } from "@/lib/utils"`.
7. **Merge classes with `cn()`**, never with template strings.
8. **Throw, don't hand-roll:** `res.status(404); throw new Error("…")`. The error middleware builds the envelope.

[CLAUDE.md](./CLAUDE.md) holds the full list in a form that Claude Code and other AI assistants read automatically.

---

## The asset pack

| Path | What it holds |
| :-- | :-- |
| `apps/web/public/images/` | Transparent product cut-outs, plus journal, editorial, home-promo, people and shop photos (50 in all) |
| `apps/web/public/favicon.svg`, `apps/admin/public/favicon.svg` | The leaf icon |
| `apps/api/data/grovia/content.json` | Seed source: products, categories, sellers, journal posts, hero slides, FAQs, coupons |
| `apps/api/data/grovia/image-manifest.json` | Each local image mapped to its CDN copy |
| `apps/api/data/grovia/catalog-gallery.json` | Extra gallery angles for each product |

Nothing reads the JSON yet: there is no model to load it into and no seed script. A few licensed editorial photos are not in this repo because their licence doesn't allow redistribution — add your own under the same file names if you need them.

---

## Deploying

| App | Host | Root directory |
| :-- | :-- | :-- |
| Storefront | Vercel (Next.js) | `apps/web` |
| Dashboard | Vercel (Next.js) | `apps/admin` |
| API | Any Node 22 host (Render, Railway, Fly.io, a VPS) or Vercel serverless | `apps/api` (`pnpm run build`, then `node dist/server.js`) |

Set the same variables as in `.env`, using production URLs for `CLIENT_URL`, `ADMIN_URL` and every `NEXT_PUBLIC_*`. A production build of either Next.js app on Vercel fails on purpose if a `NEXT_PUBLIC_*` URL is still `localhost`. Note that the API serves only `/api/health` until you add routes.

---

## Troubleshooting

| Symptom | Fix |
| :-- | :-- |
| `Missing required environment variables: MONGO_URI, JWT_SECRET` | There is no `.env` at the repo root. Run `node setup.mjs`, or `cp _.env.example .env`. |
| API health shows `503` / `"db":"disconnected"` | MongoDB isn't reachable. Start it locally, or check the Atlas connection string, the database user's password, and **Network Access** (add your IP). The API keeps retrying, so there's no need to restart it. |
| `EADDRINUSE` on 3000, 3001 or 8000 | Something else is using that port. Stop it, or run one app elsewhere, for example `pnpm --filter web exec next dev -p 3100`. |
| pnpm says the version doesn't match | The repo pins pnpm through `devEngines`, and pnpm downloads it for you. Make sure you're on pnpm 10 or newer to begin with. |
| `Ignored build scripts` warning | pnpm only runs install scripts listed under `allowBuilds` in `pnpm-workspace.yaml`. Add the package there if a dependency needs one. |
| Fonts look plain | TikTok Sans loads from Google Fonts at runtime. Check your network or ad blocker. |

Something broken or unclear? [Open an issue](https://github.com/devpayoub) with your Node and pnpm versions, the command you ran, and the full error.

---

## Author

**devpayoub** · [github.com/devpayoub](https://github.com/devpayoub)

This scaffold is derived from the open-source Grovia starter, which is MIT licensed; its original copyright notice stays in [LICENSE](./LICENSE) because that licence requires it. See [CREDITS.md](./CREDITS.md) for fonts, photographs and libraries.

---

## License

Released under the [MIT License](./LICENSE). The demo photographs come from [Pexels](https://www.pexels.com/license/) and are free to use; see [CREDITS.md](./CREDITS.md).
