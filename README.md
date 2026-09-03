# Good Standing

Static holding site for [goodstanding.co.uk](https://goodstanding.co.uk), served from GitHub Pages.

Visual tokens, spacing, and naming conventions live in [DESIGN.md](DESIGN.md). Logo masters live in `assets/logo/`. The header uses the padded circular mark plus the CSS serif wordmark — not a lockup SVG. Archive horizontal/stacked lockups use the same padded mark and serif wordmark for letterhead / email. The favicon is the tight crop of the same mark.

There is no application runtime. Pages are committed HTML plus a built stylesheet.

## Styles

Tailwind CSS is compiled to `assets/site.css` and committed, so GitHub Pages does not need a build step or the Play CDN.

Tokens live in `src/input.css`. After changing tokens or HTML classes:

```bash
npm install
npm run build:css
```

Use `npm run watch:css` while editing.
