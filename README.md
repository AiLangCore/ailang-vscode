# AiLang for Visual Studio Code

Visual Studio Code language support for AiLang `.aos`, `.aiproj`, `.aisvg`, and `.aisvg.aos` files.

This extension is intentionally a thin editor adapter:

- syntax highlighting through TextMate grammars
- bracket/comment/editor configuration
- snippets for common AiLang and AiSVG node forms
- future commands that call the AiLang CLI for check/format/run

It must not define AiLang or AiSVG semantics. Semantic authority remains in AiLang/AiVectra and the normative specs in the main repositories:

- `SPEC/IL.md`
- `SPEC/EVAL.md`
- `SPEC/VALIDATION.md`
- `SPEC/AISVG.md`

## Features

Current scaffold:

- `.aos` and `.aiproj` file association
- `.aisvg` file association for AiSVG documents
- `*.aisvg.aos` file association for AiSVG documents written with AOS-style nesting
- highlighting for core AiLang node kinds from the current IL spec
- highlighting for common AiSVG node kinds such as `AiSVG`, `Group`, `Path`, `Rect`, `Circle`, `LinearGradient`, and `Stop`
- highlighting for XML-like SVG tags and attributes in `.aisvg` files
- nested AiLang/AOS highlighting inside `{{ ... }}` regions in AiSVG files
- highlighting for common attributes such as `name`, `value`, `target`, `path`, `package`, `sdk`, `params`, `async`, `fill`, `stroke`, `viewBox`, `d`, `transform`, and gradient stop attributes
- highlighting for strings, numbers, booleans, null, comments, dotted syscall/call targets, SVG dimensions, and color literals
- language configuration for brackets, comments, surrounding pairs, and folding markers
- snippets for `Program`, `Import`, `Export`, `Let`, `Fn`, `Call`, `If`, `Return`, `Project`, `Include`, plus common AiSVG document/node forms

## AiSVG file types

- `.aisvg` — AiSVG documents, including XML-like SVG syntax and AiSVG node syntax
- `.aisvg.aos` — AiSVG documents using AiLang/AOS-style nesting

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

## Architecture rule

The extension may recognize tokens for editor presentation, but validation, formatting, evaluation, and diagnostics must come from the AiLang toolchain.

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
3. Open a `.aos`, `.aisvg`, or `.aisvg.aos` file.

## File types

- `.aos` — AiLang source files
- `.aiproj` — AiLang project manifest files
- `.aisvg` — AiSVG documents
- `.aisvg.aos` — AiSVG documents using AOS-style nesting

## License

MIT
