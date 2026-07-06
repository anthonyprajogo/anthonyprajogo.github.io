# anthonyprajogo.com

Personal portfolio site for Anthony Prajogo — writer and consultant.

Hosted on GitHub Pages at [anthonyprajogo.github.io](https://anthonyprajogo.github.io), served on the custom domain [anthonyprajogo.com](https://anthonyprajogo.com).

## Structure

Every page is `[slug]/index.html` so URLs have no `.html` extension (e.g. `/about/`, `/considered/oracles-and-insiders/`).

```
/
├── index.html           # Home (the one page not in its own folder)
├── considered/
│   ├── index.html       # Considered index
│   └── [slug]/index.html  # One folder per essay
├── macro/
│   ├── index.html       # Macro index
│   └── [slug]/index.html  # One folder per resource
├── about/index.html
└── contact/index.html
```

See [CLAUDE.md](CLAUDE.md) for the full linking convention and how to add new content.

## Development

Static HTML/CSS site — no build step. Open any `.html` file directly in a browser, or use a local server:

```bash
npx serve .
```

## Deployment

Push to `main` branch. GitHub Pages deploys automatically from the repo root.
