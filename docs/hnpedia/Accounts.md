# 账户与用户系统

Hacknet 中存在多层"账户"概念：**玩家本地账户**（存档系统）、**服务器用户**（`UserDetail`）、**游戏内数据账号**（`Account` / `OnlineAccount` / `NeopalsAccount`）。它们用途不同、互不相通。

> ⚠️ 本页所有账户、口令与用户数据均为**游戏内虚构设定**，仅作资料记录，请勿关联到任何真实系统或人物。

## 玩家账户（存档账户）

见 [存档系统](./Saves.md)：由 `SaveFileManager` 管理，含用户名/加密密码/存档文件，支持本地 + Steam 云双写。主菜单的新建/登录都走这套。

## 服务器用户（UserDetail）

每台服务器有一组登录用户，用于邮箱、IRC 等需要认证的服务：

```xml
<user name="admin" pass="..." type="1" known="false" />
```

| 字段 | 含义 |
|---|---|
| `name` | 用户名 |
| `pass` | 密码 |
| `type` | 账户类型（0=已重置/管理员，1=普通，3=代理等） |
| `known` | 是否已被人知晓（影响登录后提示） |

- 服务器构造时自带 `admin` 用户（`Computer.cs:29`，type=1，密码为管理员密码）。
- `addNewUser` 可运行时添加用户；邮件服务器给每个新用户分配 `UsernameGenerator.getName()`。
- 玩家自己的账户是 `os.defaultUser`（`UserDetail(username, "password", 1)`）。

## 数据账号（Account / OnlineAccount）

DLC 中用于模拟现实账号的数据类：

- **Account**：ID、现金、银行、公寓、车辆、天煞车辆、Rank、RP、击杀数——用于 GTA 风格数据展示（`Account.ToString()` 返回 ID）。
- **OnlineAccount**：ID、用户名、封禁状态、备注，`ToString()` 返回 `用户名#ID`——用于论坛/社交数据。

这两类数据由对应的 Daemon 填充展示，属于"游戏内数据库内容"，与玩家存档无关。

## Neopals 账户（NeopalsAccount）

DLC 虚拟宠物站账户，随机生成（见 [随机生成系统](./Generation.md)）：

- 字段：账户名、NeoPoints、BankedPoints、InventoryID（GUID）、宠物列表。
- 活跃用户（`isActiveUser`）点数更高（≤50k vs ≤5k），主要人物（Bit 等）的账户更豪华。
- 在 Neopals 任务线中用于查案与彩蛋。

## 与存档的关系

- 玩家账户是存档的**门禁**：登录后 `os.SaveGameUserName` 指向存档文件名，`os.SaveUserAccountName` 是显示名。
- 服务器 `UserDetail`、`Account`、`OnlineAccount`、`NeopalsAccount` 随网络节点一起被序列化进存档，读档后原样恢复。

> 相关页面：[存档系统](./Saves.md) · [随机生成系统](./Generation.md) · [服务器服务（Daemon）](./Daemons.md)