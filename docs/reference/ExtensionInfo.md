# ExtensionInfo（扩展信息）

`ExtensionInfo.xml` 是每个 Extension 必须包含的描述文件，位于扩展根目录下。游戏通过它来识别和加载一个扩展。

如果扩展目录下不存在 `ExtensionInfo.xml`，游戏不会将其识别为有效的 Extension。

## 根元素

```xml
<HacknetExtension>
  ...
</HacknetExtension>
```

根元素为 `<HacknetExtension>`，其中包含多个子元素来描述扩展的各项配置。

## 所有元素

### `Name`（必填）

```xml
<Name>MyFirstExtension</Name>
```

扩展的名称。

- 最大长度为 128 个字符，且必须包含至少一个字母或数字。在游戏中只能显示 ASCII 字符。

在生成存档目录的时候名称中的空格会被替换为 `_`，非法文件名字符会被移除。

> [!WARNING]
> 缺少 `Name` 或或者名称中只存在非法文件名字符时（`<>:"/\|?*` 及控制字符等）会导致扩展加载失败。

### `Language`

```xml
<Language>zh-cn</Language>
```

扩展的语言。默认值为 `en-us`。

可用语言：

| 语言 | 值 |
| --- | --- |
| English | `en-us` |
| German | `de-de` |
| French | `fr-be` |
| Russian | `ru-ru` |
| Spanish | `es-ar` |
| Korean | `ko-kr` |
| Japanese | `ja-jp` |
| Chinese, simplified | `zh-cn` |

### `AllowSaves`

```xml
<AllowSaves>true</AllowSaves>
```

是否允许玩家在该扩展中存档。默认值为 `true`。

### `StartingVisibleNodes`

```xml
<StartingVisibleNodes>node1,node2,node3</StartingVisibleNodes>
```

游戏开始时默认可见的 Node 列表。多个 Computer ID 之间可以用逗号 `,`、空格、制表符、换行或 `/` 分隔。

默认值为空（没有初始可见的 Node）。

### `StartingMission`

```xml
<StartingMission>Missions/StartingMission.xml</StartingMission>
```

扩展启动时加载的 Mission 的文件路径。

- 如果不需要起始 Mission，可以设为 `NONE` 或删除该元素。

> [!WARNING]
> 如果 `StartingMission` 指定的文件不存在，扩展验证会报错。

### `StartingActions`

```xml
<StartingActions>Actions/StartingActions.xml</StartingActions>
```

扩展启动时加载的 ConditionalActions 的文件路径。用于初始化，例如给玩家程序、设置 Faction 等。

- 如果不需要，可以设为 `NONE` 或删除该元素。

### `Description`

```xml
<Description>这是一个示例扩展的描述。</Description>
```

扩展的描述文本，会显示在 Hacknet 的扩展列表中。默认值为空。

### `Faction`

```xml
<Faction>Factions/StartingFaction.xml</Faction>
<Faction>Factions/SecondFaction.xml</Faction>
```

扩展中使用的 Faction 文件路径。可以有多个 `Faction` 元素。

默认没有 Faction。

### `StartsWithTutorial`

```xml
<StartsWithTutorial>False</StartsWithTutorial>
```

扩展是否以教程模式启动。默认值为 `False`。

### `HasIntroStartup`

```xml
<HasIntroStartup>false</HasIntroStartup>
```

扩展启动时是否播放标准的重启启动序列（bootup sequence）。默认值为 `true`。

如果项目目录下存在 Intro.txt，那么会在首次启动后逐字展示它。

### `StartingTheme`

```xml
<StartingTheme>Themes/ExampleTheme.xml</StartingTheme>
```

扩展的初始主题。可以是自定义主题的文件路径，也可以是内置主题的名称。

内置主题名称：`TerminalOnlyBlack`、`HacknetBlue`、`HacknetTeal`、`HacknetYellow`、`HackerGreen`、`HacknetWhite`、`HacknetPurple`、`HacknetMint`

默认值为 `HacknetBlue`。

### `IntroStartupSong`

```xml
<IntroStartupSong>The_Quickening</IntroStartupSong>
```

扩展启动时播放的音乐。

- 引用原版音乐：直接写文件名（不含路径），适用于 `Content/Music` 和 `Content/DLC/Music`（需要 Labyrinths DLC）中的音乐。
- 引用自定义音乐：写扩展内的相对路径，例如 `Music/MySong.ogg`。文件格式为 `.ogg`。

默认值为 `null`（不播放）。

### `IntroStartupSongDelay`

```xml
<IntroStartupSongDelay>2.0</IntroStartupSongDelay>
```

启动音乐的延迟播放时间（秒）。默认值为 `0`。

### ESequencer 相关

Sequencer 是 Hacknet 本体终局阶段使用的特殊机制，其变体 ESequencer 被专门用于扩展中。以下元素可以控制 ESequencer 的行为。

#### `SequencerTargetID`

```xml
<SequencerTargetID>advExamplePC</SequencerTargetID>
```

ESequencer 的目标 Node 的 Computer ID。

#### `SequencerSpinUpTime`

```xml
<SequencerSpinUpTime>10.5</SequencerSpinUpTime>
```

ESequencer 的启动时间（秒）。默认值为 `17`。

#### `SequencerFlagRequiredForStart`

```xml
<SequencerFlagRequiredForStart>testFlag</SequencerFlagRequiredForStart>
```

ESequencer 启动所需的 Flag。

#### `ActionsToRunOnSequencerStart`

```xml
<ActionsToRunOnSequencerStart>Actions/ThemeSwapActions.xml</ActionsToRunOnSequencerStart>
```

ESequencer 启动时执行的 ConditionalActions 的文件路径。

### Workshop 相关

以下元素用于 Steam 创意工坊发布。如果不需要发布到创意工坊，可以忽略。

#### `WorkshopDescription`

```xml
<WorkshopDescription>这是显示在 Steam 创意工坊上的描述。最多 8000 个字符。</WorkshopDescription>
```

创意工坊页面的描述文本。最大 8000 个字符，支持换行。

#### `WorkshopLanguage`

```xml
<WorkshopLanguage>English</WorkshopLanguage>
```

创意工坊条目的语言。

#### `WorkshopVisibility`

```xml
<WorkshopVisibility>2</WorkshopVisibility>
```

创意工坊条目的可见性。默认值为 `2`。

| 值 | 可见性 |
| -- | ------ |
| `0` | 公开 |
| `1` | 仅好友可见 |
| `2` | 仅自己可见 |

#### `WorkshopTags`

```xml
<WorkshopTags>Extension</WorkshopTags>
```

创意工坊条目的标签，以逗号分隔。目前 Hacknet 创意工坊只有 "Extension" 一个分类，保持默认即可。

#### `WorkshopPreviewImagePath`

```xml
<WorkshopPreviewImagePath>Logo.png</WorkshopPreviewImagePath>
```

创意工坊预览图的文件路径。

- 必须位于扩展文件夹内。
- 必须为 16:9 比例，且小于 1MB。
- 支持 `.png`、`.jpg`、`.gif` 格式。

#### `WorkshopPublishID`

```xml
<WorkshopPublishID>NONE</WorkshopPublishID>
```

创意工坊发布 ID。首次发布时由 Steam 自动填充。

> [!WARNING]
> 不要手动修改此值。如果丢失该 ID，再次发布会被视为新条目，导致丢失所有订阅者。仅当值为 `NONE` 时才会生成新的 ID。

扩展根目录下的 Logo 图片不属于 `ExtensionInfo.xml` 的一部分，见 [参考：Logo](./Logo.md)。

## 参考

参考：[ExtensionInfo.cs](https://github.com/UnHacknet/OpenHacknet/blob/main/Extensions/ExtensionInfo.cs)
