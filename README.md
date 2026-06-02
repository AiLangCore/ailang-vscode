# AiLang for Visual Studio Code

Visual Studio Code language support for AiLang `.aos` and `.aiproj` files.

This extension is intentionally a thin editor adapter:

- syntax highlighting through a TextMate grammar
- bracket/comment/editor configuration
- snippets for common AiLang node forms
- future commands that call the AiLang CLI for check/format/run

It must not define AiLang semantics. Semantic authority remains in AiLang and the normative specs in the main repository:

- `SPEC/IL.md`
- `SPEC/EVAL.md`
- `SPEC/VALIDATION.md`

## Features

Current scaffold:

- `.aos` and `.aiproj` file association
- highlighting for core node kinds from the current IL spec
- highlighting for common attributes such as `name`, `value`, `target`, `path`, `package`, `sdk`, `params`, and `async`
- highlighting for strings, numbers, booleans, null, comments, and dotted syscall/call targets
- language configuration for brackets, comments, surrounding pairs, and folding markers
- snippets for `Program`, `Import`, `Export`, `Let`, `Fn`, `Call`, `If`, `Return`, `Project`, and `Include`

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
3. Open a `.aos` file.

## File types

- `.aos` — AiLang source files
- `.aiproj` — AiLang project manifest files

## License

MIT
