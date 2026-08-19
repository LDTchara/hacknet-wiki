# 敌对黑客入侵事件

这是 Labyrinths DLC 中的特殊机制：当 DLC 剧情触发"恶意文件注入"时，游戏会**跳出到真实操作系统**，在磁盘上写入一个"木马 DLL"，并让你的系统进入"被锁死"状态。`HostileHackerBreakinSequence` 负责这一整套演出。

## 触发方式

`HackerScriptExecuter` 执行攻击脚本中的 `systakeover` 指令时触发 `HostileHackerBreakinSequence.Execute()`：

```bash
systakeover
```

- `Execute` 会先设置屏幕闪白转场，给 OS 加上 `startupBreakinTrapActivated` 标记并立即存档。
- 然后把 `Content/DLC/Misc/VMBootloaderTrap.dll` 复制到你的真实硬盘，并**加锁防止删除**。
- 短暂延迟后调用 `os.thisComputer.crash(...)`，使游戏内系统崩溃。

## 被锁状态（Blocking Hostile File）

游戏启动时会检查是否处于"被锁"状态（`IsInBlockingHostileFileState`）：

- 必须同时满足 `startupBreakinTrapActivated` 已标记、`startupBreakinTrapPassed` 未标记，并且真实磁盘上存在注入的 DLL。
- Windows 检查文件是否存在于硬盘；macOS/Linux 还额外检查注入目录是否为只读。
- 若被锁，OS 会反复 **重启并卡在开机崩溃画面**（`CrashModule` 在第 50 行启动文本处把计时器设为几乎无限，标记 `IsInHostileFileCrash`），直到你修复为止。

## 修复流程

要解除锁死，需要**在真实系统上手动删除**注入的文件：

| 平台 | 注入路径 |
|---|---|
| Windows | `我的文档\My Games\<扩展名>\Hacknet\Libs\Injected\VMBootloaderTrap.dll` |
| Linux | `$XDG_DATA_HOME` 或 `~/.local/share/<扩展名>/Hacknet/Libs/Injected/...`（目录被 `chmod 000` 锁死） |
| macOS | `~/Library/Application Support/<扩展名>/Hacknet/Libs/Injected/...` |
| WASM | 仅检查文件是否存在 |

- 扩展模式下路径会追加扩展的文件夹名（`<扩展名>`）。
- 修复后首次成功启动：`IsFirstSuccessfulBootAfterBlockingState` 判定通过，播放 DLC 追逃主题曲 `DLC\Music\World_Chase`，OS 重启进入正常流程。

## 界面辅助（BootCrashAssistanceModule）

被锁时开机画面会显示 `BootCrashAssistanceModule`，提供自助修复按钮：

- **Windows**：点击 "Proceed" 会把 `Win_AllyHelpFile.txt` 复制为 `VM_Recovery_Guide.txt`、打开记事本显示帮助、打开一个 `OpenCMD.bat` 终端，然后退出游戏。
- **Linux/macOS**：提供 "README"（在游戏内显示帮助文本）、"Terminal"（打开系统终端到注入目录）和 "Crash VM" 三个按钮。
- 帮助文件按平台读取 `Content/DLC/Misc/Win_AllyHelpFile.txt` 或 `Unix_AllyHelpFile.txt`（支持本地化）。

## 相关命令

调试命令 `testhhbs`（需启用 `-enabledebug`）可在终端查询当前状态：

```bash
testhhbs
```

输出 `BLOCKED` 或 `SAFE`，取决于是否处于被锁状态（见 `ProgramRunner` 的实现）。

> 相关页面：[Labyrinths DLC](./Labyrinths.md) · [程序与工具大全](./Programs.md)