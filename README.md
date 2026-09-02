# Good Standing

Static holding site for [goodstanding.co.uk](https://goodstanding.co.uk), served from GitHub Pages.

There is no application runtime. Pages are committed HTML plus a built stylesheet.

## Styles

Tailwind CSS is compiled to `assets/site.css` and committed, so GitHub Pages does not need a build step or the Play CDN.

Tokens live in `src/input.css`. After changing tokens or HTML classes:

```bash
npm install
npm run build:css
```

Use `npm run watch:css` while editing.
