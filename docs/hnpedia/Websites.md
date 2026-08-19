# 游戏内网站

游戏内的大部分网站由 `WebServerDaemon`（网页服务）提供：它把一份 HTML 存进节点的 `web/index.html`，用系统自带浏览器（WebRenderer）渲染。少数预设页面存放在 `Content/Web/Presets/<站点>/` 下，各公司官网通过 `<addWebServer>` 引用它们。

## 预设网站

`Content/Web/Presets/` 下共有 7 个预设站点：

| 站点 | 页面文件 | 服务器上的名称 | 剧情作用 |
|---|---|---|---|
| **DEC** | `DEC/DECMainPage.html` | www.DEC-Solutions.com | DEC 加密套件官网 |
| **EnTech** | `EnTech/EnTechPublicPage.html` | www.EnTech.com | EnTech 国际安防官网 |
| **Expo** <Badge type="info" text="DLC" /> | `Expo/Expo.html` | SecurityCoNnEXION | 黑客大会日程表 |
| **KFC** | `KFC/KFCHomePage.html` | CFC Homepage | 炸鸡连锁店主页 |
| **lelzSec** | `lelzSec/ShrineOfThePolarSnake.html` | Shrine | 极星神社试炼页 |
| **Neopals** <Badge type="info" text="DLC" /> | `Neopals/Neopals.html` | Neopals! | 宠物养成网站 |
| **Nortron** | `Nortron/NortronPublicPage.html` | www.NortronSecurity.com | Nortron 安防官网 |

### DEC — Data Security Solutions

`DEC/DECMainPage.html`，页头图 + 加密业务宣传文案。页面介绍两位核心人物：

- **David Leatrou**，CEO，宣称"运营 IT 公司 28 年"（剧情里他正是 DEC 的双重加密幕后黑手）。
- **Joseph Scott**，CTO，"前 Macrosoft 员工"（彩蛋致敬 Microsoft）。

在 DEC 任务线中，`DECSoftWebsiteComp.xml` 的网页服务器（`101.0.89.154`）虽然放着官网，真正的秘密藏在 `home/` 下的三个 `.dec` 加密文件（Staff_Memo、New_Policies、DECE_TestBuild_pdb），需要用 Decypher 解密。

### EnTech — EnTech International Security Solutions

`EnTech/EnTechPublicPage.html`，居中深色风格的极简主页：大 Logo + 标语 "EnTech International Security Solutions / - The future in digital security -"。

剧情上是玩家黑客组织 Entropy 攻击的目标公司之一。网页服务器 `EnTechWeb.xml` 位于 EnTech 网络边缘（`security=4`、5 端口、无 admin 密码设定），是进入 EnTech 内网的第一站。

### Expo — SecurityCoNnEXION <Badge type="info" text="DLC" />

`Expo/Expo.html`，条栅条纹背景上排出一张**黑客大会日程表**（Labyrinths DLC 第 4 组任务"Expo"）：

| 编号 | 房间 | 时间 | 议题 |
|---|---|---|---|
| 31953 | Room 204 | 10:30 AM | "JSON exploits for fun and profit" by Faridah Wick |
| 31955 | Room 204 | 11:30 AM | "Vulnerabilities in SQL Database link-layers" by Howard Grave |
| 31948 | Room 210 | 10:30 AM | "Human-computer interface hacking via bluetooth" by Amanda Teller |
| 31949 | Room 210 | 11:30 AM | "Smarthomes, and how many security holes we can stuff into your fridge" by Kevin McElroy |
| 31954 | Room 211 | 12:30 PM | "Deceiving the heavens to cross the sea" by Jayson Street |

网页服务器的 `home/talks/files/` 下存放对应编号的演讲资料包（`.gz`），任务需要下载并校验它们。Jayson Street 是致敬真实世界白帽黑客。

### KFC — Colonel's Food Corner

`KFC/KFCHomePage.html`，恶搞 KFC 的炸鸡店主页：大图 + 大红标题 "Everyone Loves Chicken!" 与 "$22.00"、副标 "Vendors of fine friend chicken!"（原文就拼错成 friend）。

它由 `KFCWebServer.xml` 以 "CFC Homepage" 提供，节点名 `www.cfc.com`（`type=4` 空节点、`security=2`）。服务器上 `ServerStructure.txt` 说明它是纯前端内容服务器，只能连回 KFC 主框架（`kfcMainframe`）。

### lelzSec — Shrine of the Polar Star

`lelzSec/ShrineOfThePolarSnake.html`，神社风格黑底页面，只放一张极星巨蛇图。**页面 HTML 源码注释里藏着 4 个试炼 IP**：

```
Trial of Patience   103.31.7.38
Trial of Haste      103.31.7.41
Trial of Diligence  103.31.8.1
Trial of Focus      103.31.7.211
```

这就是 lelzSec 主线"极星试炼"的四关地址，逐关破解后才能获得进组资格（对应 `PSTrial01~04.xml`）。神社本体 `polarSnake`（`103.31.7.34`）上还有带随机密文的 `Talismin.ttgl`。

### Neopals <Badge type="info" text="DLC" />

`Neopals/Neopals.html`，宠物养成站（恶搞 Neopets）：Logo + 注册/登录按钮 + 版权行 "© 1999-2016 Neopals, Inc."。对应 `Pets_Website.xml`（Labyrinths DLC Neopals 任务线）。

它只是前端的入口——网页服务器通过 `dlink` 连到 Neopals 认证服务器（`dPets_Auth`）、主框架（`dPets_MF`）与版本控制系统（`dPets_VC`），`Server_Config_Info.txt` 明确写着"别手动部署文件，这里是自动配置的"。

### Nortron — Nortron Security Services

`Nortron/NortronPublicPage.html`，深色工业风官网："Nortron Security Services / Premier Digital Security Experts" + 世界地图小图，页脚 "Copyright - 2012"。

在 lelzSec 主线里，Nortron 是被 lelzSec 黑掉的公司。网页服务器 `nortronWebServer`（`security=2`、`<firewall level="1" solution="NORTRON">`）连着 Nortron 内网服务（`nortronInternalServices`），需要特定防火墙命令通过。

## 通用模板

`Content/Web/` 根目录下还有几个通用 HTML 模板，供 `WebServerDaemon` 与公司生成器使用：

| 模板 | 用途 |
|---|---|
| `BaseImageWebPage.html` | `WebServerDaemon` 的默认页面（未指定 url 时使用） |
| `BaseWebPage.html` | 通用网站骨架模板 |
| `BaseCorporatePage.html` | 公司官网模板，含 `#$#COMPANYNAME#$#` 占位符 |
| `BaseCorporateRecruiterPage.html` | 公司招聘页模板，附 `#$#LC_COMPANYNAME#$#@jmail.com` 招聘邮箱 |
| `404Page.html` | 404 错误页 |

`BaseCorporatePage` 用 `corporatestyle.css` 渲染；`generateBaseCorporateSite(companyName)` 会把 `#$#COMPANYNAME#$#`（及下划线变体 `#$#LC_COMPANYNAME#$#`）替换成公司名，并另存一份 `index_BACKUP.html` 到 `home/`。

## 网页服务机制

- **页面来源**：`<addWebServer name="..." url="Content/Web/Presets/.../xxx.html" />`（见 `ComputerLoader.cs`）把预设 HTML 读入节点的 `web/index.html`；`WebServerDaemon` 进入节点后默认加载它。
- **渲染**：`ShowPage` 把页面数据写入 `Content/Web/Cache/HN_OS_WebCache.html` 再由 `WebRenderer` 渲染，因此预设页引用的样式表与图片都解析自 `Cache/` 目录（`Cache/*.css` 与 `Cache/Images/*.png`）。
- **浏览**：底部工具栏有 "Exit Web View"（退出）与 "View Source"（查看源码）。View Source 会 `cd` 进 `web` 目录并 `cat` 当前页面——Shrine 试炼 IP 的线索就是这么挖出来的。
- **Extension**：扩展模式下行页面写入 `Extensions/<扩展名>/Web/Cache/`，不会污染本体缓存。

> 相关页面：[服务器速查表](./Servers.md) · [服务器服务（Daemon）](./Daemons.md) · [游戏彩蛋](./EasterEggs.md)