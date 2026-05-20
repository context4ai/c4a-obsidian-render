# C4A Render

> [English](./README.md)

C4A Render 是 [C4A Context](https://github.com/context4ai/context) 的 Obsidian 渲染插件。它用于在 Obsidian 中阅读和检查 C4A 已编译出的知识工作区，让人和 AI Agent 使用同一份可追溯的知识内容。

## 它是什么

这个插件专注可视化增强，不负责采集、抽取、对齐或编译知识，也不会替代 [C4A Context](https://github.com/context4ai/context)。知识生产仍然通过 Context 和 AI Agent 完成；C4A Render 负责把生成后的 Markdown 与元数据在 Obsidian 里展示得更清楚。

## 可视化增强：在 Obsidian 中安装

当你已经有一个 C4A 知识工作区，或者准备在 Obsidian 中打开 C4A 工作区时，安装 C4A Render。

插件会渲染：

- 知识 Markdown 中的 Section 元数据标签：类型、版本范围、状态、溯源引用、关系等。
- Obsidian 命令面板中的 C4A Workspace 工作台：总览、节点浏览、Force 图、问题检查、as-of、预览链接样式等。

当前版本不会额外增加左侧 ribbon 图标或独立工具栏按钮；主要可见增强会直接出现在生成后的知识 Markdown 文件里。

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

3. 将三个文件复制到该目录。
4. 重启 Obsidian。
5. 在 Obsidian 设置中启用 "C4A Render"。

### 安装后怎么用

1. 在 Obsidian 中打开 C4A 数据根目录作为 vault，知识文件在 `knowledge/` 下。
2. 打开生成后的知识 Markdown 文件。
3. 将 Markdown 切到阅读视图或预览模式。
4. C4A Render 会自动为 C4A section 增加小标签，例如 `spec`、版本范围、`source_ref`、状态、关系等。
5. 按 `Cmd+P`（macOS）或 `Ctrl+P`（Windows/Linux）打开 Obsidian 命令面板，输入 `C4A`，运行 `C4A: 查看工作区`。

C4A Workspace 会打开一个统一工作台。你可以在里面看到工作区总览、版本视图设置、节点和关系筛选、Force 关系图、渲染问题检查；点击节点会打开对应知识文件。

如果 C4A 面板找不到工作区，它会显示探测到的状态和预期目录结构，不会再只显示空标题。

## 知识生产：在 Obsidian 中使用 Context 生产

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

## 预期工作区结构

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

## 发布与兼容性

- Obsidian 插件 id：`c4a-render`
- Release 必需文件：`manifest.json`、`main.js`、`styles.css`
- 最低 Obsidian 版本：以 `manifest.json` 为准
- 桌面端和移动端：manifest 支持两端；部分较复杂视图取决于 Obsidian 运行环境能力。
- Alpha / beta 标签格式：`vX.Y.Z-alpha.N` 或 `vX.Y.Z-beta.N`
- GA 标签格式：`vX.Y.Z`
- Git tag 版本必须与 `manifest.json` 中的版本一致。

本仓库是 Obsidian 插件分发仓库。Release 产物由 C4A 项目生成；插件源码包不发布到 npm。
