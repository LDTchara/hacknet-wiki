
# 服务器服务（Daemon）

服务器上的各类后台服务程序（Daemon）控制着游戏世界中的每一项在线服务。数据提取自游戏源码中所有继承自 `Daemon` 的类。

## 核心服务

| Daemon | 功能 | 出现位置 |
|---|---|---|
| **MailServer** | 邮件服务：收发邮件，含自动回复 | jmail.com、boatmail.com 及各公司邮件服务器 |
| **WebServerDaemon** | 网页服务：渲染服务器上的 HTML 网页 | 各公司官网 |
| **IRCDaemon** | IRC 聊天系统：频道、日志与消息 | /el 论坛、Bibliotheque 内部频道 |
| **MessageBoardDaemon** | 留言板论坛：多主题帖子 | /el Message Board |
| **DatabaseDaemon** | 通用数据库：可浏览的数据记录服务 | 各类数据服务器 |

## 专用数据库

| Daemon | 功能 | 出现位置 |
|---|---|---|
| **AcademicDatabaseDaemon** | 学历数据库：搜索/浏览学术学位记录 | International Academic Database |
| **MedicalDatabaseDaemon** | 医疗数据库：为全体 NPC 生成病历 | Universal Medical |
| **DeathRowDatabaseDaemon** | 死刑犯数据库：德克萨斯死刑记录 | Death Row Records Database |

## 安全与认证

| Daemon | 功能 | 出现位置 |
|---|---|---|
| **AuthenticatingDaemon** | 认证服务：登录界面流程（基类） | 需登录的服务器 |
| **WhitelistConnectionDaemon** | 白名单认证：校验来源 IP 白名单 | 太平洋航空预售服务器等 |
| **ISPDaemon** | ISP 管理：用户管理界面 | ISP Management Server |
| **UploadServerDaemon** | 上传服务器：Drop 文件夹（可选认证） | CSEC / Bibliotheque 的 Drop 服务器 |

## 剧情与特殊服务

| Daemon | 功能 | 出现位置 |
|---|---|---|
| **HeartMonitorDaemon** | 心脏监控：病人生命体征/心率界面 | Kellis Biotech（起搏器任务） |
| **PorthackHeartDaemon** | PortHack 核心动画（结局相关） | porthackHeart（终局） |
| **PointClickerDaemon** | 点金富翁小游戏（含成就） | PointClicker 服务器 |
| **AircraftDaemon** | 航班监控：小地图上显示航线 | 太平洋航空（DLC） |
| **SongChangerDaemon** | 音乐切换：随机切换当前曲目 | 音乐相关服务器 |
| **MarkovTextDaemon** | 马尔可夫文本生成器 | 聊天机器人式服务器 |
| **LogoDaemon** | Logo 展示页：纯展示服务 | 公司/组织页面 |
| **AddEmailDaemon** | 邮件添加服务：收集邮箱地址 | 注册/订阅页面 |
| **DLCCreditsDaemon** | 制作人员字幕滚动（DLC 结局） | DLC 结局 |
| **FastActionHost** | 快速动作宿主：托管延迟动作系统 | 任务脚本服务器 |

## 任务相关

| Daemon | 功能 | 出现位置 |
|---|---|---|
| **DLCHubServer** | DLC 任务中心：任务列表发布/领取 | Bibliotheque 的 DHS 任务板 |
| **MissionHubServer** | 任务中心服务器 | CSEC 契约中心 |
| **MissionListingServer** | 任务列表服务器 | 任务发布服务器 |

> 部分 Daemon 为抽象基类（如 `AuthenticatingDaemon`、`CustomConnectDisplayOverride`），实际使用其派生类（如 `MessageBoardDaemon`、`UploadServerDaemon`）。

> 相关页面：[核心机制](./Mechanics.md) · [服务器速查表](./Servers.md) · [组织历史](./Organizations.md)
