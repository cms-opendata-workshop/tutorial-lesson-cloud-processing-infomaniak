# Hugo Styles Template

This is the thin starter repository for lessons that use the shared `hugo-styles` module.

## What stays here

- lesson-specific content
- lesson metadata in `hugo.toml`
- local branding or small layout overrides
- vendored Hugo module files in `_vendor/` for no-Go local authoring
- synced maintainer workflows and helper scripts
- Dependabot configuration for module updates

## What stays upstream

- shared layouts
- pedagogy shortcodes
- shared CSS and JS
- aggregated resource pages
- migration and validation tooling

## First steps

1. Check local setup first.
   Install Hugo Extended, then run `hugo version` and `hugo server`.
   Go is optional for normal authoring because this template commits `_vendor/`.
2. Update `baseURL`, title, description, contact, and repository links in `hugo.toml`.
   `params.lesson.repo` powers the source and edit links in the page footer.
   `params.lesson.docsRepo` powers the default footer citation link and should usually match the lesson repository.
   `params.lesson.copyrightHolder` is used in the footer copyright line.
   The top-nav GitHub icon is configured separately in `[[menus.main]]`.
   If you will deploy on GitHub Pages, set `baseURL` to `https://<account>.github.io/<repo>/`.
   In the repository settings, enable Pages and choose `GitHub Actions` as the source before the first push to `main`.
   Versioned deployment is configured through `[params.versioning]`.
   By default the workflow publishes the default branch as `Latest`; add branch/tag refs or patterns only if you want archived versions too.
3. Replace the repository metadata placeholders.
   Update `CITATION.cff`, `AUTHORS`, and `LICENSE.md` so citation and licensing details match your lesson.
   If you keep the default footer citation link, it will point to the root `CITATION.cff` in your lesson repository.
4. Add or replace the sample content.
5. Use the shared docs in the `hugo-styles` repository or on its published site when you need deeper authoring, deployment, or update guidance:
   - [Quickstart](https://oer-particle-physics.github.io/hugo-styles/docs/quickstart/)
   - [Authoring Guide](https://oer-particle-physics.github.io/hugo-styles/docs/authoring/)
   - [Front Matter](https://oer-particle-physics.github.io/hugo-styles/docs/frontmatter/)
   - [Components](https://oer-particle-physics.github.io/hugo-styles/docs/components/)
   - [Deployment](https://oer-particle-physics.github.io/hugo-styles/docs/deployment/)
   - [Versioned Sites](https://oer-particle-physics.github.io/hugo-styles/docs/versioned-sites/)

## Why `_vendor/` is committed

This repository commits `_vendor/` so authors can run local lesson builds without installing Go.
As long as `go.mod`, `go.sum`, and `_vendor/` are in sync, `hugo server` works with Hugo Extended alone.

For local rendered-site link checks, install
[lychee](https://lychee.cli.rs/guides/getting-started/)
and run:

```bash
python3 scripts/build-versioned-site.py --base-url / --destination .cache/linkcheck-site --no-minify
lychee --cache --config lychee.toml --no-progress --root-dir .cache/linkcheck-site '.cache/linkcheck-site/**/*.html'
```

The GitHub Pages workflow runs the same validation build plus `lychee` on pull requests and on pushes to `main`.

## Updating shared module versions

### Preferred: GitHub Actions (no local Go required)

Run the **Refresh vendored Hugo modules** workflow from the Actions tab.
It bumps `github.com/oer-particle-physics/hugo-styles` to the latest release, re-syncs the managed maintainer files from that exact pinned module version, refreshes `_vendor/`, and then opens a PR if anything changed.

Dependabot still manages `gomod` version discovery and can trigger update PRs on its normal schedule.

### Local maintainer path (Go available)

```bash
hugo mod get -u github.com/oer-particle-physics/hugo-styles@latest
hugo mod tidy
./scripts/sync-template-files.sh
hugo mod vendor
hugo --gc --minify
```

Commit these files together:

- `.github/workflows/pages.yml`
- `.github/workflows/refresh-vendored-modules.yml`
- `.github/workflows/reusable-pages.yml`
- `.github/workflows/reusable-refresh-vendored-modules.yml`
- `go.mod`
- `go.sum`
- `lychee.toml`
- `scripts/build-versioned-site.py`
- `scripts/sync-template-files.sh`
- `_vendor/`

The synced workflow files stay intentionally thin: the triggers live in this repository, while the canonical workflow logic is maintained upstream in `hugo-styles` and copied into the managed reusable workflow files during sync.

For local co-development with a sibling `hugo-styles` checkout, use a temporary local `replace` or `go.work`
setup while testing, but do not commit that override to the template repository.
