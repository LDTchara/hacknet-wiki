# ExtensionInfo（扩展信息）

`ExtensionInfo.xml` 是每个 Extension 都包含的描述文件，位于扩展根目录下。游戏通过它来识别和加载一个扩展：如果目录下没有 `ExtensionInfo.xml`，游戏就不会把它当作一个有效的 Extension。

在[准备工作](./preparing.md)中，我们把 `EDIT_ME_ExtensionInfo.xml` 改名为 `ExtensionInfo.xml`，并填写了名称和语言。本章我们来看看这个文件里还能写些什么。

## 基本结构

`ExtensionInfo.xml` 的根元素是 `<HacknetExtension>`，其中的每个子元素描述扩展的一项配置：

```xml
<HacknetExtension>
  <Name>MyFirstExtension</Name>
  <Language>zh-cn</Language>
  <AllowSaves>true</AllowSaves>
  <Description> --- Blank Extension ---
Edit this with your own extension info and start building!</Description>
  ...
</HacknetExtension>
```

其中最基础的几个元素：

- `Name`（必填）：扩展的名称，会显示在扩展列表中。最大长度 128 个字符，且必须包含至少一个字母或数字。
- `Language`：扩展的语言。默认值为 `en-us`，中文扩展一般填写 `zh-cn`。
- `AllowSaves`：是否允许玩家存档。默认值为 `true`。
- `Description`：扩展的描述，显示在扩展列表中。

## 起始内容

以下元素决定了扩展启动时加载的内容，它们与我们前面章节介绍的内容一一对应：

- `StartingMission`：启动时加载的 [Mission](./Mission.md) 的路径。不需要时设为 `NONE` 或删除该元素。
- `StartingActions`：启动时加载的 ConditionalActions 的路径，也就是我们见过的 `Actions/StartingActions.xml`。不需要时设为 `NONE` 或删除该元素。
- `StartingVisibleNodes`：启动时默认可见的 Node 列表，多个 Computer ID 之间用逗号分隔。
- `Faction`：扩展中使用的 Faction 文件路径。可以写多个 `Faction` 元素。

`BlankExtension` 的 `ExtensionInfo.xml` 中就有这样一段：

```xml
<StartingVisibleNodes></StartingVisibleNodes>
<StartingMission>Missions/StartingMission.xml</StartingMission>
<StartingActions>Actions/StartingActions.xml</StartingActions>
<Faction>Factions/StartingFaction.xml</Faction>
```

## 外观与声音

- `StartingTheme`：初始主题。可以是自定义主题文件的路径，也可以是内置主题的名称（如 `HacknetBlue`）。
- `HasIntroStartup`：启动时是否播放标准的重启启动序列。默认值为 `true`，`BlankExtension` 将其设为了 `false`。如果扩展项目目录下存在 `Intro.txt`，还会在首次启动后逐字展示它的内容。
- `StartsWithTutorial`：是否以教程模式启动。默认值为 `False`。
- `IntroStartupSong`：启动时播放的音乐。可以引用原版音乐的文件名，也可以引用扩展内 `.ogg` 文件的路径。
- `IntroStartupSongDelay`：启动音乐延迟播放的时间（秒）。

## 动手试试

打开 `MyFirstExtension/ExtensionInfo.xml`，把 `Description` 改成自己的描述，再把 `HasIntroStartup` 改为 `true`：

```xml
<Description> --- Blank Extension --- <!-- [!code del] -->
Edit this with your own extension info and start building!</Description> <!-- [!code del] -->
<Description>我的第一个 Hacknet 扩展！</Description> <!-- [!code add] -->

<HasIntroStartup>false</HasIntroStartup> <!-- [!code del] -->
<HasIntroStartup>true</HasIntroStartup> <!-- [!code add] -->
```

保存后启动 Hacknet，打开 Extensions 页面，可以看到 `MyFirstExtension` 的描述已经更新。启动扩展后，还能看到一段重启启动序列。

你还可以在 `MyFirstExtension/` 根目录下修改 `Intro.txt`，写上你的欢迎语：

```txt
Welcome to MyFirstExtension!
```

启动扩展后，这段文字会在首次启动后逐字展示。`IntroExtension` 的根目录下就附带了一个 `Intro.txt`，可以作为参考。


> [!WARNING]
> `Name` 是必填元素且只能显示 ASCII 字符，缺少它，或者名称中只存在非法文件名字符（`<>:"/\|?*` 及控制字符等），都会导致扩展加载失败。

## 参考

`ExtensionInfo.xml` 中还包含 Sequencer（序列器）、Steam 创意工坊（Workshop）等进阶元素，本章不再赘述。

你可以在 [参考：ExtensionInfo](../reference/ExtensionInfo.md) 中查看所有元素的完整说明。
