# 世界地点系统

游戏中的每个 NPC 都有一个**出生地**（`birthplace`），记录在世界地点数据库里。数据来自 `Content/PersonData/LocationData.txt`，共 **51 个城市**，由 `WorldLocationLoader` 在游戏启动时解析，`WorldLocation` 类保存。

## 数据格式

`LocationData.txt` 每行一个城市，用 `#` 分隔 7 个字段：

```
城市名#国家#教育指数#生活指数#就业指数#负担能力指数#权重
```

例如 `Paris#France#85#91#96#54#421`。

| 字段 | 含义 | 取值范围 |
|---|---|---|
| 教育指数 | `educationLevel` | 0 ~ 100（越高教育水平越好） |
| 生活指数 | `lifeLevel` | 0 ~ 100 |
| 就业指数 | `employerLevel` | 0 ~ 100 |
| 负担能力指数 | `affordabilityLevel` | 0 ~ 100（越高生活成本越低） |
| 权重 | 最后一位 | 用于排序，加载时被忽略 |

解析使用 `en-au` 文化设置（小数点为 `.`）。

## 城市列表（51 条记录）

按文件中的权重（综合评分）从高到低排列。注意 **Adelaide 出现两次**（两条记录，指数不同），这是官方数据本身的重复，加载时会都加入列表：

| 城市 | 国家 | 教育 | 生活 | 就业 | 负担 |
|---|---|---|---|---|---|
| Paris | France | 85 | 91 | 96 | 54 |
| London | United Kingdom | 87 | 88 | 89 | 41 |
| Boston | United States | 85 | 89 | 83 | 44 |
| Melbourne | Australia | 100 | 94.5 | 84 | 28 |
| Adelaide | Australia | 100 | 94.5 | 84 | 28 |
| Vienna | Austria | 99 | 99.5 | 81 | 62 |
| Sydney | Australia | 94 | 97 | 81 | 25 |
| Zurich | Switzerland | 84 | 99 | 81 | 51 |
| Berlin | Germany | 81 | 95 | 57 | 71 |
| Dublin | Ireland | 92 | 91.5 | 70 | 43 |
| Montreal | Canada | 85 | 93 | 68 | 46 |
| Barcelona | Spain | 76 | 87 | 71 | 61 |
| Singapore | Singapore | 78 | 92 | 100 | 35 |
| Munich | Germany | 79 | 98.5 | 63 | 69 |
| Lyon | France | 88 | 87.5 | 43 | 81 |
| Chicago | United States | 62 | 85.5 | 72 | 44 |
| Madrid | Spain | 73 | 85.5 | 64 | 66 |
| San Francisco | United States | 67 | 91 | 72 | 43 |
| New York | United States | 63 | 83.5 | 73 | 35 |
| Tokyo | Japan | 43 | 84 | 83 | 46 |
| Hong Kong | Hong Kong | 74 | 50 | 92 | 42 |
| Milan | Italy | 63 | 86 | 89 | 54 |
| Brisbane | Australia | 94 | 88.5 | 63 | 30 |
| Seoul | Korea, South | 67 | 50 | 82 | 53 |
| Buenos Aires | Argentina | 54 | 50 | 93 | 75 |
| Perth | Australia | 95 | 93.5 | 53 | 28 |
| Toronto | Canada | 70 | 95.5 | 75 | 35 |
| Stockholm | Sweden | 66 | 94 | 79 | 34 |
| Beijing | China | 48 | 50 | 87 | 62 |
| Adelaide | Australia | 88 | 91 | 45 | 34 |
| Washington DC | United States | 60 | 85.5 | 55 | 45 |
| Vancouver | Canada | 78 | 98 | 54 | 37 |
| Mexico City | Mexico | 38 | 50 | 68 | 95 |
| Helsinki | Finland | 58 | 89.5 | 53 | 69 |
| Taipei | Taiwan | 49 | 50 | 59 | 78 |
| Manchester | United Kingdom | 82 | 50 | 66 | 64 |
| Amsterdam | Netherlands | 62 | 96.5 | 57 | 44 |
| Moscow | Russia | 57 | 50 | 78 | 52 |
| Brussels | Belgium | 71 | 93 | 40 | 60 |
| Shanghai | China | 43 | 50 | 70 | 68 |
| Copenhagen | Denmark | 64 | 97.5 | 56 | 31 |
| Santiago | Chile | 33 | 50 | 89 | 63 |
| Philadelphia | United States | 56 | 50 | 64 | 52 |
| Kyoto | Japan | 60 | 83 | 44 | 47 |
| Kuala Lumpur | Malaysia | 58 | 50 | 45 | 86 |
| Sao Paulo | Brazil | 27 | 50 | 79 | 58 |
| Toulouse | France | 86 | 50 | 26 | 81 |
| Birmingham | United Kingdom | 64 | 50 | 55 | 64 |
| Cairo | Egypt | 47 | 50 | 55 | 96 |
| Bangkok | Thailand | 27 | 50 | 63 | 68 |
| Glasgow | United Kingdom | 68 | 50 | 43 | 63 |

> 表中为 `LocationData.txt` 内收录的全部 51 条记录。数据仅为游戏内虚构设定，不代表真实统计。

## 地点在游戏中的使用

- **NPC 出生地**：`Person` 生成时 `birthplace = WorldLocationLoader.getRandomLocation()`，即随机取一个城市。`People.cs` 加载存档中的人物时，会按名字匹配 `getClosestOrCreate()`，找不到则现场创建一个随机指数的新地点。
- **人物档案展示**：`Person.ToString()` 输出 `Born: 城市, 国家`，出现在学术数据库、医疗数据库等的人物资料里。
- **医学档案**：`MedicalRecord` 构造时传入出生地，用于生成就诊记录（就诊次数、时间线等），地点本身不参与数值计算。

## 扩展开发相关

- 添加自定义城市：直接往 `LocationData.txt` 追加一行即可（`城市#国家#教育#生活#就业#负担#权重`），无需改代码。
- `getClosestOrCreate(name)` 会优先匹配已有城市（不区分大小写），匹配不到就随机生成一个地点并缓存。
- 游戏不会因为地点数据问题崩溃：解析用 `Convert.ToDouble` 且固定 `en-au` 文化，保证小数点格式一致。

> 相关页面：[人物数据库档案](./PeopleDB.md) · [游戏内可读文档](./Files.md)