# 网络地图（Network Map / Netmap）

网络地图是游戏主界面右上方的拓扑图：所有可入侵的节点（Node）与节点间的连线（Link）构成了整个游戏世界。数据提取自游戏源码中的 `NetworkMap.cs`、`NetmapSortingAlgorithms.cs` 与 `ComputerLoader.cs`。

## 节点类型

每个节点（`Computer`）都有一个 `type` 字段，决定其在世界中的"角色"。保存文件与 XML 中对应的值为：

| 常量 | 值 | 含义 |
|---|---|---|
| `CORPORATE` | `1` | 企业计算机 |
| `HOME` | `2` | 家用计算机 |
| `SERVER` | `3` | 服务器 |
| `EMPTY` | `4` | 空节点（无内容，仅占位） |
| `EOS` | `5` | EOS 节点（终局，含 `EOSComp` 生成的特殊文件夹） |

在 `ComputerLoader` 解析 XML 时，`type` 也可写作字符串 `"empty"`（等价于 `4`）。

## 节点位置

节点在地图上的坐标由 `Computer.location` 决定，是一个 **0~1 归一化的二维向量**（`Vector2`），渲染时再乘上地图的实际宽高。

### 排序算法（NetmapSortingAlgorithm）

节点显示位置由 `NetworkMap.SortingAlgorithm` 决定，共有四种算法：

| 算法 | 效果 |
|---|---|
| **Scatter**（默认） | 直接使用每个节点自身的 `location` 坐标（0~1 归一化） |
| **Grid** | 按网格排列，5 列；节点多时自动扩为 7 列、9 列 |
| **Chaos** | 每次随机散布 |
| **LockGrid** | 按"扫描序列"顺序网格排列，仅排布已可见节点 |

游戏内可通过 `NetmapOrganizer`（网络地图整理器）程序切换排序方式；存档会记录 `sort` 属性，加载时恢复。

### 定位方式对比

| 方式 | 说明 |
|---|---|
| **随机位置** | 加载时默认调用 `netMap.getRandomPosition()`，会尝试 50 次避免与已有节点重叠 |
| **`positionNear`** | 围绕某个指定节点环绕排布（详见下文 Extension 章节） |

## 节点的可见与发现

`NetworkMap.visibleNodes` 记录**当前已揭示节点**的索引列表：

- 未在列表中的节点在地图上不可见、不可点击。
- `discoverNode(computer)` 会把节点加入可见列表并触发高亮闪烁；`lastAddedNode` 记录最近发现的节点。
- 任务、剧情通过 `SAShowNode` 动作调用 `discoverNode` 揭示节点；`SAHideNode` 反之。

## 节点连线

节点间的连线存储在每个节点的 `links` 列表中（值是目标节点在 `netMap.nodes` 中的索引），连线是**单向**的：A 连到 B 不代表 B 连到 A。

- 两条连线可达性由 `drawLine` 绘制，仅当两端节点都可见时才会画出。
- 链接目标通过 IP、`idName` 或节点名称查找，例如 `<link target="someNodeID" />`。
- 存在"延迟连线" `dlink`：在**全部节点加载完成之后**才解析目标，适合引用尚未加载的节点。

## Extension 作者的节点放置机制

这是本页的重点：如何让扩展中的节点出现在**你想要的位置**。

### 节点加载入口

扩展模式启动时（`Settings.IsInExtensionMode`），游戏会遍历 `Extensions/<扩展名>/Nodes/` 目录下的**所有 `.xml` 文件**，每个文件作为一个 `Computer` 加载进网络地图：

- 根元素为 `<Computer>`（首字母大写）。
- 可选属性：`id`（节点 ID）、`name`、`ip`（不写则随机生成）、`security`（安全等级）、`type`（节点类型）、`icon`（自定义图标）、`allowsDefaultBootModule`。

### 定位：`positionNear`

`positionNear` 是 Extension 作者最常用的定位手段——让本节点环绕在**另一个已知节点**周围：

```xml
<Computer id="myNode" name="My Node" ip="10.0.0.1" security="3">
  <positionNear target="someHubNode" position="2" total="6" />
</Computer>
```

| 属性 | 含义 | 默认 |
|---|---|---|
| `target` | 参照节点（按 id / IP / 名称匹配） | 必填 |
| `position` | 本节点在环绕序列中的序号（从 1 开始） | `1` |
| `total` | 环绕总数（决定每圈几个节点） | `3` |
| `force` | 是否强制使用该位置 | `false` |
| `extraDistance` | 额外偏移距离（-1 ~ 1） | `0` |

环绕计算来自 `Corporation.getNearbyNodeOffset`：节点会以 `COMPUTER_SEPERATION`（约 0.066，归一化单位）为半径、按角度均匀分布在 `target` 周围，Y 方向额外拉伸（`Y_ASPECT_RATIO_BIAS = 1.9`，适配地图纵向比例）。当节点数量超过一圈时，下一圈自动增大半径。若 `force` 为 false 且该位置与已有节点重叠，会尝试其他位置。

> 内置服务器（如 CSEC 契约中心的资产服务器）就是用它排列的：`ContractHubAssetsComp.xml` 中的 `<positionNear target="mainHub" position="5" total="6" />`。

### 连线：`link` 与 `dlink`

```xml
<Computer id="hub" name="Hub" ip="10.0.0.2" security="2" />
<Computer id="leaf" name="Leaf" ip="10.0.0.3" security="1">
  <link target="hub" />   <!-- 立即解析，目标必须已加载 -->
</Computer>
```

如果被连的节点可能尚未加载，使用 `dlink`（延迟到所有节点加载完后解析）：

```xml
<Computer id="leaf" name="Leaf" ip="10.0.0.3" security="1">
  <dlink target="hub" />
</Computer>
```

### 初始可见节点

节点加载后默认不可见。在 `ExtensionInfo.xml` 中通过 `StartingVisibleNodes` 指定**开局即揭示**的节点 ID（逗号分隔）：

```xml
<StartingVisibleNodes>myNode,leaf</StartingVisibleNodes>
```

剧情中再通过 `SAShowNode` 动作逐批揭示其余节点。

### 自定义节点图标（icon）

`<Computer>` 的 `icon` 属性可为节点指定连接界面显示的 Logo 图片。`DisplayModule` 内置以下可选图标：

`laptop`、`chip`、`kellis`、`tablet`、`ePhone`、`ePhone2`，以及 DLC（Labyrinths）限定：`Psylance`、`PacificAir`、`Alchemist`、`DLCLaptop`。

```xml
<Computer id="kellisComp" name="Kellis" ip="..." security="5" icon="kellis" />
```

> 该图标影响**连接界面**（进入节点后顶部的计算机形象），网络地图上的节点本身始终绘制为圆形节点图标。

### 最小示例

下面是一个完整的迷你网络：1 个中枢 + 2 个环绕节点，开局全部可见：

```xml
<!-- Nodes/Hub.xml -->
<Computer id="hub" name="Main Hub" ip="10.0.0.2" security="2" />

<!-- Nodes/LeafA.xml -->
<Computer id="leafA" name="Leaf A" ip="10.0.0.3" security="1">
  <positionNear target="hub" position="1" total="3" />
  <dlink target="hub" />
</Computer>

<!-- Nodes/LeafB.xml -->
<Computer id="leafB" name="Leaf B" ip="10.0.0.4" security="1">
  <positionNear target="hub" position="2" total="3" />
  <dlink target="hub" />
</Computer>
```

```xml
<!-- ExtensionInfo.xml -->
<StartingVisibleNodes>hub,leafA,leafB</StartingVisibleNodes>
```

## 相关页面

[核心机制](./Mechanics.md) · [服务器速查表](./Servers.md) · [Extension 项目结构](../extension-tutorial/structure.md)