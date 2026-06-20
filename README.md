# AiLang for Visual Studio Code

Visual Studio Code support for the AiLang ecosystem.

This extension provides syntax highlighting, snippets, file associations, and editor support for:

- `.aos` — AiLang source files
- `.aiproj` — AiLang project files
- `.aisvg` — AiSVG view files
- `.aisvg.aos` — AiLang/AOS code-behind files for AiSVG views

## AiLang

AiLang is an open-source programming language designed for deterministic, AI-native software development.

The AiLang Core ecosystem includes:

- **AiLang** — language and tooling
- **AiVM** — deterministic virtual machine
- **AiVectra** — cross-platform vector UI framework
- **AiSVG** — AI-friendly vector UI markup

Website:

```text
https://ailang.codes
```

GitHub:

```text
https://github.com/AiLangCore
```

## Features

- `.aos` and `.aiproj` file association
- `.aisvg` file association for AiSVG view documents
- `*.aisvg.aos` code-behind files remain AiLang/AOS files because they end in `.aos`
- VS Code Explorer nests `*.aisvg.aos` under the matching `*.aisvg` file
- highlighting for core AiLang node kinds from the current IL spec
- highlighting for common AiSVG node kinds such as `AiSVG`, `Group`, `Path`, `Rect`, `Circle`, `LinearGradient`, and `Stop`
- highlighting for XML-like SVG tags and attributes in `.aisvg` files
- nested AiLang/AOS highlighting inside `{{ ... }}` regions in AiSVG files
- highlighting for common attributes such as `name`, `value`, `target`, `path`, `package`, `sdk`, `params`, `async`, `fill`, `stroke`, `viewBox`, `d`, `transform`, and gradient stop attributes
- highlighting for strings, numbers, booleans, null, comments, dotted syscall/call targets, SVG dimensions, and color literals
- language configuration for brackets, comments, surrounding pairs, and folding markers
- snippets for `Program`, `Import`, `Export`, `Let`, `Fn`, `Call`, `If`, `Return`, `Project`, `Include`, plus common AiSVG document/node forms

## Installing AiLang

Install the AiLang toolchain from the main repository:

```bash
git clone https://github.com/AiLangCore/AiLang.git
cd AiLang
./build.sh
```

Verify the toolchain:

```bash
ailang --version
```

For current documentation and install notes, visit:

```text
https://ailang.codes
```

## Quick start

Create or open an AiLang project, then use the extension with `.aos`, `.aiproj`, and `.aisvg` files.

Build from the command line:

```bash
ailang build .
```

Run from the command line:

```bash
ailang run .
```

## AiSVG file pairing

AiSVG uses a view/code-behind pairing:

```text
MainView.aisvg
└── MainView.aisvg.aos
```

- `MainView.aisvg` is the AiSVG view file.
- `MainView.aisvg.aos` is the AiLang/AOS code-behind file for that view.
- The VS Code Explorer displays the code-behind file nested under the matching view file.
- The code-behind file is not a separate AiSVG document type; it is AiLang/AOS attached to an AiSVG view.

The extension contributes this default file nesting rule:

```json
{
  "explorer.fileNesting.patterns": {
    "*.aisvg": "${capture}.aisvg.aos"
  }
}
```

## AiSVG embedded expressions

AiSVG files may embed AiLang/AOS expressions using `{{ ... }}`. The extension delegates those embedded regions to the existing AiLang TextMate grammar for editor highlighting only.

Example:

```aos
AiSVG(width=800 height=600 viewBox="0 0 800 600") {
	Defs {
		LinearGradient(id="sky" x1="0%" y1="0%" x2="0%" y2="100%") {
			Stop(offset="0%" stopColor="#8fd3ff")
			Stop(offset="100%" stopColor="#ffffff")
		}
	}

	Rect(x=0 y=0 width=800 height=600 fill="url(#sky)")

	Text(x=40 y=80 fontSize=32 fill="#1f2937") {
		{{ Call(target=weather.currentSummary) }}
	}
}
```

## Support AiLang

AiLang is an independent open-source project. If AiLang helps your work, please consider supporting ongoing development.

Sponsor the project:

```text
https://github.com/sponsors/AiLangCore
```

Sponsorship helps fund:

- language development
- AiVM runtime work
- AiVectra and AiSVG UI development
- documentation
- testing and CI infrastructure
- examples and developer tooling

## Architecture rule

This extension is intentionally a thin editor adapter. It may recognize tokens for editor presentation, but validation, formatting, evaluation, and diagnostics must come from the AiLang toolchain.

Good future behavior:

```text
VS Code command -> ailang check -> Problems panel
VS Code command -> ailang fmt -> replace document text
VS Code command -> ailang run -> terminal output
```

Bad behavior:

```text
VS Code extension implements semantic validation itself
VS Code extension decides module resolution rules
VS Code extension formats using separate semantic rules
```

## Development

Install dependencies:

```bash
npm install
```

Package locally:

```bash
npx vsce package
```

Run the extension from VS Code:

1. Open this repository in VS Code.
2. Press `F5` to launch an Extension Development Host.
3. Open a `.aos` or `.aisvg` file.

## Publishing

This repository includes a GitHub Actions workflow that publishes the extension to the Visual Studio Marketplace when a GitHub release is published.

Required repository secret:

```text
VSCE_PAT
```

Manual local publish:

```bash
npx vsce package
npx vsce publish
```

Release publish flow:

1. Update `package.json` version.
2. Commit and push the change.
3. Create and publish a GitHub release.
4. The `Publish VS Code Extension` workflow publishes the extension using `VSCE_PAT`.

## File types

- `.aos` — AiLang source files
- `.aiproj` — AiLang project manifest files
- `.aisvg` — AiSVG view documents
- `.aisvg.aos` — AiLang/AOS code-behind files for matching `.aisvg` view documents

## Links

- Website: `https://ailang.codes`
- GitHub organization: `https://github.com/AiLangCore`
- Sponsorship: `https://github.com/sponsors/AiLangCore`

## License

MIT
