
# 程序与工具大全

Hacknet 中所有可执行程序（`.exe`）的图鉴：RAM 消耗、功能与获取途径。数据提取自游戏源码 `PortExploits.cs`、`OS.cs` 及各 `*Exe.cs` 类。

> 说明：表中的 RAM 为程序运行时占用的内存；端口破解程序需在目标开放对应端口后运行（如 `exe SSHcrack 22`）。

## 内置程序（开局自带，无需获取）

| 程序 | RAM | 功能 |
|---|---|---|
| `PortHack` | 246 | 万能端口破解器，需目标已开放足够端口；获取管理员权限 |
| `ForkBomb` | 10 起 | 崩溃目标系统（或自己的电脑）；运行时 RAM 持续增长至系统崩溃 |
| `Shell` | 40 | 在目标建立持久后门连接，用于压测代理 |
| `Tutorial` | 500 | 新手教程演示程序 |
| `Notes` | 37 | 内置记事本，可增删笔记 |

## 端口破解程序

| 程序 | RAM | 破解端口 | 服务 | 获取途径 |
|---|---|---|---|---|
| `SSHcrack.exe` | 242 | 22 | SSH | 开局赠送（新游戏即拥有） |
| `FTPBounce.exe` | 210 | 21 | FTP | 开局赠送 |
| `SMTPoverflow.exe` | 356 | 25 | SMTP 邮件 | Entropy 阵营任务奖励 |
| `WebServerWorm.exe` | 208 | 80 | HTTP 网页 | Entropy 阵营任务奖励 |
| `SQL_MemCorrupt.exe` | 350 | 1433 | SQL Server | 任务奖励 / Cheater's Stash |
| `KBT_PortTest.exe` | 400 | 104 | 医疗设备 | CSEC Bit 线任务 / Cheater's Stash |
| `WoWHack.exe` | — | 3724 | 游戏更新服务 | — |
| `confloodEOS.exe` | — | 3659 | eOS 连接管理 | — |
| `TorrentStreamInjector.exe` | 360 | 6881 | BitTorrent | DLC Kaguya 试炼奖励 |
| `SSLTrojan.exe` | 220 | 443 | HTTPS | DLC 任务 |
| `FTPSprint.exe` | 190 | 211 | 文件传输 | DLC 任务 |
| `PacificPortcrusher.exe` | 190 | 192 | 太平洋专用 | DLC 任务 |
| `RTSPCrack.exe` | 360 | 554 | RTSP 流媒体 | DLC 任务 |
| `GitTunnel.exe` | — | 9418 | 版本控制 | — |

## 工具类程序

| 程序 | RAM | 功能 | 获取途径 |
|---|---|---|---|
| `SecurityTracer.exe` | 150 | 主动追踪目标（在目标上运行以触发追踪） | 开局赠送 |
| `Decypher.exe` | 370 | 解密 `.dec` 加密文件 | CSEC 任务 / Cheater's Stash |
| `DECHead.exe` | 240 | 解析 `.dec` 文件头部信息 | CSEC 任务 / Cheater's Stash |
| `TraceKill.exe` | 600 | 冻结追踪计时器，逃脱追踪 | CSEC 高级任务（加密交付） |
| `themechanger.exe` | 320 | 切换系统主题 | CSEC 等级 1 奖励 |
| `eosDeviceScan.exe` | 300 | 扫描目标连接的 eOS 设备 | Entropy 任务 |
| `Sequencer.exe` | 170 | 可视化序列器（播放动画/音序） | V 系列（EnTech）任务 |
| `ESequencer.exe` | 170 | 扩展版序列器（DLC） | DLC 任务 |
| `Clock.exe` | 60 | 12 小时制时钟 | 任务奖励 |
| `ClockV2.exe` | 60 | 高级时钟（`-c` 简洁 / `-l` 大屏） | DLC 任务 |
| `HexClock.exe` | 55 | 十六进制时钟（`-s`/`-n` 模式） | 任务奖励 |
| `MemForensics.exe` | 300 | 分析内存转储 `.mem` 文件（DLC） | DLC 任务 |
| `MemDumpGenerator.exe` | 80 | 生成目标的内存转储文件（DLC） | DLC 任务 |
| `NetmapOrganizer.exe` | 300 | 整理网络地图节点排序（`-c` 混乱模式） | DLC 任务 |
| `SignalScramble.exe` | 50 | 扰乱信号，减缓追踪（DLC） | DLC 任务 |
| `Tuneswap.exe` | 300 | 切换当前音乐曲目 | DLC 任务 |
| `KaguyaTrial.exe` | 190 | DLC 卡古亚试炼入口程序 | DLC 试炼 |
| `ComShell.exe` | — | 批量控制所有 Shell（`-e` 退出 / `-o` 过载） | DLC 任务 |
| `DNotes.exe` | — | 将全部笔记导出到终端 | DLC 任务 |
| `OpShell.exe` | — | 保存/重开 Shell 会话（`-s` 保存 / `-o` 重开） | DLC 任务 |
| `hacknet.exe` | — | 彩蛋：显示 "Program hacknet.exe is already running!" | 彩蛋 |

## 获取途径汇总

- **开局赠送**：`SSHcrack.exe`、`FTPBounce.exe`、`SecurityTracer.exe` 在新建游戏时即位于本机 `/bin`。
- **Entropy 阵营**：完成 Entropy 任务奖励 `SMTPoverflow.exe`、`WebServerWorm.exe`、`eosDeviceScan.exe`。
- **CSEC 阵营**：
  - 等级 1：`themechanger.exe`
  - Bit 线任务：`Decypher.exe`、`DECHead.exe`、`KBT_PortTest.exe`、`TraceKill.exe`（后者以加密 `.dec` 交付，密码 `dx122DX`）
- **DLC（Labyrinths）**：`TorrentStreamInjector.exe`、`SSLTrojan.exe`、`FTPSprint.exe`、`PacificPortcrusher.exe`、`RTSPCrack.exe` 及各 DLC 工具类程序。
- **Cheater's Stash**（`1337.1337.1337.1337`）：隐藏黑市服务器，集齐 `SSHcrack`、`FTPBounce`、`SMTPoverflow`、`WebServerWorm`、`SQL_MemCorrupt`、`KBT_PortTest`、`Decypher`、`eosDeviceScan`、`DECHead`。
- **随机节点**：部分随机生成的服务器 `/bin` 中可能藏有端口破解程序，可入侵盗取。

## 小贴士

1. **RAM 有限**：总 RAM 有限，高消耗程序（TraceKill 600、KBT 400）需合理规划运行顺序。
2. **`exe` 命令**：运行端口破解程序需指定端口，如 `exe SSHcrack 22`；工具类程序直接 `exe <程序名>`。
3. **程序文件可拷贝**：程序以文件形式存在，可通过 `scp` 从已破解的服务器复制到本机 `/bin`。

> 相关页面：[基础操作与命令](./Commands.md) · [核心机制](./Mechanics.md) · [服务器速查表](./Servers.md)
