# 游戏内可读文档

Hacknet 服务器上可 `cat` 读取的各种文档文件。这些文件藏在游戏 `Content/Post/` 与 `Content/files/` 目录，一部分是剧情关键情报，一部分是开发者塞进去的彩蛋与恶搞内容。

## 剧情关键文档

| 文件 | 内容 | 位置 |
|---|---|---|
| `BitSpeech.txt` | **Bit 的遗言**："My name is Bit, and if you're hearing this, i'm already dead…" | 主线结局演出 |
| `eosScannerMail.txt` | vtfx 发给 Entropy 的邮件，泄露**所有 eOS 设备管理员密码均为 `alpine`** | Entropy 线 eOS 开场任务（邮件） |
| `DeathRow.txt` | 真实德州死刑犯数据库（226 条记录：姓名、TDCJ 编号、前科、犯罪摘要、临终遗言） | Death Row Records Database |
| `DeathRowServerInfo.txt` | 死刑数据库简介页（含种族/性别统计） | Death Row Records Database |
| `DeathRowSpecials.txt` | 特殊条目：Alva Curry 与 Joseph Felman（任务目标） | Death Row Records Database |
| `MedicalDatabaseInfo.txt` | 国际医疗记录库简介页 | Universal Medical |
| `AdvancedTutorialData.txt` | 进阶教学脚本（教 connect/scan/PortHack 等全部基础命令） | 游戏内 Tutorial |
| `TutorialData.txt` | 旧版教学脚本（`AdvancedTutorialData` 的前身，未启用） | 遗留内容 |
| `config.txt` | 恶搞配置文件（内含 `Not_Porn` 路径梗），教学任务目标 | 教学机 `/bin` |
| `CreditsData.txt` | 完整制作人员名单 | 结局字幕 / Credits Server |

## 现实数据彩蛋

| 文件 | 内容 |
|---|---|
| `DeathRow.txt` | **整份是真实世界数据**——德州被执行死刑人员名单及真实临终遗言（含 Kimberly McCarthy、Thomas Barefoot 等）。CSEC 任务会操作其中的记录：`03HubSet03` 删除 Joseph Felman、`09HubSet04` 把 John Boorman 遗言改为 "I'm ready, Warden" |
| `DeathRowServerInfo.txt` | 配套统计页（White 81 / Black 110 / Hispanic 79 / Other 4，共 274） |
| `CreditsData.txt` | 真实制作组名单（Matt Trobbiani、音乐 The Otherworld Agency、发行 Fellow Traveller） |
| `VehicleTypes.txt` | 约 410 条真实汽车型号清单（厂商#车型），用作随机车牌/车辆档案词库（eOS Device Scanner 数据源） |
| `siteTest.txt` | **真实 Stack Overflow 网页快照**（"Open a web browser in C# XNA?"，ID 2195439）——开发者测试网页渲染器时的残留物 |

## 开发者恶搞

| 文件 | 内容 |
|---|---|
| `DownloadRam.txt` | 4chan 风格帖子《How do I download ram?》——"下载 RAM / 删 System32 / 用磁铁清理"梗全集 |
| `walkthrough.txt` | 假攻略《Grocery Simulator 2010 FULL WALKTHROUGH》（作者 grocer211），整篇 troll |
| `readme.txt` | 假游戏《Grocery Simulator》readme，写着写着变成 Diablo II / Doom / Portal 简介拼贴 |
| `Design_doc_4_topsecret.txt` | 恶搞设计文档《Secretary Simulator 2012》（含"屏幕保护模拟器 = 旋转的猫"彩蛋） |
| `Speech_notes.txt` | 公司全员大会演讲稿（"协同增效/Synergy"式废话，抱怨打印机驱动文件夹） |
| `webpage.txt` | "Matt's Home Page"——故意写错的 90 年代 GeoCities 个人主页（`<htlml>`、跑马灯 "WELOCME TO MY PGAGE"） |
| `Creative_writing_assignment1.txt` | 刻意写得极差的 Doom 同人小说（"You will BE KILL BY DEMONS"，网络知名 copypasta）；同名片 John Stalvern 是 CSEC 任务彩蛋 |
| `Creative_writing_assignment2.txt` | 同一故事的文笔成熟重写版（对比展示"从烂稿到成稿"） |
| `philosophy_thesis.txt` | 学生论文：论证"儿童是四维生物制造的时空扭曲"（引用《How is Babby Formed》meme） |
| `shopping_list.txt` | 胡诌购物清单（"Mofn、Blanabba、Mershed perderder、artichoke hearts"） |
| `Assignment4.txt` | 七年级《人体蜈蚣》观影作业（黑色幽默，故意踩线） |
| `Freedom.txt` | 俳句："Worker bees can leave / Even drones can fly away / The queen is their slave" |

## 开发者测试残留

| 文件 | 内容 |
|---|---|
| `InvalidCharsTestFile.txt` | 仅含 `~ \ ' |` 四个特殊字符（测试终端显示） |
| `KeyboardTest.txt` | 键盘输入测试草稿（"im click, click click. herrrrr."） |
| `siteTest.txt` | Stack Overflow 快照（见上） |
| `DownloadRam.txt` / `TutorialData.txt` | 未接线的遗留内容 |

## 阅读位置说明

- **Death Row** 三件套：位于 Death Row Records Database（`168.51.178.4`，密码 `texas`）。
- **Universal Medical**：`MedicalDatabaseInfo.txt` 位于医疗库服务器。
- **eOS 开场任务**：`eosScannerMail.txt` 作为邮件发送给玩家（Entropy 线）。
- **结局**：`BitSpeech.txt` + `CreditsData.txt` 在主线结局与 DLC 结局（Credits Server `226.187.99.3`）读取。
- **其余 `files/` 文档**：随机散布于各服务器的电脑中，位置不固定，可通过 `cat` 在任意节点发现。

> 相关页面：[游戏彩蛋](./EasterEggs.md) · [人物与剧情档案](./Characters.md) · [服务器速查表](./Servers.md)