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

### 链式作画：用 positionNear 搭图形

`positionNear` 的计算是**相对**的：最终坐标 = 目标节点坐标 + 一个偏移向量。因此可以让节点依次指向"上一个节点"，把它们像贪吃蛇一样连起来，从而在 Scatter 模式下画出线条、折线和环。

偏移的规律：

- **角度**：`position / total` 决定方向，会被量化到 **30 个固定方位槽**（`position` 值内部会先 +1，所以 `total=1` 表示正右方 0°，依次向顺时针方向排布）。`total=30` 时正好每槽 `1/30` 圈。
- **距离**：`extraDistance + 0.066`（归一化单位），每多绕一圈额外加 `0.04`。
- **Y 拉伸**：偏移的 Y 分量会被乘以 `1.9`，所以图形纵向会被拉高，需预留余量。
- **`force="true"`**：跳过碰撞检测，保证每次落点确定、不会随机回退（这是作画的关键）。
- **顺序无关**：`positionNear` 通过 `postAllLoadedActions` 延迟到**所有节点加载完后**统一解析，所以参照节点写在后面也没关系，可以放心链式引用。

#### 示例：水平直线

三个节点依次向右排开，间距约 `0.106` 个归一化单位：

```xml
<Computer id="dot1" name="Dot 1" ip="10.0.0.1" security="1">
  <positionNear target="hub" position="1" total="1" force="true" />
</Computer>
<Computer id="dot2" name="Dot 2" ip="10.0.0.2" security="1">
  <positionNear target="dot1" position="1" total="1" force="true" />
</Computer>
<Computer id="dot3" name="Dot 3" ip="10.0.0.3" security="1">
  <positionNear target="dot2" position="1" total="1" force="true" />
</Computer>
```

#### 示例：折线与环

- **折线**：中途换一个 `position/total` 组合改变方位槽，例如 `position="8" total="30"` 大约向下转 96°。
- **环**：以中枢为 `target`，`total=6`、`position=1..6` 即可得到六边形一圈；多圈用更大的 `position`（内部会递增圈数，半径随之扩大）。

#### 限制

- 方位被锁定在 30 个槽，**无法**摆出任意精确角度（例如精确的 90° 垂直）；更自由的坐标只能通过 C# 代码设置 `Computer.location` 完成。
- 只有 **Scatter** 排序模式会尊重这些坐标；玩家若在游戏里用 NetmapOrganizer 切到 Grid / LockGrid / Chaos，图形会被重新排布打乱。
- 地图边界为 0~1，偏移超出边界时 `force="true"` 也会失效并随机回退，作画范围别铺太大。

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