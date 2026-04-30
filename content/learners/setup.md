+++
title = "Setup"
weight = 10
+++

For normal lesson authoring in this template, you only need Hugo Extended.
The shared module is vendored in `_vendor/`, so local builds do not require Go.

{{< callout type="note" title="When Go is needed" >}}
Go is only needed if you maintain module updates locally
(for example, running `hugo mod tidy`, `hugo mod vendor`, or the migration checker).
{{< /callout >}}

{{< tabs >}}
{{< tab name="macOS" selected=true >}}
```bash
brew install hugo
```
{{< /tab >}}
{{< tab name="Linux" >}}
```bash
sudo apt install hugo
```
{{< /tab >}}
{{< tab name="Windows" >}}
```powershell
winget install Hugo.Hugo.Extended
```
{{< /tab >}}
{{< /tabs >}}

{{< tabs >}}
{{< tab name="bash" selected=true >}}
```bash
hugo version
```
{{< /tab >}}
{{< tab name="zsh" >}}
```zsh
hugo version
```
{{< /tab >}}
{{< tab name="fish" >}}
```fish
hugo version
```
{{< /tab >}}
{{< /tabs >}}

If you plan to update `_vendor/` locally, install Go and verify with `go version`.

## Next step

Once `hugo version` and `hugo server` work, return to
[Getting started]({{< relref "/episodes/01-getting-started" >}})
and continue with step 2: update `hugo.toml` before replacing the sample content.
