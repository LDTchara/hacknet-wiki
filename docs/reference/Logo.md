# Logo

除了 `ExtensionInfo.xml`，扩展根目录下还可以包含一个 Logo 图片，由游戏加载，显示在 Hacknet 的扩展列表中。

游戏加载扩展时会依次检查 `Logo.png`、`Logo.jpg`，并将找到的文件作为扩展的 Logo 图片。

> [!WARNING]
> 源码中存在一个 bug：当 `Logo.png` 不存在而 `Logo.jpg` 存在时，代码仍然会以 `.png` 的路径打开文件，导致抛出异常。因此实际上只有 `Logo.png` 能正常工作。
>
> 如果把 GIF 图片改名为 `Logo.png`，可以正常渲染，但只会显示第一帧，没有动态效果。
>
> 参考：[ExtensionInfo.cs#L173-L191](https://github.com/UnHacknet/OpenHacknet/blob/main/Extensions/ExtensionInfo.cs#L173-L191)

## 相关

如果想在 Steam 创意工坊使用不同的预览图，可以通过 `WorkshopPreviewImagePath` 设置，见 [参考：ExtensionInfo#WorkshopPreviewImagePath](./ExtensionInfo.md#workshoppreviewimagepath)。
