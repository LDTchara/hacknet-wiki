
# Hacknet 主线全流程攻略

本攻略按 Hacknet 主线实际推进顺序整理，涉及的关键服务器、IP、密码均来自游戏真实数据。

> 提示：全程记得**清日志**——入侵后 `cd log` → `rm *`。你留下的日志不仅是破绽，甚至会在剧情里成为别人反攻你的把柄。

## 剧情线总览

```
Bit 初次见面系列邮件
    ↓
Entropy（加入黑客活动组织）
    ↓
玩家被 Naix 入侵 ★剧情转折
    ↓
抉择：
  ├─ A. 回复脏话 → Entropy 剩余任务
  └─ B. 通过 log 反黑回去（删 Naix 的 x-server.sys）→ /el 论坛 → 北极星
    ↓
归拢 → CSEC（邀请 → 自动测试 → 肯德基）
    ↓
CSEC 任务板系列任务
    ↓
V 系列（bit:初章 ~ bit:阻止）
```

> 💡 在 Entropy 和 CSEC 阶段之间，可以选择参加 **Labyrinths DLC** 的试炼及其任务（独立剧情线）。


## 第一章 · Bit 初次见面系列邮件

**任务文件**: `BitMissionIntro` / `BitMission0/1/2` | 发件人：**Bit**

游戏开场，一位自称 **Bit** 的神秘人发来邮件 **《First Contact》**：

> "I don't know you, and I'm sad to say that I never will, but if you're reading this it means you might be the only person that can make things right."
> "Right now I'm trapped. There's no way out, and not enough time, and I need your help."

**任务 1**：连接你自己的节点（netMap 上绿色的那台），在 `bin` 里找到并删除 `SecurityTracer.exe`——Bit 说 HacknetOS 里内置的自动追踪器不该被发布出来。

**任务 2~3**（Bit 系列）：Bit 会一步步引导你破解：
- **Bitwise Test PC**（sec=1）— 学习 scan / probe / SSHcrack / PortHack 基础
- **P. Anderson's Bedroom PC**（sec=1）— 进一步练习

**任务 4**：Bit 让你去找 **Entropy 的测试服务器**——`Entropy test Server` `199.59.149.230`（sec=1，带代理）。这正是引出 Entropy 线的一步。


## 第二章 · Entropy

**任务文件**: `Entropy/EntropyMission1.1 → 1.2 → 2 → 3` | 发件人：**Entropy / Entropy Mailbot**

通过测试服务器，你接触到 **Entropy**——一群崇尚混乱与黑客行动主义的组织。SlashBot 新闻对他们评价负面。帮 Entropy 干活的阶段，你依次拿下：

| 服务器 | IP | 安全等级 | 防御 |
|---|---|---|---|
| Entropy Asset Cache | 动态 | 1 | 防火墙 |
| Slash-Bot News Network | 动态 | 1 | 代理 |
| PP Marketing Inc. | `74.125.237.119` | 2 | 防火墙 + 代理 |
| PointClicker | 动态 | 2 | 防火墙 + 代理 |
| X-C Project Tablet | 动态 | 2 | 防火墙 + 代理 |
| Milburg High IT Office | 动态 | 2 | 防火墙 + 代理 |
| Entropy Asset Server | 动态 | 4 | — |
| Entropy Contract Database | 动态 | 5 | — |

**相关密码**：X-C Project Tablet 管理员密码 `cyberdisk`。

> ⚠️ 到这一步，你已经开始给 Entropy 卖命。但接下来发生的事，会彻底改变游戏的走向——**你被人反入侵了**。


## 第三章 · 玩家被 Naix 入侵 ★剧情转折

**任务文件**: `Theme/ThemeHackStartMission` | 发件人：**Entropy（Warning）**

一次针对 `Proxy_Node-X04`（`173.194.35.163`）的入侵后，你收到 Entropy 的 **《Warning》** 邮件：

> "Connect-Responder.exe has noticed a foreign connection from your IP address to 173.194.35.163. External connections are prohibited on this device."

你留下的 **log** 成了破绽——一名叫 **Naix** 的黑客顺着日志反入侵了你的电脑。这是主线的重要转折点。

**当前处境**：你的机器可能已被 Naix 动过手脚，系统文件被动过。此时面临**抉择**。


## 第四章 · 抉择：两条分支

面对 Naix 的入侵，你有两条路：

### 分支 A · 回复脏话 → Entropy 剩余任务

在 Naix 的 **"gg wp"** 邮件上**回复任意脏话**（比如 `fuck`），向 Naix 表明态度。

- 触发 Naix 的"rude response"剧情（对应成就 `rude_response`，源码 trigger 名为 `rudeNaixResponse`）
- 你继续留在 Entropy 阵营，完成 **Entropy 的剩余任务**
- 属于相对"顺走"的路线，不深挖 Naix 那条支线

### 分支 B · 通过 log 反黑回去 → /el 论坛 → 北极星

从**你自己的日志**里找出 Naix 的痕迹，反追踪回去：

1. 黑进 **Naix Root Gateway** `173.194.35.172`（管理员密码 `roxxane`）
2. 找到并**删除 `sys/x-server.sys`**——没有它，Naix 的整台系统直接崩掉
3. Naix 认输，发来 **"gg wp"** 邮件：
   > "Ok, ok, I get it. Fair enough. Stop messing with my shit."
   > "A bit sloppy leaving those logs on your box, but, seriously, it kinda looked like you had no idea what you're doing."
   > 附赠一个"测试"：去把垃圾安全公司 **Nortron** 的网站改成满屏 "DICKS"。
4. 由此进入 **lelzSec** 线：
   - **Nortron 系列**：Nortron Mail Server（sec=1）、Web Server（sec=2）、Internal Services（sec=2）、Mainframe（sec=5），全是防火墙
   - **/el 论坛**：`/el Message Board`（sec=6，全游戏最高安全等级之一）；要撬 `/el's Secure SecuLock Drive`（`69.172.201.208`，密码 `ithoughtyouweremakingtea`）和 **Stormrider**（`199.89.130.68`，同密码）
   - **北极星试炼**（Polar Snake）——一组考验耐心/速度/勤奋/专注的试炼：

| 试炼 | IP | 安全等级 | 特点 |
|---|---|---|---|
| Shrine of the Polar Star | `103.31.7.34` | 0 | 起点 |
| Trial of Patience | `103.31.7.38` | 1 | 代理 |
| Head of the Polar Star | `103.33.8.162` | 1 | — |
| Tail of Diligence | `103.31.8.2` | 2 | 防火墙 |
| Trial of Haste | `103.31.7.41` | 3 | 代理 |
| Trial of Diligence | `103.31.8.1` | 4 | 防火墙 |
| Trial of Focus | `103.31.7.211` | 5 | 防火墙 + 代理 |

### 归拢

无论选 A 还是 B，两条分支在完成后**殊途同归**——都会把你导向 **CSEC**。


## 第五章 · CSEC：邀请、自动测试、肯德基

**任务文件**: `MainHub/Intro`、`MainHub/KFC` | 发件人：**CSEC Hub Services**

CSEC 是一个专业的雇佣黑客组织。通过一系列考验后，CSEC 正式接纳你：

1. **邀请** — CSEC 向你发出招募邀请
2. **自动测试**（Gauntlet 试炼）— 依次拿下：
   - CSEC Crossroads Server `101.0.89.154`（sec=0，入口）
   - CSEC Public Drop Server（sec=3，代理）
   - CSEC Invitation Gauntlet（sec=2，防火墙+代理）
   - CSEC Gauntlet 02（sec=2，代理）
   - CSEC Gauntlet 03（sec=3，防火墙+代理）
3. **肯德基（KFC）** — 任务板上的 CFC 公司系列任务：
   - **www.cfc.com**（sec=2）
   - **CFC Records Repository** `202.7.177.65`（sec=2，防火墙+代理）
   - **CFC Corporate Mainframe**（sec=5，防火墙+代理）

通过后获得访问 **CSEC** 主服务器（管理员密码 `admin`）和 **CSEC Assets Server**（密码 `csec-5xg3D11`）的权限，任务板向你完全开放。


## 第六章 · CSEC 任务板系列任务

**发件人**: CSEC Hub Services / #CLIENT#（匿名客户）

自由接单阶段，围绕几张地图展开：

### Decypher 系列（解密）
- DEC Solutions Web Server `101.0.89.154`（sec=2，防火墙+代理）
- Joseph Scott's Battlestation `101.0.89.154`（sec=3）
- DEC Solutions Mainframe `101.0.89.154`（sec=5，防火墙+代理）
- Macrosoft Workhorse Server 04 `168.61.82.245`（sec=3，防火墙+代理）
- Macrosoft Storage Server `168.61.82.246`（sec=5）

### Pacemaker 系列（医疗起搏器）
- KBT-PM 2.44 `202.6.141.219`（sec=1）
- Kellis Biotech Client Services `66.96.147.84`（sec=4，防火墙）
- Kellis Biotech Production Asset `66.96.148.1`（sec=4，防火墙）
- Eidolon Soft Production Server `111.105.22.1`（sec=4，防火墙）

### 核心数据库（中后期解锁）
- ISP Management Server `68.144.93.18`（sec=5，防火墙+代理，密码 `tracert`）
- Death Row Records Database `168.51.178.4`（sec=5，防火墙+代理，密码 `texas`）
- Universal Medical `208.93.170.15`（sec=5，防火墙+代理，密码 `codeine`）
- International Academic Database `129.67.0.11`（sec=5，防火墙+代理，密码 `techtonic`）
- JMail.com `74.125.237.119`（sec=5）
- Timekeeper's Vault `24.43.69.78`（sec=5）
- www.reddit.com（sec=4）／ Network Education Archives（sec=6）
- Jason's LackBook Pro `37.187.27.55`（sec=2，密码 `DANGER`）


## 第七章 · V 系列（bit:初章 ~ bit:阻止）

**任务文件**: `BitPath/BitAdv_Intro01~08 → BitAdv_Recovery → BitAdv_ZFinale01` | 发件人：**V**

CSEC 任务板刷到后期，**V** 开始向你发布 **bit 系列**任务（"bit:初章" → "bit:阻止"）——这其实是**主线终局**。你要潜入 **EnTech** 公司的整套基础设施，揭开 Bit 生前的真相：

| 服务器 | IP | 安全等级 | 管理员密码 |
|---|---|---|---|
| EnTech Workstation Core | `156.151.0.0` | 1 | — |
| EnTech Workstation _001~_012 | `156.151.0.x` | 5 | — |
| En_Prometheus | `156.151.1.1` | 5 | `d88vAnnX` |
| En_Romulus | `156.151.1.12` | 5 | `h7ggNKl2` |
| EnTech Web / Mail / Relay | `156.151.59.x` | 4~5 | `ax889msjA` |
| EnTech_Offline_Cycling_Backup | 动态 | 5 | 防火墙+代理 |
| Bitwise Repo Base | `23.236.62.147` | 5 | `tenzen` |
| Bitwise Drop Server | `108.160.165.139` | 5 | 防火墙 |

**Bitwise** 系列服务器（`Bitwise Repo Base`、`Bitwise Drop Server`、`Bitwise Relay 01`）藏有揭示 Bit 身份的关键资料——你会在那里读到 Bit 留下的自动化遗言（详见 [人物与剧情档案](./Characters)）。

**最终目标**：`Porthack.Heart`（sec=0）——PortHack 系统的核心。摧毁它，"终结 PortHack"，触发结局。


## 结局

结局演职员表在 **Credits Server** `226.187.99.3`。

> 💡 `BitSpeech.txt` 记载了一段 **"14 DAY TIMER EXPIRED : INITIALIZING FAILSAFE"** 的文本——
>
> > "My name is Bit, and if you're reading this, I'm already dead."
>
> 整个剧情都建立在 Bit 失联 14 天后自动触发的"故障保险"之上。


## 可选 · Labyrinths DLC

在 Entropy 与 CSEC 阶段之间，可选择参加 **Labyrinths DLC** 的试炼及其任务——一条独立的剧情线。不打也不影响主线通关。

### D1 · Kaguya 试炼 → 加入 Bibliotheque

DLC 开头，你要通过 **Kaguya 的试炼**（`#ALERT# - The Kaguya Trials`）：

| 试炼 | IP | 安全等级 |
|---|---|---|
| Kaguya Sprint Trial | `74.125.23.121` | 4 |
| Kaguya Push Trial | `216.239.32.181` | 1 |

> "Hey! Nice work - you're the third one through. I'm connecting you to our home base now, hold on."

通过后加入黑客团队 **Bibliotheque（图书馆）**。Hub 服务器（`69.58.186.114`，sec=6）用 DHS 任务板发任务，成员三人：

- **Kaguya** — 团队领导，负责管理、侦察
- **D3f4ult** — 解密 / SSL / 木马专家
- **Coel** — 成员

> 💡 Bibliotheque 的三名 agent 在 Hub 上的登录密码：`colamaeleon`（Coel）、`dj820mmaCb`（Kaguya）、`kanzaz`（D3f4ult）。Drop 服务器 `69.58.186.118`（sec=6）密码 `ka2gs69`。

### D2 · 初入团队（Set 1~2）

- **The Ricer** — 破解 Ricer PC（sec=1），给改装车玩家的电脑"上点颜色"
- **Build Decryption Tools** — **Crypsis**（另一个组织）委托 D3f4ult 做解密工具
- **DDOSer / Trace Grindcraft server outages** — 处理 DDoS 攻击者 r00t_Tek 的服务器

### D3 · Striker 的报复

你接的任务目标之一是 **Striker**（`Striker_Battlestation` `72.60.4.119` sec=3、`Striker Proxy` `72.52.4.119` sec=6、`Striker Cache` `72.52.4.117` sec=3）。删掉他的大学作业后，他气炸了：

> "It was you, wasn't it? You broke in and deleted my uni work. Well, ya know what? Fuck you. FUCK YOU. ... To conclude: Fuck you. Love, StrikeR"

**但报复还没完**——Striker 从攻击转为反攻，你发现自己的 PC 也遭到了 **VM 级攻击**（"@#PLAYERNAME# is setting off warnings all over the place"）。Coel 紧急介入帮你调查，D3f4ult 负责追踪来源。这段剧情呼应主线里"清日志"的教训：你留下的日志又成了破绽。

### D4 · Take Flight（Pacific Air 空航线）

**PacificAir 航空公司**的服务器全套 sec=5（Bookings Mainframe、Mail、Network Hub、Whitelist Authenticator）。团队分工协作：

- 白名单服务器挡路 → 团队配合拉闸几秒 → 趁机进入 Bookings Mainframe
- **Psylance**（Psylance Cyber Security，一家安全公司）正调查这些飞机相关系统
- **Coel - Nisei MK III**：调查中发现了 **Nisei MK III** ——一种危险物质/技术的实验报告

> 👤 **Anon**（匿名者）：他后来在一架飞机上发现了你留下的日志，主动帮你清掉，并顺藤摸瓜查出 **Nisei MK III** 的来龙去脉（"I'm guessing you and your group are why I can't find any actual files on how to produce it"）。他在暗处帮了你一把。

### D5 · 坠机危机（Airline 2 / Finale）★DLC 结局

针对 **Pacific ATC**（航空管制）系统的深入渗透（`Pacific_ATC_RoutingHub`、`Pacific_ATC_Skylink` `208.91.196.94`、`WhitelistAuthenticator`），最终目标是两架航班：

- **PA_747_0022 Flight Computer** `209.15.13.134`（sec=5）
- **PA_747_0018 Flight Computer** `208.73.211.70`（sec=5）

**终局反转**：在危机解决的关键时刻，**Coel 背叛了团队**——她试图关掉 Bibliotheque 的频道、踢掉 D3f4ult 的网关（`*** CHANNEL mode set: +b (ban) for user @Coel`、"Looks like Coel was trying to shut us down"）。

最后的收尾在 **Kaguya_Gateway**（`54.183.231.31`）、**Kaguya_Projects**（`54.183.231.34`）、**Labyrinths_DevChat**（`54.192.133.33`）：你们要确保敌人不会再发动攻击——清掉他们的 `/bin` 和 `/sys`（"I'll take care of D3f4ult, you get Kaguya"）。

### D6 · 其他内容

- **Memory Forensics（内存取证）**：MemForensics 系列任务（1/3 ~ 3/3），用内存取证工具分析服务器；涉及 `Lihota Productions`（`17.5.3.9`，密码 `4TL4S`）、`Snackintosh`（`185.160.171.69`，同密码）、`iodependency~Atlas`（`88.29.144.6`）、`Raven Dataworks`（`208.73.49.146`）等
- **Neopals**：数字宠物公司（主服务器 sec=8，密码 `n8u5v432kj`；认证服务器 sec=8，密码 `mlk3fsdie321`；版本控制 sec=5，密码 `undc321b9y`）
- **Hermetic Alchemists（炼金术士）**：一个神秘的炼金术组织（`School of the Hermetic Alchemists`，HA_Coagula/Solve/Rebis，`66.96.149.16`；管理员 Nate Wesson 的服务器密码见速查表）
- **The Gibson**（💀 终极彩蛋）：安全等级 **99**、双管理员 + 超级管理员、trace 95s、firewall 6、proxy 5s 的怪物服务器。破解成功者会被记入 **Victors.txt**（历代通关玩家榜）
- **Pellium Box**（`69.172.201.153`，Naix 彩蛋）：一台藏着 **Naix 主题文件**的服务器（`x-server_Green.sys`、`x-server_RiptideStandard.sys`、`x-server_StarfieldClassic.sys`、`sys/x-server.sys`）——和主线里 Naix 的 x-server.sys 系统联动，IP 与主线 SecuLock（`69.172.201.208`）同网段
- **PointClicker 2**、**It Follows**、**Cleanup**、**Expo**、**Keebs**、**Bean Stalk** 等支线任务


## 关键密码速查

| 服务器 | 密码 |
|---|---|
| CSEC | `admin` |
| CSEC Assets Server | `csec-5xg3D11` |
| Naix Root Gateway | `roxxane` |
| SecuLock / Stormrider | `ithoughtyouweremakingtea` |
| X-C Project Tablet | `cyberdisk` |
| ISP Management Server | `tracert` |
| Death Row Database | `texas` |
| Universal Medical | `codeine` |
| International Academic DB | `techtonic` |
| Jason's LackBook Pro | `DANGER` |
| Bitwise Repo Base | `tenzen` |
| EnTech 系（Web/Mail/Relay） | `ax889msjA` |
| En_Prometheus | `d88vAnnX` |
| En_Romulus | `h7ggNKl2` |

> 普通用户的密码往往来自常见密码库（`passwords.txt`），破解不了就试试那些常用弱密码。
