# 黑客技能与派系进流程

Hacknet 的"进展"围绕各黑客组织的**声望值（rank）**展开：完成任务加分，达标升级，升级解锁新任务与工具。数据提取自游戏源码 `Faction.cs` 及各派系子类。

## 声望值（Rank）机制

每个派系都有自己的**玩家价值**（playerValue）与**达标值**（neededValue）：

- 初始 `playerValue = 0`，通过完成任务获得 `addRank` 加分
- **Rank 与数值成反比**：`playerValue` 越接近达标值，Rank 越低（1 级最高，100 级最低）
- 公式：`rank = max(1, (int)((1 - playerValue/neededValue) * 100))`

| 派系 | 达标值 | 玩家价值 → Rank |
|---|---|---|
| **Entropy** | 5 | 0→100 · 1→80 · 2→60 · 3→40 · 4→20 · ≥5→1 |
| **CSEC** | 10 | 0→100 · 1→90 … · 9→10 · ≥10→1 |
| **lelzSec** | 1000 | 0→100 · 500→50 · ≥1000→1 |

## 完成任务加分

- 任务 XML 中的 `<missionEnd val="N">` 决定完成奖励的数值
- 完成时执行 `addRank N`：给当前派系加 N 值，并发送 **"Contract Successful"** 邮件（含当前 Rank/最大 Rank/派系名）
- `<missionStart val="N">` 同理可在任务开始时加分
- 常规任务每次多为 **+1**；另有 `addRankSilent`（加分不发邮件）、`addRankFaction:<id>`（给指定派系加分，不切换当前派系）

## 弃约惩罚

- 在 CSEC 任务中心点 **Abandon Contract** 会发送"Contract Abandoned"邮件
- **实际上基础游戏弃约不扣分**（`PlayerLosesValueOnAbandon` 对所有内置派系均为 `false`），不用担心弃约损失声望
- 自动生成任务同样支持弃约

## 各派系升级奖励

### Entropy（达标值 5）

| 阈值 | 奖励/动作 |
|---|---|
| value≥3 | 触发 eOS 剧情线（`eosPathStarted`） |
| value≥4 | 注入 DLC 过渡任务（装 DLC 时） |
| value≥5 | 加载 `ThemeHackTransitionMission.xml` 过渡任务（引向 Naix 反黑线） |

### CSEC（达标值 10）——奖励最丰富

| 阈值 | 奖励/动作 |
|---|---|
| value≥1 | 发放 **ThemeChanger.exe**（`mainHubAssets/bin`）+ 通知邮件 |
| value≥4 | 向任务板注入 Bit 线任务组（`BitHubSet01.xml`） |
| value≥7 | 发 **Project Junebug** 邮件（"Flagged for Critical Contract"） |
| value≥10 | 启动 **Bit 主线**（ForceStartBitMissions）：发放 **Decypher.exe**、**DECHead.exe**、**KBT_PortTest.exe** 到 `mainHubAssets/bin/Misc` |

### lelzSec（达标值 1000）

- 无特殊奖励代码，纯数值累计
- 完成 lelzSec 线解锁成就 `secret_path_complete`

### Bibliotheque（DLC）

- 使用 `CustomFaction` 从 `DLC/DLCFaction.xml` 加载，由 XML 配置触发条件与奖励

## 任务门槛

- 任务 XML 可设置 `<posting requiredRank="N">`：玩家的 `playerValue` 低于该值时，任务灰置不可接——形成软进度门控
- `<posting difficulty="N">` 仅为元数据，不参与奖励计算

## 派系对应关系

| 游戏内阵营 | 源码类 | 达标值 | 加入方式 |
|---|---|---|---|
| **Entropy** | `EntropyFaction` | 5 | Bit引导玩家加入 |
| **CSEC** | `HubFaction` | 10 | 通过测试后由系统指派 |
| **lelzSec** | `Faction`（基类） | 1000 | 反黑线完成后指派 |
| **Bibliotheque**（DLC） | `CustomFaction` | 取自 XML | DLC 试炼通过 |

> 派系之间可通过 `setFaction:<id>` 切换；每派系的声望独立保存。

> 相关页面：[组织历史](./Organizations.md) · [全流程攻略](./Walkthrough.md) · [程序与工具大全](./Programs.md) · [成就列表](./Achievements.md)