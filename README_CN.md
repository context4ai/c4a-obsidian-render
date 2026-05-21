# C4A Render

> [English](./README.md)

C4A Render 是 [C4A Context](https://github.com/context4ai/context) 的 Obsidian 配套插件。它用于在 Obsidian 中阅读和检查 C4A 已编译出的可追溯知识工作区，让人和 AI Agent 使用同一份知识内容。

## 它做什么

C4A Render 是可视化层。它不负责采集、抽取、对齐或编译知识，也不会替代 [C4A Context](https://github.com/context4ai/context)。知识生产仍然通过 Context 和 AI Agent 完成；这个插件负责把生成后的 Markdown 与元数据在 Obsidian 里展示得更清楚。

插件会提供：

- 知识 Markdown 中的 Section 元数据标签：类型、版本范围、状态、溯源引用、关系等。
- 一个命令面板入口：`C4A: 查看工作区`，用于打开 C4A Workspace 工作台。
- 工作区总览：节点、边、Section、问题、数据根、知识文件等数量。
- 节点浏览：支持按包、类型、关系类型和文本筛选。
- Force 关系图：查看筛选后的图，或查看选中节点的邻域。
- 渲染问题检查，以及 as-of、预览链接样式等本地视图设置。

当前插件不会额外增加左侧 ribbon 图标。主要入口是命令面板；自动可见增强会直接出现在生成后的知识 Markdown 文件里。

## 安装

### 通过 BRAT 安装

1. 在 Obsidian 中安装 BRAT 插件。
2. 在 BRAT 中选择 "Add Beta plugin"。
3. 输入仓库：`context4ai/c4a-obsidian-render`。
4. 回到 Obsidian 设置中启用 "C4A Render"。

### 手动安装

1. 从本仓库的 Release 下载以下三个文件：
   - `manifest.json`
   - `main.js`
   - `styles.css`
2. 在 Obsidian 中打开 C4A 数据根目录作为 vault，然后在该 vault 内创建目录：

   ```text
   .obsidian/plugins/c4a-render/
   ```

3. 将三个 Release 文件复制到该目录。
4. 重启 Obsidian。
5. 在 Obsidian 设置中启用 "C4A Render"。

## 使用

1. 在 Obsidian 中打开 C4A 数据根目录作为 vault，知识文件在 `knowledge/` 下。
2. 打开生成后的知识 Markdown 文件。
3. 将 Markdown 切到阅读视图或预览模式。
4. C4A Render 会自动为 C4A Section 增加小标签，例如 `spec`、版本范围、`source_ref`、状态、关系等。
5. 按 `Cmd+P`（macOS）或 `Ctrl+P`（Windows/Linux）打开 Obsidian 命令面板，输入 `C4A`，运行 `C4A: 查看工作区`。

C4A Workspace 工作台包含四个 Tab：

- **Overview**：工作区数量、视图设置、高度节点、包分布、关系类型分布。
- **Explorer**：可搜索、可筛选的节点列表；选中节点后展示元数据和关系。
- **Force**：筛选后的 Force 关系图，或选中节点的邻域图。
- **Issues**：从当前 vault 中检测到的渲染和元数据问题。

点击节点会在 Obsidian 中打开对应知识文件。

如果插件找不到 C4A 工作区，面板会显示探测到的状态和预期目录结构，不会只显示空标题。

## 工作区结构

推荐直接打开 C4A 数据根目录，不要只打开 `knowledge/` 目录。

推荐布局：

```text
c4a-data-root/
├── .obsidian/
│   └── plugins/
│       └── c4a-render/
├── config.yaml
├── knowledge/
└── raw/
```

如果你的 C4A 数据根是项目里的隐藏目录 `.context/`，Obsidian 的文件树通常不会显示它；因此不建议把项目根作为首选 vault。确实需要打开项目根时，结构如下：

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

不建议为了 Obsidian 把已有项目里的 `.context/` 改名成 `context/`。如果你确实使用 `context/` 这种非隐藏数据根目录，需要从这个目录内部运行 Context 命令，让它按 root layout 工作。

如果系统文件选择器默认隐藏 `.context/`，先显示隐藏文件。macOS 文件选择器里可以按 `Cmd+Shift+.`。如果仍然不方便，可以创建一个非隐藏软链接，例如 `context -> .context`，然后把这个软链接作为 vault 打开；不要直接改名或移动原始 `.context/` 目录。

## 知识生产

如果希望直接在 Obsidian 中生产 C4A 知识，建议通过 Claude + Context 完成生产流程，再用 Claudian 把同一套 Claude 环境桥接到 Obsidian 内。

推荐配置：

1. 本地安装 Claude SDK，并完成登录，或完成 API Token 配置。
2. 从 [context4ai/context](https://github.com/context4ai/context) 安装 Context CLI：

   ```bash
   npm i -g @c4a/context-cli
   # 或
   bun add -g @c4a/context-cli
   ```

3. 从 [context4ai/context](https://github.com/context4ai/context) 安装 Context 的 Claude Plugin：

   ```text
   /plugin marketplace add context4ai/context
   /plugin install context@context
   ```

4. 在 Obsidian 中安装 Claudian，并配置它使用同一个本地 Claude SDK 环境。
5. 在 Obsidian 中打开 C4A 数据根目录作为 vault，通过 Claudian 运行 Context 流程，例如 `/context:init`、`/context:capture`、`/context:align`、`/context:compile`、`/context:query`。

这条链路能让 Obsidian 内的知识生产能力与常规 Claude + Context 使用方式保持一致；C4A Render 则负责 Obsidian 侧的阅读和可视化。

## 发布

- Obsidian 插件 id：`c4a-render`
- Release 必需文件：`manifest.json`、`main.js`、`styles.css`
- 最低 Obsidian 版本：以 `manifest.json` 为准
- 桌面端和移动端：manifest 支持两端；部分复杂视图取决于 Obsidian 运行环境能力。
- Alpha / beta 标签格式：`vX.Y.Z-alpha.N` 或 `vX.Y.Z-beta.N`
- GA 标签格式：`vX.Y.Z`
- Git tag 版本必须与 `manifest.json` 中的版本一致。

本仓库是 Obsidian 插件分发仓库。Release 产物由 C4A 项目生成；插件源码包不发布到 npm。
