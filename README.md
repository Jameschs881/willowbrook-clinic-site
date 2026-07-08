# Willowbrook Family Clinic

A responsive, single-page marketing site for a fictional family medical clinic — built with plain HTML, CSS, and vanilla JavaScript (no build step, no dependencies).

**Live site:** https://jameschs881.github.io/willowbrook-clinic-site/

## Features

- Sticky, responsive navbar with a mobile hamburger menu
- Hero section with a stats card and trust indicators
- Services grid, patient testimonials, and a contact/appointment section
- Client-side form validation (name, email, phone) with accessible error messaging and focus management
- Scroll-triggered fade-in animations via `IntersectionObserver`
- Semantic HTML with skip links, `aria-live` regions, and visible focus states for keyboard users

## Project structure

```
index.html   # entire site — markup, styles, and scripts in one file
```

## Local development

No build tools required. Just open the file directly in a browser:

```bash
# from the project root
start index.html      # Windows
open index.html        # macOS
```

Or serve it locally for a closer-to-production experience:

```bash
npx serve .
```

## Deployment

The site auto-deploys to GitHub Pages on every push to `main` via the workflow at [`.github/workflows/pages.yml`](.github/workflows/pages.yml), using `actions/upload-pages-artifact` and `actions/deploy-pages`.

## Notes

The appointment form does not submit to a real backend — on success it logs the payload to the browser console. Wire up the `fetch(...)` call marked `API HOOK` in `index.html` to connect it to a real endpoint.
