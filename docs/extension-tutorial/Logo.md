# Logo

`BlankExtension` 和 `IntroExtension` 的根目录下都附带了一个 `Logo.png`，它就是显示在 Hacknet 扩展列表中的那个图标。

`MyFirstExtension` 是从 `BlankExtension` 复制而来的，根目录下也有一个 `Logo.png`。我们来试着把它换成自己的 Logo。

## 动手试试

准备一张自己的 Logo 图片，命名为 `Logo.png`，替换掉 `MyFirstExtension/Logo.png`。

保存后启动 Hacknet，打开 Extensions 页面，可以看到扩展的 Logo 已经更新。

> [!NOTE]
> Logo 图片必须是名为 `Logo.png` 的 PNG 文件。虽然源码也会检查 `Logo.jpg`，但由于一个 bug，实际上只有 `Logo.png` 能正常工作；如果把 GIF 图片改名为 `Logo.png`，可以渲染，但只会显示第一帧，没有动态效果。

## 参考

Logo 的完整加载机制见 [参考：Logo](../reference/Logo.md)。
