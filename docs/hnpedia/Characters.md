
# 人物与剧情档案

本页整理了 Hacknet 主线剧情中的主要人物与组织，信息基于游戏源码中的任务、邮件与对话数据。


## 人物总览

| 人物/组织 | 身份 | 在剧情中的作用 |
|---|---|---|
| **Bit** | 神秘黑客 | 开场即与你联系，全程引导主线 |
| **Entropy** | 黑客行动主义组织 | 玩家最先加入的阵营，发布早期任务 |
| **Naix** | 敌对黑客 | 顺着你的日志反入侵，是主线的关键转折 |
| **CSEC** | 雇佣黑客组织 | 后期招募你，运营任务板 |
| **V** | CSEC 相关联系人 | 发布 bit 系列任务（bit:初章 ~ bit:阻止） |
| **/el** | 消息板运营者 | lelzSec 板块的幕后运营者 |
| **Polar Star / 北极星** | 组织 | 一组"试炼"性质的服务器 |
| **X-C Proj. : Bradford** | 项目联络人 | Entropy 线的项目联系人 |
| **Tom Wilkins** | — | Hacknet-os.com域名所有者 |
| **Matt Trobbiani** | — | 游戏创作者 |

### DLC（Labyrinths）角色

| 人物/组织 | 身份 | 在剧情中的作用 |
|---|---|---|
| **Bibliotheque** | 黑客团队 | DLC 主线阵营（"图书馆"） |
| **Kaguya** | Bibliotheque 领导 | 负责管理/侦察，DLC 的主线推动者 |
| **D3f4ult** | Bibliotheque 成员 | 解密 / SSL / 木马专家 |
| **Coel** | Bibliotheque 成员 | DLC 终局背叛团队的关键人物 |
| **Striker** | 大学学生 | 玩家攻击目标，被删作业后发邮件骂人并反攻 |
| **Anon** | 匿名黑客 | 暗处帮玩家清理日志、查明 Nisei MK III |
| **Crypsis** | 委托组织 | 委托解密工具的第三方 |
| **Psylance** | 安全公司 | 调查 PacificAir 航班相关的安防公司 |


## 主要人物

### Bit 🅱️

- **首封邮件**: 《First Contact》
- **关键台词**:
  > "I don't know you, and I'm sad to say that I never will, but if you're reading this it means you might be the only person that can make things right."
  > "Right now I'm trapped. There's no way out, and not enough time, and I need your help."

Bit 是最神秘也最重要的角色。他从头到尾引导你完成一系列任务（`BitMissionIntro` → `BitMission0/1/2` → `BitPath` 终局），把你卷入 Entropy、CSEC、EnTech 的漩涡。

**真相线索**：游戏数据文件 `BitSpeech.txt` 中，Bit 留下一段以 **"14 DAY TIMER EXPIRED : INITIALIZING FAILSAFE"** 开头的自白：

> "My name is Bit, and if you're reading this, I'm already dead."

这暗示 **Bit 在你读到这些文字时已经遇害**——他设置了一个定时故障保险机制，在失联 14 天后自动把信息逐步发给下一个接手的人。终局剧情（`BitPath` / EnTech 公司）会揭示 Bit 生前的工作与遭遇。

### Entropy

- **发件人**: `Entropy`（14 封）、`Entropy Mailbot`（8 封）
- **角色**: 玩家加入的**第一个黑客组织**，崇尚混乱的黑客行动主义。

经 Bit 的测试服务器引荐，你为 Entropy 干活（`EntropyMission1.1 → 1.2 → 2 → 3`），依次拿下 PP Marketing、PointClicker、X-C Project 等目标。SlashBot 新闻对他们评价负面。

**转折点**：正是在 Entropy 阶段，你的一次入侵留下日志，引来了 **Naix** 的反入侵——这是主线剧情的分水岭。

### Naix

- **发件人**: 4 封（含 `naix@jmail.com`）
- **相关服务器**: **Naix Root Gateway** `173.194.35.172`（密码 `roxxane`）、`Proxy_Node-X04` `173.194.35.163`、`KyonBox` `176.194.38.111`
- **角色**: 玩家的**对手**。在 Entropy 阶段顺着你留下的日志**反入侵了你的电脑**，是主线剧情的重要转折。

被入侵后，你有两条路：

1. **回脏话**：在 Naix 的 "gg wp" 邮件回复任意脏话（如 `fuck`），Naix 回以"rude response"（成就 `rudeNaixResponse`），你留在 Entropy 继续剩余任务；
2. **反黑**：顺着日志反追踪回去，黑进 Naix Root Gateway，删掉 `sys/x-server.sys`，逼 Naix **认输**——他发来 **"gg wp"** 邮件：
   > "Ok, ok, I get it. Fair enough. Stop messing with my shit."
   > "A bit sloppy leaving those logs on your box, but, seriously, it kinda looked like you had no idea what you're doing."

认输之余 Naix 还给你留了个"测试"——去把安全公司 **Nortron** 的网站改成满屏 "DICKS"，由此把你引向 lelzSec 与北极星。

### CSEC

- **发件人**: `CSEC Hub Services`（17 封）、`CSEC Admin`（5 封）
- **角色**: 后期正式招募你的**雇佣黑客组织**，运营任务板（Hub）。

在 Entropy（或反黑后的 lelzSec / 北极星）阶段完成后，两条路线**殊途同归**，CSEC 向你发出招募。通过 **试炼（Gauntlet）** 自动测试（`CSEC Crossroads Server` → `CSEC Invitation Gauntlet` → `CSEC Gauntlet 02/03`）和任务板的 **KFC（CFC 公司）** 系列任务后，获得访问 **CSEC** 主服务器（密码 `admin`）的权限，开始承接 `#CLIENT#` 匿名客户的任务。

### V

- **发件人**: 9 封
- **角色**: **CSEC 阶段后期出现的关键联系人**，Bit 的朋友。在 CSEC 任务板任务推进到后期，V 开始向你发布 **bit 系列**任务（"bit:初章" ~ "bit:阻止"），引导你潜入 EnTech，揭开 Bit 生前的真相——这直接通向主线终局。

### /el（slash bot 运营者）

- **相关服务器**: `/el Message Board`（sec=6，全游戏最高安全等级之一）、`/el's Secure SecuLock Drive`（密码 `ithoughtyouweremakingtea`）
- **角色**: lelzSec 消息板的运营者，掌握着高端信息渠道。在"反黑 Naix"分支里，玩家会经 /el 论坛接触 lelzSec 与北极星试炼。

### Polar Star / 北极星

- **发件人**: `Polar Star`（2 封）
- **相关服务器**: `Shrine of the Polar Star` → `Trial of Patience/Haste/Diligence/Focus` → `Head of the Polar Star`
- **角色**: 一套以"试炼"命名的组织，考验玩家的耐心、速度、勤奋与专注。出现在"反黑 Naix"分支之后。


## DLC（Labyrinths）人物

### Bibliotheque（图书馆）📚

- **Hub 服务器**: `Bibliotheque` `69.58.186.114`（sec=6）、`Bibliotheque DropServer` `69.58.186.118`（sec=6，密码 `ka2gs69`）
- **角色**: DLC 主线阵营。通过 Kaguya 试炼后加入的黑客团队，用 DHS 任务板发布委托。

### Kaguya 🐇

- **相关服务器**: `Kaguya_Gateway` `54.183.231.31`、`Kaguya_Projects` `54.183.231.34`（均为 sec=4）
- **角色**: Bibliotheque 的**领导**，负责管理、侦察和协调。试炼通过后你成为第三个加入者。

### D3f4ult

- **角色**: Bibliotheque 成员，**解密 / SSL / 木马专家**。负责 Build Decryption Tools、Decypher Pacific Air Protocols 等解密类任务。

### Coel ⚠️

- **相关服务器**: `Coel__Gateway` `208.73.210.214`（sec=4，密码 `colamaeleon`）
- **角色**: Bibliotheque 成员，**DLC 终局的反派/叛徒**。在坠机危机的关键时刻，她试图关掉 Bibliotheque 频道、打掉 D3f4ult 的网关（Finale 里她被 ban 踢出频道："Looks like Coel was trying to shut us down"）。

### Striker

- **相关服务器**: `Striker_Battlestation` `72.60.4.119`（sec=3）、`Striker Proxy` `72.52.4.119`（sec=6）、`Striker Cache` `72.52.4.117`（sec=3）
- **角色**: 玩家的攻击目标——你删掉了他的大学作业。他发来骂人邮件（"Fuck you. Love, StrikeR"），并反过来对你的 PC 发动 **VM 级攻击**报复。

### Anon 🕵️

- **角色**: 匿名黑客。在一架 PacificAir 航班（PA_0022）上发现你留下的日志后主动帮你清理，还顺藤摸瓜查出了 **Nisei MK III** 的危险实验报告，确认你的团队已销毁其制造文件。他在暗处帮你擦屁股、善后。

### Crypsis

- **角色**: 另一个委托组织。在 DLC 早期委托 Bibliotheque 制作解密工具（Build Decryption Tools）。

### Psylance

- **相关服务器**: `Psylance Cyber Security_Employee IRC` `50.31.225.93`（sec=5）、`Psylance Internal Services/Archives`（sec=4）
- **角色**: 一家安全公司，正在调查 PacificAir 航班相关系统，是 DLC 空航线段的主要对手（Subvert Psylance Investigation）。


## 剧情时间线

```
Bit 初次见面系列邮件（First Contact → 删 SecurityTracer.exe → BitMission 0/1/2）
    ↓
加入 Entropy（EntropyMission 1.1 → 1.2 → 2 → 3）
    ↓
玩家被 Naix 入侵 ★转折
    ↓
抉择：
  ├─ A. 回复脏话 → Entropy 剩余任务
  └─ B. 反黑（删 Naix 的 sys/x-server.sys → "gg wp" 认输）
         → Nortron 系列 → /el 论坛 → lelzSec → 北极星试炼
    ↓
归拢 → CSEC（邀请 → 自动测试 Gauntlet → 肯德基 KFC）
    ↓
CSEC 任务板系列任务（#CLIENT# 匿名委托）
    ↓
V 系列（bit:初章 ~ bit:阻止）→ EnTech 公司 → Bit 的真相
    ↓
结局 → Credits Server（226.187.99.3）
```

> 💡 在 Entropy 与 CSEC 阶段之间，可选择参加 **Labyrinths DLC** 的试炼及其任务（涉及坠机调查、Striker 角色线）。


## 背景设定速记

- **HacknetOS**：游戏中的系统环境，按 Bit 的说法"本不该以现在的形态发布"——其中内置了会自动激活的追踪器。
- **EnTech**：终局剧情的关键公司，Bit 生前与之相关，其基础设施（`156.151.x.x` 网段）是终局任务的主要战场。
- **Bitwise**：与 Bit 相关的服务器集群（`Bitwise Repo Base`、`Bitwise Drop Server`、`Bitwise Relay 01`），藏有揭示 Bit 身份与结局的关键资料。
- **清日志**：游戏中最重要的习惯。Naix 之所以能入侵你，正是因为你入侵时留下的日志。
