# Labyrinths DLC 攻略

《Hacknet》付费 DLC **Labyrinths**（迷宫）是一条独立于主线之外的剧情线。它围绕黑客 **Kaguya** 发出的"试炼"招募展开——通过一系列难度不低的测试后，你会加入一支名为 **Bibliotheque（图书馆）** 的小团队，与她、**Coel**、**D3f4ult** 一起执行合同任务。DLC 拥有独立的主线（航空公司线）、新的组织（Crypsis、Psylance、炼金术师）与新的工具机制（内存取证、合同系统、IRC 协作）。

> 💡 不打 Labyrinths 不影响主线通关。你可以在主线 Entropy 与 CSEC 阶段之间任意时间进入，中途退出也不会损失任何东西。

---

## 🚪 入口：Kaguya 的试炼

DLC 的入口来自 **Tex** 转来的"Kaguya 的试炼"招募挑战（Entropy 与 CSEC 都会转发这条消息）：

> "我收到一个链接，看起来这是某人想组建一个团队，要进行招募测试。有人知道这个 Kaguya 是谁吗？我从没听过这个名字。"

**Kaguya** 自称是 DigiSec 社区领导者、多个派系的管理员，向数个知名黑客社区发出了招募挑战。测试本身只给了一个 IP 地址和一句"开始试炼"。

### 试炼目标

| 试炼 | IP | 安全等级 | 说明 |
|---|---|---|---|
| **Kaguya Sprint Trial** | `74.125.23.121` | 4 | 速度试炼 |
| **Kaguya Push Trial** | `216.239.32.181` | 1 | 推进试炼 |

通过全部试炼后，你会收到加入信息：

> "Nice work — you're the third one through. I'm connecting you to our home base now, hold on."

至此你正式加入 **Bibliotheque**，获得 **Hub 服务器**（`69.58.186.114`，sec=6）的访问权限——团队通过 DHS 任务板在此发放合同。

> 🗝 三名 agent 在 Hub 上的登录密码：`colamaeleon`（Coel）、`dj820mmaCb`（Kaguya）、`kanzaz`（D3f4ult）。Drop 服务器 `69.58.186.118`（sec=6）密码 `ka2gs69`。

---

## 🏛 Bibliotheque 团队

**Bibliotheque**（法语"图书馆"）是 DLC 中的团队化组织——不是传统黑客组织，更像一支分工明确的小队，通过 **IRC 频道**协作。详见 [组织历史](./Organizations.md)。

| 成员 | 角色 |
|---|---|
| **Kaguya** | 团队领导，负责管理、侦察；"确保大家各司其职，实现我们的目标" |
| **Coel** | 前线执行成员（航空线核心，结局有重大剧情） |
| **D3f4ult** | 解密 / SSL / 木马专家 |

### 协作机制

- **合同（Contract）**：按先到先得分配，玩家可随时在任务栏接取
- **IRC 频道**：团队成员的日常沟通与任务协作都在 IRC 进行
- **共享服务器**：团队把工具、下载到的数据上传到 Hub 的共享文件夹

---

## 🎯 任务线总览

DLC 任务分几条线，多数为**团队协作**——你负责核心渗透，Coel / D3f4ult 在后台同步推进。

### ✈ 航空公司线（Airline / 主线核心）

DLC 的主线剧情，分两阶段。

**阶段一 · 夺机**（Airline Mission 1）

获取国际航空公司 **太平洋航空（Pacific Air）** 预售服务器的访问权限：

> "直奔主题，为了日后的一些保卫项目，我需要国际航空公司'太平洋航空'的预售服务器的访问权限。它不仅安全性出色，还有一个专用的白名单服务器。它会直接拒绝外部的连接，包括我们。我们需要使用其他进入点进入网络，以找到绕过它的方法。该网络由 **Psylance 网络安全集团** 运行，注意掩盖好你的踪迹。"

任务要点：白名单服务器挡路 → 团队配合拉闸几秒 → 趁机进入 Bookings Mainframe。PacificAir 的服务器全套 sec=5（Bookings Mainframe、Mail、Network Hub、Whitelist Authenticator）。

**阶段二 · 后门**（Airline Mission 2）

在太平洋航空的链接服务器上安装后门，作为**备用计划**防止 Coel 可能的失手：

> "你的任务是搭建备用计划以保证我们的安全，防止 Coel 可能的失手。如果一切顺利，我们可能甚至用不到你的备用计划。我已经拿到了一套软件，能够让我们劫持太平洋航空的广播硬件。他们的硬件由一个名为 **Skylink** 的服务器运行。把后门程序（`sysinfo.bat`，已在共享服务器的 Home 文件夹）上传到它的 `~/sys/`。"

### 🔥 坠机危机（Airline 2 / Finale）★ DLC 结局

针对 **Pacific ATC**（航空管制）系统的深入渗透（`Pacific_ATC_RoutingHub`、`Pacific_ATC_Skylink` `208.91.196.94`、`WhitelistAuthenticator`），最终目标是两架航班：

- **PA_747_0022 Flight Computer** `209.15.13.134`（sec=5）
- **PA_747_0018 Flight Computer** `208.73.211.70`（sec=5）

**终局反转**：在危机解决的关键时刻，**Coel 背叛了团队**——她试图关掉 Bibliotheque 的频道、踢掉 D3f4ult 的网关（`*** CHANNEL mode set: +b (ban) for user @Coel`）。最后的收尾在 **Kaguya_Gateway**（`54.183.231.31`）、**Kaguya_Projects**（`54.183.231.34`）、**Labyrinths_DevChat**（`54.192.133.33`）：清掉敌人的 `/bin` 和 `/sys`，确保他们无法再发动攻击。

> 🕵 **Anon（匿名者）**：他后来在一架飞机上发现了你留下的日志，主动帮你清掉，并顺藤摸瓜查出 **Nisei MK III** 的来龙去脉。他在暗处帮了你一把。

### 🧪 炼金术士线（Alchemists）

一个奇怪的哲学团体 **炼金术师（Hermetic Alchemists）** 的管理者开发了一种**追踪转移软件**，团队需要它：

> "我们寻找的这个程序是一款先进的追踪转移软件，属于内部开发。初步扫描显示，这个网络非常庞大，并为一个名为'炼金术师'的哲学团体托管文件和各种服务。"

- 服务器：`School of the Hermetic Alchemists`（HA_Coagula / Solve / Rebis，`66.96.149.16`）
- 管理员 **Nate Wesson** 的服务器密码见[服务器速查表](./Servers.md)
- 团队项目：把调查到的信息发到聊天频道共享

### 🎯 Striker 线

**Striker** 是一个黑客——有两处相关剧情：

- **攻击线（藏匿处）**：Kaguya 委托拿到 Striker 制作的**信息转储生成器**（`Striker_Battlestation` `72.60.4.119` sec=3、`Striker Proxy` `72.52.4.119` sec=6、`Striker Cache` `72.52.4.117` sec=3）
- **报复线**：删掉他的大学作业后，Striker 气炸，从攻击转为**反攻**——你的 PC 遭到 VM 级攻击。Coel 紧急介入帮你调查，D3f4ult 追踪来源。这段剧情呼应主线里"清日志"的教训。

### 🧠 内存取证（MemForensics）

Kaguya 引入**内存取证**新工具——把目标的整个 RAM 状态拷贝出来，用工具筛选数据：

> "作为代价，'卖家'要求我带领团队检查一些随之而来的内存转储文件。这很明显是一个陷阱，或是某种测试——卖家有工具（她自己构建的工具！）来做这件事，而把这件事丢给我们很显然是要记录谁在使用这种工具。"

- 涉及服务器：`Lihota Productions`（`17.5.3.9`，密码 `4TL4S`）、`Snackintosh`（`185.160.171.69`，同密码）、`iodependency~Atlas`（`88.29.144.6`）、`Raven Dataworks`（`208.73.49.146`）

### 🐾 Neopals（倒数第二项任务）

**Neopals** 是数字宠物公司，任务是修改他们的主机——经典老派传奇目标，被称为"骇客技能的试金石"：

> "所以说，这不是我的决定。该任务是倒数第二项任务了。我的一个朋友为我做了大量挖掘工作，但她现在想要看看团队能给她多大信心。这是团队工作，由你和 D3f4ult 负责。"

- 主服务器 sec=8，密码 `n8u5v432kj`
- 认证服务器 sec=8，密码 `mlk3fsdie321`
- 版本控制 sec=5，密码 `undc321b9y`

### 📋 支线任务（Set 系列）

- **定制狂**（DSet1）：破解一台"定制狂"的 PC（非标准端口/配置），拿 **SSL 端口破解软件**的拷贝
- **Grindcraft DDoS**（DSet2）：处理 DDoS 攻击者 r00t_Tek 的服务器，清除 LOIC 攻击文件夹（Coel 加入小组的条件）
- **清除 / Cleanup**（DSet3）：D3f4ult 中途离开，回去清除他的工作痕迹，完成"佛兰肯斯坦"计划的调查
- **It Follows**（DSet3）：有人要在黑客论坛给 Kaguya 泼脏水，核实指控并必要时"让他们闭嘴"
- **豆茎 / Beanstalk**（DSet4）：黑掉一家咖啡店，偷取欧洲政治家 Asdis Dagrunsdottir 的立法草案
- **Expo Grave**（DSet4）：用假补丁替换安全研究者 **Howard Grave** 的 SQL 漏洞演讲演示文件
- **键盘人生 / Keebs**（DSet4）：把用户 "ChucklingKumquat" 从机械键盘设计师的黑名单上移除

### 🕵 Psylance 线（CSEC 注入任务）

**Psylance** 是 DLC 中的网络安全公司。一条独立任务（CSEC 侧）：有人因非她所为的攻击事件遭陷害，需要替换 Psylance 正在编写的攻击报告：

> "我在网络安全领域的一个好朋友正在因为一件非她所为的攻击事件而遭受陷害。Psylance 网络安全公司正在编写一份该事件的报告，而我想用我自己的版本将其替换。加密过的报告现已上传至 hub 服务器：`home/PA_0022_Incident.dec`。"

---

## 🏁 结局

DLC 主线的终点是 **Kaguya 的网关与项目服务器**——在 Coel 背叛、敌人反扑之后，团队必须清空对手的 `/bin` 与 `/sys`，确保危机彻底结束。整个 DLC 的核心主题：**团队协作**（你永远不是一个人在战斗）与**信任的脆弱**（即使是队友也可能在最关键的时刻背刺）。

**相关页面**：[主线剧情](./Story.md) · [组织历史](./Organizations.md) · [服务器速查表](./Servers.md) · [全流程攻略](./Walkthrough.md)
