# apps/admin: the dashboard

A second Next.js 16 app on the same design tokens as the storefront, plus a KPI palette for dashboard tiles. **Foundation state: the design system, primitives and utility libs are done and tested; the only page is a placeholder header.**

```bash
pnpm run dev:admin      # from the repo root: http://localhost:3001
pnpm --filter admin run test
```

## What's here

| Path | Job |
| :-- | :-- |
| `src/app/globals.css` | The storefront's token set plus the dashboard's KPI colours |
| `src/app/layout.tsx` | The document shell. Metadata sets `robots: noindex`, because the dashboard is private |
| `src/app/(dashboard)/page.tsx` | A placeholder landing page — just a `PageHeader`. Add the sidebar shell and the live overview on top of it |
| `src/components/ui/` | shadcn/ui primitives, plus dashboard wrappers: `Drawer` (over Sheet), `ConfirmDialog` (over AlertDialog), `DatePicker`, `FormSelect`, `TableFilters`, `PriceTag`, `Rating`, `Primitives` (`PageHeader`, `Card`…) |
| `src/lib/utils.ts` | `cn()`, `money()`, `price()`, `discountPercent()` and `slugify()` |
| `src/lib/pagination.ts`, `list-query.ts` | Page sizes, the list fetch cap, the "Showing x–y of z" label, and list URLs that leave out default values (tested) |
| `src/lib/recover.ts` | Tells a stale-chunk error, which needs a reload, from an ordinary one, which needs a re-render (tested) |
| `src/lib/observability.ts` | Reports a browser error once per page load, to Sentry (when a DSN is set) and to the API's client-error intake (that API route does not exist yet) |

## Rules for this app

- Same tokens and rules as the storefront: semantic classes only, `cn()`, `@/` imports, no `dark:`.
- Use the wrappers: drawers are `Drawer`, confirmations are `ConfirmDialog`, toggles are `Switch`, icon-only buttons get a `Tooltip`.

## What's not here yet

Sessions and sign-in, the sidebar shell, the CRUD registry, the overview, and every admin screen (products, orders, staff jobs, people, sellers, promotions, reports, the seller portal) are all still to be built on top of this foundation.
