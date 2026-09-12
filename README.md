# Yarra Biosystems — website

Static marketing site for Yarra Biosystems. No build step, no server-side code.

**Live:** https://yarrabio.com/ (also at https://savanh1.github.io/yarra-biosystems/)

## Making updates

Edit the files, then commit and push to `main`. That's it — pushing to `main`
triggers the GitHub Actions workflow in `.github/workflows/deploy.yml`, which
redeploys the site automatically. No manual publish step.

```bash
git add -A
git commit -m "Update copy on the applications page"
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
| `404.html` | Required — see below |
| `.nojekyll` | Required — see below |

### Three things not to change without care

- **`.nojekyll` must stay.** Without it, GitHub Pages runs Jekyll, which
  silently deletes folders whose names start with an underscore. That would
  remove `_ds/` and the site would render with no styling at all.
- **`404.html` must stay.** The site is one page; /news, /applications, /about
  and /contact are views, not files. GitHub Pages has nothing to serve at those
  paths, so it falls back to `404.html`, which forwards the path to
  `index.html`. Delete it and every direct link breaks — only the bare domain
  would work.
- **`index.html` must keep that name.** GitHub Pages serves `index.html` as the
  page at the site root. This file was originally exported from Claude Design as
  `Yarra Biosystems Site.dc.html`; if you re-open it there for editing, rename it
  back to `index.html` before committing.

## Worth knowing

- **The contact form relays through FormSubmit.** Submissions are POSTed to
  formsubmit.co, which emails them to hello@yarrabio.com. **Nothing is delivered
  until the form is activated:** the first submission sends an "Activate Form"
  email to that inbox, and its link must be clicked. Until then, and whenever
  FormSubmit is unreachable, visitors see an error pointing them to
  hello@yarrabio.com — the thank-you panel only appears once delivery is
  confirmed, so no message is dropped silently. After activation FormSubmit
  offers a random alias; swapping it in for the address in `CONTACT_ENDPOINT`
  keeps the inbox out of the form's endpoint.
- **The views share one file.** /applications, /science, /about, /news and
  /contact are states of `index.html`, not separate files. They *are* linkable —
  `404.html` forwards the path — but GitHub Pages answers with HTTP 404 before the
  redirect runs, so crawlers may only index the home page copy.
- **/approach still resolves.** The page was renamed to Applications; the old slug
  is kept in `PATH_TO_PAGE` so older links don't break. /applications is canonical.
- **React is loaded from unpkg.com at page load.** If that CDN is slow or blocked,
  visitors see a blank dark page. Worth hosting those two files in this repo if
  the site becomes business-critical.

## Custom domain

`yarrabio.com` is configured in the repo's **Settings → Pages**. Because this repo
deploys via GitHub Actions rather than from a branch, the domain lives in that
setting and there is deliberately **no `CNAME` file** in the repo — don't add one.
