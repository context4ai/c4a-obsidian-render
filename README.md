# C4A Render

> [中文版本](./README_CN.md)

Obsidian render plugin for C4A knowledge workspaces.

## What It Is

C4A Render is the Obsidian companion plugin for [C4A Context](https://github.com/context4ai/context). It renders compiled C4A knowledge inside an Obsidian vault so people and AI agents can inspect the same source-linked knowledge base.

The plugin focuses on visualization. It does not extract source material, align sources, compile knowledge, or replace [C4A Context](https://github.com/context4ai/context). Knowledge production still happens through Context and an AI agent; C4A Render makes the resulting Markdown and metadata easier to read, verify, and navigate in Obsidian.

## Visualization Enhancement In Obsidian

Install C4A Render when you already have, or plan to open, a C4A knowledge workspace in Obsidian.

It renders:

- Section metadata chips: kind, version range, status, source reference.
- Source evidence popover: click a source_ref chip to inspect the raw record.
- As-of version filtering: set --as-of and sections show visibility by validity range.
- Graph views: local relationship views for compiled knowledge.
- Status and diagnostics: C4A workspace status and verify output when available.

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
2. Open the project root that contains `.context/` as your Obsidian vault, then create this directory:

   ```text
   .obsidian/plugins/c4a-render/
   ```

3. Copy the three files into that directory.
4. Restart Obsidian.
5. Enable "C4A Render" in Obsidian settings.

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
5. Open the project root that contains, or will contain, `.context/` as the Obsidian vault and run the Context workflow through Claudian, for example `/context:init`, `/context:capture`, `/context:align`, `/context:compile`, and `/context:query`.

This setup keeps knowledge production consistent with normal Claude-based Context usage, while C4A Render provides the Obsidian-side reading and visualization layer.

## Expected Vault Layout

Open the parent project directory as the Obsidian vault, not `.context/` itself and not only `.context/knowledge/`.

Expected layout:

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

C4A Render discovers the workspace from the vault root by checking `.context/config.yaml`, `.context/knowledge`, and `.context/raw`.

The plugin is safe to install in a normal Obsidian vault, but C4A-specific panels only become useful when C4A metadata is present.

## Release And Compatibility

- Obsidian plugin id: `c4a-render`
- Required release assets: `manifest.json`, `main.js`, `styles.css`
- Minimum Obsidian version: see `manifest.json`
- Desktop and mobile: supported by manifest; some richer views may depend on Obsidian runtime capability.
- Alpha and beta tags use `vX.Y.Z-alpha.N` or `vX.Y.Z-beta.N`.
- GA tags use `vX.Y.Z`.
- The tag version must match the `manifest.json` version.

This repository is the Obsidian distribution repository for the plugin. Release assets are generated from the C4A project; the source package is not published to npm.

## FAQ: .context not visible in the Obsidian file explorer

Obsidian's default file explorer does not display dot-prefixed folders, so the `.context/` workspace directory won't appear in the file tree. The plugin's own panels (Status, Verify, Graph) do not rely on file tree visibility and work normally.

In the current release, use a non-dot-prefixed workspace name at init time (for example `context/` instead of `.context/`). The directory then appears in the Obsidian file tree by default. Improved in-vault browsing support is planned for a future update.
