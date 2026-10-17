# Credits

Third-party assets bundled with or loaded by this scaffold.

## Fonts

| Font | Source | Licence |
| :-- | :-- | :-- |
| Public Sans | [Google Fonts](https://fonts.google.com/specimen/Public+Sans), self-hosted through `next/font` | SIL Open Font Licence 1.1 |
| TikTok Sans | [Google Fonts](https://fonts.google.com/specimen/TikTok+Sans), loaded from the Google Fonts CSS API | SIL Open Font Licence 1.1 |

## Images

The product cut-outs, journal photography, editorial stills, shop covers and people portraits in `apps/web/public/images/` come from [Pexels](https://www.pexels.com/) under the [Pexels Licence](https://www.pexels.com/license/): free for commercial use, no attribution required. None of the product shots carry a third-party brand mark. The people portraits are stock models, not real staff.

A few licensed editorial photos (journal covers, two banners and two shop covers) are not redistributed in this repository.

## Libraries

Each app's `package.json` declares its own. The notable runtime libraries:

- Next.js 16, React 19, React DOM
- Tailwind CSS v4, class-variance-authority, clsx, tailwind-merge
- Radix UI primitives, through components that follow the [shadcn/ui](https://ui.shadcn.com/) pattern and are restyled onto the project's own tokens
- lucide-react, react-day-picker, sonner, Zustand
- Express 5, Mongoose 9, Helmet, CORS, compression, dotenv
- Sentry (inactive until you set a DSN)

## Icons

The storefront's icons come from [Lucide](https://lucide.dev) (ISC licence), mapped to the project's own names in `apps/web/src/components/ui/Icon.tsx`. The dashboard's `Icon.tsx` draws the same set as original stroke paths, and Lucide covers a few of its controls. The leaf in the logo is original.

## Scope of the MIT licence

The MIT licence in [LICENSE](./LICENSE) covers the source code in this repository. The demo photographs are covered by their own licence (above). The upstream "Grovia" name and leaf logo identify the original project this scaffold is derived from; they are not a trademark grant, and this repository does not claim them. Some internal identifiers (the `grovia` database name, the `apps/api/data/grovia/` path) still carry the original name because renaming them would touch code, which is out of scope here.
