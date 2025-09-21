# Architecture

Last updated: 2025-09-21 00:00:00 UTC
Commit: b0f71fc

This repository is a static site / presentation built with a JavaScript web framework (see `package.json`). The codebase layout shows the following high-level pieces:

- `pages/` – site pages and routing.
- `components/` – React/Vue components used by pages.
- `assets/`, `styles/` – static assets and styling.
- `snippets/`, `slides.md` – content and presentation material.

Flow (simplified):

```mermaid
flowchart LR
  A["Source Markdown (slides.md)"] --> B["Pages & Components"]
  B --> C["Build (static site)"]
  C --> D["Deployed site (GitHub Pages / Netlify)"]
```

Notes:
- The project appears intended as a static presentation site; the build/deploy pipeline is configured for Netlify and GitHub Pages (`netlify.toml`, README links).

