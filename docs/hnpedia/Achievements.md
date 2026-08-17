
# 成就列表

Hacknet 的成就通过 Steam 解锁。下表为基础游戏的全部 12 个成就，触发条件提取自游戏源码（`AchievementsManager.Unlock(...)` 调用位置）。

> 基础游戏共 12 个成就。DLC《Labyrinths》另有其专属成就（在 Steam 客户端内查看）。

| 成就 ID | 触发条件 | 说明 |
|---|---|---|
| `kill_tutorial` | 走完新手教程 | 在 AdvancedTutorial 流程结束时解锁 |
| `clock_run` | 运行 ClockV2.exe | 运行游戏内的时钟彩蛋程序 |
| `themeswitch_run` | 运行 ThemeChanger.exe | 运行主题切换程序 |
| `pointclicker_basic` | PointClicker 通关基础模式 | 升级完 PointClicker 的改造项 |
| `pointclicker_expert` | PointClicker 进入专家模式 | 在专家模式中得分（`points <= -1` 分支） |
| `progress_entropy` | 加入 Entropy | 完成 Entropy 试炼/入伙 |
| `progress_csec` | 加入 CSEC | 通过 CSEC 的 Gauntlet 试炼 |
| `progress_lelz` | 加入 lelzSec | 进入 /el 论坛所在的 lelzSec 线 |
| `rude_response` | 对 Naix 回复脏话 | 被 Naix 入侵后，在 "gg wp" 邮件回复任意脏话；源码 trigger 名为 `rudeNaixResponse` |
| `secret_path_complete` | 完成 lelzSec 隐藏线 | 反黑 Naix 后走完 lelzSec 线（源码 trigger 名为 `lelzSecVictory`） |
| `trace_close` | 惊险逃脱追踪 | 被系统追踪时在 0.5 秒内完成逃脱（`timer < 0.5`） |
| `progress_complete` | 主线通关 | 摧毁 Porthack.Heart、看完结局 |


## 彩蛋成就的小提示

- **`clock_run`**：游戏内藏着运行时钟程序（ClockV2.exe）的服务器。找到它运行一次就能拿到。
- **`rude_response` 与 `secret_path_complete`**：对应主线抉择的两条分支——在 Naix 的 "gg wp" 邮件回脏话，或反黑回去删 `x-server.sys`。后者走通 lelzSec 线即为隐藏路径。
- **`trace_close`**：把 `tracert` 留着，追踪警报响起的瞬间断开连接——离成功只差半秒。

> 详见 [全流程攻略](./Walkthrough) 的抉择章节与 [服务器速查表](./Servers)。
