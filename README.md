# C4A Render

> [中文版本](./README_CN.md)

Obsidian render plugin for C4A knowledge workspaces.

## What It Is

C4A Render is the Obsidian companion plugin for [C4A Context](https://github.com/context4ai/context). It renders compiled C4A knowledge inside an Obsidian vault so people and AI agents can inspect the same source-linked knowledge base.

The plugin focuses on visualization. It does not extract source material, align sources, compile knowledge, or replace [C4A Context](https://github.com/context4ai/context). Knowledge production still happens through Context and an AI agent; C4A Render makes the resulting Markdown and metadata easier to read, verify, and navigate in Obsidian.

## Visualization Enhancement In Obsidian

Install C4A Render when you already have, or plan to open, a C4A knowledge workspace in Obsidian.

It renders:

- Section metadata chips in rendered Markdown: kind, version range, status, source reference, and relations.
- A C4A Workspace command palette workbench for overview, node exploration, force graph, issue checks, as-of, and preview-link settings.

The current release does not add a separate ribbon icon or toolbar button. The main visible enhancement appears directly on generated knowledge Markdown files.

### Install With BRAT

1. Install the Obsidian BRAT plugin.
2. In BRAT, choose "Add Beta plugin".
3. Enter this repository: `context4ai/c4a-obsidian-render`.
4. Enable "C4A Render" in Obsidian settings.

### Manual Install

1. Download the release assets from this repository:
   - `manifest.json`
   - `main.js`
   - `styles.css`
2. Open the C4A data root itself as your Obsidian vault. Then create this directory inside that vault:

   ```text
   .obsidian/plugins/c4a-render/
   ```

3. Copy the three files into that directory.
4. Restart Obsidian.
5. Enable "C4A Render" in Obsidian settings.

### How To Use After Install

1. Open the C4A data root itself as the Obsidian vault. Knowledge files live under `knowledge/`.
2. Open a generated knowledge Markdown file.
3. Switch the Markdown file to Reading view or preview mode.
4. C4A Render automatically adds small chips for C4A section metadata, such as `spec`, version range, `source_ref`, status, and relations.
5. Open the Obsidian command palette with `Cmd+P` on macOS or `Ctrl+P` on Windows/Linux, type `C4A`, then run `C4A: 查看工作区`.

C4A Workspace opens one integrated workbench. It shows workspace overview, version-view settings, node and relation filters, a force-directed graph, and render issue checks. Clicking a node opens its knowledge file.

If a C4A panel cannot find a workspace, it will show the detected state and the expected layout instead of staying blank.

## Knowledge Production In Obsidian

To produce C4A knowledge from inside Obsidian, use Context through Claude and bridge that Claude environment into Obsidian with Claudian.

Recommended setup:

1. Install the Claude SDK locally, then complete login or configure an API token.
2. Install the Context CLI from [context4ai/context](https://github.com/context4ai/context):

   ```bash
   npm i -g @c4a/context-cli
   # or
   bun add -g @c4a/context-cli
   ```

3. Install the Context Claude plugin from [context4ai/context](https://github.com/context4ai/context):

   ```text
   /plugin marketplace add context4ai/context
   /plugin install context@context
   ```

4. Install Claudian in Obsidian and configure it to use the same local Claude SDK environment.
5. Open the C4A data root itself as the Obsidian vault and run the Context workflow through Claudian, for example `/context:init`, `/context:capture`, `/context:align`, `/context:compile`, and `/context:query`.

This setup keeps knowledge production consistent with normal Claude-based Context usage, while C4A Render provides the Obsidian-side reading and visualization layer.

## Expected Vault Layout

Prefer opening the C4A data root directly. Do not open only the `knowledge/` directory.

Recommended layout:

```text
c4a-data-root/
├── .obsidian/
│   └── plugins/
│       └── c4a-render/
├── config.yaml
├── knowledge/
└── raw/
```

If your C4A data root is the hidden `.context/` directory inside a project, Obsidian usually will not show that folder in the file tree. For that reason, opening the project root is not the first-choice layout. If you still need to open the project root, the layout looks like this:

```text
project-root/
├── .obsidian/
│   └── plugins/
│       └── c4a-render/
├── .context/
│   ├── config.yaml
│   ├── knowledge/
│   └── raw/
└── ...
```

Do not rename an existing project `.context/` directory to `context` only for Obsidian. If you use a non-hidden data-root directory such as `context/`, run Context commands from inside that directory so it behaves as root layout.

If your OS file picker hides `.context/`, show hidden files first. On macOS, press `Cmd+Shift+.` in the file picker. If that is still inconvenient, create a non-hidden symlink such as `context -> .context` and open the symlink as the vault; avoid renaming or moving the original `.context/` directory.

## Release And Compatibility

- Obsidian plugin id: `c4a-render`
- Required release assets: `manifest.json`, `main.js`, `styles.css`
- Minimum Obsidian version: see `manifest.json`
- Desktop and mobile: supported by manifest; some richer views may depend on Obsidian runtime capability.
- Alpha and beta tags use `vX.Y.Z-alpha.N` or `vX.Y.Z-beta.N`.
- GA tags use `vX.Y.Z`.
- The tag version must match the `manifest.json` version.

This repository is the Obsidian distribution repository for the plugin. Release assets are generated from the C4A project; the source package is not published to npm.
