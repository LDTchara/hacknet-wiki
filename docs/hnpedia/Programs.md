
# 程序与工具大全

Hacknet 中所有可执行程序（`.exe`）的图鉴：RAM 消耗、运行时长、完成/退出时长与内存变化。数据提取自游戏源码 `PortExploits.cs`、`NetworkMap.cs` 及各 `*Exe.cs` 类的常量，与 [Hacknet Wiki（Fandom）Executables](https://hacknet.fandom.com/wiki/Executables) 交叉核对。

> **字段说明**：
> - **RAM**：程序运行时占用的内存（`ramCost`）
> - **Runtime**：从启动到完成（`Completed()`）的破解/运行时间（`DURATION` / `RUN_TIME` / `CRACK_TIME`）
> - **Fail / Exit**：完成后残留的动画/成功提示时长（`sucsessTimer` / `COMPLETE_TIME` / `AFTER_COMPLETION_STALL`）
> - **Total**：Runtime + Fail/Exit 的总界面停留时间
> - **内存变化**：运行期间 RAM 是否动态增减（`RAM_CHANGE_PS`，即每秒增减速率）；标"—"表示固定占用

## 内置程序（无需获取，始终可运行）

| 程序 | RAM | Runtime | Fail/Exit | Total | 内存变化 | 功能 |
|---|---|---|---|---|---|---|
| `PortHack` | 246 | 6s（`CRACK_TIME`） | +5s | 11s | 固定 | 万能端口破解器，需目标已开放足够端口；获取管理员权限 |
| `ForkBomb` | 10 起 | 持续 | — | — | **+150/s**（`RAM_CHANGE_PS`）持续增长至系统崩溃 | 崩溃目标系统（或自己的电脑） |
| `Shell` | 40 | — | — | — | **±200/s** 趋向目标值（`RAM_CHANGE_PS`） | 在目标建立持久后门连接，用于压测代理 |
| `Tutorial` | 500 | — | — | — | — | 新手教程演示程序 |
| `Notes` | 37 | — | — | — | **±350/s** 趋向目标值（`RAM_CHANGE_PS`） | 内置记事本，可增删笔记 |

> 这 5 个程序由 `ProgramList.getExeList()` 硬编码，始终可用，不依赖文件存在。

## 端口破解程序

| 程序 | RAM | Runtime | Fail/Exit | Total | 内存变化 | 破解端口 | 服务 | 获取途径 |
|---|---|---|---|---|---|---|---|---|
| `SSHcrack.exe` | 242 | 8s（`DURATION`） | — | 8s | 固定 | 22 | SSH | 教程引导 `scp`（从教程目标机取得） |
| `FTPBounce.exe` | 210 | 15s（`DURATION`） | — | 15s | 固定 | 21 | FTP Server | Entropy 任务流程发放（非开局赠送） |
| `SMTPoverflow.exe` | 356 | 12s（`DURATION`） | +0.5s | 12.5s | 固定 | 25 | SMTP MailServer | Entropy 阵营任务奖励 |
| `WebServerWorm.exe` | 208 | 14s（`DURATION`） | +1s（`AFTER_COMPLETION_STALL`） | 15s | 固定 | 80 | HTTP WebServer | Entropy 阵营任务奖励 |
| `SQL_MemCorrupt.exe` | 350 | 3+3+5+1.2s（四阶段） | — | ~12.2s | 固定 | 1433 | SQL Server | 任务奖励 / Cheater's Stash |
| `KBT_PortTest.exe` | 400 | 22s（`RUNTIME`） | +2s（`COMPLETE_TIME`） | 24s | 固定 | 104 | Medical Services | CSEC Bit 线任务 / Cheater's Stash |
| `WoWHack.exe` | — | — | — | — | — | 3724 | Blizzard Updater | — |
| `confloodEOS.exe` | — | — | — | — | — | 3659 | eOS Connection Manager | — |
| `TorrentStreamInjector.exe` | 360 | 4.8s（`RUN_TIME`） | — | — | 固定 | 6881 | BitTorrent | DLC Kaguya 试炼奖励 |
| `SSLTrojan.exe` | 220 | 12s（`RUN_TIME`） | — | — | 固定 | 443 | HTTPS | DLC 任务 |
| `FTPSprint.exe` | 190 | 7s（`RUN_TIME`） | — | — | 固定 | 211 | 文件传输 | DLC 任务 |
| `PacificPortcrusher.exe` | 190 | 6s（`RUN_TIME`） | — | — | 固定 | 192 | 太平洋专用 | DLC 任务 |
| `RTSPCrack.exe` | 360 | 6.3s（`RUN_TIME`） | — | — | 固定 | 554 | RTSP 流媒体 | DLC 任务 |
| `GitTunnel.exe` | — | — | — | — | — | 9418 | 版本控制 | — |

## 工具类程序

| 程序 | RAM | 功能 | 获取途径 |
|---|---|---|---|
| `SecurityTracer.exe` | 150 | 主动追踪目标（在目标上运行以触发追踪） | **开局本机 `/bin` 自带**（`NetworkMap.cs`） |
| `Decypher.exe` | 370 | 解密 `.dec` 加密文件（`LOADING 3.5s` + `WORKING 10s` + `COMPLETE 3s`） | CSEC 任务 / Cheater's Stash |
| `DECHead.exe` | 240 | 解析 `.dec` 文件头部信息（`LOADING 3.5s` + `COMPLETE 10s`） | CSEC 任务 / Cheater's Stash |
| `TraceKill.exe` | 600 | 冻结追踪计时器，逃脱追踪 | CSEC 高级任务（加密交付） |
| `themechanger.exe` | 320 | 切换系统主题（`START_LOADING 25.5s`） | CSEC 等级 1 奖励（`HubFaction`） |
| `eosDeviceScan.exe` | 300 | 扫描目标连接的 eOS 设备（`TOTAL 8s` / `SHORTCUT 3.5s`） | Entropy 任务 |
| `Sequencer.exe` | 170 | 可视化序列器（播放动画/音序）；RAM **+100/s**（`RAM_CHANGE_PS`） | V 系列（EnTech）任务 |
| `ESequencer.exe` | 170 | 扩展版序列器（DLC）；RAM **+100/s** | DLC 任务 |
| `Clock.exe` | 60 | 12 小时制时钟 | 任务奖励 |
| `ClockV2.exe` | 60 | 高级时钟（`-c` 简洁 / `-l` 大屏） | DLC 任务 |
| `HexClock.exe` | 55 | 十六进制时钟（`-s`/`-n` 模式） | 任务奖励 |
| `MemForensics.exe` | 300 | 分析内存转储 `.mem` 文件（DLC） | DLC 任务 |
| `MemDumpGenerator.exe` | 80 | 生成目标的内存转储文件（DLC；`FailTime 2s` + `ExitTime 5s`） | DLC 任务 |
| `NetmapOrganizer.exe` | 300 | 整理网络地图节点排序（`-c` 混乱模式） | DLC 任务 |
| `SignalScramble.exe` | 50 | 扰乱信号，减缓追踪（DLC） | DLC 任务 |
| `Tuneswap.exe` | 300 | 切换当前音乐曲目 | DLC 任务 |
| `KaguyaTrial.exe` | 190 | DLC 卡古亚试炼入口程序 | DLC 试炼 |
| `ComShell.exe` | — | 批量控制所有 Shell（`-e` 退出 / `-o` 过载） | DLC 任务 |
| `DNotes.exe` | — | 将全部笔记导出到终端 | DLC 任务 |
| `OpShell.exe` | — | 保存/重开 Shell 会话（`-s` 保存 / `-o` 重开） | DLC 任务 |
| `hacknet.exe` | — | 彩蛋：显示 "Program hacknet.exe is already running!" | 彩蛋 |

## 获取途径汇总

- **开局本机 `/bin`**：仅 `SecurityTracer.exe`（`NetworkMap.generateSPNetwork()` 添加）。`PortHack`/`ForkBomb`/`Shell`/`Tutorial`/`Notes` 为内置程序，不占文件。
- **教程流程**：`SSHcrack.exe` 由教程引导玩家从教程目标机的 `/bin` 用 `scp` 拷回——**不是开局赠送**。
- **Entropy 阵营**：任务流程发放 `FTPBounce.exe`、`SMTPoverflow.exe`、`WebServerWorm.exe`、`eosDeviceScan.exe`。
- **CSEC 阵营**：
  - 等级 1：`themechanger.exe`（`HubFaction` 发放）
  - Bit 线任务：`Decypher.exe`、`DECHead.exe`、`KBT_PortTest.exe`、`TraceKill.exe`（后者以加密 `.dec` 交付，密码 `dx122DX`）
- **DLC（Labyrinths）**：`TorrentStreamInjector.exe`、`SSLTrojan.exe`、`FTPSprint.exe`、`PacificPortcrusher.exe`、`RTSPCrack.exe` 及各 DLC 工具类程序。
- **Cheater's Stash**（`1337.1337.1337.1337`）：隐藏黑市服务器，集齐 `SSHcrack`、`FTPBounce`、`SMTPoverflow`、`WebServerWorm`、`SQL_MemCorrupt`、`KBT_PortTest`、`Decypher`、`eosDeviceScan`、`DECHead`。
- **随机节点**：部分随机生成的服务器 `/bin` 中可能藏有端口破解程序，可入侵盗取。

## 小贴士

1. **RAM 有限**：总 RAM 有限，高消耗程序（TraceKill 600、KBT 400）需合理规划运行顺序；动态内存程序（ForkBomb、Shell）运行时会持续占用直至 OOM。
2. **`exe` 命令**：运行端口破解程序需指定端口，如 `exe SSHcrack 22`；工具类程序直接 `exe <程序名>`。
3. **程序文件可拷贝**：程序以文件形式存在，可通过 `scp` 从已破解的服务器复制到本机 `/bin`。

> 相关页面：[基础操作与命令](./Commands.md) · [核心机制](./Mechanics.md) · [服务器速查表](./Servers.md)
