# C4A Render

> [English](./README.md)

C4A Render 是 [C4A Context](https://github.com/context4ai/context) 的 Obsidian 渲染插件。它用于在 Obsidian 中阅读和检查 C4A 已编译出的知识工作区，让人和 AI Agent 使用同一份可追溯的知识内容。

## 它是什么

这个插件专注可视化增强，不负责采集、抽取、对齐或编译知识，也不会替代 [C4A Context](https://github.com/context4ai/context)。知识生产仍然通过 Context 和 AI Agent 完成；C4A Render 负责把生成后的 Markdown 与元数据在 Obsidian 里展示得更清楚。

## 可视化增强：在 Obsidian 中安装

当你已经有一个 C4A 知识工作区，或者准备在 Obsidian 中打开 C4A 工作区时，安装 C4A Render。

插件会渲染：

- Section 元数据标签：类型、版本范围、状态、溯源引用。
- 来源证据弹窗：点击 source_ref 查看原始记录。
- as-of 版本过滤：设置 as-of 后，按 Section 有效期显示可见状态。
- 关系图：查看已编译知识之间的本地关系。
- 状态与诊断：查看 C4A 工作区状态和 verify 诊断结果。

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
2. 在 Obsidian 中打开包含 `.context/` 的项目根目录作为 vault，然后创建目录：

   ```text
   .obsidian/plugins/c4a-render/
   ```

3. 将三个文件复制到该目录。
4. 重启 Obsidian。
5. 在 Obsidian 设置中启用 "C4A Render"。

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
5. 在 Obsidian 中打开包含或即将包含 `.context/` 的项目根目录作为 vault，通过 Claudian 运行 Context 流程，例如 `/context:init`、`/context:capture`、`/context:align`、`/context:compile`、`/context:query`。

这条链路能让 Obsidian 内的知识生产能力与常规 Claude + Context 使用方式保持一致；C4A Render 则负责 Obsidian 侧的阅读和可视化。

## 预期工作区结构

在 Obsidian 里应打开 `.context/` 的上层项目目录，不要直接打开 `.context/`，也不要只打开 `.context/knowledge/`。

预期结构：

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

C4A Render 会从 vault 根目录检查 `.context/config.yaml`、`.context/knowledge` 和 `.context/raw` 来识别工作区。

插件可以安装在普通 Obsidian vault 中，但只有当 vault 内存在 C4A 元数据时，C4A 专属面板才有实际内容可展示。

## 发布与兼容性

- Obsidian 插件 id：`c4a-render`
- Release 必需文件：`manifest.json`、`main.js`、`styles.css`
- 最低 Obsidian 版本：以 `manifest.json` 为准
- 桌面端和移动端：manifest 支持两端；部分较复杂视图取决于 Obsidian 运行环境能力。
- Alpha / beta 标签格式：`vX.Y.Z-alpha.N` 或 `vX.Y.Z-beta.N`
- GA 标签格式：`vX.Y.Z`
- Git tag 版本必须与 `manifest.json` 中的版本一致。

本仓库是 Obsidian 插件分发仓库。Release 产物由 C4A 项目生成；插件源码包不发布到 npm。

## 常见问题：.context 在 Obsidian 文件列表中不可见

Obsidian 的默认文件浏览器不显示以 `.` 开头的文件夹，因此 `.context/` 工作区目录不会出现在左侧文件树中。插件自身的面板（Status、Verify、Graph）不依赖文件树可见性，可正常使用。

当前版本建议在 `/context:init` 时选用不带 `.` 前缀的工作区名称（例如 `context/` 代替 `.context/`），该目录在 Obsidian 文件树中默认可见。未来将提供更完善的浏览支持。
