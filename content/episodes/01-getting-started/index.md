+++
title = "Getting started"
weight = 10
teaching = 10
exercises = 10
questions = ["How do I start a lesson from this template?"]
objectives = ["Follow a reliable first-run order: setup, metadata update, then content replacement.", "Update the key `hugo.toml` fields so links, branding, and deployment targets are correct."]
keypoints = ["Start with `learners/setup`, then update `hugo.toml` before writing lesson content.", "Keep lesson prose local and framework behavior in the shared `hugo-styles` module.", "Use the shared `hugo-styles` docs after the local lesson skeleton is running."]
+++

Use this order for a clean start:

1. complete local setup checks
2. update lesson metadata in `hugo.toml`
3. replace sample pages with your own lesson material
4. use the shared docs when you need deeper authoring or deployment detail

For module refresh and `_vendor/` maintenance details, see
[Reference]({{< relref "/reference" >}}).

## 1. Run setup checks first

Before editing content, confirm your toolchain from the
[Setup]({{< relref "/learners/setup" >}}) page.

Then run:

```bash
hugo version
hugo server
```

This works without Go because the template commits a vendored module tree in `_vendor/`.

If setup is unclear, use the shared docs:

- [hugo-styles Quickstart](https://oer-particle-physics.github.io/hugo-styles/docs/quickstart/)
- [hugo-styles Troubleshooting](https://oer-particle-physics.github.io/hugo-styles/docs/troubleshooting/)

## 2. Update `hugo.toml` early

Edit these fields before replacing episode content:

| Field | Why it matters |
| --- | --- |
| `baseURL` | Production URL for deployed pages and canonical links, for example `https://<account>.github.io/<repo>/`. |
| top-level `title` | Site title used in browser/title surfaces. |
| `[params.lesson].title` | Lesson title shown in theme components. |
| `[params.lesson].tagline` and `description` | Homepage framing and metadata summary. |
| `[params.lesson].contact` | Contact shown in lesson metadata contexts. |
| `[params.lesson].repo` | Footer source link target in the UI. |
| `[params.lesson].editBranch` | Branch used for “Edit this page” links. |
| `[params.versioning]` | Controls whether the deployment workflow publishes only `Latest` or also archived branch/tag builds. |
| `[[menus.main]]` GitHub `url` | Top-nav GitHub link target. |

If you plan to deploy on GitHub Pages, enable Pages in the repository settings and choose
`GitHub Actions` as the source before the first push to `main`.
The included workflow deploys on pushes to `main`, so that first push should already have a configured Pages target.

{{< callout type="warning" title="Keep these as-is unless you know why" >}}
Do not remove the `module.imports` block that points to `github.com/oer-particle-physics/hugo-styles`.
That import is what provides the shared layouts, shortcodes, and supporting behavior.
{{< /callout >}}

## 3. Replace template content with your lesson

After metadata is set:

- replace this sample episode with your first real episode
- set episode order with front matter `weight` values (`10`, `20`, `30`, ...) rather than filename prefixes
- update `content/_index.md` homepage copy
- add or rename glossary/profile pages if needed

For structure and conventions, use:

- [Authoring Guide](https://oer-particle-physics.github.io/hugo-styles/docs/authoring/)
- [Components](https://oer-particle-physics.github.io/hugo-styles/docs/components/)
- [Front Matter](https://oer-particle-physics.github.io/hugo-styles/docs/frontmatter/)
- [Hextra Features for Physics Lessons](https://oer-particle-physics.github.io/hugo-styles/docs/hextra-features/)
- [Deployment](https://oer-particle-physics.github.io/hugo-styles/docs/deployment/)

{{< challenge title="Local vs shared" >}}
List two things that should stay in this lesson repository and two things that should stay in the shared module.

{{< hint >}}
Think about what is specific to your lesson topic versus what should stay reusable across many lessons.
{{< /hint >}}

{{< solution >}}
Lesson prose, schedule, glossary/profile content, and local branding should stay in this repository.
Shared layouts, pedagogy shortcodes, CSS/JS behavior, and validation tooling should stay upstream in `hugo-styles`.
{{< /solution >}}
{{< /challenge >}}

{{< instructor >}}
Point maintainers to the update guide early so they know updates should arrive through module version bumps:
[Updating Downstream Lessons](https://oer-particle-physics.github.io/hugo-styles/docs/updates/).
{{< /instructor >}}
