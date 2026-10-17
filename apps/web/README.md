# apps/web: the storefront

Next.js 16 (App Router, Turbopack) with React 19, Tailwind CSS v4 and shadcn/ui on the project's own design tokens. **Foundation state: the design system, primitives and helpers are done and tested; the only page is a temporary token/primitive test page.**

```bash
pnpm run dev:web        # from the repo root: http://localhost:3000
pnpm --filter web run test
```

## What's here

| Path | Job |
| :-- | :-- |
| `src/app/globals.css` | The whole design in one file: colours, semantic tokens, radii, shadows, motion, and base-layer typography. Every utility in the app resolves to a token here |
| `src/app/layout.tsx` | The document shell: Public Sans through `next/font`, TikTok Sans from Google Fonts, metadata, `data-scroll-behavior="smooth"` |
| `src/app/(storefront)/page.tsx` | A temporary page that proves the fonts, tokens and primitives render. Replace it with the real home page when you build one |
| `src/components/ui/` | shadcn/ui primitives (button, dialog, sheet, select, tabs, slider, calendar…) restyled onto the tokens, plus `Icon.tsx` and `SmartImage.tsx` |
| `src/components/Logo.tsx` | The leaf mark and wordmark |
| `src/components/site/SiteSettingsProvider.tsx` | Runtime site settings for client components, starting with one switch: whether remote images go through `next/image` (`SmartImage` reads it) |
| `src/lib/utils.ts` | `cn()`, `discountPercent()` and `slugify()`, with tests |
| `public/images/` | The asset pack: product cut-outs, plus journal, editorial, home-promo, people and shop photos |
| `next.config.ts` | Loads the root `.env`, security headers, image hosts, and an optional Sentry wrapper (silent without a DSN) |

## Rules for this app

- Semantic tokens only: `bg-primary`, `text-muted-foreground`, `rounded-tile`. No raw palette classes, no hex, no `dark:`.
- Merge classes with `cn()`.
- Import through `@/`.
- Product photos are transparent cut-outs. Never `object-cover` them.

## What's not here yet

The data layer, sessions/server actions, client state, product building blocks, the header, sign-in, the home page, shop and product pages, basket/checkout/tracking, the account area, and journal/seller/content pages are all still to be built on top of this foundation.
