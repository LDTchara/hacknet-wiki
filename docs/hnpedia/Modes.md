# 游戏模式与设置

Hacknet 有若干**编译期/启动期开关**决定游戏以什么模式运行：普通战役、Demo、教育安全版、Extension 模式等。数据来自 `Settings.cs`、`Program.cs` 与 `SettingsLoader.cs`。

## 启动参数

命令行参数在 `Program.Main` 中解析：

| 参数 | 作用 |
|---|---|
| `-disableweb` | 禁用网页渲染器（`WebRenderer.Enabled = false`），网页浏览功能停用 |
| `-disablebackground` | 关闭十六进制网格背景 |
| `-altmonitor` | 在副显示器上启动（`Settings.StartOnAltMonitor = true`） |
| `-enablefc` | 启用强制通关命令（`forceCompleteEnabled`，受 `emergencyForceCompleteEnabled` 保护） |
| `-enabledebug` | 启用调试命令（`debugCommandsEnabled` + `OS.DEBUG_COMMANDS`，受 `emergencyDebugCommandsEnabled` 保护） |
| `-extstart <扩展名>` | 直接以指定 Extension 启动，跳过主菜单（自动建号 test/test 并进入） |
| `-allowextpublish` | 允许 Extension 发布到创意工坊 |

## 运行模式（Settings 开关）

| 开关 | 默认 | 含义 |
|---|---|---|
| `MenuStartup` | `true` | 启动后进主菜单（false 则直接进 OS） |
| `slowOSStartup` | `true` | 慢速启动动画 |
| `osStartsWithTutorial` | `= slowOSStartup` | 新 OS 是否从教程开始 |
| `AllowExtensionMode` | `true` | 是否允许 Extension 模式 |
| `AllowExtensionPublish` | `false` | 是否允许创意工坊发布 |
| `EducationSafeBuild` | `false` | 教育安全版：过滤部分文件内容（详见下文） |
| `EnableDLC` | `true` | 是否启用 Labyrinths DLC |
| `IsInExtensionMode` | `false` | 当前是否为 Extension 会话 |
| `ForceEnglish` | `false` | 强制使用 `en-us` 本地化 |
| `soundDisabled` | `false` | 禁用声音 |

## Demo 模式

Demo 相关开关（默认均关闭，发行版中不启用）：

| 开关 | 含义 |
|---|---|
| `isDemoMode` | Demo 模式：用 `Content/Computers/DemoLoadList.txt` 的网络加载，`exitdemo`/`resetdemo` 命令可退出 |
| `isConventionDemo` | 展会 Demo：窗口偏移、启动提速、回车跳过文本等 |
| `isLockedDemoMode` | 锁定 Demo：主菜单只显示 Demo 按钮（无新用户/登录） |
| `isPressBuildDemo` | 媒体发布会 Demo：终局 ESequencer 相关流程缩短 |
| `isAlphaDemoMode` | Alpha 早期 Demo |
| `isSpecialTestBuild` | 特殊测试版：跳过 `resetOS` 重置，进 CSEC 测试任务 |
| `MultiLingualDemo` | 多语言 Demo：`exitdemo` 后切回 zh-cn |
| `DLCEnabledDemo` | Demo 是否包含 DLC 内容（默认 true） |
| `ShuffleThemeOnDemoStart` | Demo 启动时随机切换主题（默认 true） |

Demo 模式与普通模式的差异（源码中）：

- **网络**：`NetworkMap` 在 demo 模式额外加载 `generateDemoNodes()`；OS 启动用 `BootLoadList.getDemoList()`。
- **命令**：终端中可执行 `exitdemo` / `resetdemo` 返回主菜单（普通版无此命令）。
- **界面**：`DemoEndScreen` 在时间到/按键后结束；展会 Demo 回到主菜单，普通 Demo 直接退出游戏。
- **速度**：展会 Demo 下开机动画（`BASE_BOOT_TIME`）、崩溃模块、音乐淡入、打字速度均大幅提速。

## Extension 模式

Extension 模式由主菜单的扩展列表进入，或通过 `-extstart <扩展名>` 直接加载：

- `Settings.IsInExtensionMode = true` 后，`NetworkMap` 不生成内置网络，改为加载 `Extensions/<扩展名>/Nodes/` 下的节点。
- 扩展会话允许独立存档（`AllowSave`），且不会污染本体网络。
- 详细机制见 [网络地图结构](./NetworkMap.md) 与 [Extension 教程](../extension-tutorial/index.md)。

## 教育安全版（EducationSafeBuild）

面向课堂的"净化"版本，开关为 `Settings.EducationSafeBuild`。主要差异：

- `BashLogs` 文件改用 `Content/BashLogs_StudentSafe.txt`（学生安全版日志）。
- 部分 `<file>` 内容在加载时被过滤（`ComputerLoader` 与 `Folder` 中有 `EducationSafeBuild` 分支）。
- 适合在学校环境演示而不展示敏感内容。

## 设置文件（Settings.txt）

图形/声音设置存于 `Settings.txt`，由 `SettingsLoader` 读写：

```
分辨率宽
分辨率高
全屏(true/false)
bloom: true
scanlines: true
muted: true
volume: 0.8
fontConfig: 字体名
hasSaved: True
shouldMultisample: true
defaultLocale: zh-cn
drawMusicVis: true
```

其中 `defaultLocale` 决定默认本地化语言（见 [多语言与本地化](./Locales.md)）。

> 相关页面：[核心机制](./Mechanics.md) · [多语言与本地化](./Locales.md) · [Extension 教程](../extension-tutorial/index.md)