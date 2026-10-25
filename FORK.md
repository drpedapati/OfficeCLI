# ClinVision fork of OfficeCLI

This is a fork of [iOfficeAI/OfficeCLI](https://github.com/iOfficeAI/OfficeCLI) (Apache License 2.0).
The `clinvision` branch is based on upstream tag `v1.0.158` (`2900a4a2`). ClinVision vendors this
branch as source and builds it itself; it never installs an upstream release binary.

ClinVision handles clinical data, so this build must not reach the network, change itself, or run
other programs on its own. Each change below is marked in its file with a `ClinVision fork` comment.

| Change | File |
|---|---|
| Never checks for, downloads, or applies an update (`UpdateChecker.OfflineBuild`). | `src/officecli/Core/UpdateChecker.cs` |
| Image URLs are refused; files and data URIs still work. | `src/officecli/Core/ImageSource.cs` |
| File URLs (CSV data, media) are refused. | `src/officecli/Core/FileSource.cs` |
| Mermaid rendering is off: it loads its script from `d.officecli.ai` or jsDelivr and starts a browser. | `src/officecli/Core/Diagram/MermaidImageRenderer.cs` |
| Plugin discovery finds nothing, so no external `officecli-*` program is started. | `src/officecli/Core/Plugins/PluginRegistry.cs` |
| The Word preview never adds the web-font loader (it sent document font names to `d.officecli.ai` / Google Fonts). | `src/officecli/Handlers/Word/WordHandler.HtmlPreview.cs` |
| `view html` output carries a Content-Security-Policy that blocks every network load; only inline content and data URIs render. | `src/officecli/CommandBuilder.View.cs` |

Not changed, and not used by ClinVision: `watch` (local HTTP preview server), `mcp` and its
installers, `view screenshot` (starts a browser), and resident mode (set
`OFFICECLI_NO_AUTO_RESIDENT=1`).

To move to a newer upstream release: merge the upstream tag into `clinvision`, keep every change in
the table, rebuild, and re-run ClinVision's OfficeCLI checks before re-vendoring.
