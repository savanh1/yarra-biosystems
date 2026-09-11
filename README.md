# Yarra Biosystems — website

Static marketing site for Yarra Biosystems. No build step, no server-side code.

**Live:** https://yarrabio.com/ (also at https://savanh1.github.io/yarra-biosystems/)

## Making updates

Edit the files, then commit and push to `main`. That's it — pushing to `main`
triggers the GitHub Actions workflow in `.github/workflows/deploy.yml`, which
redeploys the site automatically. No manual publish step.

```bash
git add -A
git commit -m "Update copy on the approach page"
git push
```

The deploy takes about a minute. You can watch it under the repo's **Actions**
tab; the green tick means it's live. A hard refresh (Ctrl+F5) helps if you still
see the old version in your browser.

## Layout

| Path | What it is |
|---|---|
| `index.html` | The whole site — all three pages live in this one file |
| `support.js` | Rendering runtime that turns `index.html` into the live page |
| `image-slot.js` | Component behind the `<image-slot>` image areas |
| `_ds/` | Design system: fonts, colours, spacing, base styles |
| `uploads/` | Site images, including team portraits |
| `logo.webp` | Brand wordmark, transparent background — used in the header and footer |
| `favicon.svg` | Browser tab icon |
| `.nojekyll` | Required — see below |

### Two things not to change without care

- **`.nojekyll` must stay.** Without it, GitHub Pages runs Jekyll, which
  silently deletes folders whose names start with an underscore. That would
  remove `_ds/` and the site would render with no styling at all.
- **`index.html` must keep that name.** GitHub Pages serves `index.html` as the
  page at the site root. This file was originally exported from Claude Design as
  `Yarra Biosystems Site.dc.html`; if you re-open it there for editing, rename it
  back to `index.html` before committing.

## Worth knowing

- **The contact form doesn't send anything.** Submitting shows a thank-you
  message, but there's no backend — nothing is emailed or stored. It needs a form
  service (Formspree, Netlify Forms, or similar) wired up before it collects real
  enquiries.
- **Home / Approach / Contact are not separate URLs.** They're one page swapping
  content, so they can't be linked to directly, and search engines mostly see the
  home page copy.
- **React is loaded from unpkg.com at page load.** If that CDN is slow or blocked,
  visitors see a blank dark page. Worth hosting those two files in this repo if
  the site becomes business-critical.

## Custom domain

`yarrabio.com` is configured in the repo's **Settings → Pages**. Because this repo
deploys via GitHub Actions rather than from a branch, the domain lives in that
setting and there is deliberately **no `CNAME` file** in the repo — don't add one.
