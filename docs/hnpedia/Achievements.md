
# 成就列表

Steam 版本的 Hacknet 存在成就系统，DLC 没有成就。下表为游戏代码中的全部 12 个成就，触发条件提取自游戏源码（`AchievementsManager.Unlock(...)` 调用位置）。

> 代码中共 12 个成就，但基础游戏仅有 11 个成就。其中 `progress_lelz` 没有对应的Steam成就。

| 成就 ID | Steam 中对应成就名 | Steam 中对应成就描述 | 触发条件 | 说明 |
|---|---|---|---|---|
| `kill_tutorial` | Quickdraw | Complete the tutorial earlier than normal | 提前 kill 教程 | 在 Tutorial 流程结束前 kill 其进程解锁 |
| `clock_run` | TRUE ULTIMATE POWER | Run Clock.exe | 运行 Clock.exe | 运行游戏内的时钟彩蛋程序 |
| `themeswitch_run` | Makeover! | Run ThemeSwitcher | 运行 ThemeChanger.exe | 运行主题切换程序 |
| `pointclicker_basic` | PointClicker | Purchase the final PointClicker upgrade | 在 PointClicker 中购买最后一个升级 | 升级完 PointClicker 的改造项 |
| `pointclicker_expert` | You better not have clicked for those... | Have a truly absurd number of pointclicker points | 使 PointClicker 的点数溢出 | 点数溢出会变为负数 |
| `progress_entropy` | Entropy | Be granted an account within Entropy | 加入 Entropy | 完成 Entropy 试炼/入伙 |
| `progress_csec` | CSEC | Be granted an account within CSEC | 加入 CSEC | 加入 CSEC，Entropy 线或 lelzSec 线均可 |
| `progress_lelz` | 无 | 无 | 加入 lelzSec | 进入 /el 论坛所在的 lelzSec 线 |
| `rude_response` | Rude | That's just impolite | 对 Naix 回复脏话 | 被 Naix 入侵后，在 "你在耍我吗？" 邮件回复f**k；源码 trigger 名为 `rudeNaixResponse` |
| `secret_path_complete` | /el Sec Champion | Master of /el's friendly community | 完成 lelzSec 隐藏线 | 反黑 Naix 后走完 lelzSec 线（源码 trigger 名为 `lelzSecVictory`） |
| `trace_close` | To the Wire | Disconnect from a traced system with less than half a second to spare | 惊险逃脱追踪 | 被系统追踪时在 0.5 秒内完成逃脱（`timer < 0.5`） |
| `progress_complete` | Hacknet | Complete Bit's final request | 主线通关 | 摧毁 Porthack.Heart、看完结局 |

> 你知道吗：Hacknet 的汉化组为 Hacknet 做了 Steam 成就的中文翻译，但最终未被采用。Steam 上的 Hacknet 成就仍为英文版本。此处附 Steam 成就汉化版链接：[成就汉化](https://github.com/ShiveryMoon/HacknetLocalization/blob/master/%E6%88%90%E5%B0%B1%E6%B1%89%E5%8C%96.txt)

## 彩蛋成就的小提示

- **`clock_run`**：Jmail是关键。此外还与最终幻想10有关。
- **`rude_response` 与 `secret_path_complete`**：此处对于部分玩家可能存在误区，即认为骂完 Naix 后不能再进入 Naix 线，但其实根据被攻击留下的日志反黑回去找到 Naix 的网关删掉他的 x-server.sys 后仍可进入 Naix 线。
- **`trace_close`**：试试`dc`，建议在加入 CSEC 前完成，否则若失败则会被迫重置 IP。

> 详见 [全流程攻略](./Walkthrough) 的抉择章节与 [服务器速查表](./Servers)。
