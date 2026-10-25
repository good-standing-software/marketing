---
paths:
  - "**/*.html"
  - "src/**/*.css"
  - "assets/**"
  - "llms.txt"
---

# Design

A letterhead, not a product landing page: few words, generous space, no chrome.

## Tokens

Tokens live in `src/input.css` (`@theme`). After changing tokens or classes, run `npm run build:css` and commit `assets/site.css`.

| Token | Hex | Role |
|---|---|---|
| `paper` | `#F4F1EA` | Page background |
| `ink` | `#1A1916` | Text and filled controls |
| `muted` | `#4F4B45` | Secondary text |
| `rule` | `#D4CFC4` | Hairline borders |
| `clay` | `#D2644A` | Focus and small accents only |

There is no other palette.

## Mark

`assets/logo.svg` beside the CSS `.wordmark` is the header. `favicon.svg`, `assets/logo.png` (512×512, transparent) and `assets/logo-white.png` (on `paper`) are the same artwork; `assets/og.png` is the social preview. Never redraw the mark or nudge it into place with negative margins, clipping, or transforms.

- `.brand`: `inline-flex items-center gap-1.5`, not underlined.
- `.brand-mark`: 34×34, `object-contain`.
- `.wordmark`: `text-[1.625rem]`, tracking `0.02em`, normal weight, stack `"Iowan Old Style", "Palatino Linotype", Palatino, Georgia, "Times New Roman", serif`.
- Body text is the system sans; no webfonts. The wordmark's size never spreads to body, tagline, footer, or nav.
- Header `py-6`; inset `px-6 sm:px-10`; footer `pt-8 pb-7`.
- Focus is a 2px `clay` outline with a 2px offset. Links are underlined at about 55% ink, full ink on hover.

## Pages

Spacing between paragraphs comes from `gap-y-*` or margins; never `[&_p]:m-0`, and never `flex flex-col` around `main p` without a gap.

- **Home:** tagline and two short paragraphs (the work as it is; research before product), `max-w-[34rem]`, first paragraph `mt-8`. One text survey link, not a button, with no `email=` and no mailto in `main`.
- **Thanks:** an `h1` and a few short paragraphs; same chrome; no pitch and no extra nav row.
- **Privacy:** long form, space after the `h1` before "Last updated", `mt-10` before each `h2`.

## Footer

The legal block comes first: registered name; place of registration, company number, and ICO number; the registered office address with no label. Then the links:

| Label | Target |
|---|---|
| Contact | `mailto:info@goodstanding.co.uk` |
| Survey | `https://research.goodstanding.co.uk/r/Bz5oLN?ref=footer` |
| Privacy | `privacy/`, or `./` on that page |

## Static site

- GitHub Pages serves committed HTML and `assets/site.css`; there is no runtime. Tally's redirect to `/thanks/` is configured in Tally.
- No cards, gradients, or animation.
- Umami Cloud in each public `<head>` is the only tracker. The `assets/site.css` preload is the only preload.
- One JSON-LD graph, using the existing schema.org types.
- `llms.txt` is the LLM-facing index. Any change to a public page, the product one-liner, or the company names updates it in the same pull request.
