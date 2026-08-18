
# OS 主题系统

Hacknet 的操作系统支持更换主题外观。主题通过 `themechanger.exe` 切换，或由任务脚本强制切换。数据提取自游戏源码 `OSTheme.cs`、`CustomTheme.cs` 与 DLC 主题 XML。

## 内置主题

基础游戏内置以下主题（`OSTheme` 枚举）：

| 主题 | 说明 |
|---|---|
| `HacknetBlue` | 默认蓝色主题 |
| `HacknetTeal` | 青色 |
| `HacknetYellow` | 黄色 |
| `HackerGreen` | 绿色（经典黑客配色） |
| `HacknetWhite` | 白色 |
| `HacknetPurple` | 紫色 |
| `HacknetMint` | 薄荷绿 |
| `TerminalOnlyBlack` | 纯黑终端（无主题） |
| `Custom` | 自定义主题（XML 定义） |
| `Colamaeleon` | 变色龙（DLC） |
| `GreenCompact` | 紧凑绿 |
| `Riptide` / `Riptide2` | Riptide 系列（DLC） |

> 游戏中还可通过主题文件（`x-server.sys`）获得其他主题，详见下文"DLC 主题"。

## DLC（Labyrinths）主题

`Content/DLC/Themes/` 目录下共 15 个主题定义 XML：

| 主题文件 | 说明 |
|---|---|
| `CautionTheme.xml` / `CautionThemeStandard.xml` | Caution 主题（含标准配色版） |
| `CoelTheme.xml` | Coel 主题（背景为 dino.png，取自 Coel 角色） |
| `FloatVoidTheme.xml` / `FloatVoidThemeStandard.xml` | FloatVoid（漂浮虚空） |
| `HoraTheme.xml` / `HoraThemeAlt.xml` | Hora |
| `MiamiTheme.xml` / `MiamiThemeLight.xml` / `MiamiThemeLightBlue.xml` | Miami 三版（紫/青霓虹） |
| `RainTheme.xml` | 雨 |
| `RiptideTheme.xml` / `RiptideClassicTheme.xml` / `RiptideThemeStandard.xml` | Riptide 三版 |
| `StarfieldClassicTheme.xml` | 星空 |

### 主题 XML 结构

每个自定义主题以 `<CustomTheme>` 为根节点，通过颜色字段覆盖 OS 全部界面配色：

- `themeLayoutName` — 映射基础布局主题（如 `riptide2`、`colamaeleon`）
- `backgroundImagePath` — 桌面背景图（如 Miami→Starfield2.png、Coel→dino.png）
- `shellColor` / `shellButtonColor` — 终端文字与按钮色
- `defaultHighlightColor` / `defaultTopBarColor` — 网络地图节点/顶栏主色
- `moduleColorSolidDefault` / `moduleColorStrong` / `moduleColorBacking` — 模块窗口边框三层色
- `exeModuleTopBar` / `exeModuleTitleText` — EXE 模块（PortHack、SSHcrack 等）顶栏与标题色
- `warningColor` — 警告色
- `lockedColor` / `unlockedColor` 等 — 扫描（probe）界面四色
- 进阶字段（CoelTheme 独有）：`terminalTextColor`、`scanlinesColor`、`netmapToolTipColor` 等

## 主题切换

### themechanger.exe

- **获取**：加入 CSEC 后，等级 1 时自动发放到 `mainHubAssets` 的 `/bin`（`HubFaction` 逻辑）；部分任务也会奖励。
- **运行**：`exe themechanger`，打开主题切换界面（RAM 320）。
- **功能**：列出远程与本地（`/sys`、`/home`）中所有可解析为主题的文件；点 **Activate Theme** 会把选中主题覆盖写入本机 `sys/x-server.sys` 并立即生效。
- **成就**：运行即解锁 `themeswitch_run`。

### 主题文件机制

- 每个 `OSTheme` 枚举值对应一段确定性数据串；主题文件即 `x-server.sys` 的数据。
- 自定义主题经 `CustomTheme` 反序列化后，通过反射将字段写回 OS。
- 切换前会把当前 `x-server.sys` 备份为 `x-serverBACKUP*.sys`，可随时还原。
- 任务脚本可用 `SASwitchToTheme` 强制切换主题（如剧情场景）。

## 彩蛋与联动

- **Pellium Box**（`69.172.201.153`）：一台藏着 Naix 主题文件仓库的服务器，内有 `x-server_Green.sys`、`x-server_RiptideStandard.sys`、`x-server_StarfieldClassic.sys` 等（详见 [游戏彩蛋](./EasterEggs.md)）。
- 主线里删掉 Naix 的 `sys/x-server.sys` 会让他整台系统崩溃——主题文件与系统文件是同一机制。

> 相关页面：[程序与工具大全](./Programs.md) · [游戏彩蛋](./EasterEggs.md) · [成就列表](./Achievements.md)
