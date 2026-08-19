# 随机生成系统

Hacknet 会程序化生成大量游戏内容：公司名、NPC 账号、学历学位、车辆登记、医学档案、Neopals 账户等。这些都由各自的生成器类驱动，全部基于统一的 `Utils.random` 随机源。

## 公司名生成（NameGenerator）

游戏里随机出现的公司、任务客户端/目标名称由 `NameGenerator` 生成：

- 从 `main` 表取一个词干（如 `Holopoint`、`Macrosoft`、`20%Cooler`、`Banished Stallion`），再拼一个 `postfix` 后缀（如 ` Inc`、` Interactive`、`.com`、` Consortium`）。
- `main` 共 24 个词干，`postfix` 共 12 个后缀，组合出 288 种公司名。
- 用于随机网络节点命名、`MissionGenerator` 的任务客户端/目标、`Corporation` 生成等。

## NPC 用户名生成（UsernameGenerator）

- 从 `Content/Usernames.txt` 读取约 232 个用户名（以空行分隔），按顺序轮询（`nameIndex` 递增），每次登录/新建账号取下一个。
- 初始随机选一个起点，因此每局顺序不同，但内容固定。
- 用于邮箱、IRC 等服务的 `UserDetail`。

## 人物生成（Person + PeopleAssets）

创建 NPC 时（`Person` 构造器）按流程随机生成：

| 项目 | 生成规则 |
|---|---|
| **用户名 handle** | `UsernameGenerator.getName()` |
| **出生地** | `WorldLocationLoader.getRandomLocation()`（见 [世界地点系统](./Locations.md)） |
| **出生日期** | 18~72 岁随机（黑客 18~45 岁），按天计 |
| **血型** | 两枚硬币：A/B/O × A/B/O |
| **身高** | 155~220cm 区间，用 `random²` 偏置（越矮概率越高） |
| **过敏原** | 从 `Content/PersonData/Allergies.txt` 抽 0~4 种，不重复 |
| **就诊记录** | `MedicalRecord` 随机生成若干次就诊（时间、地点=出生地或国家、公立/私立医院） |
| **学位** | 见下节 |
| **车辆** | 见下节 |
| **Neopals 账户** | DLC 下约 80% 概率生成（含活跃用户 50k 点、宠物等） |

## 学位生成（PeopleAssets）

`getRandomDegree` / `getRandomHackerDegree` 生成一个 `Degree`：

- 学位名 = 前缀（`Bachelor of` / `Masters in` / `PHD in`）+ 专业。
- 普通专业 12 种（含 Computer Science、Business、Medicine 等）；黑客专属专业 5 种（Digital Security、Computer Networking 等）。
- GPA = `3 + 随机×2`，在 1~4 之间，普通人与黑客分布不同。
- 大学名 = `University of <出生城市>` 或 `<出生城市> University`（随机翻转）。
- 每个人有递降概率获得多个学位（普通 60% 起、黑客 90% 起，链式概率递减）。

## 车辆登记生成（VehicleInfo）

- 车型从 `Content/files/VehicleTypes.txt` 读取（411 个型号，`厂商#型号` 格式，含真实汽车品牌）。
- 车牌：3 个随机字母 + `-` + 3 个随机字母。
- 执照号：12 位随机字符，每 4 位加 `-` 分隔（70% 字母 / 30% 数字）。
- 每个人以 70% 起、概率递减地获得 0~3 辆车。

## 随机字符工具（Utils）

| 方法 | 行为 |
|---|---|
| `getRandomLetter()` | 随机大写字母 A~Z |
| `getRandomChar()` | 70% 字母、30% 数字 |
| `getRandomNumberChar()` | 随机数字 0~9 |
| `flipCoin()` | 50% 真/假 |
| `randm(range)` | 随机 ±range 浮动值 |

所有生成器共享 `Utils.random`（`Random` 实例），因此给同一随机种子即可复现整局内容。

> 相关页面：[世界地点系统](./Locations.md) · [人物数据库档案](./PeopleDB.md) · [游戏内可读文档](./Files.md)