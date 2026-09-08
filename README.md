# Good Standing

Static holding site for [goodstanding.co.uk](https://goodstanding.co.uk), served from GitHub Pages.

Visual tokens, spacing, naming, and voice live in `.cursor/rules/` (`brand.mdc`, `design.mdc`). Membership-body CRM research lives in `crm.mdc`. The header uses `assets/logo.svg` plus the CSS serif wordmark. `favicon.svg` is a copy of the same mark. `assets/logo.png` is a 512×512 transparent raster of that mark for the GitHub org/profile avatar. `assets/logo-white.png` is the same 512×512 mark on paper `#F4F1EA`. `assets/og.png` is the social preview image.

There is no application runtime. Pages are committed HTML plus a built stylesheet.

## Styles

Tailwind CSS is compiled to `assets/site.css` and committed, so GitHub Pages does not need a build step or the Play CDN.

Tokens live in `src/input.css`. After changing tokens or HTML classes:

```bash
npm install
npm run build:css
```

Use `npm run watch:css` while editing.
