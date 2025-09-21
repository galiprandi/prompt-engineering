# Stack

Last updated: 2025-09-21 17:35:29 UTC
Commit: 0fb5a83

Runtime and tooling used by the project (grounded in `package.json`):

- Node.js / pnpm as package manager (pnpm lock present).
- Vue 3 (`vue` dependency).
- Slidev (`@slidev/cli`, `slidev-theme-geist`, `@slidev/theme-default`) for slide authoring and serving.
- Playwright Chromium (`playwright-chromium`) present as devDependency (likely for automated export or testing workflows).

Common commands:

```
pnpm run dev   # start slidev dev server
pnpm run build # build static slides
pnpm run export# export slides to PDF/images
```

Notes:
- No backend runtime detected; this is a static presentation repository.
- All dependencies are listed in `package.json` and locked in `pnpm-lock.yaml`.

