
# Hacknet 主线剧情

基于 Hacknet 游戏源码中的任务文件与脚本内容还原，按实际主线推进顺序整理。


## 序幕：Bit 的遗言

> *"My name is Bit, and if you're reading this, I'm already dead."*

游戏开场是一段来自 **Bit** 的录音——一名已经遇害的黑客。他通过预先录制的邮件系统，在死后一步步引导玩家完成他的遗愿。


## 第一阶段：教程与初次接触

玩家启动 HacknetOS，完成系统初始化后，收到 Bit 的第一封邮件 **"First Contact"**：

> 我不知道你是谁，也永远不会知道。但如果你读到这封信，你可能是唯一能让事情回到正轨的人。
>
> 我现在被困住了。没有出路，时间也不够。我需要你的帮助。

Bit 的指令很简单：连接到自己的节点，找到并删除 `SecurityTracer.exe`。这个程序是 HacknetOS 自带的追踪器——它不应该被发布出来。

完成之后，Bit 发来 **"Maiden Flight"**，指导玩家破解第一台测试服务器 `bitMission00`，学习使用 `scan`、`probe`、`SSHcrack` 和 `PortHack`。

Bit 的后续邮件 **"Something in return"** 告诫玩家一个最重要的习惯：**清日志**。

> 如果你会忘记我告诉你的任何事，那请记住这一件——别马虎。删除你的日志。
> 你在任何现代操作系统上做的每一件事都会被记录在 \~/log 文件夹里。进去，\>rm \*。

> 🔑 **这条忠告会在后面救你（或坑你）**：Bit 一开始就警告你别留日志，可 Naix 之所以能反入侵你的电脑，正是因为你入侵时留下的日志。


## 第二阶段：加入 Entropy

Bit 的测试服务器把你引向 **Entropy**——一群崇尚混乱的黑客行动主义者。你经 `Entropy test Server`（`199.59.149.230`）加入，为 Entropy 干活（`EntropyMission1.1 → 1.2 → 2 → 3`），目标包括 PP Marketing、PointClicker、X-C Project 等。SlashBot 新闻对他们评价负面。

在 Entropy 阶段，你开始接触更高级的目标（Entropy Asset Server、Entropy Contract Database），组织信任度逐步提升。


## 第三阶段：Naix 入侵与抉择 ★主线转折

一次针对 `Proxy_Node-X04`（`173.194.35.163`）的入侵后，你收到 Entropy 的 **《Warning》** 邮件：

> "Connect-Responder.exe has noticed a foreign connection from your IP address to 173.194.35.163. External connections are prohibited on this device."

你留下的 **log** 成了破绽——黑客 **Naix** 顺着日志**反入侵了你的电脑**。这印证了 Bit 早先关于清日志的警告。

面对入侵，你有两条路：

### 分支 A · 回复脏话

在 Naix 的 **"gg wp"** 邮件上**回复任意脏话**（如 `fuck`），Naix 回以 "rude response"（成就 `rudeNaixResponse`）。你继续留在 Entropy，完成**剩余任务**。

### 分支 B · 通过 log 反黑回去

从自己的日志反追踪 Naix，黑进 **Naix Root Gateway**（`173.194.35.172`，密码 `roxxane`），删除 **`sys/x-server.sys`**。Naix 的整套系统崩溃，他**认输**，发来 **"gg wp"** 邮件：

> "Ok, ok, I get it. Fair enough. Stop messing with my shit."
> "A bit sloppy leaving those logs on your box, but, seriously, it kinda looked like you had no idea what you're doing."

认输之余，Naix 给了你一个"测试"——去把安全公司 **Nortron** 的网站改成满屏 "DICKS"。由此你进入 **lelzSec** 线：**/el 论坛**、**北极星试炼**（Polar Snake：Shrine → Patience/Haste/Diligence/Focus → Head）。


## 第四阶段：CSEC（邀请、自动测试、肯德基）

两条分支完成后**殊途同归**：无论你走 Entropy 剩余任务还是反黑线，都会收到 **CSEC** 的招募。这是一个专业的雇佣黑客组织。

- **自动测试（Gauntlet）**：依次拿下 `CSEC Crossroads Server` → `CSEC Public Drop Server` → `CSEC Invitation Gauntlet` → `CSEC Gauntlet 02/03`
- **肯德基（KFC）**：任务板上的 CFC 公司系列任务（www.cfc.com、CFC Records Repository、CFC Corporate Mainframe）

通过后获得 **CSEC** 主服务器（密码 `admin`）与 **CSEC Assets Server**（密码 `csec-5xg3D11`）权限，任务板完全开放。


## 第五阶段：CSEC 任务板系列任务

自由接单阶段，承接 `#CLIENT#` 匿名客户的任务：Decypher 解密系列、Pacemaker 起搏器系列、核心数据库（ISP Management、Death Row、Universal Medical、International Academic DB 等）。这也是刷声望、攒装备的时期。


## 第六阶段：V 系列 · Bit 之路（主线核心）

CSEC 任务板推进到后期，**V**——Bit 的朋友——开始向你发布 **bit 系列**任务（"bit:初章" ~ "bit:阻止"）。你潜入 **EnTech** 公司（`156.151.x.x` 网段）的基础设施：

1. **BitAdv_Intro** — 潜入 EnTech 的系统，搜集情报
2. **BitAdv_Recovery** — 恢复被加密的数据，使用 Decypher.exe 破解 `.dec` 文件
3. **Bitwise** 系列服务器藏有揭示 Bit 身份的关键资料
4. 揭露 EnTech 开发危险技术的真相

Bit 生前在 **EnTech** 工作，创造了 PortHack，发现了 EnTech 的危险项目并因此丧命。整套主线都在回应他的遗言。


## 最终章：终结 PortHack

Bit 的最后一封邮件 **"Terminal"** 发来：

> 我意识到我没有时间写完这一切了——要学的东西太多，而我一件都来不及做。

Bit 请求玩家摧毁 **`Porthack.Heart`** 服务器——PortHack 系统的核心，一旦被摧毁，PortHack 就再也不能被用作武器。

连接至 `porthackHeart` 服务器，在其上运行 PortHack。PorthackHeart 守护进程检测到这一操作后，会"碎裂心脏"。随后播放 Bit 的最终演说录音，屏幕滚动显示 Bit 的遗言，演职员表伴随 "Bit(Ending)" 音乐滚动。


## 结局

- **主线结局（正统）**：V 系列 → bit:阻止 → 摧毁 PorthackHeart → Bit 的最终演讲 → 演职员表（Credits Server `226.187.99.3`）
- **Labyrinths DLC 结局**：独立剧情线，涉及飞机失事调查、Striker 角色线


## 关键人物

| 角色 | 说明 |
|---|---|
| **Bit** | 已故黑客，游戏叙述者。整个游戏是 Bit 死前录制的邮件序列 |
| **Entropy** | 玩家最先加入的黑客行动主义组织 |
| **Naix** | 玩家的对手，顺着日志反入侵玩家；被反黑后认输（"gg wp"） |
| **CSEC** | 后期招募玩家的雇佣黑客组织 |
| **V** | CSEC 阶段后期出现的联系人，Bit 的朋友，发布 bit 系列任务 |
| **/el** | lelzSec 消息板运营者（反黑分支涉及） |
| **Polar Star / 北极星** | 反黑分支的试炼组织 |
| **Striker** | DLC 角色，涉及航空公司任务线 |

## 关键地点

| 节点 id | 说明 |
|---|---|
| `bitMission00` | Bit 的测试服务器 |
| `entropy00` | Entropy 总部 |
| `naixGateway` | Naix Root Gateway（`173.194.35.172`，密码 `roxxane`） |
| `mainHub` | CSEC 总部，任务发布中心 |
| `EnTechWeb / EnTechMainframe` | EnTech 公司系统 |
| `porthackHeart` | PortHack 核心服务器（最终目标） |
| `pacemaker01` | 医院心脏起搏器服务器 |
