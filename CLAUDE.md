# Claude Code guide for anthonyprajogo.com

## Project overview

Static HTML/CSS portfolio site for Anthony Prajogo. No build step, no frameworks, no JavaScript dependencies. All styling is inline per-page using CSS custom properties.

## Design system

- **Fonts:** Libre Baskerville (serif, headings/display), DM Sans (sans, body), DM Mono (mono, labels/meta)
- **Colour tokens:** defined in `:root` in each file — copy from any existing page
- **Panel layout:** every page wraps content in `.panel` (max-width 1400px, cream background, rounded corners, subtle shadow)
- **Nav:** consistent across all root-level pages; article pages in subfolders use `../` relative paths

## File structure

```
/
├── index.html
├── considered.html     # Article index with filter/sort JS
├── macro.html          # Resource index with filter/sort JS
├── about.html
├── contact.html
├── considered/         # One HTML file per Considered essay
└── macro/              # One HTML file per Macro resource
```

## Adding a new Considered article

1. Create `considered/[slug].html` using `considered/oracles-and-insiders.html` as the template
2. Add an `.article-card` entry in `considered.html` — follow the data-tags/data-date/data-name pattern in the comments
3. Add a `.featured-card` entry in `index.html` if it should be featured on the home page
4. Tags: `tag-systems`, `tag-strategy`, `tag-ideas`, `tag-world`, `tag-philosophy`, `tag-favourites`

## Adding a new Macro resource

1. Place the HTML file in `macro/`
2. Add a `.macro-card` entry in `macro.html` — follow the data-tags/data-date/data-name pattern
3. Tags: `tag-tech`, `tag-finance`, `tag-governance`, `tag-trade`, `tag-society`

## Conventions

- No shared CSS file — styles are self-contained per page (deliberate, avoids cascade surprises)
- Never add comments explaining what code does — only add comments for non-obvious WHY
- No JavaScript frameworks; vanilla JS only where needed (filter/sort on index pages)
- Images: profile photo lives at root; article images are external URLs or TBD
- The `disclaimer` paragraph in financial articles uses `.article-disclaimer` class

## Custom domain

Domain: anthonyprajogo.com → configure via GitHub Pages settings + DNS CNAME record pointing to `anthonyprajogo.github.io`
