# Design

Quiet paper-and-ink holding site for **Good Standing** — software for UK professional membership bodies. The page should feel like a letterhead, not a product landing: few words, generous air, no chrome.

This file is the human source of truth. Tokens live in `src/input.css` (`@theme`). Rebuild with `npm run build:css` after token or utility-class changes; commit `assets/site.css`.

## Names

| Use | Name |
|---|---|
| Product | Good Standing |
| Trading | Good Standing Software |
| Legal | Good Standing Software Ltd |

Never write “trades as Good Standing”. Do not mention CPD in marketing blurbs.

## Tokens

| Token | Hex | Role |
|---|---|---|
| `paper` | `#F4F1EA` | Page background |
| `ink` | `#1A1916` | Primary text; filled controls when a button is needed |
| `muted` | `#4F4B45` | Secondary / supporting text |
| `rule` | `#D4CFC4` | Hairline borders (footer rule) |
| `clay` | `#D2644A` | Focus rings and accent only |

Do not invent colours. Do not use clay as a fill for large CTAs on this site. If a filled control is required, use ink on paper.

## Mark

Circular GS monogram, path-based SVG, fill `#000000`. Transparent — no paper plate. `favicon.svg` is a copy of the tight mark.

The live header is **padded mark + `.wordmark` serif** (Iowan / Palatino / Georgia). Size the CSS box for optical ink diameter, not the padded viewBox — the padded fill is ≈ 54% of the box, so a `56×56` box reads as ~30px of ink. Keep clear-space padding in the SVG masters; do not rewrite viewBoxes to strip it. Tight is for the favicon only.

Letterhead / archive lockups use the **same serif wordmark system** as the site: the padded mark paths plus live SVG `<text>` (“Good Standing”) in Iowan / Palatino / Georgia, fill `#000000`, weight normal, tracking `0.02em`. Imagine’s traced sans letterforms are superseded. Do **not** drop either lockup SVG into the live header — the site header stays composed (padded mark `img` + CSS `.wordmark`).

| File | Use |
|---|---|
| `assets/logo/GoodStanding-mark-padded.svg` | Header combination mark (beside the CSS serif wordmark). Keep the padded viewBox. |
| `assets/logo/GoodStanding-mark-tight.svg` | Source for `favicon.svg` (tight crop fills 16×16). Not the live header. |
| `assets/logo/GoodStanding-lockup-horizontal.svg` | Archive / letterhead / email — padded mark left, serif “Good Standing” right. Not the live header. |
| `assets/logo/GoodStanding-lockup-stacked.svg` | Archive / letterhead / email — padded mark above, serif “Good Standing” below, centered. Not the live header. |

Locked combination-mark scale (optical: ink diameter ≈ wordmark capital height, mark ~1.0–1.2×). CSS gap is `gap-2` (8px) — the padded SVG already adds air on the right of the ink, so the flex gap stays tight.

| Piece | Size |
|---|---|
| `.brand` | `inline-flex items-center gap-2` |
| `.brand-mark` | `h-14 w-14` (56×56); HTML `width`/`height` 56; `object-contain` |
| `.wordmark` | `text-[1.625rem]`, tracking `0.02em`, weight normal |

Do not enlarge body, tagline, footer, or the Privacy nav to match. Header padding stays `py-6`. The brand link is not underlined — global ink underlines are `a:not(.brand)` so Preflight’s `text-decoration: inherit` stays in force on the lockup (Safari). Focus-visible on the brand is the clay outline only.

## Type

- Body: system sans (`system-ui`, `-apple-system`, Segoe UI, Roboto, Helvetica Neue, Arial).
- Wordmark only: `"Iowan Old Style"`, `"Palatino Linotype"`, Palatino, Georgia, `"Times New Roman"`, serif.
- Do not load webfonts on the holding site.

## Spacing

Letterhead calm. One stack system — never `[&_p]:m-0` (it beats child `mt-*` utilities, so intended gaps compute to 0).

- Homepage **main** is the single product tagline only, `max-w-[34rem]`. No audience line. No mailto or survey in main.
- If a stack of related lines is needed later, use a flex column with explicit `gap-y-*` (or margins that actually win). Do not zero paragraph margins and then fight them.
- Privacy is long-form: a little air after the `h1` before the “Last updated” meta, comfortable paragraph rhythm (`main p` in `src/input.css`), and a clear `mt-10` before each `h2`. Do not rewrite legal copy to “fix” spacing.

Header (`py-6`) and footer (`pt-8 pb-7`) stay in balance with the letterhead; do not inflate them unless the main column has made them look tight.

## Footer

Legal block first, unchanged in substance: Good Standing Software Ltd, company number (Companies House link), registered office.

Then a quiet link row, labels only — not raw email or survey prose:

`Contact` · `Survey` · `Privacy`

| Label | Target |
|---|---|
| Contact | `mailto:info@goodstanding.co.uk` |
| Survey | `https://research.goodstanding.co.uk/r/Bz5oLN` (new tab: `target="_blank"` `rel="noopener noreferrer"`) |
| Privacy | `privacy/` (or `./` when already on that page) |

## Motion and decoration

- No gratuitous chrome, cards, gradients, or animation.
- Links are underlined (ink at ~55% decoration; full ink on hover).
- Focus-visible: 2px clay outline, 2px offset.
- Brand (mark + wordmark) and nav links are not underlined at rest.

## SEO and performance

- Keep the site static HTML + committed CSS. No application runtime, no tracking pixels, no analytics SDKs.
- Do not invent new schema.org types or a second JSON-LD graph. Existing Organisation / WebSite / WebPage markup on `main` is enough.
- Keep the single CSS preload already on the pages (`assets/site.css`). Do not invent extra preloads, font theatre, or speculative optimisations.

## Tally / research survey

The live research form is **[Bz5oLN](https://research.goodstanding.co.uk/r/Bz5oLN)** (`research.goodstanding.co.uk`). It is the branded survey. Footer **Survey** opens it in a new tab; Contact (mailto) and Privacy stay same-tab.

Use the **same tokens** as this site — do not invent a second palette:

- Paper background, ink text, clay accent, ink buttons
- Inter or a system-like sans
- Tight radii

Document alignment here only. Do not drive Tally via API from this repo.

## Out of scope here

Further Lighthouse score campaigns, Tally API edits, and copy/SEO JSON-LD rewrites belong on their own changes. The existing stylesheet preload stays.
