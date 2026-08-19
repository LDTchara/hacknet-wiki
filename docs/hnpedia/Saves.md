# 存档系统

Hacknet 的存档系统由 **账户（Account）** 与 **存档文件（Save File）** 两层构成。每个游戏内用户是一个账户，账户下有一份 `.xml` 存档。存档通过 `SaveFileManager` 统一管理，可同时写入**本地**与 **Steam 云**两份。

## 存档位置

存档按平台存放于不同目录（`LocalDocumentsStorageMethod`）：

| 平台 | 路径 |
|---|---|
| Windows | `我的文档\My Games\Hacknet\Accounts\` |
| Linux | `$XDG_DATA_HOME/Hacknet/Accounts/` 或 `~/.local/share/Hacknet/Accounts/` |
| macOS | `~/Library/Application Support/Hacknet/Accounts/` |
| 失败回退 | 游戏目录下 `Accounts/`（并提示 `AccumErrors`） |

Extension 模式下，所有路径追加 `<扩展文件夹名>/`，实现**扩展存档隔离**（本地与云同规则）。

## 账户清单（Accounts.txt）

`Accounts.txt` 记录所有账户，分隔符为 `\r\n%------%`，首行为最后登录用户名：

```
<最后登录用户名>
%------%
<用户名>\r\n__<加密密码>\r\n__<最后写入时间>\r\n__<文件用户名>
%------%
...
```

- 密码经 `FileEncrypter.EncryptString` 加密，读取时解密校验。
- 存档文件名 = `save_<净化用户名>.xml`（`_` 会替换为 `-`）。
- 登录校验：用户名不区分大小写；**特殊口令 `buffalo` 可免密登录任意账户**（源码中写死）。

## 存档文件结构

存档是 UTF-8 XML，由 `OS.writeSaveGame` 生成，根元素 `<HacknetSave>`：

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<HacknetSave generatedMissionCount="2" Username="test" Language="en-us" DLCMode="False" DisableMailIcon="False">
  <DLC ... />                                  <!-- DLC 专属数据 -->
  <Flags ... />                                <!-- 剧情进度标记 -->
  <netmap ...> ... </netmap>                   <!-- 网络地图全部节点 -->
  <mission next="..." goals="none" activeCheck="none"> ... </mission>
  <branchMissions> ... </branchMissions>
  <factions ...> ... </factions>
  <other music="..." homeNode="..." homeAssetsNode="..." />
</HacknetSave>
```

各模块可独立序列化：网络节点（`netMap.getSaveString()`）、当前任务、分支任务、派系进度、音乐/家节点。

## 保存触发

- 游戏内 `save` 命令（真正的保存由内部长口令 `save!(SJN!*SNL8vAewew57WewJdwl89(*4;;;&!)@&(ak'^&#@J3KH@!*` 触发，玩家无法直接输入）。
- `saveGame()` 会在 Extension 模式下检查 `AllowSave`：不允许的扩展不写盘。
- `threadedSaveExecute` 走线程安全队列（`SaveInProgress`/`SaveInQueue` 防重入），保存时锁定 `CurrentlySaving`。

## 云存档

`SaveFileManager` 按顺序注册存储方法：

1. `LocalDocumentsStorageMethod`（本地，永远启用）
2. `SteamCloudStorageMethod`（Steam 运行且云可用时启用）

写入时遍历全部存储方法写一份；读取/加载优先取第一个（本地）。启动时 `UpdateStorageMethodsFromSourcesToLatest` 会把旧版云存档/本地存档**迁移合并**到新账户系统（大于 100 字节的文件才会迁移）。

> ⚠️ 安装 Pathfinder 会禁用 Steam 云存档。

## 账户管理 API

| 方法 | 功能 |
|---|---|
| `AddUser(username, pass)` | 新建账户（重名/空名拒绝，下划线转连字符） |
| `GetFilePathForLogin(user, pass)` | 校验登录，返回存档文件名（`buffalo` 万能口令） |
| `CanCreateAccountForName(name)` | 检查用户名是否可用 |
| `DeleteUser(username)` | 删除账户并修正最后登录者 |

主菜单登录流程：新建账户 → `AddUser` → `GetFilePathForLogin` → 设置 `os.SaveGameUserName` 与 `SaveUserAccountName` → 启动 OS。

> 相关页面：[账户与用户系统](./Accounts.md) · [游戏模式与设置](./Modes.md) · [常见问题](../hnfaq.md)