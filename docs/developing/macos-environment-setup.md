# macOS environment setup (Volta + uv)

## Homebrew rule

Use Homebrew only for CLI tools (git, uv, volta, pre-commit). Do **not** install language runtimes (node, python, ruby) with Homebrew.

## Path prioritization

Ensure `~/.volta/bin` and `~/.local/bin` appear **before** `/usr/bin` in your `PATH` so the Volta/uv shims are picked up instead of system binaries. The template in `configs/.zshrc-macos` includes the recommended ordering:

```
Volta -> Local Bin -> Homebrew -> System
```

## Node with Volta

Install Volta and let it manage Node/npm:

```
brew install volta
```

This repo pins Node and npm in `package.json` via Volta, so any `npm` command will pull the correct versions automatically. Avoid `sudo npm install -g` on macOS; Volta handles global tools without breaking permissions.

## Python with uv

Install uv and **never** use `/usr/bin/python3` (Apple's System Python):

```
brew install uv
uv venv
source .venv/bin/activate
```

## Apple Silicon optimization

Volta and uv ship native ARM64 binaries, which are significantly faster than Rosetta-translated tools on Apple Silicon.

## Docusaurus on macOS

`npm install` on macOS may require the Xcode command line tools for native modules (such as `fsevents`):

```
xcode-select --install
```
