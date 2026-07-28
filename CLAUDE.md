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
│   ├── dining-trends/index.html
│   └── electricity-industry/index.html
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

1. Create the resource at `macro/[slug]/index.html`, using an existing resource (e.g. `macro/nz-political-system/index.html`) as the template for the site-standard nav/header/footer — not the bespoke chrome of whichever source file you're starting from
2. Add a `.macro-card` entry in `macro/index.html` — follow the data-tags/data-date/data-name pattern
3. Tags: `tag-tech`, `tag-finance`, `tag-governance`, `tag-trade`, `tag-society`. Each card takes **1-3 tags** (comma-separated in `data-tags`, one `.macro-tag` span per tag) — only tag a piece with a theme it substantively covers, don't stuff tags to widen reach. The filter matches on ANY tag present (OR, not AND).
4. Add `meta name="description"` + Open Graph (`og:title`, `og:description`, `og:image`, `og:url`) + `twitter:card` tags to the page `<head>` — every page on the site carries these now. `og:image` should be the resource's own card thumbnail (from `macro/images/`); fall back to `/anthony-prajogo-profile-picture.jpg` only if there's no dedicated image.
5. If you add a thumbnail image for the card, name it descriptively (e.g. `electricity-transmission-towers-sunset.jpg`, not `IMG_1234.jpg`) and keep it in the same size/weight range as the existing images in `macro/images/` (~1000px wide, well under 100KB).

## Conventions

- No shared CSS file — styles are self-contained per page (deliberate, avoids cascade surprises)
- Never add comments explaining what code does — only add comments for non-obvious WHY
- No JavaScript frameworks; vanilla JS only where needed (filter/sort on index pages)
- Images: profile photo lives at root; article images are external URLs or TBD
- The `disclaimer` paragraph in financial articles uses `.article-disclaimer` class
- Every page carries `meta name="description"` + Open Graph tags (`og:type`, `og:title`, `og:description`, `og:image`, `og:url`) + `twitter:card` in `<head>`, so links shared on LinkedIn/Slack/etc. render a proper preview. `og:image` defaults to `/anthony-prajogo-profile-picture.jpg` for pages without a dedicated hero/thumbnail image. New pages should follow this pattern from the start.

## Testing a new or changed page

Always check mobile and tablet, not just desktop, before calling a page done — this site has no build step to catch layout bugs, and a couple of them are invisible unless you specifically go looking:

- **Check real breakpoints, not just "does it look fine on desktop."** At minimum test ~375px, ~428px, ~768px, and ~1024px widths (a quick Playwright script driving headless Chromium works — `/opt/pw-browsers/chromium-*/chrome-linux/chrome` is preinstalled in Claude Code's sandboxed environments). Block `fonts.googleapis.com` requests in the test (`page.route(...).abort()`) — the sandbox has no real internet access, and waiting on that request can make `page.goto` hang or time out.
- **`document.documentElement.scrollWidth > innerWidth` is not sufficient on its own.** `.panel` uses `overflow: hidden`, which silently *clips* overflowing descendant content instead of producing a document-level horizontal scrollbar — so this check can report "no overflow" while text is genuinely being cut off inside the panel. Also check individual elements likely to be wide (anything with `white-space: nowrap`, grids/flex rows with several items) via `element.scrollWidth` vs. its own `clientWidth`, or by comparing `getBoundingClientRect().right` against `.panel`'s right edge.
- **Take an actual screenshot at each breakpoint and look at it.** Automated geometry checks miss things a human glance catches instantly (e.g. clipped mid-word text still technically "fits" in scrollWidth terms if a sibling with `overflow:hidden` cropped it first).
- **Watch for the flex-item-shrink-to-fit trap when nesting old content into `.panel`.** `.panel` is `display:flex; flex-direction:column`. Any direct (or wrapped) child that still carries its own `max-width: Npx; margin: 0 auto;` (a pattern meant for centering inside a plain `<body>`) will size itself via shrink-to-fit instead of stretching to the panel's width once it's a flex item — and if any descendant has unbreakable content (`white-space: nowrap` labels, wide grid tracks), the whole subtree can balloon past the viewport width with no visible page-level scrollbar (silently clipped, per the point above). Fix by dropping the redundant `margin: 0 auto` (and usually the `max-width` too) from content once it lives inside `.panel` — the panel already centers and caps the width.

## Custom domain

Domain: anthonyprajogo.com → configure via GitHub Pages settings + DNS CNAME record pointing to `anthonyprajogo.github.io`
