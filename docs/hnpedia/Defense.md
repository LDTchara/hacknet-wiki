# 防御与追踪深度解析

本文深入 `Firewall`、`TraceTracker`、`Administrator` 与 Shell 的**内部实现**，是 [核心机制](./Mechanics.md) 的技术延伸。

## 防火墙内部（Firewall）

防火墙在等级 5+ 服务器上生成，构造时：

- `solutionLength = 6 + complexity`（complexity = 安全等级 − 5）。
- 密码 = `getRandomChar()`（**70% 字母 + 30% 数字**，见 `Utils`）生成后转大写。
- 支持自定义 `solution` 与 `additionalDelay`（XML 的 `solution`/`additionalDelay` 属性）。

### analyze 输出原理

每次 `analyze` 显示一行 20 字符宽的矩阵（`generateOutputLine`）：

- 前 `20 − 3×analysisPasses` 位被随机字符遮蔽，**第 0 次分析每 20 位隐藏 1 位**，此后每轮修复 3 位。
- 正确的字符被**随机插入**矩阵中的任意位置，位置完全随机。
- 每轮每字符延迟 = `0.08 + 0.06×轮次 + additionalDelay` 秒（递增）。
- 输入 `solve` 时：长度错误提示"Too few/many characters"；完整匹配（忽略大小写）即 `solved = true`。

### 保存与重置

- 存档会保存 `complexity`、`solution`、`additionalDelay`，读档后原样恢复。
- `resetSolutionProgress()` 清空分析轮次；管理员重置密码时防火墙同步重置。

## 追踪系统内部（TraceTracker）

追踪时间在 `Computer` 构造时计算：

```
traceTime = max(10 − 安全等级, 3) × 15s
```

等级 4 → 90s；等级 10 → 45s；最低 45s。

### 启动与停止

- 触发：目标主机执行 `hostileActionTaken()` 且连接者就是目标 IP 时 `traceTracker.start()`。
- 停止条件（`Update` 内）：
  - 断开连接或切换目标 → 若剩余时间 <0.5s，解锁 **`trace_close`** 成就。
  - 计时器归零 → `os.timerExpired()`（游戏结束/危险序列）。
- **TraceKill.exe**：持续运行会冻结追踪（`timeSinceFreezeRequest = 0`，追踪在 <0.2s 内不推进）；RAM 600；不追踪时不冻结。

### 警报与表现

- 剩余百分比 `timer/startingTimer×100`，红色 `TRACE :` 文本显示在左下角。
- 报警节奏随剩余量加快：>45% 每 10%、15~45% 每 5%、<15% 每 1% 响一声 `SFX/beep` 并闪屏。
- **SignalScramble.exe**（DLC 的 `DLCTraceSlower`，RAM 50→600 预热）运行时把 `trackSpeedFactor` 压到接近 0（抑制追踪），退出/被杀恢复 1×。另有 `AllTraceTimeSlowed` 全局减速开关（默认关闭，仅调试）。

## 管理员类型（Administrator）

断线后管理员反应取决于类型（`Computer` 的 `<admin type="...">`）：

| 类型 | 行为 |
|---|---|
| `BasicAdministrator` | 随机延迟 0~20s 后关闭全部端口、重置管理员 IP、重置防火墙；`ResetsPassword` 时重置密码 |
| `FastBasicAdministrator` | 立刻关端口/重置防火墙/重开代理；延迟后才重置密码与 admin IP；`IsSuper` 则立即全重置 |
| `FastProgressOnlyAdministrator` | 类似 FastBasic，但**不动密码**（只关端口、重置防火墙、重开代理）；追踪踢出也会触发 |

共同点：断线或追踪踢出后，已破端口会重新关闭，必须重新破解。`traceEjectionDetected` 默认空实现，`FastProgressOnly` 覆写为直接按断线处理。

## Shell 与代理过载（ShellExe）

Shell 有 `Overload` 与 `Trap` 两种模式：

- **Overload**（压测代理）：`state=1`，每秒扣减目标 `proxyOverloadTicks`；归零后 `proxyActive=false`，代理解除。压测期间目标持续触发 `hostileActionTaken()`（可能被追踪）。RAM 基础 40。
- **Trap**（陷阱）：`state=2`，RAM 升至 100，可对目标服务器执行 `forkBombClients` 连接洪泛。
- 日志：开壳 `[IP]_Opened_#SHELL`、压测 `#SHELL_Overload_@_<IP>`、陷阱激活 `#SHELL_TrapActivate_:_ConnectionsFlooded`、关壳 `#SHELL_Closed`。
- 若管理员权限丢失或目标禁用，Shell 立即报错退出（`SHELL ERROR: Administrator account lost`）。

## 代理时间计算（openPortsForSecurityLevel）

等级 ≥5 时按公式累加代理总时长：

```
for i in 4..安全等级: proxyTime += BASE_PROXY_TICKS / (i - 3)
```

等级越高代理越久；`proxyTime` 存入 `startingOverloadTicks` 并随存档保存。代理激活期间无法 `PortHack`，必须先 Overload 压完。

> 相关页面：[核心机制](./Mechanics.md) · [服务器服务（Daemon）](./Daemons.md) · [程序与工具大全](./Programs.md)