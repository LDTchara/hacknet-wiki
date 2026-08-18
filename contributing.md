# Hacknet.wiki 贡献指南

感谢你对 Hacknet.wiki 添砖加瓦！

目前处于开发早期阶段，建议以内容更新为主。

如果你想要在长时间内撰写某一部分，可以创建一个 Issue，Type 选择为 Task，方便交流和让大家知道你的工作。

如果你想在线编辑，推荐使用以下方法：
- 使用 VSCode 网页版: https://vscode.dev/github/UnHacknet/hacknet-wiki/ 。也可以在 https://github.com/UnHacknet/hacknet-wiki 页面点击键盘上的 <kbd>.></kbd> 键。
- 在 Stackblitz 中打开: https://pr.new/github/UnHacknet/hacknet-wiki/ 。相比于前者可以在浏览器启动 vitepress 环境从而实时预览。

## 贡献须知

Awesome-hacknet 请前往 https://github.com/UnHacknet/awesome-hacknet。

### 开发

Markdown 没有格式化的要求，但是代码有。对非 Markdown 内容进行变更后，记得在提交前使用 `pnpm run format` 格式化文件。

如果你要编辑 VitePress 的 `theme/index.mts` 与 `.vue` 组件，可能需要启用 Vue Language Support，比如在 VSCode 安装 [Vue 插件](https://marketplace.visualstudio.com/items?itemName=vue.volar)。

## Pull Request 基本要求

### 文档内容

- 内容简单清晰准确。如果是严肃内容（Extension-Tutorial 与 Reference），最好提供参考来源。
- 使用标准 Markdown 与 VitePress 内置的 [Markdown 扩展](https://vitepress.dev/zh/guide/markdown)。格式不做要求，符合 Markdown 标准即可，别太乱就行。

### 工程

- 最好不要提高贡献门槛
- 不进行非必要的功能更新，若有意愿且不确定，请先创建 Issues 咨询意见，防止浪费您的精力。
- 不要大规模改动格式

### PR 处理流程：

提交 PR → 检查 → 修改&测试（如需） → 合并
