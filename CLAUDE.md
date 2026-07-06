# Claude Code guide for anthonyprajogo.com

## Project overview

Static HTML/CSS portfolio site for Anthony Prajogo. No build step, no frameworks, no JavaScript dependencies. All styling is inline per-page using CSS custom properties.

## Design system

- **Fonts:** Libre Baskerville (serif, headings/display), DM Sans (sans, body), DM Mono (mono, labels/meta)
- **Colour tokens:** defined in `:root` in each file — copy from any existing page
- **Panel layout:** every page wraps content in `.panel` (max-width 1400px, cream background, rounded corners, subtle shadow)
- **Nav:** consistent across all root-level pages; article pages in subfolders use `../` relative paths

## File structure

Every page lives at `[slug]/index.html` so GitHub Pages serves it without a `.html` extension in the URL (e.g. `/about/`, `/considered/oracles-and-insiders/`). The only exception is the root `index.html`, which serves `/`.

```
/
├── index.html
├── considered/
│   ├── index.html              # Article index with filter/sort JS
│   ├── images/                 # Shared images for Considered articles
│   ├── oracles-and-insiders/index.html
│   └── the-proxy-problem/index.html
├── macro/
│   ├── index.html              # Resource index with filter/sort JS
│   ├── images/                 # Shared images for Macro resources
│   ├── nz-political-system/index.html
│   ├── tech-roles-by-industry-light/index.html
│   ├── nz-job-market-light/index.html
│   ├── business-models-light/index.html
│   └── dining-trends/index.html
├── about/index.html
└── contact/index.html
```

**Linking convention:** all internal `href`/`src` values use absolute root-relative paths (e.g. `href="/considered/"`, `src="/macro/images/foo.jpg"`), never `../` relative paths or bare filenames. This avoids recalculating path depth as pages get nested, and works because the site has a fixed custom domain. Never link to `index.html` directly — link to the folder (e.g. `href="/"` not `href="/index.html"`), otherwise the `.html`/filename shows in the address bar.

## Adding a new Considered article

1. Create `considered/[slug]/index.html` using `considered/oracles-and-insiders/index.html` as the template
2. Add an `.article-card` entry in `considered/index.html` — follow the data-tags/data-date/data-name pattern in the comments
3. Add a `.featured-card` entry in `index.html` if it should be featured on the home page
4. Tags: `tag-systems`, `tag-strategy`, `tag-ideas`, `tag-world`, `tag-philosophy` (displayed as "Ground Truths"), `tag-favourites`

**Ordering rule:** the most recently published entry always goes first — in the `.featured-row` on `index.html`, and left-to-right/top-to-bottom in any hand-ordered card list. This is a manual convention (cards aren't sorted by script on `index.html`), so when adding a new piece, re-check the order of existing cards and move the newest to the front.

## Adding a new Macro resource

1. Create the resource at `macro/[slug]/index.html`
2. Add a `.macro-card` entry in `macro/index.html` — follow the data-tags/data-date/data-name pattern
3. Tags: `tag-tech`, `tag-finance`, `tag-governance`, `tag-trade`, `tag-society`

## Conventions

- No shared CSS file — styles are self-contained per page (deliberate, avoids cascade surprises)
- Never add comments explaining what code does — only add comments for non-obvious WHY
- No JavaScript frameworks; vanilla JS only where needed (filter/sort on index pages)
- Images: profile photo lives at root; article images are external URLs or TBD
- The `disclaimer` paragraph in financial articles uses `.article-disclaimer` class

## Custom domain

Domain: anthonyprajogo.com → configure via GitHub Pages settings + DNS CNAME record pointing to `anthonyprajogo.github.io`
